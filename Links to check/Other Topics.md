
**LMDB**

- Maintained by [Symas](https://www.symas.com/). Official website at https://www.symas.com/lmdb.
- The [git repository](https://git.openldap.org/openldap/openldap/tree/mdb.master) is the `mdb.master` branch of the [OpenLDAP git repository](https://git.openldap.org/openldap/openldap) (located in a self-managed GitLab installation at https://git.openldap.org).
- There is a Python binding at https://github.com/jnwatson/py-lmdb, with official documentation at https://lmdb.readthedocs.io. It is also maintained as a Debian package with the name [python3-lmdb](https://packages.debian.org/en/stable/python/python3-lmdb).

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

**Go internals**

- [https://readablecode.hashnode.dev/gos-channels-and-pointers](https://readablecode.hashnode.dev/gos-channels-and-pointers)
- [https://dave.cheney.net/2017/04/29/there-is-no-pass-by-reference-in-go](https://dave.cheney.net/2017/04/29/there-is-no-pass-by-reference-in-go)
- [https://medium.com/eureka-engineering/understanding-allocations-in-go-stack-heap-memory-9a2631b5035d](https://medium.com/eureka-engineering/understanding-allocations-in-go-stack-heap-memory-9a2631b5035d)

---

**About hostname and FQDN configuration**

Motivation:

Trying to figure out the reason of the Rsyslog bug I needed to undestand what does the `gethostname` and `getaddrinfo` functions did and what the returned information mean.

Links:

- https://unix.stackexchange.com/questions/186859/understand-hostname-and-etc-hosts
- https://superuser.com/questions/394816/what-is-the-difference-and-relation-between-host-name-and-canonical-name

Questions:

- Difference between `/etc/hostname` and `/etc/hosts`
- Difference between the `hostname` and `hostnamectl` programs
- Try to formulate a "formal" (maybe mathematical) description of when an IP reverse-resolve consistently and how the canonical name enters into the picture

---

**About the PEM format**

Motivation:

I implemented the parsing of the PEM formatted certificates in `jobpipe-certbot`. This made me want to learn more about the PEM format.

Links:

- https://stackoverflow.com/questions/5355046/where-is-the-pem-file-format-specified
- https://medium.com/@yashschandra/anatomy-of-a-pem-file-727f1690df18

---

**About Git and GitHub**

There are still many Git concepts that I don't fully grasp. For example, what is the difference between a remote branch and a local branch? what is `HEAD`? what is the difference between `HEAD~1` and `HEAD^1`? what is a `refspec` and many other terminology that appears in the help pages?... and the list goes on...

There are also many concepts that emerge from Git, like the [workflows](https://www.atlassian.com/git/tutorials/comparing-workflows), that I also don't fully grasp (this emergence phenomenon is kind of like how the concept of containers emerge from the Linux kernel, but the Linux kernel itself doesn't know anything about containers).

Also I am very unfamiliar with many GitHub workflows like Pull Requests. It would be interesting to create two temporary GitHub accounts and try to replicate a full Pull Request.

---

**About cache coherence (in general and in the context of NFS)**

- The concept of cache coherence is explained in the corresponding [Wikipedia article](https://en.wikipedia.org/wiki/Cache_coherence).
- The `nfs(5)` man page has [a section](https://www.man7.org/linux/man-pages/man5/nfs.5.html#DATA_AND_METADATA_COHERENCE) on data and metadata cache coherence.

---

**About the `flock`/`fnctl` conflict in NFS**

- The `flock(2)` man page has [a section](https://www.man7.org/linux/man-pages/man2/flock.2.html#HISTORY) on how NFS emulates `flock` using `fnctl`.
- See [this blog post](https://utcc.utoronto.ca/~cks/space/blog/linux/FlockFcntlAndNFS).

---

**User sessions in Linux**

- systemd's [JSON User Records](https://systemd.io/USER_RECORD/)
- https://askubuntu.com/questions/294736/run-a-shell-script-as-another-user-that-has-no-password
- https://unix.stackexchange.com/questions/50665/what-are-the-differences-between-interactive-non-interactive-login-and-non-lo
- https://wiki.debian.org/SystemGroups

---

**Misc**

- [Why an empty .vimrc file removes the default Vim configuration](https://vi.stackexchange.com/questions/33154/why-does-an-empty-vimrc-file-change-my-configuration-e-g-disable-syntax-highli). Thus it is advisable to _always_ prepend the line `source $VIMRUNTIME/defaults.vim` to the `.vimrc` file.
- [htop explained | peteris.rocks](https://peteris.rocks/blog/htop/)
- [Google SRE Books](https://sre.google/books/)
- [Linux from scratch](https://www.linuxfromscratch.org/)