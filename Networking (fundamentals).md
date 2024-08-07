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
	- Mostly used to carry TCP/IP

**Network Layer**

- IP
- SCSI. There is also iSCSI which is kind of SCSI over IP.
