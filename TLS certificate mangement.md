### General theory

(TODO)

### Linux implementation

* Debian comes with at least to TLS libraries by default: libssl (from the [OpenSSL](https://www.openssl.org) project) and libgnutls (from the [GnuTLS](https://www.gnutls.org) project).
* There are many other TLS libraries out there (Wikipedia has a list [here](https://en.wikipedia.org/wiki/Comparison_of_TLS_implementations)). Each one of those might use in turn one or more separate libraries for cryptographic operations (Wikipedia has a list of cryptographic libraries [here](https://en.wikipedia.org/wiki/Comparison_of_cryptography_libraries)). For example, in Debian, OpenSSL uses libcrypto (also from the OpenSSL project) while GnuTLS uses libnettle and libhogweed (both from the [Nettle](https://www.lysator.liu.se/~nisse/nettle/) project).
* When establishing a TLS connection a TLS library needs a list of certificates to trust (these might be either certificates of trusted CAs or trusted self-signed certificates).
* Each TLS library might have a different set of conventions about where to search for TLS certificates in the system. It is the job of the Linux distribution to align those conventions into a unified set of conventions, which then become the official set of conventions used by the Linux distribution. Unfortunately this alignment is usually not perfect, and the user ends up having to learn all the conventions used by each of the libraries *and* the convention used by the Linux distribution.