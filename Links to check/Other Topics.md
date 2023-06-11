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

**Crypto and TLS**

- [TLS best practices by Mozilla](https://wiki.mozilla.org/Security/Server_Side_TLS)

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