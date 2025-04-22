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