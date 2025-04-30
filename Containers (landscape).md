
**Container-optimized OSes**

  * To host containers:
    * [Fedora CoreOS](https://fedoraproject.org/coreos/)
    * [Ubuntu Core](https://ubuntu.com/core) (?)
  * To be containers (containerized micro OSes):
    * [Alpine Linux](https://www.alpinelinux.org/)
    * [Google's Distroless](https://github.com/GoogleContainerTools/distroless) (also there is a [Red Hat blog post](https://www.redhat.com/en/blog/why-distroless-containers-arent-security-solution-you-think-they-are) criticizing it)
    * Red Hat's Universal Base Image (UBI) (no website, [documentation](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/9/html/building_running_and_managing_containers), Red Hat Developer [landing page](https://developers.redhat.com/products/rhel/ubi), [blog post](https://www.redhat.com/en/blog/introducing-red-hat-universal-base-image), [knowldege base article](https://access.redhat.com/articles/4238681))

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