
Links:
- Official Proxmox:
	- [HTML documentation](https://pve.proxmox.com/pve-docs/pve-admin-guide.html).
	- [Git repo](https://git.proxmox.com/).
- Official Ceph:
	- [Documentation](https://docs.ceph.com/en/latest/).
	- [Red Hat Ceph Storage Documentation](https://docs.redhat.com/en/documentation/red_hat_ceph_storage).
	- [Intro to Ceph](https://www.youtube.com/watch?v=PmLPbrf-x9g) video by Sage Weil.
- Relevant links:
	- Proxmox Wiki:
		- [Migrate to Proxmox VE](https://pve.proxmox.com/wiki/Migrate_to_Proxmox_VE).
		- [Windows VirtIO Drivers](https://pve.proxmox.com/wiki/Windows_VirtIO_Drivers).
	- [Storage Strategies Guide](https://docs.redhat.com/en/documentation/red_hat_ceph_storage/7/html/storage_strategies_guide/index) by Red Hat.
	- [Git repo](https://github.com/tcude/vmware-to-proxmox-migration-script) with scritps to migrate VMs from ESXi to Proxmox.
- Potentially useful to automate the installation of Windows VirtIO Drivers:
	- [virtio-win project](https://github.com/virtio-win)
	- [pywinrm](https://pypi.org/project/pywinrm/)
	- [msiexec](https://learn.microsoft.com/de-de/windows-server/administration/windows-commands/msiexec)
- Relevant source code for the ESXi import wizard:
	- The [`pve-storage.git`](https://git.proxmox.com/?p=pve-storage.git;a=summary) repo contains the backend Perl modules. In particular the files `src/PVE/Storage.pm` and `src/PVE/Storage/ESXiPlugin.pm`.
	- The [`pve-esxi-import-tools.git`](https://git.proxmox.com/?p=pve-esxi-import-tools.git;a=summary) repo contains the code (in Rust) that implements the FUSE file system that enable the ESXi storage.
	- The [`pve-manager.git`](https://git.proxmox.com/?p=pve-manager.git;a=tree) contains the frontend Javascript and main backend Perl modules. The frontend part of the import widget is implemented in the file `www/manager6/window/GuestImport.js`.
- Relevant source code for `qm importovf`:
	- The [`qemu-server.git`](https://git.proxmox.com/?p=qemu-server.git;a=summary) contains tools to interact with QEMU, in particular `qm`. The `importovf` action is implemented in the file `PVE/QemuServer/OVF.pm`.

---

**Notes about installing Proxmox**

The GUI installer asks very few things. In particular there is no way to configure partitions for the detected disks.

Automated installation makes use of a TOML file with the answers to the questions in the GUI (see [this wiki article](https://pve.proxmox.com/wiki/Automated_Installation)). But it seems that there is also no way to configure partitions using this method. Other considerations are:

- The `disk_list` must contain only the disks that are going to be used by Proxmox itself, not by Ceph.
- To include the answer file into the Proxmox ISO we need the `proxmox-auto-install-assistant` tool that is shipped by a Debian package of the same name. But this Debian package can only be installed from the Proxmox repositories. This creates a kind of chicken-and-egg problem.

The APT repositories have to be changed from "enterprise" to "no subscription". An additional "apt upgrade" was also necessary. All this was done using the GUI.