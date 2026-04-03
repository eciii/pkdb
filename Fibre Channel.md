Fibre Channel (FC) is a family of protocols and specifications that define a full stack of network layers, from the physical layer to the application layer. However FC does not follow the traditional OSI/Internet model. Instead it defines its own stack of layers:

- **FC-0** → Physical (cables, connectors, optical/electrical signaling)
- **FC-1** → Encoding (encoding, synchronization)
- **FC-2** → Framing & flow (framing, addressing, flow control, routing via Fabric)
- **FC-3** → Common services (Multicast, striping, hunt groups)
- **FC-4** → Application layer (maps to SCSI, IP, FICON etc.)

---

(From Claude)

**The most important insight: FC-2 collapses multiple OSI layers**

In the traditional OSI/TCP-IP world you have a clear separation:

- **Ethernet (L2)** handles local delivery with MAC addresses
- **IP (L3)** handles routing across networks with IP addresses
- **TCP (L4)** handles reliable end-to-end delivery, flow control, retransmission

FC-2 does **all of this in a single layer**. This was a deliberate design choice — FC was purpose-built for storage traffic, so instead of stacking general-purpose protocols on top of each other, it merged these functions into one tightly integrated layer optimized for:

- **Very low latency** — no overhead of multiple protocol headers
- **Lossless delivery** — FC fabric is designed to never drop frames (unlike IP networks which are inherently best-effort)
- **Hardware-level flow control** — called **buffer-to-buffer credits**, built into the protocol itself rather than handled by a transport layer like TCP

**Addressing: WWN vs MAC vs IP**

FC uses **World Wide Names (WWN)** — 64-bit globally unique identifiers burned into the HBA, conceptually similar to MAC addresses. But unlike Ethernet where MAC addresses are only relevant on the local segment and IP takes over for routing, WWNs are used end-to-end across the entire FC fabric. There's no equivalent of an IP address layered on top.

**How this compares conceptually**

In TCP/IP the philosophy is **separation of concerns** — each layer does one job and is independent of the others. This makes it flexible and general purpose but adds overhead.

FC's philosophy is **vertical integration for a specific purpose** — collapse the layers, eliminate redundancy, and optimize everything for fast, reliable, lossless storage traffic. You gain performance and predictability but lose flexibility. FC is not a general purpose networking stack — it was never meant to be.