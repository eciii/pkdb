```
SW Project --- Gitstore --< Gitrepos --< SW Deliverables

Gitrepos --* Distro Source Package --< Distro Binary Package --< Files
```

Software deliverables and their corresponding interfaces can be categorized as:

- Executables
	- Graphical applications (GUI)
	- Command-line tools (CLI)
	- Deamons (IPC Interface)
- Libraries
	- Interpreted or statically linked (API)
	- Dynamically linked (ABI)

**Code snippets**

In Fedora (with DNF5) it is possible to query the metadata of all available packages in the package repositories. This allows us to search by software project in the name, source and url like this (`<SWP>` can be replaced with the actual name of a software project, for example `libguestfs` or `nbdkit`):

```
sudo dnf repoquery --qf '%{NAME};%{SOURCE_NAME};%{URL}\n' '*' | grep '<SWP>' | column -ts';' | less
```