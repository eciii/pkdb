
_Some terminology_

- **The DNS** refers to the global Domain Name System (DNS) on the Internet

_Client side_

Client-side software that wants to access/query the DNS should do it through a component called **resolver**. The resolver:

- is located in the C standard library (libc)
- its consumer-facing interface is composed by the single function `getaddrinfo`, which replaces the now deprecated function `gethostbyname`