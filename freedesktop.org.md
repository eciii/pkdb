[freedesktop.org](https://www.freedesktop.org) (formerly _X Desktop Group_, a.k.a _XDG_) is an organization that provides hosting services to FOSS projects focused on interoperability and shared technology for graphical and desktop systems. In particular it offers:

- Dedicated project subdomains.
- Wiki services. Each project can have a wiki hosted at `[project].freedesktop.org/wiki`. The files for the corresponding wiki are still hosted in cgit site under `/wiki/[project]`.
- Git services through a self-hosted GitLab instance at https://gitlab.freedesktop.org. It seems that freedesktop.org used to provide git services using cgit. The cgit repository is still available at https://cgit.freedesktop.org.
- Mailing lists services through a self-hosted Mailman instance at https://lists.freedesktop.org.
- (Mailing list) Patch tracking services through Patchwork at https://patchwork.freedesktop.org.

_Note about the website:_ While the official freedesktop.org website is https://www.freedesktop.org (the root path `/` redirects to `/wiki/`, thus the wiki _is_ the website).

**Subdomains**

- Services:
	- `cgit` --> cgit service
	- `gitlab` --> GitLab service
	- `lists` --> Mailing list service
	- `patchwork` --> Patch tracking service (Patchwork)
- Projects:
	- `www`: freedesktop.org website
	- `specifications`: freedesktop.org specifications.
	- _... and more_