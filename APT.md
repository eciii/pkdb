**Fact**: Configuration settings are stored in `.conf` files in `/etc/apt/apt.conf.d`. These files originate in various ways and from various sources (see section "Summary of files in `/etc/apt/apt.conf.d`" below).

**Fact**: The `APT::LastInstalledKernel` mechanism.

The `APT::LastInstalledKernel` setting is used by APT to avoid the autoremoval of the last installed kernel. In order to keep this setting up-to-date APT ships with the shell script `/etc/kernel/postinst.d/apt-auto-removal`. This script is executed as part of the _install/remove hook mechanism_ of the _Debian kernel packages_. Every time a kernel package is installed the script creates/updates the configuration file `/etc/apt/apt.conf.d/01autoremove-kernels` which contains the `APT::LastInstalledKernel` setting set to the version of kernel being installed.

---

**Daily tasks**

**Fact**: APT used to 

**Fact**: Daily systemd timers.

APT ships with files that set two systemd timers (here `P=/lib/systemd/system`):

- `apt-daily`:
	Files: `$P/apt-daily.timer` and `$P/apt-daily.service`
	This timer is in charge of downloading updates.
- `apt-daily-upgrade`:
	Files: `$P/apt-daily-upgrade.timer` and `$P/apt-daily-upgrade.service`
	This timer is in charge of installing the previously downloaded updates

Both timers execute the shell script `/usr/lib/apt/apt.systemd.daily` (which also belongs to APT). The `apt-daily` timer executes the script with the argument `update` and the `apt-daily-upgrade` timer executes the script with the argument `install`.

Previously APT used a cronjob for this

**Fact**: Relation with the [[unattended-upgrades]] package.

The fact that the `unattended-upgrades` package is provided separately from APT might lead to think that APT has some sort of pluggable mechanism to perform automated periodic updates/upgrades. But this is in fact not true because some of the tasks performed by the script `/usr/lib/apt/apt.systemd.daily` use the main script of [[unattended-upgrades]]. This effectively means, that the `unattended-upgrades` package is the only package supported by APT for automated periodic updates/upgrades.

**Fact**: The rationale about why are there two different timers and when they are supposed to run is simple: the goal is to have updates spread uniformly across all timezones, and upgrades in predictable application windows. See [this bug](https://bugs.launchpad.net/ubuntu/+source/apt/+bug/1686470) and [this commit](https://salsa.debian.org/apt-team/apt/-/commit/496313fb8e83af2ba71f6ce3d729be687c293dfd).

**Fact**: The wait-online mechanism

Since network connectivity is required to do updates/upgrades the `unattended-upgrades` package needs a way to ensure that this requirement is met before trying to run the actual updates/upgrades.

Originally the solution seemed to be simple:

```
apt-daily.timer:
	After=network-online.target
	Wants=network-online.target

apt-daily-upgrade.timer:
	After=apt-daily.timer

apt-daily-upgrade.service:
	After=apt-daily.service
```

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