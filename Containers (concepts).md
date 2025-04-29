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

**Layers**

_Container runtimes_

The goal of a container runtime is to instantiate a single container form a container image and supervise it. The instantiation steps are more or less the following:

- Create the container environment.
- Launch and supervise the initial contained process.
- Clean up the container environment after all processes are done.

_TODO: if container runtimes already supervise containers then what is the role of container supervisors like conmon?_

Container runtimes are OCI-compliant if:

- They support OCI-compliant container images
- They implement the OCI Runtime specification

_Container managers_

The goal of container managers is to facilitate the local execution and management of several containers. This is accomplished by:

- Operating container runtimes to instantiate containers.
- Providing _networking_ and _storage_ facilities to allow the containers to interact between them and with the host.

Container managers are OCI-compliant if:

- They use the OCI Runtime specification to operate OCI-compliant container runtimes.

_Container image managers_