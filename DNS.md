
**Some terminology**

- **_The_ DNS:** the global Domain Name System (DNS) on _the_ Internet
- **EITIE:** Enterprise IT Infrastructure Environment

**Questions**

- How does the concept of inventory relates to all of this?

---

Named entities are (typed) entities in an EITIE that are commonly identified by name. The most prevalent named entity types across EITIEs are hosts and users. Software that want to use named entities have to first lookup them up using their name. For a given entity type, this lookup process might require to search on several data sources according to some rules. Thus the lookup process is, more concretely, a resolution algorithm. A component that implements any such resolution algorithm for a given entity type is called a resolver for that type.

Another very prevalent named entity type in EITIEs is the type that represents IP addresses. In fact, the global Internet infrastructure has already a very successful system for the conversion of names into IP addresses called Domain Name System (DNS). This system is so flexible and versatile that EITIEs can relatively easily implement local DNS systems and integrate them with the global DNS (similar to how local intranets can be implemented and integrated into the global Internet).

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
- It can be argued that the `resolve` service can fail more easily because it relies on a system service to be running. Thus some recommend to still configure the `dns` service in the `hosts` database as a last resort. This argument is anyway a bit controversial: one can start considering the `systemd-resolved` system service as an "essential" system service that _must_ be running at all times, the same way one considers the init service of even the kernel as essential.

---

https://en.wikipedia.org/wiki/Domain_Name_System
https://en.wikipedia.org/wiki/Root_name_server
https://en.wikipedia.org/wiki/DNS_zone
https://curiousprogrammer.net/posts/2023-10-31-dns-recursive-resolution
https://serverfault.com/questions/309622/what-is-a-glue-record
https://wiki.archlinux.org/title/Domain_name_resolution
https://wiki.archlinux.org/title/Systemd-resolved
https://man.archlinux.org/man/systemd-resolved.8
