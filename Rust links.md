Important references:

- Official website: <https://www.rust-lang.org>
- Rust Foundation: <https://foundation.rust-lang.org>
- Documentation of the standard library crate: <https://doc.rust-lang.org/std/index.html>
- Default cargo registry: <https://crates.io>
- Documentation hosting service for crates: <https://docs.rs/>

Components

- **rustup**: Tool used to manage multiple rust installations (a.k.a toolchains). The official way to install rust in Linux is to download and execute a script that in turn downloads and install rustup which in turn downloads and install the selected rust toolchain. (**Note:** during the installation process it is possible to customize the installation in such a way that the script does not adjust the PATH. Otherwise the default installation process is quite fine.)

Other links to check

- Blog of [Alastair Reid](https://alastairreid.github.io/), who led the now discontinued project [Rust Verification Tools](https://project-oak.github.io/rust-verification-tools/) at Google.
- [ferrous systems](https://ferrous-systems.com/) is a company that seems to have a very interesting training and consulting offering for Rust developers. They are also working on [ferrocene](https://ferrous-systems.com/ferrocene/), a project to make Rust a viable programming language for safety and mission critical systems.
- The [YouTube channel of Jon Gjengset](https://www.youtube.com/c/JonGjengset) has some interesting live-coding sessions of quite advanced Rust projects (e.g a TCP stack).
- It seems that Rusts' type system is turing complete. There are many links in the web about it. These seems to be interesting:
  - https://peppe.rs/posts/turing_complete_type_systems/
  - https://sdleffler.github.io/RustTypeSystemTuringComplete/