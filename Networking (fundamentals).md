Technologies typically used in **Telecommunications** at the lower levels of the network stack:

**Pysical layer**

- Wired
	 - [Electrical](https://en.wikipedia.org/wiki/Electrical_wiring) (all cables currently used for Telecom and IT applications are [copper-based](https://en.wikipedia.org/wiki/Copper_conductor))
		 - [Coaxial cables](https://en.wikipedia.org/wiki/Coaxial_cable) (some history [here](https://www.arrl.org/files/file/Technology/pdf/QST_Aug_2001_p62-64.pdf)):
			 - Used for early transoceanic telegraph lines (later replaced by transoceanic telephone lines).
			 - Used for early transoceanic telephone lines (later replaced by optical fiber communication lines).
			 - Used for the ARPANET and early Internet backbone (later replaced by optical fiber) \[needs verification... all I was able to find is that ARPANET used "leased lines from common carriers", e.g see [here](https://web.archive.org/web/20160324032800/http://www.packet.cc/files/arpanet-computernet.html) and [here](https://www.walden-family.com/public/1970-imp-afips.pdf)].
			 - Used for early Ethernet LANs (later replaced by twisted pair cables).
			 - Still used for cable television/internet but slowly being replaced by optical fiber using [FTTX](https://en.wikipedia.org/wiki/Fiber_to_the_x).
		 - [Twisted pair cables](https://en.wikipedia.org/wiki/Twisted_pair):
			 - Invented by Alexander Graham Bell.
			 - Used for telephone lines since the very early days (some history [here](https://www.copper.org/applications/telecomm/consumer/evolution.html)) and still used now for telephone/internet in the form of DSL, although slowly being replaced by optical fiber using [FTTX](https://en.wikipedia.org/wiki/Fiber_to_the_x).
			 - Used for Ethernet LANs.
	- [Optical](https://en.wikipedia.org/wiki/Optical_fiber)
		- [Fiber-optic cables](https://en.wikipedia.org/wiki/Fiber-optic_cable)
			- Beyond LANs, mostly replaced all other wire technologies. An interesting technology at this level is [Wavelength-division multiplexing](https://en.wikipedia.org/wiki/Wavelength-division_multiplexing).
			- On LANs, mostly used to carry Fibre Channel.
- Wireless (using [radio waves](https://en.wikipedia.org/wiki/Radio) and [microwaves](https://en.wikipedia.org/wiki/Microwave) in the electromagnetic spectrum). Used for many applications but the most relevant to LANs is:
	- [Wireless LAN (WLAN)](https://en.wikipedia.org/wiki/IEEE_802.11)
		- Part of the IEEE 802 standard family (IEEE 802.11). This is the same standard family of the Ethernet standard.
		- Uses the S band (2.4 GHz) and C band (5 and 6 GHz) of the microwave spectrum.

**Link layer**

- [Ethernet](https://en.wikipedia.org/wiki/Ethernet)
	- Part of the [IEEE 802](https://en.wikipedia.org/wiki/IEEE_802) standard family (IEEE 802.3). This is the same standard family of the WLAN standard.
	- Mostly used to carry IP.
- [Fibre Channel](https://en.wikipedia.org/wiki/Fibre_Channel)
	- Mostly used to carry [SCSI](https://en.wikipedia.org/wiki/SCSI). This setup is implemented using the [Fibre Channel Protocol (FCP)](https://en.wikipedia.org/wiki/Fibre_Channel_Protocol).
	- Also used to carry IP. This setup is called [IPFC](https://en.wikipedia.org/wiki/IPFC).
- [Wi-Fi](https://en.wikipedia.org/wiki/Wi-Fi)
	- Based on the WLAN IEEE 802.11 standard and designed to be fully compatible with Ethernet.
	- Mostly used to carry IP

**Network Layer**

- IP
- SCSI. There is also iSCSI which is kind of SCSI over IP.

---

### Other concepts

To learn more: [CCNA 200-301 Official Cert Guide Library](https://www.amazon.com/CCNA-200-301-Official-Guide-Library/dp/0138221391).

**About multihoming/aggregation**

Multihoming:

- Requirement: Network interfaces must be in different networks.
- Two approaches:
	- Basic (uses static routes).
	- Advanced (uses dynamic routing (BGP))

Problems may arise if two network interfaces are configured with IPs in the same subnet. See https://duckduckgo.com/?q=network+interfaces+in+the+same+subnet. In particular:

- [This SE question](https://serverfault.com/questions/415304/multiple-physical-interfaces-with-ips-on-the-same-subnet) mentions [Strong vs. Weak End System Models](https://wiki.treck.com/Appendix_C:_Strong_End_System_Model_/_Weak_End_System_Model).
- [This result](https://access.redhat.com/solutions/30564) by Red Hat and [this SE question](https://serverfault.com/questions/197752/several-ip-address-within-the-same-subnet-on-the-same-host).

Network aggregation (a.k.a Bonding, Teaming, Trunking, Bundling). Two main advantages: redundancy (via fail-over) and performance (via load-balancing).

- In Linux: The Linux kernel offers _bonding_. RHEL 7 tried to introduce _teaming_ but it didn't gain traction. It was removed in RHEL 9.
- IP aliasing.
- VLANs.
	- [Article](https://www.comparitech.com/net-admin/inter-vlan-routing-configuration/) about inter-VLAN routing.
- LACP/EtherChannel.


**Broader Networking topics**

Data Center Networking:

- Topologies: traditional multi-tiered, fat tree, leaf-spine (see the [NetworkChuck video](https://www.youtube.com/watch?v=6-66D9J5PkY) on this topic).
- VLAN configuration in the data center:
	- IEEE standards: Multiple VLAN Registration Protocol (MVRP). It is part of the Multiple Registration Protocol (MRP) that replaced the Generic Attribute Registration Protocol (GARP).
	- VLAN Trunking Protocol (VTP), a similar but proprietary protocol from Cisco.
- For more on this topic the book [Essentials of Cloud Computing](https://link.springer.com/book/10.1007/978-3-031-32044-6) might be interesting.

WAN Networking:

- See the [NetworkChuck video](https://www.youtube.com/watch?v=xPi4uZu4uF0) on this topic.
- See the attachments of [this seminar](https://nsrc.org/workshops/2014/mongolia-ixp/wiki/Track1Agenda.html#no1).