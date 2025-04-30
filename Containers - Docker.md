**Note about terminology**

The word "docker" might refer to:

- The company. I'll refer to it as Docker Inc.
- The binary that provides a command-line interface to interact with the docker daemon. The associated gitrepo is https://github.com/docker/cli. I'll refer to this project/componentas docker-cli.
- The actual docker gitrepo https://github.com/docker/docker. This gitrepo contained the docker daemon. It now redirects to https://github.com/moby/moby. I'll refer to this project/component as moby-dockerd (to also distinguish it from the [[Containers - Moby|Moby]] project umbrella).

---

- Gitstore: https://github.com/docker
- Docsite: https://docs.docker.com

Docker Inc. maintains a number of free software (CE) projects (see https://github.com/docker/docker-ce-packaging for the "packaging recipes").

The "foundational" project is **Docker Engine**. It is essentially the combination of two projects

- docker-cli (under the docker gitstore)
- moby-dockerd (under the moby gitstore as moby, see the note about terminology)

The Docker Engine can be extended to do provide additional functionality. This is usually done by using docker-cli plugins. Some interesting plugins are:

- [buildx](https://github.com/docker/buildx): Build images using Moby's buildkit.
- [compose](https://github.com/docker/compose): Define and run multi-container applications.

The Docker Engine also has another notable feature (rather confusingly, not implemented as a plugin): Swarm mode. This seems to be a sort of frontend to Moby's SwarmKit. The goal is to orchestrate containers across several hosts (quite similarly to Kubernetes but maybe at a lower scale?).

_\[TODO: Check out all the repos in the Gitstore to make this list comprehensive]_

---

**History**

dotCloud was founded on 2008 by Solomon Hykes and two other founders. Originally dotCloud was a PaaS provider that, like any other PaaS provider at the time, was in charge of deploying, maintaining and scaling its customers' web applications.

But to do that they also created a whole new paradigm for the packaging, distribution and execution (collectively known as shipping) of software. This new paradigm was based on a set of fairly new Linux kernel technologies at the time, _namespaces_ and _cgroups_, that where collectively known as _Linux containers_ (LXC).

To implement this new paradigm they developed a tool that they called _Docker_. The software shipping paradigm that Docker enabled came to be known later as _Docker containers_.

-

Hykes explained the reasons behind Docker at a [talk in 2013](https://www.youtube.com/watch?v=3N3n9FzebAA). According to him Docker was created to solve the very specific problem of _software shipping_. During the whole life-cycle of a software release, the software needs to move around a lot, from the developers' machines to the testing and integration machines, to the final production servers. All these machines have different hardware and software specifications and the developer needs to be sure that the software will run reliably on all of them. This is the software shipping problem that Docker intended to solve.

At the same talk Hykes also gave examples of how some technologies at the time provided unsuitable solutions to the problem. On the one hand there are the programming language environments, like Python virtualenvs, that provide useful but incomplete software encapsulation because all the system dependencies are left out. On the other hand there are the virtual machines that encapsulate the whole software system but also the hardware, making it a very inflexible and bulky shipping solution.

But Docker, being based on the Linux containers technologies, found the perfect balance and was able to encapsulate the whole software system while at the same time leaving the hardware details out. In the words of Hykes:

> \[Docker is] the result of the last five years working at dotCloud, trying to figure this problem out, and finally reaching a point where we feel like we found a good solution that is simple and elegant enough to share with everyone.

-

Docker was developed in Go, which was at the time a fairly new programming language, and used LXC under the hood.

-

On March 2013, in a [talk](https://www.youtube.com/watch?v=wW9CAH9nSLs) at  PyCon US 2013, Solomon Hykes presented Docker for the first time in public. Shortly after, also in March 2013, Docker was released as open-source.

One year later, with the release of version 0.9, Docker replaced LXC with its own component, _libcontainer_, which was also written in the Go programming language.

On October 29, 2013 dotCloud was renamed Docker.

-

In 2015 Docker Inc and the Linux Foundation formed the Open Container Initiative (OCI) to standardize some of the components inside the Docker Engine, namely the image format, the container runtime (which was back then a Go library known as libcontainer) and the container registry API. The libcontainer component was removed from the Docker Engine and donated to the OCI with the new name of runc.