**General concurrency concepts**

- In linux, to see how many CPUs are available see [here](https://unix.stackexchange.com/questions/218074/how-to-know-number-of-cores-of-a-system-in-linux). In general the answer is `CPUs = Sockets * Cores per socket * Threads per Core` where `Sockets` refer to the number of physical processor sockets the motherboard has, `Cores per socket` refers to the number of cores in each processor and `Threads per core` refers to the number of parallel threads a core is capable of executing (this last number is possible due to [hyper-threading](https://en.wikipedia.org/wiki/Hyper-threading), or more generally [simultaneous multithreading](https://en.wikipedia.org/wiki/Simultaneous_multithreading)). *Note*: all these three numbers can be tweaked in virt-manager.

**Go concurrency concepts**

- The Go runtime schedules and executes goroutines using multiple OS threads. It also distinguishes between threads for user-level Go code and threads for thread-blocking system calls that must be executed on behalf of user-level Go code. The Go runtime will create and manage at most `runtime.GOMAXPROCS(0)` threads for user-lever Go code at any time. This number is set up at startup to be `runtime.NumCPU()`, i.e. the available number of OS CPUs. However there is no limit to the number of threads for thread-blocking system calls and the runtime will create and manage any number of them at any time as necessary.

_To continue_

- Important aspects: Inter-Goroutine Communication (IGrC) and Inter-Goroutine Synchronization (IGrS)
- Goroutines
- Channels
- WaitGroups (`sync.WaitGroup`)