

Electronic Design Automation (EDA) software is the software used for the design of electronic devices. Wikipedia offers an [overview of EDA software](https://en.wikipedia.org/wiki/Comparison_of_EDA_software).

Two main disciplines in the specific field of _digital_ electronic devices:

- Integrated Circuit (IC, a.k.a _microchip_ or sometimes just _chip_) design:
	- ICs can be general-purpose (Processors (CPUs, GPUs, DSPs), FPGAs), or application-specific (ASICs). _\[Question: There are also scalar, vector, matrix and tensor units... what are these?]_
	- Architecturally, might be composed of reusable components called [Intellectual Property (IP) cores](https://en.wikipedia.org/wiki/Semiconductor_intellectual_property_core). [SiFive](https://www.sifive.com/) is a RISC-V IP provider.
	- At the highest level of their design a [Hardware Description Language (HDL)](https://en.wikipedia.org/wiki/Hardware_description_language) is used. The most widespread HDLs are Verilog and VHDL. However they are still very low-level (similar to assembly languages in software development) so even higher-level HDLs, like [Chisel](https://en.wikipedia.org/wiki/Chisel_(programming_language), can be used. Chisel is based on Scala and is part of the [CHIPS Alliance](https://www.chipsalliance.org/).
- Printed Circuit Board (PCB) design:
	- Architecturally, is composed of reusable components like ICs, hardware interfaces, buses, etc.

Interesting FOSS EDA projects relevant for the design of digital electronic devices are:

- [Verilator](https://www.veripool.org/verilator/), a Verilog/SystemVerilog simulator
- [KiCad](https://www.kicad.org/), for PCB design

Misc links:

- https://www.youtube.com/@RobertFeranec