#Linux_Technology

### General theory

A container is a special execution environment in the [[Linux kernel]] that isolates the software running inside it from the rest of the OS. Such execution environment is achieved by combining several features of the Linux kernel in the right way (the Linux kernel itself has no notion of containers). Those features are essentially namespaces and control groups (cgroups).

To create a a container one has to create the appropriate namespaces and cgroups. To start the container means to launch a process constrained to those namespaces and cgroups. Once the software running inside the container is done one has to clean up the corresponding namespaces and cgroups. A **container runtime** is a program that automates these tasks and many others (like setting up the root filesystem of the container) that facilitate the creation of *individual* containers (to better understand how exactly a container is built check out the talk [Containers from Scratch](https://www.youtube.com/watch?v=wyqoi52k5jM)).

But using a container runtime to run and manage several containers is cumbersome. A **container engine** is a program that simplifies the management of multiple containers.

(_To expand_)

Links to articles that try to sort out the mess of the container software ecosystem:

- Interesting LWN articles:
	- [Docker and the OCI container ecosystem](https://lwn.net/Articles/902049/)
	- [The container orchestrator landscape](https://lwn.net/Articles/905164/)
	- [LXC and LXD: a different container story](https://lwn.net/Articles/907613/)
- [Confusing Terms In Container Ecosystem](https://nikhilsoni.me/2019/04/05/confusing-terms-in-container-ecosystem/)
- [The differences between Docker, containerd, CRI-O and runc](https://www.tutorialworks.com/difference-docker-containerd-runc-crio-oci/)
- [A Practical Introduction to Container Terminology](https://developers.redhat.com/blog/2018/02/22/container-terminology-practical-introduction)

### Implementations

Container-related software can be broadly categorized into:

**Container orchestrators**

* **podman**
* **Docker CLI + moby** (a.k.a dockerd, a.k.a **Docker Engine**)
* **Kubernetes**

**Container managers**

- **containerd**
- **cri-o**

**Container runtimes**

- (**Open Container Initiative (OCI)** compliant): **runc**

**Container-optimized OSes**

  * To host containers:
    * **Fedora CoreOS**
    * **Ubuntu Core**
  * To be containers (containerized micro OSes):
    * **Alpine Linux**
    * **Google's Distroless**
    * **Red Hat's Universal Base Image (UBI)**

**Others (to categorize)**

- **buildah**
- **Project Moby**

### Major projects and project umbrellas

**Moby**

- Website: https://mobyproject.org
- GitHub: https://github.com/moby

Project umbrella created and mostly promoted by Docker Inc. It is a collection of components, assembly tools and reference assemblies that can be used to create custom container systems.

The projects within the project umbrella include:

_\[TODO: Check out all the repos in the GitHub account to make this list comprehensive. Also sort out which projects are "components", which are "assembly tools" and which are "reference assemblies", and how each one relates to the others]_

- **sys** (Go): No description in GitHub but this package is used by runc
- **datakit** (OCaml): Connect processes into powerful data pipelines with a simple git-like filesystem interface
- **vpnkit** (OCaml): A toolkit for embedding VPN capabilities in your application
- **swarmkit** (Go): A toolkit for orchestrating distributed systems at any scale. It includes primitives for node discovery, raft-based consensus, task scheduling and more.
- **libnetwork** (Go): Networking for containers
- **buildkit** (Go): Concurrent, cache-efficient, and Dockerfile-agnostic builder toolkit
- **hyperkit** (C): A toolkit for embedding hypervisor capabilities in your application
- **moby** (Go): A reference assembly that is intended to be the upstream of dockerd. Note that there is no dockerd git repository anywhere, so this repository is also the de-facto dockerd repository.

Previous projects that "spinned-off":

- **runc** now at the Open Container Initiative (OCI)
- **containerd** and **notary** now at the Cloud Native Computing Foundation (CNCF)
- **distribution** now at the _distribution_ GitHub account
- **linuxkit** now at the _linuxkit_ GitHub account

---

**Open Container Initiative (OCI)**

- Website: https://opencontainers.org/
- GitHub: https://github.com/opencontainers

Some history:

- The OCI is an industry body that was formed in 2015 by Docker and the Linux Foundation. Back then Docker Inc. refactored its software into a number of smaller components and some of those components, along with their specifications, were placed under the care of the OCI.

Overview of the specifications (from LWNs "Docker and the OCI container ecosystem")

> The OCI image specification defines a format for container images that consists of a JSON configuration (containing environment variables, the path to execute, and so on) and a series of tarballs called "layers". The contents of each layer are stacked on top of each other, in series, to construct the root filesystem for the container image.

> The OCI also publishes a distribution specification. \[...] This specification defines an HTTP API for pushing and pulling container images to and from a server; servers that implement this API are called container registries. Docker maintains a large public registry called Docker Hub as well as a reference implementation (called "Distribution", perhaps confusingly) that can be self-hosted. Other implementations of the specification include Red Hat's Quay and VMware's Harbor, as well as hosted offerings from \[cloud and git providers].



Overview of GitHub repositories:

- Specs
	- **image-spec** (Go): OCI Image Format Specification
	- **runtime-spec** (Go): OCI Runtime Specification
	- **distribution-spec** (Go): OCI Distribution Specification
- Software
	- **runc** (Go): Reference implementation of the OCI Runtime Specification
	- **umoci** (Go): umoci modifies Open Container images
	- **go-digest** (Go): Common digest package used across the container ecosystem
	- **selinux** (Go): common selinux implementation
	- **runtime-tools** (Go): OCI Runtime Tools
	- **image-tools** (Go): OCI Image Tooling (no longer actively maintained; some tools can be replaced by umoci but others have no replacement anywhere else; see Readme)
	- **container-images** (Dockerfile): A collection of container images used in CI across various opencontainers projects
- Websites and artwork
	- **opencontainers.org** (HTML): Main website
	- **specs.opencontainers.org** (HTML): Website of 'specs' subdomain
	- **oci-conformance** (HTML): Website of 'conformance' subdomain
	- **artwork**: OCI artwork and logos
- Workgroups
	- **wg-image-compatibility**
	- **wg-auth**
	- **wg-freebsd-runtime**
- Misc
	- **tob**: Technical Oversight Board (TOB) documentation
	- **cgroups**: Useful boilerplate and organizational information for all OCI projects
	- **.github**: Centralized documents and information for the OCI organization and projects


---

_Owned by the Cloud Native Computing Foundation (CNCF)_

**[containerd](https://github.com/containerd/containerd) Go** Created by [containerd](https://github.com/containerd)
An open and reliable container runtime

**[notary](https://github.com/notaryproject/notary) Go** Created by [notaryproject](https://github.com/notaryproject)
Notary is a project that allows anyone to have trust over arbitrary collections of data

_Owned by others (who? how independent are they from moby?)_

**[linuxkit](https://github.com/linuxkit/linuxkit) Go** Created by [linuxkit](https://github.com/linuxkit)
A toolkit for building secure, portable and lean operating systems for containers

**[distribution](https://github.com/distribution/distribution) Go** Created by [distribution](https://github.com/distribution)
The toolkit to pack, ship, store, and deliver container content