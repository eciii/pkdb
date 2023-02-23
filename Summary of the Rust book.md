# The Rust Programming Language

### 1. Getting Started
(short summary of the chapter)

#### 1.1. Installation
The recommended way to install Rust on Linux is using `rustup`, which is a command-line tool for managing Rust versions and associated tools. `rustup` and Rust are installed by running

    $ curl --proto '=https' --tlsv1.3 https://sh.rustup.rs -sSf | sh

and following the instructions. To update to a new version of Rust run

    $ rustup update

and to uninstall Rust and `rustup` run

    $ rustup self uninstall

#### 1.2. Hello, World!
"Hello, World!" example and basic compilation using `rustc`. Essentially

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

#### 1.3. Hello, Cargo!
Basic usage of Cargo (Rust’s build system and package manager).

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
       Compiling hello_cargo v0.1.0 (/home/frc/desk/rust-tests/    hello_cargo)
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

### 2. Programming a Guessing Game
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

- Rust's **standard library** is identified by `std`. Most of the items in the standard library must be brought into the scope of the program using the `use` keyword in order to be used. But some, namely those contained in the **prelude**, are always included into all programs.
- The `main` function is the entry point into the program.
- The basic syntax is very C-like: parenthesis are used to enclose function arguments, curly braces are used to enclose function bodies and semicolons to terminate statements.
- Functions whose name end in `!`, like `println!`, are in fact **macros**.
- The `let` keyword is used to declare variables.
- The `mut` keyword is used during variable declaration to specify that the variable is **mutable**. This is because, in Rust, variables are **immutable** by default, which means that their value cannot be changed after initialization.
- `String` is a string type provided by the standard library that is a growable, UTF-8 encoded bit of text.
- The `::` syntax is used to specify hierarchies in library identifiers (like in `std::io`), to specify items within a library (like in `io::stdin()`) and to specify functions associated to a type (like in `String::new`).

### 3. Common Programming Concepts
(summary here)

#### 3.1. Variables and Mutability
(summary here)

#### 3.2. Data Types
(summary here)

#### 3.3. Functions
(summary here)

#### 3.4. Comments
(summary here)

#### 3.5. Control Flow
(summary here)

### 4. Understanding Ownership
(summary here)

#### 4.1. What is Ownership?
(summary here)

#### 4.2. References and Borrowing
(summary here)

#### 4.3. The Slice Type
(summary here)

### 5. Using Structs to Structure Related Data
(summary here)

#### 5.1. Defining and Instantiating Structs
(summary here)

#### 5.2. An Example Program Using Structs
(summary here)

#### 5.3. Method Syntax
(summary here)

### 6. Enums and Pattern Matching
(summary here)

#### 6.1. Defining an Enum
(summary here)

#### 6.2. The match Control Flow Construct
(summary here)

#### 6.3. Concise Control Flow with if let
(summary here)

### 7. Managing Growing Projects with Packages, Crates, and Modules
(summary here)

#### 7.1. Packages and Crates
(summary here)

#### 7.2. Defining Modules to Control Scope and Privacy
(summary here)

#### 7.3. Paths for Referring to an Item in the Module Tree
(summary here)

#### 7.4. Bringing Paths Into Scope with the use Keyword
(summary here)

#### 7.5. Separating Modules into Different Files
(summary here)

### 8. Common Collections
(summary here)

#### 8.1. Storing Lists of Values with Vectors
(summary here)

#### 8.2. Storing UTF-8 Encoded Text with Strings
(summary here)

#### 8.3. Storing Keys with Associated Values in Hash Maps
(summary here)

### 9. Error Handling
(summary here)

#### 9.1. Unrecoverable Errors with panic!
(summary here)

#### 9.2. Recoverable Errors with Result
(summary here)

#### 9.3. To panic! or Not to panic!
(summary here)

### 10. Generic Types, Traits, and Lifetimes
(summary here)

#### 10.1. Generic Data Types
(summary here)

#### 10.2. Traits: Defining Shared Behavior
(summary here)

#### 10.3. Validating References with Lifetimes
(summary here)

### 11. Writing Automated Tests
(summary here)

#### 11.1. How to Write Tests
(summary here)

#### 11.2. Controlling How Tests Are Run
(summary here)

#### 11.3. Test Organization
(summary here)

### 12. An I/O Project: Building a Command Line Program
(summary here)

#### 12.1. Accepting Command Line Arguments
(summary here)

#### 12.2. Reading a File
(summary here)

#### 12.3. Refactoring to Improve Modularity and Error Handling
(summary here)

#### 12.4. Developing the Library’s Functionality with Test Driven Development
(summary here)

#### 12.5. Working with Environment Variables
(summary here)

#### 12.6. Writing Error Messages to Standard Error Instead of Standard Output
(summary here)

### 13. Functional Language Features: Iterators and Closures
(summary here)

#### 13.1. Closures: Anonymous Functions that Capture Their Environment
(summary here)

#### 13.2. Processing a Series of Items with Iterators
(summary here)

#### 13.3. Improving Our I/O Project
(summary here)

#### 13.4. Comparing Performance: Loops vs. Iterators
(summary here)

### 14. More about Cargo and Crates.io
(summary here)

#### 14.1. Customizing Builds with Release Profiles
(summary here)

#### 14.2. Publishing a Crate to Crates.io
(summary here)

#### 14.3. Cargo Workspaces
(summary here)

#### 14.4. Installing Binaries from Crates.io with cargo install
(summary here)

#### 14.5. Extending Cargo with Custom Commands
(summary here)

### 15. Smart Pointers
(summary here)

#### 15.1. Using Box<T> to Point to Data on the Heap
(summary here)

#### 15.2. Treating Smart Pointers Like Regular References with the Deref Trait
(summary here)

#### 15.3. Running Code on Cleanup with the Drop Trait
(summary here)

#### 15.4. Rc<T>, the Reference Counted Smart Pointer
(summary here)

#### 15.5. RefCell<T> and the Interior Mutability Pattern
(summary here)

#### 15.6. Reference Cycles Can Leak Memory
(summary here)

### 16. Fearless Concurrency
(summary here)

#### 16.1. Using Threads to Run Code Simultaneously
(summary here)

#### 16.2. Using Message Passing to Transfer Data Between Threads
(summary here)

#### 16.3. Shared-State Concurrency
(summary here)

#### 16.4. Extensible Concurrency with the Sync and Send Traits
(summary here)

### 17. Object Oriented Programming Features of Rust
(summary here)

#### 17.1. Characteristics of Object-Oriented Languages
(summary here)

#### 17.2. Using Trait Objects That Allow for Values of Different Types
(summary here)

#### 17.3. Implementing an Object-Oriented Design Pattern
(summary here)

### 18. Patterns and Matching
(summary here)

#### 18.1. All the Places Patterns Can Be Used
(summary here)

#### 18.2. Refutability: Whether a Pattern Might Fail to Match
(summary here)

#### 18.3. Pattern Syntax
(summary here)

### 19. Advanced Features
(summary here)

#### 19.1. Unsafe Rust
(summary here)

#### 19.2. Advanced Traits
(summary here)

#### 19.3. Advanced Types
(summary here)

#### 19.4. Advanced Functions and Closures
(summary here)

#### 19.5. Macros
(summary here)

### 20. Final Project: Building a Multithreaded Web Server
(summary here)

#### 20.1. Building a Single-Threaded Web Server
(summary here)

#### 20.2. Turning Our Single-Threaded Server into a Multithreaded Server
(summary here)

#### 20.3. Graceful Shutdown and Cleanup
(summary here)

### 21. Appendix
(summary here)

#### 21.1. A - Keywords
(summary here)

#### 21.2. B - Operators and Symbols
(summary here)

#### 21.3. C - Derivable Traits
(summary here)

#### 21.4. D - Useful Development Tools
(summary here)

#### 21.5. E - Editions
(summary here)

#### 21.6. F - Translations of the Book
(summary here)

#### 21.7. G - How Rust is Made and “Nightly Rust”
(summary here)
