### Issues with the Apparmor profile of sssd in Debian

**Facts**

* The debian maintainers of sssd include an apparmor profile in the sssd-common package.
* During installation of the sssd-common package the apparmor profile is enabled in "forced-complain" mode (which seems to be what the documentation calls just "complain" mode).
* The apparmor profile is attached to `/usr/sbin/sssd`. Once sssd starts it forks several processes to execute other components of sssd, like sssd_nss, whose executables are located in `/usr/libexec/sssd`. Those forks are also processes that run in complain mode (like their parent `/usr/sbin/sssd`). But apparmor seems to create a new profile for each one of them. These new profiles have names like `/usr/sbin/sssd//null-/usr/libexec/sssd/sssd_nss` and they seem to be clones of the original profile.
* The forked processes do from time to time illegal operations (according to the profile) thus generating (sometimes a lot of) log entries. These entries are marked as "ALLOWED" since profiles are in complain mode.
* Since the "cloned" profiles are not defined anywhere they can be deleted with the `aa-remove-unknown` tool. But restarting sssd recreates them again.

**Questions**

* Why is the sssd apparmor profile installed in complaint mode instead of enforce mode?
* Why is the profile catching these "illegal operations"? Shouldn't the profile just allow these?

**Howtos**

- To check the status of the apparmor profiles use `aa-status`.
- To remove unknown profiles use `aa-remove-unknown`.
- To disable the sssd apparmor profile:

      cd /etc/apparmor.d
      mv force-complain/usr.sbin.sssd disable/
      apparmor_parser -R /etc/apparmor.d/usr.sbin.sssd