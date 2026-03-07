### Getting Started (1)

#### Installation (1.1)

The recommended way to install Rust on Linux is using `rustup`, which is a command-line tool for managing Rust versions and associated tools. `rustup` and Rust are installed by running

    $ curl --proto '=https' --tlsv1.2 https://sh.rustup.rs -sSf | sh

and following the instructions. The standard installation (option 1) will:

- install `rustup` metadata and toolchains  under `$HOME/.rustup`,
- create the Cargo home directory at `$HOME/.cargo`,
- add the `cargo`, `rustc` and `rustup` binaries (among others) to Cargo's bin directory located at `$HOME/.cargo/bin`,
- add Cargo's bin directory to the PATH environment variable by appending the line `. "$HOME/.cargo/env"` to the files `.profile`, `.bash_profile` and `.bashrc` in the home directory. If the file doesn't exist it is created. That same line is later removed by `rustup` on uninstall.

If the last point is not desired then the option 2 in the installer must be selected (custom installation). Then press enter to all the questions but the last one to select the default value (as in the standard installation). In the last question (modify PATH variable) choose "n" (for "no") and then proceed with the installation. After the installation is complete you have to manually add the line `source "$HOME/.cargo/env` in the desired place.

To update to a new version of Rust run

    $ rustup update

and to uninstall Rust and `rustup` run

    $ rustup self uninstall

#### `rustc` (1.2)

`rustc` is the Rust compiler. The simplest "Hello, world!" example with`rustc` is:

    $ mkdir hello_world
    $ cd hello_world
    $ vim main.rs  # and write the content (see below)
    $ rustc main.rs
    $ ls
    main  main.rs
    $ ./main
    Hello, world!

where `main.rs` contains the following code

    fn main() {
        println!("Hello, world!");
    }

#### Cargo (1.3)

Cargo is Rust’s build system and package manager.

To create a project run

    $ cargo new hello_cargo
         Created binary (application) `hello_cargo` package

The created project has the following structure

    $ cd hello_cargo
    $ tree -aF
    .
    ├── Cargo.toml
    ├── .git/
    │   └── (...)
    ├── .gitignore
    └── src/
        └── main.rs

where `Cargo.toml` contains

    [package]
    name = "hello_cargo"
    version = "0.1.0"
    edition = "2021"
    
    [dependencies]

`.gitignore` contains

    /target

and `src/main.rs` contains

    fn main() {
        println!("Hello, world!");
    }

To build the project do

    $ cargo build
       Compiling hello_cargo v0.1.0 (/home/frc/desk/rust-tests/hello_cargo)
        Finished dev [unoptimized + debuginfo] target(s) in 1.68s

This creates a `Cargo.lock` file and a `target/debug` directory with the build products of the development profile (which omits optimizations and includes debug information)

    $ tree -aF
    .
    ├── Cargo.lock              <--- new
    ├── Cargo.toml
    ├── .git/
    │   └── (...)
    ├── .gitignore
    ├── src/
    │   └── main.rs
    └── target/                 <--- new
        ├── debug/
        │   ├── hello_cargo*
        │   └── (...)
        └── (...)
    
    $ target/debug/hello_cargo 
    Hello, world!

To build and run a project in one step do

    $ cargo run
       Compiling hello_cargo v0.1.0 (/home/frc/desk/rust-tests/hello_cargo)
        Finished dev [unoptimized + debuginfo] target(s) in 0.30s
         Running `target/debug/hello_cargo`
    Hello, world!

To check that the source code compiles without actually building a binary (often this is much faster than building, which makes it ideal for use during development) do

    $ cargo check
        Checking hello_cargo v0.1.0 (/home/frc/desk/rust-tests/hello_cargo)
        Finished dev [unoptimized + debuginfo] target(s) in 0.14s

To build the project with optimizations and without debug information (which takes longer than a development build) do

    $ cargo build --release
       Compiling hello_cargo v0.1.0 (/home/frc/desk/rust-tests/hello_cargo)
        Finished release [optimized] target(s) in 0.17s

This creates a `target/release` directory with the build products of the release profile

    $ tree -aF
    .
    ├── Cargo.lock
    ├── Cargo.toml
    ├── .git/
    │   └── (...)
    ├── .gitignore
    ├── src/
    │   └── main.rs
    └── target/
        ├── debug/
        │   ├── hello_cargo*
        │   └── (...)
        ├── release/             <--- new
        │   ├── hello_cargo*
        │   └── (...)
        └── (...)
    
    $ target/release/hello_cargo 
    Hello, world!

---
### Programming a Guessing Game (2)

Step by step coding tutorial that results in a `src/main.rs` file with the following code

    use rand::Rng;
    use std::cmp::Ordering;
    use std::io;
    
    fn main() {
        println!("Guess the number!");
    
        let secret_number = rand::thread_rng().gen_range(1..=100);
    
        loop {
            println!("Please input your guess.");
    
            let mut guess = String::new();
    
            io::stdin()
                .read_line(&mut guess)
                .expect("Failed to read line");
    
            let guess: u32 = match guess.trim().parse() {
                Ok(num) => num,
                Err(_) => continue,
            };
    
            println!("You guessed: {guess}");
    
            match guess.cmp(&secret_number) {
                Ordering::Less => println!("Too small!"),
                Ordering::Greater => println!("Too big!"),
                Ordering::Equal => {
                    println!("You win!");
                    break;
                }
            }
        }
    }

and a `Cargo.toml` file with the following content

    $ cat Cargo.toml
    [package]
    name = "guessing_game"
    version = "0.1.0"
    edition = "2021"
    
    [dependencies]
    rand = "0.8.3"            <--- This line is new

The following concepts are introduced during the tutorial:

- Rust's **standard library** is identified by `std`. Most of the items in the standard library must be brought into the scope of the program using the `use` keyword to use them. But some, namely those contained in the **prelude**, are always included into all programs.
- The `main` function is the entry point into the program.
- The basic syntax is very C-like: parenthesis are used to enclose function arguments, curly braces are used to enclose function bodies and semicolons to terminate statements.
- Functions whose name end in `!`, like `println!`, are **macros**.
- The `let` keyword is used to declare variables.
- The `mut` keyword is used during variable declaration to specify that the variable is **mutable**. This is because, in Rust, variables are **immutable** by default, which means that their value cannot be changed after initialization.
- `String` is a string type provided by the standard library that is a growable, UTF-8 encoded bit of text.
- The `::` syntax is used to specify hierarchies in library identifiers (like in `std::io`), to specify items within a library (like in `io::stdin()`) and to specify functions associated to a type (like in `String::new`).

---
### Enums and Pattern Matching (6)

_Enumerations_, also referred to as _enums_, allow us to define a type by enumerating its possible _variants_.

#### Defining an Enum (6.1)

Enums give us a way of saying a value is one of a possible set of values.

    enum MyEnumType {
        Variant1,
        Variant2,
        // and so on...
    }

`MyEnumType` is now a custom data type that we can use elsewhere in our code. We can create instances of each of the variants of `MyEnumType` like this:

    let v1 = MyEnumType::Variant1;
    let v2 = MyEnumType::Variant2;
    // and so on...

Moreover, we can put data directly into each enum variant. This allows us to avoid patterns like the following:

    enum StrKind {
        SomeKind,
        AnotherKind,
    }

    struct MyStr {
        kind: StrKind,
        data: String,
    }

    let str_of_some_kind = MyStr {
        kind: StrKind::SomeKind,
        data: String::from("some_string"),
    };

Instead we can do the same thing using enums:

    enum MyStr {
        SomeKind(String),
        AnotherKind(String),
    }

    let str_of_some_kind = MyStr::SomeKind(String::from("some_string"));

Even more, each variant can have different types and amounts of associated data:

    enum Action {
        Quit,
        Move { x: i32, y: i32 },
        Write(String),
        ChangeColor(i32, i32, i32),
    }

_TODO: similarities with structs..._

#### The `Option` Enum (also 6.1)

_TODO..._

---

### Packages, Crates and Modules (7)

#### Packages and Crates (7.1)

Crates are the units of compilation, i.e. they are the smallest amount of code that the Rust compiler considers at a time.

A crate can come in one of two forms: binary or library. _Binary crates_ are pieces of code that you can compile to an executable. Each binary crate must have a function called `main`. In contrast, _library crates_ don’t have a `main` function, and they don’t compile to an executable. Instead, they define functionality intended to be shared with multiple projects.

Most of the time when Rustaceans say “crate,” they mean library crate, and they use “crate” interchangeably with the general programming concept of a “library.”

The _crate root_ is the source file passed to the Rust compiler to start the compilation of a crate.

A _package_ is a bundle of one or more crates that provides a set of functionality. A package contains a _Cargo.toml_ file that describes how to build those crates. A package can contain as many binary crates as you like, but at most only one library crate. A package must contain at least one crate, whether that’s a library or binary crate.

_\[Packages are the units of library distribution. Since packages can contain at most only one library crate (and they, in fact, almost always do), they are used by Cargo to represent library crates for their distribution.]_

An example of a very simple package is the following:

```
$ tree -aF
./
├── Cargo.toml
└── src/
    └── main.rs

$ cat Cargo.toml
[package]
name = "my_package"

$ cat src/main.rs 
fn main() {
    println!("Hello, world!");
}
```

Cargo follows the following conventions:

- If the package has a file `src/main.rs` then this file is treated as the crate root of a binary crate with the same name as the package.
- If the package has a file of the form `src/bin/NAME.rs` then this file is treated as the crate root of a binary crate with name `NAME`. By placing several such files in the `src/bin` directory it is possible to include more than one binary crate in the package.
- If the package has a file `src/lib.rs` then this file is treated as the crate root of _the_ single library crate represented by the package. This library crate has the same name as the package.

Cargo passes the crate root files to `rustc` to build the library or binary.

---
### Error Handling (9)

Rust groups errors into two major categories: recoverable and unrecoverable errors:

* for a recoverable error we most likely just want to report the problem to the user and retry the operation
* for unrecoverable errors we want to immediately stop the program.

Rust has the type `Result<T, E>` for recoverable errors and the `panic!` macro to stop execution in the event of an unrecoverable error.

#### The `panic!` macro (9.1)

There are two ways to cause a panic:

* by taking an action that causes the Rust runtime to panic
* by explicitly calling the `panic!` macro, like this

        panic!("unexpected situation");

By default, a panic will:

1. Print a failure message similar to

        thread 'main' panicked at 'unexpected situation', src/main.rs:2:5
        note: run with `RUST_BACKTRACE=1` environment variable to display a backtrace

2. Unwind, which means walk back up the stack and clean up the data from each function.
3. Clean up the stack.
4. Quit.

There are two ways to customize panics.

1. With the environment variable `RUST_BACKTRACE=1` the call stack will also be displayed after the failure message. Note that backtraces with the filename and line number of each function requires that programs be compiled with debug symbols.

2. Since the unwinding process is quite complex it takes up a a big chunk of the Rust runtime. Projects that want to make the resulting binary as small as possible can exclude the unwinding code in the binary (and thus force the operating system to do the cleanup) by adding `panic = 'abort'` to the appropriate `[profile]` sections in the `Cargo.toml` file.

#### The `Result` enum (9.2)

The `Result` enum is defined with two variants, `Ok` and `Err`, as follows:

    enum Result<T, E> {
        Ok(T),
        Err(E),
    }

where `T` and `E` are generic type parameters.

The `Result` enum and its variants are automatically brought into scope by the prelude.

One way to handle a `Result` value is using the `match` expression like this (assuming that the return type of `some_fn` is `Result`):

    let var = match some_fn() {
        Ok(x) => x,
        Err(e) => panic!("Failed: {:?}", e),
    };

A more concrete and real-world example of this can be presented by using common filesystem operations. The following code will try to open a file, and if the open operation fails because the file doesn't exist then it'll try to create the file.

    use std::fs::File;
    use std::io::ErrorKind;
    
    fn main() {
        let f = match File::open("file.txt") {
            Ok(file) => file,
            Err(error) => match error.kind() {
                ErrorKind::NotFound => match File::create("file.txt") {
                    Ok(file) => file,
                    Err(error) => panic!("Problem creating the file: {:?}", error),
                },
                err => {
                    panic!("Problem opening the file: {:?}", err);
                }
            },
        };
    }