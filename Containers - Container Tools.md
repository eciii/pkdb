- Website: (no website)
- Gitstore: https://github.com/containers

Project umbrella created and mostly promoted by Red Hat. It is a collection of tools to create and work with containers. The "foundational" project is podman:

- **podman** (Go): A tool for managing OCI containers and pods.

Other projects within the project umbrella include:

_\[TODO: Check out all the repos in the Gitstore to make this list comprehensive]

- **podman-desktop** (TypeScript): A GUI for podman.
- **buildah** (Go): A tool that facilitates building OCI images.
- **skopeo** (Go): A tool for working with remote images registries.
- **crun** (C): An OCI runtime in C. Also a C library for running containers.
- **youki** (Rust): An OCI runtime in Rust.
- **netavark** (Rust): Container network stack

Note: some projects fall out of the stated scope of the project umbrella. Some of those are:

- **bootc**: Transactional, in-place operating system updates using OCI container images. Now in [its own gitstore](https://github.com/bootc-dev).
- **RamaLama**: Facilitate the use of OCI containers for AI applications.