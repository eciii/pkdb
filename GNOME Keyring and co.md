
**Internet resources**

Homepage
Documentation

Git repositories (read-only)

- Bare (self-hosted)
- Browser
	- cgit (self-hosted)
	- GitLab (self-hosted or as-a-service)
	- GitHub (as-a-service)

Email-Patch / Pull-Request
Issue tracker

In general an _open software project_ required the following resources available on the internet:
- Source code
- Discussions:
	- Help on Usage: For users that intend to "just use" the software and don't have any a priori motivation to contribute back.
	- Actual issues with the software -> Might lead to fixes (patches)
	- Enhancement proposals -> Might lead to enhancements (patches)

Source code availability: In the old days source code was made available using _versioned released trees_. However this practice was inconvenient for several reasons (elaborate this!) which led to the emergence of _source code management_ tools such as Git.

**Links**

Discussions:
- Mailing lists:
	- [Mailman](https://www.gnu.org/software/mailman/index.html): Used by many projects, for example those under freedesktop.org.
	- [Patchwork](http://jk.ozlabs.org/projects/patchwork/): Used by the Linux Kernel project.
- Instant messaging:
	- XMPP/Jabber
	- IRC
	- Mastodon
	- Matrix
- Others:
	- Mastodon
	- Discourse


**About freedesktop.org**

[freedesktop.org](https://www.freedesktop.org) (formerly _X Desktop Group_, a.k.a _XDG_) is an organization that provides hosting services to FOSS projects focused on interoperability and shared technology for graphical and desktop systems. In particular it offers:

- Git services through a self-hosted GitLab instance at https://gitlab.freedesktop.org.
- Mailing lists services through a self-hosted Mailman instance at https://lists.freedesktop.org.

_Note about the website:_ While the official freedesktop.org website is https://www.freedesktop.org (the root path `/` redirects to `/wiki/`) there is also the subdomain https://specifications.freedesktop.org/ that seems to be still valid and in use. However I was unable to find any mention of the subdomain in the official website!

**About XDG Desktop Portal**

[XDG Desktop Portal](https://flatpak.github.io/xdg-desktop-portal/) is a subproject of [Flatpak](https://flatpak.org/).

_Note about the website:_ The website is under the domain flatpak.github.io, which is not the official Flatpak domain.

**About Secret Management in GNOME**

The core component for secret management in GNOME is the _GNOME Keyring_. This is the component that performs the actual encryption and storage of secrets. However this component is not meant to be used directly. But to understand how applications should use it there is another concept that needs to be understood first. The security model offered by the secret Secret Management "subsystem" of GNOME distinguishes between two types of applications (see [here](https://gitlab.gnome.org/GNOME/gnome-keyring/-/issues/5#note_1876550) for more information):

- Sandboxed applications. These are applications that are sandboxed via Flatpak, Snap or other sandboxing mechanism. They are unprivileged and thus access to the desktop system and its subsystems is restricted.
- Native applications. These are applications that are not sandboxed and thus are treated as fully privileged with full access to the desktop system and its subsystems under the user's scope.

The secret management service offered by GNOME Keyring is meant to be used by sandboxed applications through a specific _XDG Desktop Portal_, namely the _Secret portal_. In this case the GNOME Keyring implements the _portal backend_ required by the Secret portal. In contrast, native applications should use the _Secret Service API_, a D-Bus API standard, developed under the stewardship of freedesktop.org, that is meant to be Desktop System agnostic, at least for Desktop Systems that are "freedesktop.org-compliant".

The _libsecret_ library provides access to the GNOME Keyring using both of the above mentioned mechanisms. The library also provides a very primitive CLI tool, `secret-tool`, to interact with the GNOME Keyring through the libsecret library.

The GNOME project also provides a GUI application for secrets management called _Seahorse_ and branded as _Passwords and Secrets_.

---

I'm getting an error when I try to use `secret-tool`. The error is described in this [SE post](https://unix.stackexchange.com/questions/473528/how-do-you-enable-the-secret-tool-command-backed-by-gnome-keyring-libsecret-an). As a solution I think the following should work:

- Create a _custom_ keyring for root (i.e not the default login keyring)
- Users of a given group should be able to access the keyring with a combination of `sudo` and `dbus-send`.