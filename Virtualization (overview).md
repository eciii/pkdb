#Linux_Technology

Some relevant projects are:

**Virt Tools**: \[[Homepage](https://www.virt-tools.org/)]

A collection of projects that provide virtualization capabilities to Linux. The projects, although strongly coupled, are independent of each other. The projects can be broadly categorized in two groups:

- A software stack (I'll call it _VirtStack_) that turns Linux into an hypervisor. The stack is composed of three projects: [KVM](http://www.linux-kvm.org/), [QEMU](http://qemu.org/) and [[libvirt]].
- A suite of supporting tools for VirtStack hypervisors (I'll call it _[[VirtTools]]_). These tools are provided in three different projects: [libguestfs](https://libguestfs.org/), [virt-manager](https://virt-manager.org/) and [libosinfo](http://libosinfo.org/).

**cloud-init**: \[[Homepage](https://cloudinit.readthedocs.io)]

A project and a de-facto standard for the initialization of cloud virtual machines.

---

**Virtualized networking**

- [kvm - virt-manager: is it possible to assign specific IP addresses to certains VMs via the virtual DHCP? - Unix & Linux Stack Exchange](https://unix.stackexchange.com/questions/174884/virt-manager-is-it-possible-to-assign-specific-ip-addresses-to-certains-vms-via)
- [libvirt: Network XML format](https://libvirt.org/formatnetwork.html#elementsAddress)
- [Networking - Libvirt Wiki](https://wiki.libvirt.org/page/Networking)
- [libvirt Networking Handbook — Jamie Nguyen](https://jamielinux.com/docs/libvirt-networking-handbook/)
- [BridgeNetworkConnections - Debian Wiki](https://wiki.debian.org/BridgeNetworkConnections)
- [iproute2 - Wikipedia](https://en.wikipedia.org/wiki/Iproute2)
- [iproute2/iproute2.git - Iproute2 routing commands and utilities](https://git.kernel.org/pub/scm/network/iproute2/iproute2.git/)
- [Canonical Netplan](https://netplan.io/)
- [rtnetlink(7) - Linux manual page](https://www.man7.org/linux/man-pages/man7/rtnetlink.7.html)