#OSS_Project_Overview

Linux distribution (i.e OS based on the [[Linux kernel]]).

**General resources**

- [Debian Wiki (DW)](https://wiki.debian.org/)
- [Debian Administrator Handbook (DAH)](https://www.debian.org/doc/manuals/debian-handbook/index.en.html)
- [Debian Policy Manual](https://www.debian.org/doc/debian-policy/)
- [Securing Debian Manual (SDM)](https://www.debian.org/doc/manuals/securing-debian-manual/index.en.html)
- [Debian Reference (DR)](https://www.debian.org/doc/manuals/debian-reference/)

**Important topics**

- Release lifecycle: [DebianReleases](https://wiki.debian.org/DebianReleases) entry in DW, [Section 1.6](https://www.debian.org/doc/manuals/debian-handbook/sect.release-lifecycle.en.html) of DAH
- Package management: [PackageManagement](https://wiki.debian.org/PackageManagement) entry in DW, Sections [5](https://www.debian.org/doc/manuals/debian-handbook/packaging-system.en.html) and [6](https://www.debian.org/doc/manuals/debian-handbook/apt.en.html) of DAH, [Section 4.2](https://www.debian.org/doc/manuals/securing-debian-manual/security-update.en.html) of SDM

**Links regarding automated installs**

- [AutomatedInstallation - Debian Wiki](https://wiki.debian.org/AutomatedInstallation)
- [DebianInstaller/Preseed - Debian Wiki](https://wiki.debian.org/DebianInstaller/Preseed)
- [DebianInstaller/Preseed/EditIso - Debian Wiki](https://wiki.debian.org/DebianInstaller/Preseed/EditIso)
- [B.2. Using preseeding](https://www.debian.org/releases/stable/i386/apbs02.en.html#preseed-bootparms)

**Notes about the base image produces by the Debian installer**

- The Debian installer produces the log `/var/log/installer/syslog`.
- The Debian installer seems to be built from several components hosted by the [Debian Installer]([https://salsa.debian.org/installer-team](https://salsa.debian.org/installer-team)) project.
- One of the components of the Debian istaller is [hw-detect]([https://salsa.debian.org/installer-team/hw-detect](https://salsa.debian.org/installer-team/hw-detect)).
- The hw-detect component detects if it is running under a virtualized environment (see [here]([https://salsa.debian.org/installer-team/hw-detect/-/blob/master/hw-detect.finish-install.d/08hw-detect)) and depending on the outcome it installs either the `open-vm-tools` package (if running on VMware) or the `qemu-guest-agent` (if running on KVM).
- The hw-detect component also installs CPU microcode update packages (from the `non-free-firmware` area) depending on the type of CPU (see [here]([https://salsa.debian.org/installer-team/hw-detect/-/blob/master/hw-detect.post-base-installer.d/50install-firmware)). Unfortunately the detection does not take into account if the installation is being done in a virtualized environment. This is not desirable since CPU microcode updates are meaningless in such environments (see [here]([https://serverfault.com/questions/895294/are-cpu-microcode-updates-always-ignored-by-hypervisors](https://serverfault.com/questions/895294/are-cpu-microcode-updates-always-ignored-by-hypervisors)) and [here]([https://unix.stackexchange.com/questions/572754/do-i-need-cpu-or-any-microcode-in-a-qemu-kvm-virtual-machine)). The consequence is that the Debian VM ends up with `non-free-firmware` configured in the `sources.list` file even when there is no need for it.