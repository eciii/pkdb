
The _physical layer_ is concerned with:

- Signals
- The _medium_, which can be:
	- Guided (using cables and connectors):
		- Electrical (copper)
		- Optical
	- Unguided (a.k.a wireless):
		- Radio waves

Two types of (local) networks:

- Ethernet network (a.k.a LAN)
- Fibre Channel (FC) network (a.k.a SAN)

Using the IP protocol for _routing_ it is possible to interconnect Ethernet networks, giving rise to _(private) intranets_ and _the (public) internet_.

To connect a physical computing machine to a network (either Ethernet or FC) the machine needs a _Network Interface Card (NIC)_ that speaks the specific protocol of the network. Thus there are two types of NICs: Ethernet NICs and FC NICs (a.k.a HBAs).

NICs work at the _local-network layer_ (this is my term for what is known as the link layer in Ethernet networks and the FC-2 layer in FC networks). This level handles the logic of the network protocol and is completely unaware of the physical implementation.

The local-network layer handles information in digital bits while the physical layer does it using analog signals. The conversion of digital bits into signals and vice-versa is performed by an intermediate level that I'll call the _transceiver layer_, because the transceiver is the physical component that performs the conversion.

There are two options for the location and form of the transceiver:

- It can be an integrated chip on the NIC itself (also commonly called the _PHY chip_). In this case the port is "media-dependent", meaning that the physical media is already determined by the port. This is usually the case in low-end Ethernet NICs, which come with standard RJ45 ports (which means that only electrical cables can be used).  
- The NIC can have "media-independent" ports. In this case the transceiver is a "modular" and "swappable" piece of hardware that is plugged into the port. Nowadays SFPs are used for that but  

A NIC has one or more _ports_, each one with its own transceiver. If the NIC has more than one port then the NIC exposes each of them separately to the machine. Thus the machine treats them independently; they just happen to share the same _expansion bus_. 





Fibre Channel is a **transport layer** protocol. It doesn't inherently "speak" storage commands - it needs a higher-level protocol on top of it.

The most common protocol used over Fibre Channel is **FCP (Fibre Channel Protocol)**, which encapsulates SCSI commands. So ironically, even though SCSI as a physical interface is obsolete, SCSI as a _command set_ is still very much alive and running over Fibre Channel! This is often called "SCSI over FC."

There are other protocols that can run over FC too (like FICON for mainframes), but FCP/SCSI is by far the most common in standard server environments.

So to directly answer your question: FC by itself isn't enough - you need SCSI commands (or another block storage protocol) running on top of it to actually communicate with storage devices.

---

Hi, I'm trying to understand the typical devices attached to enterprise grade "hyperconverged" servers. I have the following list:

- Network cards with network ports (ethernet and fiber)
- Storage devices for OS (usually M.2 and in a RAID configuration)
- Additional direct-attached storage
- SAN cards