
Homepage: https://www.virt-tools.org

**Virt Tools** is an initiative that aggregates several projects with the aim of providing an integrated virtualization framework on Linux. The projects, although strongly coupled, are independent of each other.

The list of projects can be broadly categorized in two groups:

- A software stack (I'll call it the _Virt stack_) that turns Linux into a hypervisor. The stack is composed of three projects:
	- [KVM](http://www.linux-kvm.org/)
	- [QEMU](http://qemu.org/)
	- [libvirt](http://libvirt.org/).
- A suite of supporting tools for the Virt stack (I'll call it _Virt toolkit_). These tools are provided by three different projects:
	- [virt-manager](https://virt-manager.org/)
	- [libguestfs](https://libguestfs.org/)
	- [libosinfo](http://libosinfo.org/).

A project related to the Virt toolkit is [Virt Viewer](https://gitlab.com/virt-viewer). It is not mentioned in the Virt Tools website but it is mentioned in the virt-manager website. There seems to be some confusion because the virt-manager website lists virt-viewer as part of its toolset but virt-viewer is nowhere under its gitstore. In fact virt-viewer has its own gitstore at https://gitlab.com/virt-viewer, but unusually has no website of its own.

Another project related to the Virt toolkit is [ndbkit](https://gitlab.com/nbdkit). It is also not mentioned in the Virt Tools website but some of its gitrepos were previously located under the gitstore of libguestfs. Now they live under the new ndbkit project, with gitstore https://gitlab.com/nbdkit but no website of its own.

The Virt stack is the virtualization stack officially promoted and supported by Red Hat. This is described in the Red Hat blog post [All you need to know about KVM userspace](https://www.redhat.com/en/blog/all-you-need-know-about-kvm-userspace) by Paolo Bonzini (maintainer of KVM and Red Hat employee).

---

Overview of the projects:

**libvirt**

Website: https://libvirt.org/
Gitstore: https://gitlab.com/libvirt

libvirt is both a library and a daemon that provide a unified API for managing VMs and containers from several technologies/vendors including: QEMU, KVM, Xen, LXC, OpenVZ and [more](https://libvirt.org/drivers.html).

**virt-manager (a.k.a Virtual Machine Manager)**

Website: https://virt-manager.org/
Gitstore: https://github.com/virt-manager

The gitstore contains three gitrepos:

- virt-manager: Desktop application to manage QEMU/KVM/Xen VMs and LXC containers, all of them via libvirt. It also includes three command-line tools:
	- `virt-install`: automates the installation of OS in VMs (uses libosinfo)
	- `virt-clone`: clones existing inactive guests
	- `virt-xml`: simplifies the editing of libvirt domain XMLs
- virt-bootstrap: Command-line tool that provides an easy way to setup the root file system for LXC containers.
- virt-manager-web: Source code of the virt-manager.org website.

**virt-viewer**

Website: (none)
Gitstore: https://gitlab.com/virt-viewer

**libguestfs**

Website: https://libguestfs.org/
Gitstore: https://github.com/libguestfs

- Temporary summary of global aspects in `~/Projects/summary-libguestfs.txt`.
- Temporary summary of `virt-sysprep` in `~/Projects/summary-virt-sysprep.txt`.

**libosinfo**

Website: http://libosinfo.org/
Gitstore: https://gitlab.com/libosinfo

**ndbkit**

Website: (none)
Gitstore: https://gitlab.com/nbdkit