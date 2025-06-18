
**Package Management**

**Fact:** Fedora uses the [[Pkg Manager - RPM & DNF|RPM & DNF package management stack]].

---

**Infrastructure**

**Fact:** Gitforges.

The sources of all packages are located in a gitforge at https://src.fedoraproject.org. The project also operates another gitforge at https://pagure.io, which is promoted to host open source software in general.

Both gitforges are instances of the Pagure gitforge project but, during 2024, Fedora decided to stop using Pagure and move to Forgejo.

**Fact:** The Pagure project.

- Website: (none)
- Gitrepo: https://pagure.io/pagure
- Languages: Python

- Pagure doesn't implement the notion of gitstore as GitHub or GitLab do. Instead the project implements groups, but they differ from gitstores quite strongly:
	- it is not mandatory for a project to belong to a group
	- it seems that there is no easy way to tell if a project actually belongs to a group
- Pagure also implements the concept of namespaces. These are used, for example, by the gitforge src.fedoraproject.org to partition all projects into rpms, modules, flatpacks, etc.

**Fact:** The Forgejo project.

- Website: https://forgejo.org
- Gitstore: https://codeberg.org/forgejo
- Languages: Go

The Forgejo domains are in custody of the non-profit [Codeberg e.V.](https://codeberg.org/), which itself offers a public gitforge based on Forgejo and aimed at open source projects.
