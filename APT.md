**APT Facts**

**Fact**: Configuration settings are stored in `.conf` files in `/etc/apt/apt.conf.d`. These files originate in various ways and from various sources (see section "Summary of files in `/etc/apt/apt.conf.d`" below).

**Fact**: The `APT::LastInstalledKernel` mechanism.

The `APT::LastInstalledKernel` setting is used by APT to avoid the autoremoval of the last installed kernel. In order to keep this setting up-to-date APT ships with the shell script `/etc/kernel/postinst.d/apt-auto-removal`. This script is executed as part of the _install/remove hook mechanism_ of the _Debian kernel packages_. Every time a kernel package is installed the script creates/updates the configuration file `/etc/apt/apt.conf.d/01autoremove-kernels` which contains the `APT::LastInstalledKernel` setting set to the version of kernel being installed.

**Fact**: Daily systemd timers.

APT ships with files that set two systemd timers (here `P=/lib/systemd/system`):

- `apt-daily`:
	Files: `$P/apt-daily.timer` and `$P/apt-daily.service`
	This timer is in charge of downloading updates.
- `apt-daily-upgrade`:
	Files: `$P/apt-daily-upgrade.timer` and `$P/apt-daily-upgrade.service`
	This timer is in charge of installing the previously downloaded updates

Both timers execute the shell script `/usr/lib/apt/apt.systemd.daily` (which also belongs to APT). This script is executed by the `apt-daily` timer with the argument `update` and by the `apt-daily-upgrade` timer with the argument `install`. Some of the tasks performed by the script use the the main `unattended-upgrade` script (hard-coded, the script expects that there is an executable with the name `unattended-upgrade`).

---

**Summary of files in `/etc/apt/apt.conf.d`**

- `00CDMountPoint`: _???_
- `00trustcdrom`: _???_
- `01autoremove`: Belongs to `apt`. Used by `apt` to fine-tune some autoremove-related settings.
- `01autoremove-kernels`: Created/updated as part of the `APT::LastInstalledKernel` mechanism.
- `20auto-upgrades`: Installed by `unattended-upgrades`. Meant for the script `apt.systemd.daily` to enable unattended updates/upgrades.
- `20listchanges`: Belongs to `apt-listchanges`. Some settings are meant for `apt` (for what exactly?) and the rest for the `apt-listchanges` script.
- `50unattended-upgrades`: Installed by `unattended-upgrades`. Meant for the `unattended-upgrades` script for fine-control.
- `70debconf`: Belongs to `debconf`. Meant for `apt` (for what exactly?).
- `99ipv4`: _???_