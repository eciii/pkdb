The steps are more or less the following:

- A crash occurs. Either the kernel or systemd notices it and creates a crash report file in `/var/crash`.
- The systemd path unit `update-notifier-release.path` watches `/var/crash`. It triggers the shell script `/usr/lib/update-notifier/update-notifier-crash` whenever a new crash report file is created. Both the systemd path unit and the triggered script belong to the update-notifier package.
- The triggered script `update-notifier-crash` executes `apport-gtk`. This program shows a window that asks the user if the crash report should be sent to the developers. To send the report, `apport-gtk` creates an empty `.upload` file next to the report file.
- Another program called `whoopsie` also watches `/var/crash` for files that end with `.upload`. Once such file is seen then the corresponding report file is sent to Ubuntu.

Involved executables and packages:

- update-notifier
- apport-gtk
- whoopsie

Links to check:

- Ubuntu's wiki articles:
	- https://wiki.ubuntu.com/Apport
	- https://wiki.ubuntu.com/ErrorTracker
	- https://wiki.ubuntu.com/UpdateNotifier
- https://github.blog/security/vulnerability-research/whoopsie-daisy-chaining-accidental-features-of-ubuntus-crash-reporter-to-get-local-privilege-escalation/