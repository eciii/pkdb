Standard I/O syscalls in Linux (like `open`, `read`, `write`, etc) are blocking by default. Those calls can be made non-blocking using the `O_NONBLOCK` flag when the file is opened. However this won't work for buffered disk I/O operations (basically operations on regular files and directories). For this reason, concurrency for buffered disk I/O must be treated separately from concurrency for other I/O types.

### Concurrent non-buffered-disk I/O

The `O_NONBLOCK` flag provides a very primitive and limited way to achieve concurrency. More complex concurrency strategies can be achieved by one of three methods (all of them explained in chapter 63, "Alternative I/O models", of The Linux Programming Interface)

- I/O multiplexing (using the `select` or `poll` syscalls)
- Signal-driven I/O
- The Linux-specific `epoll` API

The `epoll` API is usually considered the most performant and flexible of the three. For example the runtime poller of Go in Linux uses `epoll` (see [here](https://cs.opensource.google/go/go/+/refs/tags/go1.18.1:src/runtime/netpoll_epoll.go;l=5))

### Concurrent buffered disk I/O

The traditional approach has always been to spawn "helper threads" to deal with buffered disk I/O, allowing the remaining threads to continue to do other work. This approach is efficient if the I/O operations result in page cache misses, but otherwise it adds unnecessary inter-thread communication overhead.

The problems with concurrent buffered disk I/O in Linux seem to have a long history (see all the LWN articles about it [here](https://lwn.net/Kernel/Index/#Asynchronous_IO)):

- Long ago (when?) POSIX specified the POSIX asynchronous I/O (AIO) API. Since this was just an API specification, platform developers had to implement it using whatever concurrency technology was available in that platform. At that time Linux lacked of such technology and thus the API was implemented by glibc using standard threads (instead of providing an in-kernel implementation). See the Notes section in [aio(7)](https://man7.org/linux/man-pages/man7/aio.7.html).
- Later on, starting with Linux 2.5, an in-kernel implementation of AIO was provided but it never really supported buffered disk I/O. This implementation uses system calls that have no glibc wrapper, thus one can either call the system calls directly or use the [libaio](https://pagure.io/libaio/tree/master) library instead (see the Notes section of any of these system calls, for example [io_submit(2)](https://man7.org/linux/man-pages/man2/io_submit.2.html)).  Many attempts have been made to add support to buffered disk I/O in AIO but all of them seem to have failed (see LWN articles [here](https://lwn.net/Articles/73847/), [here](https://lwn.net/Articles/216200/) and [here](https://lwn.net/Articles/724198/)).
- The latest attempt, called io_uring, to provide buffered disk I/O in the kernel has been more successful (see LWN articles [here](https://lwn.net/Articles/776703/) and [here](https://lwn.net/Articles/810414/)). An explanation can be found [here](https://kernel.dk/io_uring.pdf).

### C libraries for portable concurrent I/O

Other platforms may have different concurrency mechanisms (e.g BSD uses `kqueue` and Windows uses its own version of `select`). Two widely used libraries to write *portable* concurrent I/O are [libevent](https://libevent.org/) and [libuv](https://libuv.org/).