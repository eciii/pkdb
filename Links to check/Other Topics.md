**Performance of regexps in Go**

See issues [26623](https://github.com/golang/go/issues/26623) and [11646](https://github.com/golang/go/issues/11646).

---

**Network Time Protocol (NTP)**

- General theory
- Relevant implementations: ntpd (reference implementation), chrony, systemd-timesyncd
- More info:
  - [https://kb.vmware.com/s/article/1006427](https://kb.vmware.com/s/article/1006427)
  - https://anarc.at/blog/2022-01-23-chrony/
  - [Paul's Blog](https://www.libertysys.com.au/) and [this answer](https://serverfault.com/questions/1128302/does-installing-ntp-mean-im-installing-an-ntp-server) from him

---

**`/etc/shadow`**

See https://www.baeldung.com/linux/shadow-passwords

- Format of the file
- Supported encryption algorithms: sha512, yescrypt, etc…
- Tools to manage it: passwd, chpasswd, chage
- Libraries that interface with it: pam, crypt(3)
- Tools to encrypt/decrypt: openssl passwd, perl crypt, python crypt

---

**Virtualized networking**

- [kvm - virt-manager: is it possible to assign specific IP addresses to certains VMs via the virtual DHCP? - Unix & Linux Stack Exchange](https://unix.stackexchange.com/questions/174884/virt-manager-is-it-possible-to-assign-specific-ip-addresses-to-certains-vms-via)
- [libvirt: Network XML format](https://libvirt.org/formatnetwork.html#elementsAddress)
- [Networking - Libvirt Wiki](https://wiki.libvirt.org/page/Networking)
- [libvirt Networking Handbook — Jamie Nguyen](https://jamielinux.com/docs/libvirt-networking-handbook/)
- [BridgeNetworkConnections - Debian Wiki](https://wiki.debian.org/BridgeNetworkConnections)
- [iproute2 - Wikipedia](https://en.wikipedia.org/wiki/Iproute2)
- [iproute2/iproute2.git - Iproute2 routing commands and utilities](https://git.kernel.org/pub/scm/network/iproute2/iproute2.git/)
- [Canonical Netplan](https://netplan.io/)
- [rtnetlink(7) - Linux manual page](https://www.man7.org/linux/man-pages/man7/rtnetlink.7.html)

---

**Crypto and TLS**

- [TLS best practices by Mozilla](https://wiki.mozilla.org/Security/Server_Side_TLS)
- [Signing JWTs with Go's crypto/ed25519 - Blain Smith](https://blainsmith.com/articles/signing-jwts-with-gos-crypto-ed25519/)
- [x/crypto/ssh: please provide unified parameter types for ed25519.PrivateKey · Issue #51974 · golang/go · GitHub](https://github.com/golang/go/issues/51974)
- [OpenSSH public key file format? - Super User](https://superuser.com/questions/1477472/openssh-public-key-file-format)
- [A Graduate Course in Applied Cryptography](https://toc.cryptobook.us/)
- [Introduction](https://nacl.cr.yp.to/index.html)
- [Introduction - libsodium](https://doc.libsodium.org/)

---

**Terminals, consoles and ttys**

- History of computer terminals.
- The difference between a terminal and a console is a bit subtle. A console is just a special type of terminal which is incorporated in the computer and is assumed to be always available. Physical access to the computer is required to access its console. Consoles are usually granted special privileges and are dedicated to administrative tasks.
- Nowadays physical terminals are rarely used. We have instead virtual terminals and pseudo-terminals. More information about these can be found in:
	- Some chapters of The Linux Programming Interface
	- The systemd documentation

---

**Linux Power Management**

- https://unix.stackexchange.com/questions/484550/pm-suspend-vs-systemctl-suspend
- https://wiki.archlinux.org/title/Power_management
- https://www.freedesktop.org/software/systemd/man/systemd-sleep.html
- https://www.kernel.org/doc/html/v4.15/admin-guide/pm/sleep-states.html

---

**Email**

GNU Mailutils (MUA ???, IMAP/POP3 client among others)
Postfix (MTA, in particular SMTP client and server)
Procmail (MDA)
Dovecot (IMAP/POP3 server (for mail storage))

---

**Linux Networking**

- The [personal website of Martin A. Brown](http://linux-ip.net/) has multiple documentation resources about linux networking (maybe a bit outdated but worth checking out). In particular there is the [Guide to IP Layer Network Administration With Linux](http://linux-ip.net/pages/the-guide.html).
- The [personal website of David Guyton](https://datahacker.blog/) has multiple documentation resources about very disparate technology stuff. In particular the [section about linux networking](https://datahacker.blog/industry/technology-menu/networking) seems worth checking out.

---

**About IdM, FreeIPA and SSSD**

**IdM** stands for **Identity Management**. The most mature IdM solution in the Linux ecosystem is [FreeIPA](https://www.freeipa.org), a software suite promoted and maintained by Red Hat.

Links to check:

- Apart from the main documentation in the [official website of SSSD](https://sssd.io/) there seems to be also a very good documentation of SSSD [here](https://docs.pagure.org/sssd.sssd/index.html).

---

**Go internals**

- [https://readablecode.hashnode.dev/gos-channels-and-pointers](https://readablecode.hashnode.dev/gos-channels-and-pointers)
- [https://dave.cheney.net/2017/04/29/there-is-no-pass-by-reference-in-go](https://dave.cheney.net/2017/04/29/there-is-no-pass-by-reference-in-go)
- [https://medium.com/eureka-engineering/understanding-allocations-in-go-stack-heap-memory-9a2631b5035d](https://medium.com/eureka-engineering/understanding-allocations-in-go-stack-heap-memory-9a2631b5035d)

---

**Misc**

- [Why an empty .vimrc file removes the default Vim configuration](https://vi.stackexchange.com/questions/33154/why-does-an-empty-vimrc-file-change-my-configuration-e-g-disable-syntax-highli). Thus it is advisable to _always_ prepend the line `source $VIMRUNTIME/defaults.vim` to the `.vimrc` file.
- [htop explained | peteris.rocks](https://peteris.rocks/blog/htop/)
- [Google SRE Books](https://sre.google/books/)
- [Product Documentation for Red Hat Enterprise Linux 9 | Red Hat Customer Portal](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/9)
- [Linux from scratch](https://www.linuxfromscratch.org/)