**unattended-upgrades Facts**

**Fact**: Main script and corresponding man page.

The main script of the package (a Python script) and the corresponding man page are (note the missing "s" at the end of the name):

```
/usr/bin/unattended-upgrade
/usr/share/man/man8/unattended-upgrade.8.gz
```

There are also symlinks to the main script and man page with the name `unattended-upgrades` (note the trailing "s"):

```
/usr/bin/unattended-upgrades
/usr/share/man/man8/unattended-upgrades.8.gz
```

**Fact**: Configuration files.

The package comes with two configuration files that are installed on `/etc/apt/apt.conf.d` by the postinst hook using `debconf`. The files are:

- The file `20auto-upgrades`, which control the execution of the main `unattended-upgrade` script in `/usr/lib/apt/apt.systemd.daily` via some settings in `APT::Periodic`.
- The file `50unattended-upgrades`, which holds settings consumed only by the main `unattended-upgrade` script.

The corresponding files shipped by the package are:

```
/usr/share/unattended-upgrades/20auto-upgrades
/usr/share/unattended-upgrades/20auto-upgrades-disabled
/usr/share/unattended-upgrades/50unattended-upgrades
/usr/share/unattended-upgrades/50unattended-upgrades.md5sum
```

**Fact**: The `reboot-required` mechanism.

The package comes with the shell script `/etc/kernel/postinst.d/unattended-upgrades` that is executed as part of the _install/remove hook mechanism_ of the _Debian kernel packages_. Every time a kernel package is installed the script creates the (empty) file `/var/run/reboot-required` and appends the name of the corresponding `linux-image-*` package to the file `/var/run/reboot-required.pkgs`.

```
/etc/kernel/postinst.d/unattended-upgrades
```

The motd hook `92-unattended-upgrades` just calls the `update-motd-unattended-upgrades` script. This script checks for the existence of the file `/var/lib/unattended-upgrades/kept-back`, which contains a space-separated list of kept-backed packages from the last upgrade. If the file is present, the script prints a message telling how many packages were kept-back.

```
/etc/update-motd.d/92-unattended-upgrades
/usr/share/unattended-upgrades/update-motd-unattended-upgrades
```

---

**Summary of files installed by `unattended-upgrades`**

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