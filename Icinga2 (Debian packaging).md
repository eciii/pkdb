Since Icinga2 supports two database engines, MySQL/MariaDB and PostgreSQL, the installation of Icinga2 in Debian must expose this flexibility to the users. This is achieved by providing two additional packages, one for each database engine. For MySQL/MariaDB the package is called `icinga2-ido-mysql`. The installation of this package must do two things automatically:

- Install specific configuration files and enable the `ido-mysql` feature. This is done using the [debconf](https://packages.debian.org/stable/debconf) and [ucf](https://packages.debian.org/stable/ucf) packages in the maintainer scripts.
- Create the icinga2 database user and create and populate the icinga2 database. This is done using [dbconfig-common](https://packages.debian.org/stable/dbconfig-common) also in the maintainer scripts.

Both debconf and dbconfig-common are shell scripts that are to be sourced by the maintainer scripts to automate configuration during installation (and removal).