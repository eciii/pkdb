#Linux_Technology

### General theory

A container is a special execution environment in the [[Linux kernel]] that isolates the software running inside it from the rest of the OS. Such execution environment is achieved by combining several features of the Linux kernel in the right way (the Linux kernel itself has no notion of containers). Those features are essentially namespaces and control groups (cgroups).

To create a a container one has to create the appropriate namespaces and cgroups. To start the container means to launch a process constrained to those namespaces and cgroups. Once the software running inside the container is done one has to clean up the corresponding namespaces and cgroups. A **container runtime** is a program that automates these tasks and many others (like setting up the root filesystem of the container) that facilitate the creation of *individual* containers (to better understand how exactly a container is built check out the talk [Containers from Scratch](https://www.youtube.com/watch?v=wyqoi52k5jM)).

---

Two usecases (these were [mentioned](https://github.com/moby/moby/issues/40222) by Solomon Hykes when talking about the Docker/Moby split):

- Development --> containers help to solve the **shipping problem** (here docker was the "winner")
- Infrastructure --> containers help to solve the **scaling problem** (here kubernetes was the "winner")

**The shipping problem**

How to package an application in such a way that:

- contains all its dependencies, except from the Linux kernel, and no more
- is easy to launch anywhere with a supported Linux kernel
- runs fully isolated from the rest of the system

**The scaling problem**

_TODO_

---

### Layers

**Container runtimes**

The goal of a container runtime is to instantiate a single container from a container image and supervise it. The instantiation steps are more or less the following:

- Create the container environment.
- Launch and supervise the initial contained process.
- Clean up the container environment after all processes are done.

_TODO: if container runtimes already supervise containers then what is the role of container supervisors like conmon?_

Container runtimes are OCI-compliant if:

- They support OCI-compliant container images
- They implement the OCI Runtime specification

Examples:

- runc (OCI, Go)
- crun (Container Tools, C)
- youki (Container Tools, Rust)

**Container managers**

The goal of container managers is to facilitate the local execution and management of several containers. This is accomplished with the following tasks:

- Operate container runtimes to instantiate containers.
- Provide _networking_ and _storage_ facilities to allow the containers to interact between them and with the host.
- Manage a local container image repository by interacting with remote container image registries.

Container managers are OCI-compliant if:

- They use the OCI Runtime specification to operate OCI-compliant container runtimes.
- They use the OCI distribution specification to interact with OCI-compliant container image registries.

Container managers have two main use-cases:

- As part of container suites
- As the local container managers of a container orchestrator. In the case of Kubernetes, a container manager has to implement the CRI interface.

Examples:

- containerd implements only a core subset of the functionality of a container manager (_TODO: which subset? which criteria is used to decide the scope of containerd?_). To get a full container manager, containerd needs to be paired with an "intelligent" client that implements the rest of the functionality. Examples of intelligent clients for containerd are:
	- nerdctl
	- moby-dockerd (the daemon of the Docker Engine container suite)
	- containerd's cri plugin (turns containerd into a local container manager for Kubernetes)
- CRI-O (a local container manager for Kubernetes)

**Container suites**

These are container managers that also provide all the facilities to create, modify, test and debug container images. The amount of features they provide makes them unsuitable for use as local container managers in container orchestrators.

Examples:

- Podman (Container Tools, Go)
- Docker Engine (Docker)

**Container orchestrators**

The goal of container orchestrators is to orchestrate containers across several hosts.

Examples:

- Kubernetes
- SwarmKit (Moby)

**Container image registries**

Examples:

- Distribution (from Docker/Moby)
- Harbor (from VMware; CNCF graduated)
- Quay (from Red Hat)