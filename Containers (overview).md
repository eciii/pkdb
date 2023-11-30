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

### Major projects


- [Moby project](https://mobyproject.org/) (upstream of some Docker projects)

