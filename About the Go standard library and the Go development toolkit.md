The go directory that is extracted from **the Go distribution installer** during the installation of Go in Linux contains two primary components: **the standard library source code** and **the Go development toolkit**.

The standard library source code is located in `src/`. The content of `src/` can be classified into:

- The actual standard library source code composed of:
  - The `go.mod` and `go.sum` files that define the standard library as a go module. The same dependencies are shipped in the `vendor` directory to support building in GOPATH mode. See the notes in the `README.vendor` file.
  - All other directories except `cmd` and `testdata`. With the exception of these two directories, all other directories contain the packages that constitute the standard library.
- The `cmd` directory. This is a separate Go module that contains the source code of the commands in the Go development toolkit.
- The remaining files not mentioned here so far, which, with the exception of `Make.dist`, always end in either `.bash`, `.bat` or `.rc`. These files are used to build the Go distribution installer for Linux, Windows and Plan9 respectively.
- The `testdata` directory that contains just one file used to test the `compress` packages of the standard library. There was once a [failed attempt](https://github.com/golang/go/commit/43cd90701725278ff403c03e9304bbfc76f8bc0c) to move this directory into the `compress` package.

The Go development toolkit is the set of programs defined in `src/cmd`. The content of `src/cmd` can be classified into:

-  The `go.mod` and `go.sum` files that define the Go development toolkit as a go module. The same dependencies are shipped in the `vendor` directory to support building in GOPATH mode. See the notes in the `README.vendor` file.
- The `internal` directory, which contains helper packages for internal use by the tools in the toolkit.
- All other directories. Each directory corresponds to a tool in the toolkit.

With the exception of `api` all other tools are also shipped in binary form with the Go distribution installer. The primary and most important of these tools is the `go` tool, whose binary is located in `bin/`. All other tools, with the exception of `api` and `gofmt`, have the following properties:

- Their binaries are located in `pkg/tool/[GOOS]_[GOARCH]/`. Conversely, all binaries there correspond to a tool in the Go development toolkit.
- In addition to direct invocation they can also be invoked using the `go tool` subcommand. Conversely, all the available subcommands of `go tool` correspond to a tool in the Go development toolkit.

The `gofmt` is special because its binary is located in `bin/`. Since this makes the invocation of `gofmt` as easy as the invocation of the `go` tool itself, there is no need to include it as a subcommand of `go tool`.

The `gofmt`, `doc`, `fix` and `vet` tools also have the particularity that they are also direct subcommands of the `go` tool (in the case of `gofmt` the subcommand is just `fmt`). Run like this, the tool has a different set of options and operates on complete packages.

Finally the `api` tool is special because it is not shipped in binary form at all. It is meant for internal use by the Go team to ensure that the Go API remains backwards-compatible.

### Summary of the main `go` tool (to improve...)

    go
    ├── version   print Go version
    ├── env       print Go environment information
    │
    ├── help      print help about go commands or additional topics
    ├── doc       show documentation for package or symbol
    │
    ├── generate  generate Go files by processing source
    ├── build     compile packages and dependencies
    ├── install   compile and install packages and dependencies
    ├── run       compile and run a Go program
    ├── test      test packages
    ├── clean     remove object files and cached files
    │
    ├── fmt       gofmt (reformat) package sources
    ├── fix       update packages to use new APIs
    ├── vet       report likely mistakes in packages
    │
    ├── get       add dependencies to current module and install them
    ├── list      list packages or modules
    ├── mod       module maintenance
    │
    ├── bug       start a bug report
    │
    └── tool      run specified go tool
        ├── doc
        ├── fix
        ├── vet
        │
        ├── cgo
        ├── compile
        ├── asm
        ├── link
        │
        ├── dist
        │
        ├── api
        ├── objdump
        │
        ├── addr2line
        ├── buildid
        ├── cover
        ├── nm
        ├── pack
        ├── pprof
        ├── test2json
        └── trace

### Summary of the standard library packages (to improve...)

    # Packages that offer language-dependent functionality
    builtin
    debug
    embed
    go
    plugin
    reflect
    runtime
    testing
    unsafe

    # Packages that offer language-agnostic functionality
    archive
    bufio
    bytes
    compress
    container
    context
    crypto
    database
    encoding
    errors
    expvar
    flag
    fmt
    hash
    html
    image
    index
    io
    log
    math
    mime
    net
    os
    path
    regexp
    sort
    strconv
    strings
    sync
    syscall
    text
    time
    unicode