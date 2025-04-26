The processor executes instructions in order by default. Jumps are allowed as part of the normal flow of execution to implement conditional and looping statements. But sometimes the normal flow of execution is disrupted by special events:

- Sudden interruptions because of an I/O event (_Interrupts_)
- Intentional requests to the kernel (_system calls_; implemented using _traps_? what's the difference?)
- Unintentional abnormal/erroneous instructions (_exceptions_). Two types can be distinguished:
    - _Faults_ might be recoverable (from the point of view of the CS, but the OS may decide never to recover)
    - _Aborts_ are never recoverable

Interrupts are asynchronous whereas traps and exceptions are synchronous.

Interrupts and traps always resume execution on the next instruction, whereas exceptions, when recovered, resume execution on the same instruction that generated them.

Interrupts and traps are defined by the OS whereas exceptions are defined by the CS.

The CS handles these disruptive events by means of a table allocated by the OS on boot (_the exception handler table_, so named because it originally handled only exceptions?). Each disruptive event has an integer code associated to it. Exceptions come first, usually the first 32 items, and then come the interrupts and traps. The code of the event is used to index the corresponding _handler_ that will be executed when such an event occurs. System calls are implemented by defining another table, the _system call table_, that is used to determine the handler of each system call.

---

Modern Operating Systems
1.3 Computer Hardware Review
1.3.1 Processors

Each CPU has a specific set of instructions that it can execute: the **ISA (Instruction Set Architecture)**. CPUs also contain a number of **registers**. Some examples are:

- General purpose registers to hold variables and temporary results
- Special registers:
    - program counter: points to the next instruction to be fetched
    - stack pointer: points to the top of the current stack in memory
    - PSW (Program Status Word): contains, among other things, the condition code bits that are set by comparison instructions, the CPU priority and the mode (kernel or user)

The basic cycle of every CPU is to fetch the instruction pointed by the program counter register, decode it to determine it's type and operands, and execute it. Once the execution is done then the same cycle is repeated wit the next instruction and so on.

But to improve performance, CPU designers have long abandoned that simple model. Some advances in this respect are:

- Pipelines: While one instruction is being executed the next one is being decoded and the next one is being fetched, all at the same time. In reality longer pipelines are common. One issue with this design is that if an instruction is in the pipeline it _must_ be executed, even if a previous instruction branched to somewhere else (how is this solved?).
- Superscalar CPUs: The CPU contains separate execution units for things like integer arithmetic, floating -point arithmetic, and so on. Also múltiple fetch-decode pipelines are present, such that several decoded instructions can be temporarily stored in a _holding buffer_ that then sends them to the corresponding execution unit once it is available for execution. This design implies that instructions are executed out-of-order (how is this solved?).
- Multithreading (or as Intel calls it, hyperthreading): Each CPU is able to hold the state of a number of threads (of the same process) at the same time and is then able to switch execution between in nanoseconds. While this does not offer true parallelism, thread-switching time is reduced to the order of nanoseconds. One issue with this design is that, since each CPU-thread is presented to the OS as a separate CPU, the OS might get confused and schedule work meant to be executed with true parallelism, on different threads of the same CPU instead of on different CPUs (how is this solved?).
- Multi-core CPUs and multi-socket boards: this do offer true parallelism.
- Microcode: Complex instructions are break up into multiple simpler internal instructions (architectures with such complex instructions are called CISC and architectures without them are called RISC). This implies that an instruction is no longer either executed or not executed at the end of a cycle but it might be only partially executed (how is this solved?).

GPUs are processors with literally thousands of tiny cores. they are good for many small computations done in parallel but not so good at serial tasks.
