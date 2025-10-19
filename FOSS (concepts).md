
A **software product** is one or more pieces of software that are advertised/marketed, packaged and distributed as a unit.

An **open-source software project** is a concentrated effort of a group of people, the **project participants**, that **contribute** to the production of one or more related software products.

The nature of the project participants might vary greatly. Examples of project participants are: private companies, public communities, single contributors, foundations, etc.

Similarly the nature of the contributions might vary greatly. Examples of types of contributions are:

- Product-related
  - enhancement of source code (bug fixes, new features, etc)
  - enhancement of documentation
  - enhancement of translations (language additions, translation coverage)
- Publicity-related
  - Marketing, i.e. to promote adoption of the software products
  - Engagement, i.e. to promote contribution to the project
- Administration-related
  - IT Infrastructure
  - Legal support

---

The "_community_" is healthy if:

- is actively engaged
- It is diverse but united in principles and goals.

A software project can have the following characteristics:

- Its software products:
	- are FOS (Free and Open Source), i.e their source code is open and has a "free" license
	- have ODev (Open Development), i.e their development is made in the open
- It has a HC (Healthy Community)
	- It has CDev (Community Development), i.e the development is made by its HC.
	- It has CGov (Community Governance), i.e the governance represents the interests of its HC.

_Open core_ projects are software projects where:

- A single for-profit enterprise has full trademark rights and administrative power (e.g ownership of the DNS domain). Thus they have no CG.
- There is a _Community Edition_ (CE) and an _Enterprise Edition_ (EE) of the software products:
	- The CE is open source, but the source might or might not have a "free" license.

---

**Internet resources**

- Homepage
- Documentation

Public (read-only) git repositories:

- One form of classification
    - self-hosted
    - managed
        - by a non-profit
        - by a for-profit
- Another form of classification
    - Bare only (thus necessarily self-hosted). This is very rare.
    - Browsable (using some gitforge)
- By gitforge
    - cgit (self-hosted)
    - GitLab (self-hosted or as-a-service)
    - GitHub (as-a-service)

In general an _open software project_ required the following resources available on the internet:

- Source code. In the old days source code was made available using _versioned released trees_. However this practice was inconvenient for several reasons (elaborate this!) which led to the emergence of _source code management_ tools such as Git.
- Discussions. There are mainly two types of discussions around an open software project:
    - Help on Usage: For users that intend to "just use" the software and don't have any a priori motivation to contribute back.
    - With intent to contribute:
        - Actual issues with the software. Might lead to fixes.
        - Enhancement proposals. Might lead to enhancements.

Actual contributions are performed using one of two main mechanisms:

- Email-Patch (git-agnostic)
- Pull-Request (git-specific)

Issue tracker

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

---

**Relations between concepts**

```
SW Project --- Gitstore --< Gitrepos --< SW Deliverables

Gitrepos --* Distro Source Package --< Distro Binary Package --< Files
```

Software deliverables and their corresponding interfaces can be categorized as:

- Executables
	- For users
		- Graphical applications (GUI)
		- Command-line tools (CLI)
	- Deamons (IPC Interface)
- Libraries
	- Interpreted or statically linked (API)
	- Dynamically linked (ABI)
	- Command-line tools (CLI)

Note that some command-line tools are designed to be used by both users and programs. In such case those tools can also be considered "libraries" and not just tools for users.

**Code snippets**

In Fedora (with DNF5) it is possible to query the metadata of all available packages in the package repositories. This allows us to search by software project in the name, source and url like this (`<SWP>` can be replaced with the actual name of a software project, for example `libguestfs` or `nbdkit`):

```
sudo dnf repoquery --qf '%{NAME};%{SOURCE_NAME};%{URL}\n' '*' | grep '<SWP>' | column -ts';' | less
```