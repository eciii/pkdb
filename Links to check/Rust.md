Important references:

- Official website: <https://www.rust-lang.org>
- Rust Foundation: <https://foundation.rust-lang.org>
- Documentation of the standard library crate: <https://doc.rust-lang.org/std/index.html>
- Default cargo registry: <https://crates.io>
- Documentation hosting service for crates: <https://docs.rs/>

Components

- **rustup**: Tool used to manage multiple rust installations (a.k.a toolchains). The official way to install rust in Linux is to download and execute a script that in turn downloads and install rustup which in turn downloads and install the selected rust toolchain. (**Note:** during the installation process it is possible to customize the installation in such a way that the script does not adjust the PATH. Otherwise the default installation process is quite fine.)

Other links to check

- [Ferrous Systems](https://ferrous-systems.com/) is a company that seems to have a very interesting training and consulting offering for Rust developers. They are also the creators of [ferrocene](https://ferrous-systems.com/ferrocene/).
- The [YouTube channel of Jon Gjengset](https://www.youtube.com/c/JonGjengset) has some interesting live-coding sessions of quite advanced Rust projects (e.g a TCP stack).
- The [blog](https://burntsushi.net/) of Andrew Gallant ([BurntSushi](https://github.com/BurntSushi) in GitHub). He is the creator of ripgrep and maintainer of Rust's `regex` crate.
- It seems that Rusts' type system is turing complete. There are many links in the web about it. These seems to be interesting:
  - https://peppe.rs/posts/turing_complete_type_systems/
  - https://sdleffler.github.io/RustTypeSystemTuringComplete/

---

**Formal verification and Rust**

The _Verify Rust Standard Library Effort:_

- [Official announcement](https://aws.amazon.com/blogs/opensource/verify-the-safety-of-the-rust-standard-library/) by AWS.
- [Official documentation](https://model-checking.github.io/verify-rust-std/) and related [GitHub repository](https://github.com/model-checking/verify-rust-std) and [GitHub organization](https://github.com/model-checking).
- Related official Rust Project Goals: [2024H2](https://rust-lang.github.io/rust-project-goals/2024h2/std-verification.html) and [2025H1](https://rust-lang.github.io/rust-project-goals/2025h1/std-contracts.html).
- Follow up [talk](https://www.youtube.com/watch?v=rGhL3lSM_NU) at RustConf 2025.
- Interesting [article](https://arxiv.org/pdf/2510.01072v2) that discusses the status of the project as of October 2025.

_Ferrocene_, an open-source qualified Rust compiler toolchain for safety- and mission-critical systems:

- [Official website](https://ferrous-systems.com/ferrocene/).
- Official announcements about the integration of the _Ferrocene Language Specification (FLS)_ into the effort of an official language specification: in the [Rust Foundation blog](https://rustfoundation.org/media/ferrous-systems-donates-ferrocene-language-specification-to-rust-project/) and the [Rust Project blog](https://blog.rust-lang.org/2025/03/26/adopting-the-fls/).

Discontinued project _Rust Verification Tools_, which was part of Google's Project Oak:
- [Official website](https://project-oak.github.io/rust-verification-tools/) and [GitHub repository](https://github.com/project-oak/rust-verification-tools/).
- About Project Oak: Google [blog post](https://security.googleblog.com/2023/06/bringing-transparency-to-confidential.html) and [GitHub organization](https://github.com/project-oak).

---

**Interesting Rust projects for learning**

BurntSushi's `fst`:

- [GitHub repository](https://github.com/BurntSushi/fst) and corresponding [DeepWiki documentation](https://deepwiki.com/BurntSushi/fst).
- "Official" [blog post](https://burntsushi.net/transducers/).

---

**Interesting big Rust projects**

Dioxus, a (Rust) framework for building cross-platform apps:

- [Official website](https://dioxuslabs.com/).

CoverDrop (a.k.a Secure Messaging in The Guardian app):

- [Official website](https://www.coverdrop.org/) and related [GitHub repository](https://github.com/guardian/coverdrop).
- Related [talk](https://www.youtube.com/watch?v=8n13Oh8c0r4) at RustConf 2025.

Cloud Hypervisor

Some Signal components?
