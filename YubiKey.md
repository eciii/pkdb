Yubico ([homepage](https://www.yubico.com/)) is the company behind the YubiKey. The products offered by Yubico (as of March/2023) are:

- YubiKey 5 Series: The main product line. Available in all form-factors mentioned below.
- YubiKey 5 FIPS Series: Exactly like the YubiKey 5 Series but FIPS 140-2 certified.
- Security Key Series: like the YubiKey 5 Series but:
  - Only supports FIDO standards.
  - Available only in the first two form-factors (USB-A/C + NFC).
- YubiKey Bio Series: Like the YubiKey 5 Series but:
  - Includes fingerprint scanner
  - Available only in the first two form-factors but without NFC support.
  - Only supports FIDO standards.

YubiKey 5 CSPN Series
YubiHSM 2 & YubiHSM 2 FIPS

Form-factors:

    USB-A + NFC    |    USB-C + NFC
    -              |    USB-C + Lightning
    -              |    USB-C
    USB-A (Nano)   |    USB-C (Nano)


[Developer resources](https://developers.yubico.com/), [GitHub](https://github.com/Yubico).

---

The following is a list of Ubuntu packages whose homepage is 

---

**Current instructions for LUKS in Ubuntu**

 This will setup a YubiKey slot in LUKS:
 
 - Install the `yubikey-luks` package (this will also install `yubikey-personalization`)
 - Plug in the YubiKey and run `ykpersonalize -2 -ochal-resp -ochal-hmac -ohmac-lt64 -oserial-api-visible`
 - Run `sudo cryptsetup luksAddKey --key-slot *temporary-slotnumber* /dev/*partition*` (this is just to create a temporary password to be removed later) and type a temporary password.
 - Run `sudo yubikey-luks-enroll -d /dev/*partition* -s *permanent-slotnumber* -c` and use the temporary password to create the actual slot with the actual password.
 - Run `sudo cryptsetup luksRemoveKey /dev/*partition*` and remove the temporary password.
 - Adjust `/etc/crypttab` with `*partition-name* UUID=*uuid* none luks,keyscript=/usr/share/yubikey-luks/ykluks-keyscript`
 - Run `sudo update-initramfs -u`
 - Reboot and test

---

**Links**

- Fedora Magazine article: [How to use a YubiKey with Fedora Linux](https://fedoramagazine.org/how-to-use-a-yubikey-with-fedora-linux/)
- LWN article about a FOSSDEM 2023 talk: [Passwordless authentication with FIDO2—beyond just the web](https://lwn.net/Articles/923656/)
- [Blog post](https://www.guyrutenberg.com/2022/02/17/unlock-luks-volume-with-a-yubikey/) with some interesting instructions for LUKS. The comments are also interesting.
- [Blog post](https://subshell.com/de/blog/authentifizierung-yubikey100.html) (in german) also about instructions for LUKS.