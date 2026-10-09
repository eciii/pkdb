
**Some terminology**

- **_The_ DNS:** the global Domain Name System (DNS) on _the_ Internet
- **EITIE:** Enterprise IT Infrastructure Environment

**Questions**

- How does the concept of inventory relates to all of this?

---

Named entities are (typed) entities in an EITIE that are commonly identified by name. The most prevalent named entity types across EITIEs are hosts and users. Software that want to use named entities have to first look them up using their name. For a given entity type, this lookup process might require to search on several data sources according to some rules. Thus the lookup process is, more concretely, a resolution algorithm. A component that implements any such resolution algorithm for a given entity type is called a resolver for that type.

Another very prevalent named entity type in EITIEs is the type that represents IP addresses. In fact, the global Internet infrastructure has already a very successful system for the conversion of names into IP addresses called Domain Name System (DNS). This system is so flexible and versatile that EITIEs can relatively easily implement local DNSes and integrate them with the global DNS (similar to how local intranets can be implemented and integrated into the global Internet).

---

The Name Service Switch (NSS) is a standarized component of the C standard library that provides a pluggable mechanism to configure resolution algorithms for a given set of entity types, including users and hosts.

Entity types in NSS are called _databases_. The resolution algorithm of a database consists of an ordered sequence of _services_ (these are the data sources). Each service is implemented by a shared object module that is loaded on runtime. NSS already provides some services but other services are offered by external projects (like systemd for example).

Each service must provide a standard interface required by NSS. This interface is fairly simple: for a service of a given entity type, given a name, the service must return a (possibly empty) collection of entities of that type that match the given name. The resolution algorithm searches the name on each service in the sequence until a non-empty result is found, which is then returned as the result of the resolution.

NSS also allows to specify a limited set of rules regarding the behavior of the algorithm when an error occurs, or when an empty collection is returned by a given service.

The configuration of NSS is located in the single file `/etc/nsswitch.conf`, which has a very specific syntax.

---

The NSS database that handles the resolution of IP addresses is the `hosts` database. The resolution algorithm for the `hosts` database is exposed by the function `getaddrinfo`, which replaces the now deprecated function `gethostbyname`.

NSS provides two services for the `hosts` database: `files` and `dns`. The `files` service queries the information contained in the `/etc/hosts` file, whereas the `dns` service perform actual DNS queries to the recursive DNS servers listed in `/etc/resolv.conf`.

The systemd project also provides services for the `hosts` database of NSS: `resolve`, `mymachines` and `myhostname`.

The `resolve`service queries the `systemd-resolved` system service, which provides an integrated _and cached_ mechanism to query:

- other recursive DNS servers but with support for DNSSEC and DNS over TLS
- Multicast DNS (mDNS)
- Link-Local Multicast Name Resolution (LLMNR)

---

In theory the `resolve` service provided by systemd should be enough to replace the `dns` service provided by NSS, thus rendering the file `/etc/resolv.conf` useless. But in practice there are two issues:

- Some existing software performs IP address resolution bypassing NSS altogether by directly performing DNS queries to the recursive DNS servers listed in `/etc/resolv.conf`. To force such software to use `systemd-resolved` a stub DNS server is provided by systemd which must be configured as the only DNS server in `/etc/resolv.conf`.
- It can be argued that the `resolve` service can fail more easily because it relies on a system service to be running. Thus some recommend to still configure the `dns` service in the `hosts` database as a last resort. However this argument is a bit controversial: one can consider the `systemd-resolved` system service as an "essential" system service that _must_ be running at all times, the same way one considers the init service of even the kernel as essential.

---

In environments like data centers the network configuration, and thus also the DNS configuration, almost never change. In laptops, on the other hand, the network/DNS configuration must be updated every time the laptop is moved to a different "network environment" (e.g a different office, restaurant or home).

This problem is mostly solved by [[Networking - DHCP|DHCP]]. In general DHCP clients always adjust DNS configuration as part of their automatic network configuration:

- In systems that use the traditional `dns` service provided by NSS for IP address resolution, DHCP clients automatically adjust the `/etc/resolv.conf` file every time the network configuration changes.
- In systems that use the newer `resolve` service provided by systemd, DHCP clients should be able to adjust the `systemd-resolved` service automatically (e.g by using DBus).

This should be the case for the two most common DHCP client implementations nowadays: systemd-networkd and Network Manager.

---

A simpler and somewhat older mechanism for managing the `/etc/resolv.conf` file was provided by the _resolvconf_ program. There are some implementations of this program:

- [resolvconf](https://salsa.debian.org/debian/resolvconf): The original implementation.
- openresolv: A newer implementation by [[Networking (landscape)#^RoyMarples|RoyMarples]].
- systemd-resolved: A limited just-for-compatibility implementation provided by systemd.

---

**Links**

- https://en.wikipedia.org/wiki/Domain_Name_System
- https://en.wikipedia.org/wiki/Root_name_server
- https://en.wikipedia.org/wiki/DNS_zone
- https://wiki.archlinux.org/title/Domain_name_resolution
- https://wiki.archlinux.org/title/Systemd-resolved
- https://man.archlinux.org/man/systemd-resolved.8
- https://curiousprogrammer.net/posts/2023-10-31-dns-recursive-resolution
- https://serverfault.com/questions/309622/what-is-a-glue-record

---

**Formal concepts around the meaning of A/AAAA and PTR records in local DNSes in EITIEs**

I'll consider the meaning of A and PTR records but all the ideas should be valid if we replace A records with AAAA records.

In DNS, A records offer a way to map names to IP addresses. Let `NAMES` be the set of all names currently in use and `IPs` be the set of all IP addresses available in the network. One might think naively that A records define a map/function form `NAMES` to `IPs`, but that's not actually the case. The DNS standard allows for multiple A records to have the same name, so long they have different values (i.e they point to different IP addresses). Thus what A records actually define is a _relation_ between `NAMES` and `IPs`, i.e a subset of `NAMES x IPs`.  And the same happens for PTR records: these records define a relation between `IPs` and `NAMES`. Let `A` be the relation on `NAMES x IPs` defined by the A records and let `PTR` be the relation on `IPs x NAMES` defined by the PTR records.

We say that the `A` and `PTR` relations agree if `PTR` is the [converse](https://en.wikipedia.org/wiki/Converse_relation) of `A`. In such a case we then talk about _the_ `A/PTR`relation (over `NAMES x IPs`), since both relations are effectively the same.

We can now formally state a very important principle:

> There is absolutely no point in using PTR records if they don't agree with the corresponding A records. In such a case one might as well not use PTR records at all.

This means that EITIEs have two options:

- Not use PTR records at all.
- Use PTR records but ensure that they agree with the A records at all times.

Now let's assume that we have an `A/PTR` relation. This condition alone allows for very wild configurations like:

- Every name is associated with every IP address and vice versa (i.e the `A/PTR` relation is `NAMES x IPs`).
- Every subgroup of `IPs` is associated with a different name.

Such configurations make no sense in general but when applied to small subsets of `NAMES` and `IPs` they might have real-world use-cases like load-balancing and redundancy. However, for the vast majority of cases in EITIEs a simple 1-to-1 correspondence between names and IPs is enough.

Thus tools for A and PTR record management in EITIEs should by default:

- Either not use PTR records at all or ensure PTR records agree with A records at all times.
- Assume the user wants a simple 1-to-1 correspondence between names and IP addresses, but also give options to handle more complex use-cases for specific sets of names and IP addresses.