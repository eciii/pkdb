**unattended-upgrades Facts**

**Fact**: Overview of installed files (see section "Summary of files installed by `unattended-upgrades`" below for more details):

- Main script (in Python 3), man page (both with the name `unattended-upgrade`, note the missing "s" at the end) and corresponding symlinks with name `unattended-upgrades`.
- Configuration files in `/etc/apt/apt.conf.d`. These files are installed by the postinst hook of `unattended-upgrades` using `debconf`. The files are:
	- The file `20auto-upgrades` control the execution of the main `unattended-upgrade` script in `/usr/lib/apt/apt.systemd.daily` via some settings in `APT::Periodic`.
	- The file `50unattended-upgrades` holds settings consumed only by the main `unattended-upgrades` script.

---

**Summary of files installed by `unattended-upgrades`**

Main script and corresponding man documentation

```
/usr/bin/unattended-upgrade
/usr/share/man/man8/unattended-upgrade.8.gz
```

Symlinks to their corresponding unattended-upgrade version

```
/usr/bin/unattended-upgrades
/usr/share/man/man8/unattended-upgrades.8.gz
```

Configuration files installed on `/etc/apt/apt.conf.d` by the postinst maintainer script

```
/usr/share/unattended-upgrades/20auto-upgrades
/usr/share/unattended-upgrades/20auto-upgrades-disabled
/usr/share/unattended-upgrades/50unattended-upgrades
/usr/share/unattended-upgrades/50unattended-upgrades.md5sum
```

The systemd service `unattended-upgrades.service` starts the `unattended-upgrade-shutdown` script with the `--wait-for-signal` option.

The hook `unattended-upgrades` in `/lib/systemd/system-sleep` and the hook `10_unattended-upgrades-hibernate` in `/etc/pm/sleep.d` both execute the `unattended-upgrade-shutdown` script with the `--stop-only` option when system hibernation is triggered.

The script uses the D-Bus python bindings to interact with the `systemd-logind` D-Bus API exposed at `org.freedesktop.login1` (see [1]). In particular, this script delays the shutdown using Inihibitor Locks (see [2]), waits for the `PrepareForShutdown` signal (see [3]) and then runs unattended-upgrades.

The file `unattended-upgrades-logind-maxdelay.conf` in `/usr/lib/systemd/logind.conf.d` sets a default `InhibitDelayMaxSec` of 30 seconds.

_TODO:_ The logic of the script `unattended-upgrades-shutdown` is a bit complex and should be better understood.

[1] https://www.freedesktop.org/software/systemd/man/org.freedesktop.login1.html
[2] https://www.freedesktop.org/wiki/Software/systemd/inhibit/)
[3] https://www.freedesktop.org/software/systemd/man/org.freedesktop.login1.html#Signals

```
/lib/systemd/system/unattended-upgrades.service
/lib/systemd/system-sleep/unattended-upgrades
/etc/pm/sleep.d/10_unattended-upgrades-hibernate
/usr/lib/systemd/logind.conf.d/unattended-upgrades-logind-maxdelay.conf
/usr/share/unattended-upgrades/unattended-upgrade-shutdown
```

Kernel packages (of the form `linux-image-{version}-{platform}`) ship with "(pre|post)(inst|rm)" hook scripts that are executed by `dpkg` during install and removal/purge. These hook scripts in turn execute an additional layer of hook scripts located at `/etc/kernel/(pre|post)(inst|rm).d` respectively.

The `unattended-upgrades` package uses a postinst hook script to create the (empty) file `/var/run/reboot-required` and append the name of the corresponding `linux-image-*` package to the file `/var/run/reboot-required.pkgs`.

```
/etc/kernel/postinst.d/unattended-upgrades
```

The motd hook `92-unattended-upgrades` just calls the `update-motd-unattended-upgrades` script. This script checks for the existence of the file `/var/lib/unattended-upgrades/kept-back`, which contains a space-separated list of kept-backed packages from the last upgrade. If the file is present, the script prints a message telling how many packages were kept-back.

```
/etc/update-motd.d/92-unattended-upgrades
/usr/share/unattended-upgrades/update-motd-unattended-upgrades
```

Log rotation policy

```
/etc/logrotate.d/unattended-upgrades
```

Misc

- `/etc/init.d`: Directory used by the legacy System V init.
- `apport`: Ubuntu's crash report tool. The hook tells to attach specific log files in the report.
- `lintian`: Debian tool to check binary and source packages for compliance with the Debian policy and for other common packaging errors. This file tells lintian to ignore some known policy violations in the package.

```
/etc/init.d/unattended-upgrades
/usr/share/apport/package-hooks/source_unattended-upgrades.py
/usr/share/lintian/overrides/unattended-upgrades
```

Empty dirs

```
/etc/apt/apt.conf.d
/var/lib/unattended-upgrades
/var/log/unattended-upgrades
```

---

**Summary of check-unattended-upgrades options**

```
 -D, --short-description    Show a short description of this check plugin.
 -h, --help                 Show this help message.
 -v, --version              Show the version number.



 -a, --autoclean        Check if the configuration 'APT::Periodic::AutocleanInterval'               is set properly.
 -d, --download         Check if the configuration 'APT::Periodic::Download-Upgradeable-Packages'   is set properly.
 -e, --enable           Check if the configuration 'APT::Periodic::Enable'                          is set properly.
 -l, --lists            Check if the configuration 'APT::Periodic::Update-Package-Lists'            is set properly.
 -s, --sleep            Check if the configuration 'APT::Periodic::RandomSleep'                     is set properly.
 -u, --unattended       Check if the configuration 'APT::Periodic::Unattended-Upgrade'              is set properly.
 -m, --mail             Check if the configuration 'Unattended-Upgrade::Mail'                       is set properly.
 -r, --remove           Check if the configuration 'Unattended-Upgrade::Remove-Unused-Dependencies' is set properly.



 -w, --warning          Time interval since the last execution to result in a warning state (seconds).
 -c, --critical         Time interval since the last execution to result in a critical state (seconds).



 -A, --anacron          Check if the package 'anacron' is installed.
 -n, --dry-run          Check if 'unattended-upgrades --dry-run' is working. Warning: If you use this option the performance data last_ago is always 0 or near to 0.
 -p, --repo             Check if 'Unattended-upgrades' is configured to include the specified custom repository.
 -R, --reboot           Check if the machine needs a reboot.
 -S, --security         Check if 'Unattended-upgrades' is configured to handle security updates.
 -t, --systemd-timers   Check if the appropriate Systemd timers are enabled ( apt-daily-upgrade.timer, apt-daily.timer ).
```

Performance data:

```
  - last_ago
          Time interval in seconds for last unattended-upgrades execution.
  - warning
          Interval in seconds.
  - critical
          Interval in seconds.
```