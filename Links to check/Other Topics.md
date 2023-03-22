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

**About Debian and binary blobs**

- [https://lwn.net/Articles/906380/](https://lwn.net/Articles/906380/)
- [https://en.wikipedia.org/wiki/Binary_blob](https://en.wikipedia.org/wiki/Binary_blob)