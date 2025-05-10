#OSS_Project_Overview

Linux distribution (i.e OS based on the [[Linux kernel]]).

**General resources**

- [Debian Wiki](https://wiki.debian.org/) (DW)
- [Debian Administrator Handbook](https://www.debian.org/doc/manuals/debian-handbook/index.en.html) (DAH)
- [Debian Policy Manual](https://www.debian.org/doc/debian-policy/) (DPM)
- [Securing Debian Manual](https://www.debian.org/doc/manuals/securing-debian-manual/index.en.html) (SDM)
- [Debian Reference](https://www.debian.org/doc/manuals/debian-reference/) (DR)
- [Debian Installation Manual](https://www.debian.org/releases/stable/installmanual.en.html) (DIM)

---

**Debian Wiki**

**Fact**: Articles in the wiki seem to be organized in hierarchical way. Directories themselves are also articles.

**Fact**: Paths of english articles start at the root of the hierarchy. Paths of articles in the language `<ln>` (e.g `de` or `es`) start from the directory `/<ln>`.

**Fact**: The wiki offers a [Title Index](https://wiki.debian.org/TitleIndex), which lists all the articles in the hierarchy (effectively listing all articles in all languages). The Title Index can be useful to get the list of all existing sub-articles of a given article (e.g the article _DebianInstaller_ contains many sub-articles under it).

**Debian installer**

**Fact**: The Debian installer produces the log `/var/log/installer/syslog`.

**Fact**: The Debian installer seems to be built from several components hosted by the [Debian Installer](https://salsa.debian.org/installer-team) project.

**Fact**: One of the components of the Debian installer is [hw-detect](https://salsa.debian.org/installer-team/hw-detect).

**Fact**: The hw-detect component detects if it is running under a virtualized environment (see [here](https://salsa.debian.org/installer-team/hw-detect/-/blob/master/hw-detect.finish-install.d/08hw-detect)) and depending on the outcome it installs either the `open-vm-tools` package (if running on VMware) or the `qemu-guest-agent` (if running on KVM).

**Fact**: The hw-detect component also installs CPU microcode update packages (from the `non-free-firmware` area) depending on the type of CPU (see [here](https://salsa.debian.org/installer-team/hw-detect/-/blob/master/hw-detect.post-base-installer.d/50install-firmware)). Unfortunately the detection does not take into account if the installation is being done in a virtualized environment. This is not desirable since CPU microcode updates are meaningless in such environments (see [here](https://serverfault.com/questions/895294/are-cpu-microcode-updates-always-ignored-by-hypervisors) and [here](https://unix.stackexchange.com/questions/572754/do-i-need-cpu-or-any-microcode-in-a-qemu-kvm-virtual-machine)). The consequence is that the Debian VM ends up with `non-free-firmware` configured in the `sources.list` file even when there is no need for it.

**Debian kernel packages**

**Fact**: Linux kernel packages in Debian are named like `linux-image-{version}-{platform}`.

**Fact**: The install/remove hook mechanism.
The Debian kernel packages come with "(pre|post)(inst|rm)" hook scripts that are executed by `dpkg` during install and removal/purge. These hook scripts in turn execute an additional layer of hook scripts located at `/etc/kernel/(pre|post)(inst|rm).d` respectively.

---

**Other topics to investigate**

- Release lifecycle:
	- DW: [DebianReleases](https://wiki.debian.org/DebianReleases)
	- DAH: [Section 1.6](https://www.debian.org/doc/manuals/debian-handbook/sect.release-lifecycle.en.html)
- Package management:
	- DW: [PackageManagement](https://wiki.debian.org/PackageManagement)
	- DAH: Sections [5](https://www.debian.org/doc/manuals/debian-handbook/packaging-system.en.html) and [6](https://www.debian.org/doc/manuals/debian-handbook/apt.en.html)
	- SDM: [Section 4.2](https://www.debian.org/doc/manuals/securing-debian-manual/security-update.en.html)
- Automated installs:
	- DW: [AutomatedInstallation](https://wiki.debian.org/AutomatedInstallation), [DebianInstaller/Preseed](https://wiki.debian.org/DebianInstaller/Preseed), [DebianInstaller/Preseed/EditIso](https://wiki.debian.org/DebianInstaller/Preseed/EditIso)
	- DIM: Appendix [B.2. Using preseeding](https://www.debian.org/releases/stable/amd64/apbs02.en.html)