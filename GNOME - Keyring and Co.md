
**About Secret Management in GNOME**

The core component for secret management in GNOME is the _GNOME Keyring_. This is the component that performs the actual encryption and storage of secrets. However this component is not meant to be used directly. But to understand how applications should use it there is another concept that needs to be understood first. The security model offered by the secret Secret Management "subsystem" of GNOME distinguishes between two types of applications (see [here](https://gitlab.gnome.org/GNOME/gnome-keyring/-/issues/5#note_1876550) for more information):

- Sandboxed applications. These are applications that are sandboxed via Flatpak, Snap or other sandboxing mechanism. They are unprivileged and thus access to the desktop system and its subsystems is restricted.
- Native applications. These are applications that are not sandboxed and thus are treated as fully privileged with full access to the desktop system and its subsystems under the user's scope.

The secret management service offered by GNOME Keyring is meant to be used by sandboxed applications through a specific _XDG Desktop Portal_, namely the _Secret portal_. In this case the GNOME Keyring implements the _portal backend_ required by the Secret portal. In contrast, native applications should use the _Secret Service API_, a D-Bus API standard, developed under the stewardship of freedesktop.org, that is meant to be Desktop System agnostic, at least for Desktop Systems that are "freedesktop.org-compliant".

The _libsecret_ library provides access to the GNOME Keyring using both of the above mentioned mechanisms. The library also provides a very primitive CLI tool, `secret-tool`, to interact with the GNOME Keyring through the libsecret library.

The GNOME project also provides a GUI application for secrets management called _Seahorse_ and branded as _Passwords and Secrets_.