[WireGuard](https://www.wireguard.com/) is essentially a **secure network tunnel specification** designed by [[Jason A. Donenfeld]]. The design is described in the [WireGuard official whitepaper](https://www.wireguard.com/papers/wireguard.pdf). He also develops and releases a number of **implementations**, each of which supports one or more **platforms** (currently supported platforms are Linux/Android, Windows, macOS/iOS, FreeBSD and OpenBSD)

Implementations (among other related software) can be found in Donenfeld's official cgit website and/or in [WireGuard's official GitHub website](https://github.com/WireGuard).

An implementation can be either a **userspace implementation** or an **in-kernel implementation**. Userspace implementations are normally less performant than in-kernel implementations, since userspace implementations require additional context-switching for the processing of (UDP) packets.

The **wireguard-tools** ([cgit](https://git.zx2c4.com/wireguard-tools)) project defines a number of tools to make it easier to work with current implementations. The tools are:

- `wg(8)`, a tool used to manage the configuration of WireGuard tunnel interfaces. It is written in C and works on all supported platforms. The scope of this tool is to only manage _existing_ tunnel interfaces.
- `wg-quick(8)`, a tool to easly create/delete WireGuard tunnel interfaces. It is intended to be used in very small and simple setups. This tool is implemented as a bash script for most platforms (Linux, macOS/iOS, FreeBSD and OpenBSD; note that there is a separate bash script for each platform) and as a C program for Android. Notably there is no `wg-quick(8)` implementation for Windows.
- Additional useful tools under the `contrib` directory. Some of these tools are C programs, some are bash scripts and there is no explicit intention to make them cross-platform in any special way. Each tool works where it is intended to work and no more.

There are currently two official userspace implementations:

- **wireguard-go** ([cgit](https://git.zx2c4.com/wireguard-go)): Written in Go and essentially fully developed. It works on all supported platforms. For Windows a custom virtual tun device called **wintun** ([website](https://www.wintun.net/), [cgit](https://git.zx2c4.com/wintun)) was developed (in C) as a separate project. Go bindings reside in yet another project called **wintun-go** ([cgit](https://git.zx2c4.com/wintun-go)).
- **wireguard-rs** ([cgit](https://git.zx2c4.com/wireguard-rs/)): Written in Rust and still in development. Currently it works only in Linux but there are plans to support FreeBSD, OpenBSD and Windows.

There are currently four official in-kernel implementations (notably there is no in-kernel implementation for macOS/iOS):

- **wireguard-linux** ([cgit](https://git.zx2c4.com/wireguard-linux)): Dynamic kernel module (written in C). The **wireguard-linux-compat** ([cgit](https://git.zx2c4.com/wireguard-linux-compat)) project provides backports to older Linux kernels. This implementation also works on Android but the building and integration of it in Android seems to be more involved because there are two additional projects just to do that (both projects are essentially collections of shell scripts and makefiles):
  - **android_kernel_wireguard** ([cgit](https://git.zx2c4.com/android_kernel_wireguard)): Provides scripts for integrating WireGuard into Android
  - **android-wireguard-module-builder** ([cgit](https://git.zx2c4.com/android-wireguard-module-builder)): Provides scripts for building wireguard kernel modules for Android.
- **wireguard-nt** ([cgit](https://git.zx2c4.com/wireguard-nt/)): Windows kernel DLL written in C.
- **wireguard-freebsd** ([cgit](https://git.zx2c4.com/wireguard-freebsd)): Kernel module for FreeBSD. Written in C.
- **wireguard-openbsd** ([cgit](https://git.zx2c4.com/wireguard-openbsd)): Kernel module for OpenBSD. Written in C.

Additional "GUI client" projects exist that are based on one of the above-mentioned implementations depending on the platform:

- **wireguard-windows** ([cgit](https://git.zx2c4.com/wireguard-windows)): Written in Go and based on the wireguard-nt implementation. Official recommended way of using WireGuard on Windows.
- **wireguard-apple** ([cgit](https://git.zx2c4.com/wireguard-apple)): Written in Swift and based on the wireguard-go implementation. Official recommended way of using WireGuard on macOS/iOS.
- **wireguard-android** ([cgit](https://git.zx2c4.com/wireguard-android)): Written in Kotlin. It opportunistically uses the kernel implementation, otherwise falls back to using the non-root userspace implementation. Official recommended way of using WireGuard on Android.