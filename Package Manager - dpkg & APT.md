
**Fact**: Configuration settings are stored in `.conf` files in `/etc/apt/apt.conf.d`. These files originate in various ways and from various sources (see section "Summary of files in `/etc/apt/apt.conf.d`" below).

**Fact**: The `APT::LastInstalledKernel` mechanism.

The `APT::LastInstalledKernel` setting is used by APT to avoid the autoremoval of the last installed kernel. In order to keep this setting up-to-date APT ships with the shell script `/etc/kernel/postinst.d/apt-auto-removal`. This script is executed as part of the _install/remove hook mechanism_ of the _Debian kernel packages_. Every time a kernel package is installed the script creates/updates the configuration file `/etc/apt/apt.conf.d/01autoremove-kernels` which contains the `APT::LastInstalledKernel` setting set to the version of kernel being installed.

---

**APT > Daily tasks**

**Fact**: Daily systemd timers.

APT ships with files that set two systemd timers (here `P=/lib/systemd/system`):

- `apt-daily`:
	Files: `$P/apt-daily.timer` and `$P/apt-daily.service`
	This timer is in charge of downloading updates.
- `apt-daily-upgrade`:
	Files: `$P/apt-daily-upgrade.timer` and `$P/apt-daily-upgrade.service`
	This timer is in charge of installing the previously downloaded updates

Both timers execute the shell script `/usr/lib/apt/apt.systemd.daily` (which also belongs to APT). The `apt-daily` timer executes the script with the argument `update` and the `apt-daily-upgrade` timer executes the script with the argument `install`.

**Fact**: Before using systemd timers APT used a cronjob in the cron daily hook.

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

**APT > APT daily tasks > `apt-systemd-daily` script**

**Fact**: The very first thing that the script does is to use [`flock`]([https://man7.org/linux/man-pages/man1/flock.1.html](https://man7.org/linux/man-pages/man1/flock.1.html)) on the file `[Dir::State]/daily_lock` to prevent the script from running twice at the same time.

**Fact**: After locking the execution the script does a backup of APT's extended states. _\[TODO: understand and explain this in more detail]_

**Fact**: All the settings that affect the behaviour of the script are located under `APT::Periodic`. The script also uses the settings `Dir::{Cache{,::{Archives,Backup}},State}` to locate resources.

**Fact**: General settings

The following settings influence the general behaviour of the script:

- `APT::Periodic::Enable` (0=disable, 1=enable): If disabled the script will exit without doing anything. If unset the script will do as if the setting was enabled.
- `APT::Periodic::Verbose`: Verbosity level. The values are:
	- unset or 0: no output
	- 1: debug messages
	- 2: 1 + command outputs (remove -qq, remove 2>/dev/null, add -d)
	- 3: 2 + trace on

**Fact**: Periodic tasks

The script performs the tasks described below. With the exception of the last task, all other tasks are controlled by an _interval setting_. Such interval settings accept as values either:

- the keyword "always", which means that the task is always executed
- the number 0, which means that the task is never executed
- a positive integer optionally suffixed with one of the letters "s" for seconds, "m" for minutes, "h" for hours or "d" for days (the default if no suffix is specified); whith such a value the task won't be executed if less than the specified amount of time has passed since the last execution of the task

The tasks are:

- Backup the archive. Controlled by the interval setting `APT::Periodic::BackupArchiveInterval`.
	- This task also understands the setting `APT::Periodic::BackupLevel`. _\[TODO: understand and explain this setting in more detail]_
- Update the package lists (a.k.a `apt-get update`). Controlled by the interval setting `APT::Periodic::Update-Package-Lists`.
- Download upgradable packages (a.k.a `apt-get upgrade --download-only`). Controlled by the interval setting `APT::Periodic::Download-Upgradeable-Packages`.
	- This task also understands the setting `APT::Periodic::Download-Upgradeable-Packages-Debdelta`. If set, the downloads are done using [debdelta]([https://debdelta.debian.net/](https://debdelta.debian.net/)).
- The following two separate tasks require the package `unattended-upgrades` and are both controlled by the same interval setting `APT::Periodic::Unattended-Upgrade`:
	- Download upgradable packages using `unattended-upgrade --download-only` (this downloads only those packages that will be actually installed later)
	- Install the packages using `unattended-upgrade`.
- Completely clean the archive (a.k.a `apt-get clear`). Controlled by the interval setting `APT::Periodic::CleanInterval`.
- Clean from the archive only those packages that can no longer be downloaded (a.k.a `apt-get autoclean`). Controlled by the interval setting `APT::Periodic::AutocleanInterval`.
- Clean old packages from the archive depending on the age of the package and/or on the overall size of the archive. Those conditions are specified with the settings `APT::Periodic::MaxAge`, `APT::Periodic::MinAge` and `APT::Periodic::MaxSize`. This task is not controlled by an interval setting.

**Fact**: None of the interval settings are set when APT is initially installed

**Fact**: Deprecated settings for the last task

The settings used to specify the conditions for the last task (`MaxAge`, `MinAge` and `MaxSize`) were previously located under `APT::Archives`. Those settings are now deprecated in favor of the settings with the same name under `APT::Periodic`.

**Thoughts**

- Bug. The script checks for the existence of `apt-config` after using it when trying to hold the lock.
- Bug. The `check_stamp` function compares "midnight today to midnight the day the stamp was updated". This prevents the script to check intervals shorter than a day.
- It seems to me, that the possibility of specifying a time interval for the interval settings is mostly a relic from the past, when it was triggered by `cron.daily`. Back then the script ran everyday and the interval settings provided an easy way to control how often the script should run (with day granularity). But I think that feature is more confusing than useful. Instead of using such settings to control the frequency of execution the user should adjust the systemd timer (or back then the cron task) to the desired frequency. The interval settings shouldn't be interval settings at all; they should be boolean settings instead, where each setting either enable or disable the corresponding task.

----------------------------------------------

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