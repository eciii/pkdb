## Deliverables

The libguestfs project provides:

- the main library (`guestfs`)
- the main daemon (`guestfsd`)
- several tools to interact with Linux guests:
	- `guestfish`: interactive shell
	- `guestmount`: mount guest filesystem in host
	- `guestunmount`: unmount guest filesystem
	- `libguestfs-make-fixed-appliance`: make libguestfs fixed appliance
	- `libguestfs-test-tool`: test libguestfs
	- `supermin`: tool for building supermin appliances
	- `virt-alignment-scan`: check alignment of virtual machine partitions
	- `virt-builder`: quick image builder
	- `virt-builder-repository`: create virt-builder repositories
	- `virt-cat`: display a file
	- `virt-copy-in`: copy files and directories into a VM
	- `virt-copy-out`: copy files and directories out of a VM
	- `virt-customize`: customize virtual machines
	- `virt-df`: free space
	- `virt-dib`: safe diskimage-builder
	- `virt-diff`: differences
	- `virt-edit`: edit a file
	- `virt-filesystems`: display information about filesystems, devices, LVM
	- `virt-format`: erase and make blank disks
	- `virt-get-kernel`: get kernel from disk
	- `virt-inspector`: inspect VM images
	- `virt-list-filesystems`: list filesystems
	- `virt-list-partitions`: list partitions
	- `virt-log`: display log files
	- `virt-ls`: list files
	- `virt-make-fs`: make a filesystem
	- `virt-p2v`: convert physical machine to run on KVM
	- `virt-p2v-make-disk`: make P2V ISO
	- `virt-p2v-make-kickstart`: make P2V kickstart
	- `virt-rescue`: rescue shell
	- `virt-resize`: resize virtual machines
	- `virt-sparsify`: make virtual machines sparse (thin-provisioned)
	- `virt-sysprep`: unconfigure a virtual machine before cloning
	- `virt-tail`: follow log file
	- `virt-tar`: archive and upload files
	- `virt-tar-in`: archive and upload files
	- `virt-tar-out`: archive and download files
	- `virt-v2v`: convert guest to run on KVM

Additional components are provided to handle Windows guests:

- a library (`hivex`) to extract data from the Windows Registry hive
- several tools to interact with Windows guests
	- `hivexget`: extract data from Windows Registry hive
	- `hivexml`: convert Windows Registry hive to XML
	- `hivexregedit`: merge and export Registry changes from regedit-format files
	- `hivexsh`: Windows Registry hive shell
	- `virt-win-reg`: export and merge Windows Registry keys


## Git repositories

The gitstore is https://github.com/libguestfs. The following is a list of active gitrepos in the gitstore. The "Debian" column shows if the repository has a corresponding Debian source package (of the same name).

| Repository                  | Language | Debian | Description                                                               |
| --------------------------- | -------- | ------ | ------------------------------------------------------------------------- |
| `libguestfs`                | C        | X      | Library and tools for accessing and modifying virtual machine disk images |
| `guestfs-tools`             | OCaml    | X      | Tools for accessing and modifying guest disk images                       |
| `libguestfs-common`         | OCaml    |        | Common code shared between libguestfs and tools                           |
| `libguestfs-analysis-tools` | C        |        | Utilities for profiling and testing libguestfs                            |
| `virt-v2v`                  | OCaml    | X      | Tool to convert guests from foreign hypervisors to run on KVM             |
| `virt-p2v`                  | C        | X      | Tool to convert physical machines into virtual machines                   |
| `virt-similarity`           | OCaml    |        | Tool to find clusters of similar/cloned virtual machines                  |
| `supermin`                  | OCaml    | X      | Tool to create supermin appliances                                        |
| `hivex`                     | C        | X      | windows registry hive extraction library                                  |

Archived/unmaintained repositories:

| Repository    | Language | Description                             |
| ------------- | -------- | --------------------------------------- |
| `febootstrap` | C        | Deprecated. Use supermin instead        |
| `nbdkit`      | C        | Now at https://gitlab.com/nbdkit/nbdkit |
| `libnbd`      | C        | Now at https://gitlab.com/nbdkit/libnbd |

## Debian packages

#### Source package `libguestfs`

The binary packages can be divided in three categories:

- main packages
- language binding packages
- file system support packages

Note that not all packages are installed by default on Debian 12 with `apt install libguestfs-tools`. Those that do are marked in the "Default" column.

Main packages:

| Package             | Default | Description                |
| ------------------- | ------- | -------------------------- |
| `libguestfs-tools`  | X       | Main tools                 |
| `guestfish`         | X       | Shell                      |
| `guestmount`        | X       | Mount utility (FUSE-based) |
| `libguestfs0`       | X       | Shared library             |
| `guestfsd`          |         | Daemon (via virtio serial) |
| `libguestfs-dev`    |         | Development headers        |
| `libguestfs-rescue` |         | virt-rescue                |

Language bindings packages:

| Package                   | Default | Description                 |
| ------------------------- | ------- | --------------------------- |
| `libguestfs-perl`         | X       | Perl bindings               |
| `python3-guestfs`         |         | Python 3 bindings           |
| `golang-guestfs-dev`      |         | Golang bindings             |
| `php-guestfs`             |         | PHP bindings                |
| `ruby-guestfs`            |         | Ruby bindings               |
| `libguestfs-java`         |         | Java bindings               |
| `lua-guestfs`             |         | Lua bindings                |
| `libguestfs-ocaml`        |         | OCaml bindings              |
| `libguestfs-ocaml-dev`    |         | OCaml development files     |
| `libguestfs-gobject-1.0-0 |         | GObject bindings            |
| `libguestfs-gobject-dev`  |         | GObject development headers |
| `gir1.2-guestfs-1.0`      |         | GObject introspection files |

File system support packages:

| Package               | Default | Description      |
| --------------------- | ------- | ---------------- |
| `libguestfs-hfsplus`  | X       | HFS+ support     |
| `libguestfs-reiserfs` | X       | ReiserFS support |
| `libguestfs-xfs`      | X       | XFS support      |
| `libguestfs-gfs2`     |         | GFS2 support     |
| `libguestfs-jfs`      |         | JFS support      |
| `libguestfs-nilfs`    |         | NILFS v2 support |
| `libguestfs-rsync`    |         | rsync support    |
| `libguestfs-zfs`      |         | ZFS support      |

**Files of the default main packages**

The default main packages are: `libguestfs-tools`, `guestfish`, `guestmount` and `libguestfs0`.    

| Package            | File                                        | Related files             |
| ------------------ | ------------------------------------------- | ------------------------- |
| `libguestfs-tools` | `/usr/bin/libguestfs-test-tool`             | `man1`, `bash-completion` |
| `libguestfs-tools` | `/usr/sbin/libguestfs-make-fixed-appliance` | `man1`                    |
| `guestfish`        | `/usr/bin/guestfish`                        | `man1`, `bash-completion` |
| `guestfish`        | `/usr/bin/virt-copy-in`                     | `man1`, `bash-completion` |
| `guestfish`        | `/usr/bin/virt-copy-out`                    | `man1`, `bash-completion` |
| `guestfish`        | `/usr/bin/virt-rescue`                      | `man1`, `bash-completion` |
| `guestfish`        | `/usr/bin/virt-tar-in`                      | `man1`, `bash-completion` |
| `guestfish`        | `/usr/bin/virt-tar-out`                     | `man1`, `bash-completion` |
| `guestmount`       | `/usr/bin/guestmount`                       | `man1`, `bash-completion` |
| `guestmount`       | `/usr/bin/guestunmount`                     | `man1`, `bash-completion` |
 **_TODO:_** Finish the categorization of these files
 
```
guestfish         /etc/libguestfs-tools.conf                 [man5]


libguestfs0       /usr/lib/x86_64-linux-gnu/guestfs/supermin.d/base.tar.gz
libguestfs0       /usr/lib/x86_64-linux-gnu/guestfs/supermin.d/daemon.tar.gz
libguestfs0       /usr/lib/x86_64-linux-gnu/guestfs/supermin.d/excludefiles
libguestfs0       /usr/lib/x86_64-linux-gnu/guestfs/supermin.d/hostfiles
libguestfs0       /usr/lib/x86_64-linux-gnu/guestfs/supermin.d/init.tar.gz
libguestfs0       /usr/lib/x86_64-linux-gnu/guestfs/supermin.d/packages
libguestfs0       /usr/lib/x86_64-linux-gnu/guestfs/supermin.d/udev-rules.tar.gz

libguestfs0       /usr/lib/x86_64-linux-gnu/libguestfs.so.0  [locale, symlink]
libguestfs0       /usr/lib/x86_64-linux-gnu/libguestfs.so.0.510.0

libguestfs0       /usr/share/man/man1
libguestfs0       /usr/share/man/man1/guestfs-building.1.gz
libguestfs0       /usr/share/man/man1/guestfs-faq.1.gz
libguestfs0       /usr/share/man/man1/guestfs-hacking.1.gz
libguestfs0       /usr/share/man/man1/guestfs-internals.1.gz
libguestfs0       /usr/share/man/man1/guestfs-performance.1.gz
libguestfs0       /usr/share/man/man1/guestfs-recipes.1.gz
libguestfs0       /usr/share/man/man1/guestfs-release-notes-1.10.1.gz
libguestfs0       /usr/share/man/man1/guestfs-release-notes-1.12.1.gz
libguestfs0       /usr/share/man/man1/guestfs-release-notes-1.14.1.gz
libguestfs0       /usr/share/man/man1/guestfs-release-notes-1.16.1.gz
libguestfs0       /usr/share/man/man1/guestfs-release-notes-1.18.1.gz
libguestfs0       /usr/share/man/man1/guestfs-release-notes-1.20.1.gz
libguestfs0       /usr/share/man/man1/guestfs-release-notes-1.22.1.gz
libguestfs0       /usr/share/man/man1/guestfs-release-notes-1.24.1.gz
libguestfs0       /usr/share/man/man1/guestfs-release-notes-1.26.1.gz
libguestfs0       /usr/share/man/man1/guestfs-release-notes-1.28.1.gz
libguestfs0       /usr/share/man/man1/guestfs-release-notes-1.30.1.gz
libguestfs0       /usr/share/man/man1/guestfs-release-notes-1.32.1.gz
libguestfs0       /usr/share/man/man1/guestfs-release-notes-1.34.1.gz
libguestfs0       /usr/share/man/man1/guestfs-release-notes-1.36.1.gz
libguestfs0       /usr/share/man/man1/guestfs-release-notes-1.38.1.gz
libguestfs0       /usr/share/man/man1/guestfs-release-notes-1.4.1.gz
libguestfs0       /usr/share/man/man1/guestfs-release-notes-1.40.1.gz
libguestfs0       /usr/share/man/man1/guestfs-release-notes-1.42.1.gz
libguestfs0       /usr/share/man/man1/guestfs-release-notes-1.44.1.gz
libguestfs0       /usr/share/man/man1/guestfs-release-notes-1.46.1.gz
libguestfs0       /usr/share/man/man1/guestfs-release-notes-1.48.1.gz
libguestfs0       /usr/share/man/man1/guestfs-release-notes-1.6.1.gz
libguestfs0       /usr/share/man/man1/guestfs-release-notes-1.8.1.gz
libguestfs0       /usr/share/man/man1/guestfs-release-notes.1.gz
libguestfs0       /usr/share/man/man1/guestfs-security.1.gz
libguestfs0       /usr/share/man/man1/guestfs-testing.1.gz
```

#### Source package `guestfs-tools`

Contains only one binary package: `guestfs-tools`.