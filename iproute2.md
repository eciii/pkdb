#OSS_Project_Overview 

iproute2 is a collection of utilities for controlling TCP/IP networking in Linux. It is the new recommended toolkit to manage networking in Linux.

The source code of the project is located in the [[cgit]] repository of the [[Linux kernel]], under the paths `/iproute2/iproute2.git` and `/iproute2/iproute2-next.git`.

Previously a collection of tools from various projects where used to manage networking in Linux. In particular those from the [net-tools](https://net-tools.sourceforge.io/) (`ifconfig` and `route` among others) and [bridge-utils](https://git.kernel.org/pub/scm/network/bridge/bridge-utils.git/) (`brctl`) projects. The use of all this tools is now deprecated in favor of iproute2.

iproute2 uses internally **netlink sockets** ([man](https://man7.org/linux/man-pages/man7/netlink.7.html)) to communicate with the kernel for all the networking management tasks. Netlink sockets are one of the four mechanisms offered by the kernel that allow userspace to interact with the kernel (the other ones being system calls, the proc filesystem and ioctl). Netlink sockets are essentially sockets from the `AF_NETLINK` domain. The Linux Kernel Documentation website has a very good [introduction to Netlink](https://docs.kernel.org/next/userspace-api/netlink/intro.html).