```
USAGE
  virt-sysprep --help
  virt-sysprep {-V|--version}

  virt-sysprep --list-operations

  virt-sysprep [options] [-c|--connect URI] {-d|--domain NAME}
  virt-sysprep [options] { [--format auto|raw|qcow2|...] {-a|--add FILE|URI}... }...

where the options are

  [-n|--dry-run]
  [-q|--quiet] [-v|--verbose] [-x]
  [--colors|--colours] [--echo-keys]

  [--key SELECTOR] [--keys-from-stdin]
  [--mount-options MP:OPTS;...]
  [--network|--no-network]

  [(--enable OP,...) | (--operation|--operations OPLIST)]


----------------
"customize" opts
----------------

--commands-from-file FILENAME
--no-logfile

====

--script SCRIPT
--scriptdir SCRIPTDIR

--run SCRIPT
--run-command 'CMD+ARGS'

--firstboot SCRIPT
--firstboot-command 'CMD+ARGS'

--firstboot-install PKG,PKG...
--install PKG,PKG...
--uninstall PKG,PKG...
--update

====

--touch FILE
--link TARGET:LINK[:LINK...]
--mkdir DIR
--chmod PERMISSIONS:FILE

--truncate FILE
--truncate-recursive PATH
--append-line FILE:LINE
--write FILE:CONTENT
--edit FILE:EXPR

--copy SOURCE:DEST
--move SOURCE:DEST

--delete PATH
--scrub FILE

--upload FILE:DEST
--copy-in LOCALPATH:REMOTEDIR

====

--hostname HOSTNAME
--timezone TIMEZONE

--keep-user-accounts USERS
--remove-user-accounts USERS
--root-password SELECTOR
--password USER:SELECTOR
--password-crypto md5|sha256|sha512
--ssh-inject USER[:SELECTOR]

--selinux-relabel

====

--sm-attach SELECTOR
--sm-credentials SELECTOR
--sm-register
--sm-remove
--sm-unregister


----------
operations
----------

# Remove "generic" files
* backup-files
* logfiles
* tmp-files

# Remove application-specific files
* bash-history
* cron-spool
* mail-spool

* pacct-log

* abrt-data
* crash-data

# Remove/reset/truncate system files/settings
* blkid-tab
* lvm-uuids
* machine-id
* net-hostname
* net-hwaddr


* pam-data
* passwd-backups
* rh-subscription-manager
* rhn-systemid
* smolt-uuid
* udev-persistent-net
* utmp

* ssh-hostkeys
* ssh-userdir

* package-manager-cache
* rpm-db
* yum-uuid

* dhcp-client-state
* ipa-client
* kerberos-hostkeytab
* sssd-db-log

* dhcp-server-state
* dovecot-data
* puppet-data-log
* samba-db-log

* customize
* script

ca-certificates
firewall-rules
flag-reconfiguration
fs-uuids
kerberos-data
user-account
```