### FIDO2/WebAuthn

(_TODO_)

### FIDO U2F

(_TODO_)

### PIV Smart card

PIV stands for Personal Identity Verification. It is an acronym used by the United States federal government in the FIPS 201 standard that specifies PIV requirements for federal employees and contractors.

Smart cards are typically plastic credit-card-sized cards with an embedded integrated circuit chip that stores one or more PKI certificates that identify the user. Smart cards are typically used for PIV.

### OTP (TOTP/HOTP)

OTP stands for One-Time Password. An OTP is a password that is valid for only one login session or transaction.

OTPs are accessed by the user using one of the following mechanisms:

- By means of a special electronic device that is carried by the user. The device generates OTPs and shows them using a small display.
- By means of software that runs on the user's mobile phone. The software generates OTPs and shows them.
- By means of an out-of-band channel (such as SMS messaging) through which the OTPs are sent to the user by a server.
- By means of a list of OTPs, printed on paper, that the user is required to carry.

All OTP access mechanisms require the user to possess something, making OTP a form of 2FA.

OTPs are generated using special algorithms which can be broadly categorized as:

- [Time-based] Based on time-synchronization between the authentication server and the client (OTPs are valid only for a short period of time).
- [Challenge-based] Using a mathematical algorithm where the new password is based on a challenge (e.g. a random number or random pattern chosen by the authentication server or transaction details).
- [Counter-based] Using a mathematical algorithm where the new password is based on a counter. This requires a mechanism for the server and client to synchronize the counter.
- [Chain-based] Using a mathematical algorithm to generate a new password based on the previous password (OTPs are effectively a chain and must be used in a predefined order).

Standardized OTP mechanisms exists, for example:

- RFC 1760 (S/KEY) [Chain-based]
- RFC 2289 (OTP) [Chain-based?]
- RFC 4226 (HOTP) [Counter-based]
- RFC 6238 (TOTP) [Time-based]

The HOTP and TOTP standards were developed by the Initiative for Open
Authentication (OATH). OATH is an industry-wide collaboration to develop open standards to promote the adoption of strong authentication.

### OpenPGP

OpenPGP is a data encryption standard defined by the OpenPGP Working Group of the Internet Engineering Task Force (IETF). It was originally derived from the PGP software, created by Phil Zimmermann.

OpenPGP is the most widely used email encryption standard.

OpenPGP also defines a standard format for certificates which, unlike most other certificate formats, enables "webs of trust".

The standard is defined in multiple IETF RFCs:

- RFC 4880: OpenPGP Message Format (the main specification)
- RFC 3156: MIME Security with OpenPGP
- RFC 6637: Elliptic Curve Cryptography (ECC) in OpenPGP
- RFC 6091: Using OpenPGP Keys for Transport Layer Security (TLS) Authentication
- RFC 5581: The Camellia Cipher in OpenPGP

Multiple proposed extensions/modifications that are still IETF drafts can be found at: [https://www.openpgp.org/about/standard/](https://www.openpgp.org/about/standard/)