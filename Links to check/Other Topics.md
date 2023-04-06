**Performance of regexps in Go**

See issues [26623](https://github.com/golang/go/issues/26623) and [11646](https://github.com/golang/go/issues/11646).

**Network Time Protocol (NTP)**

- General theory
- Relevant implementations: ntpd (reference implementation), chrony, systemd-timesyncd
- More info:
  - [https://kb.vmware.com/s/article/1006427](https://kb.vmware.com/s/article/1006427)
  - https://anarc.at/blog/2022-01-23-chrony/

**`/etc/shadow`**

See https://www.baeldung.com/linux/shadow-passwords

- Format of the file
- Supported encryption algorithms: sha512, yescrypt, etc…
- Tools to manage it: passwd, chpasswd, chage
- Libraries that interface with it: pam, crypt(3)
- Tools to encrypt/decrypt: openssl passwd, perl crypt, python crypt

**Terminals, consoles and ttys**

- History of computer terminals.
- The difference between a terminal and a console is a bit subtle. A console is just a special type of terminal which is incorporated in the computer and is assumed to be always available. Physical access to the computer is required to access its console. Consoles are usually granted special privileges and are dedicated to administrative tasks.
- Nowadays physical terminals are rarely used. We have instead virtual terminals and pseudo-terminals. More information about these can be found in:
	- Some chapters of The Linux Programming Interface
	- The systemd documentation

**About Debian and binary blobs**

- [https://lwn.net/Articles/906380/](https://lwn.net/Articles/906380/)
- [https://en.wikipedia.org/wiki/Binary_blob](https://en.wikipedia.org/wiki/Binary_blob)