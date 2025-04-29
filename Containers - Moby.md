- Website: https://mobyproject.org
- Gitstore: https://github.com/moby

Project umbrella created and mostly promoted by Docker Inc. It is a collection of components, assembly tools and reference assemblies that can be used to create custom container systems.

The "foundational" project is moby-dockerd (see the terminology section in [[Containers - Docker|Docker]] for the reason why I call it moby-dockerd instead of just moby):

- **moby** (Go): The reference assembly for the docker deamon (a.k.a dockerd).

Note that there is no dockerd git repository anywhere, so this repository is also the de-facto dockerd repository. Thus I'll refer to the moby project as moby-dockerd to distinguish it from the Moby project umbrella.

Other projects within the project umbrella include:

_\[TODO: Check out all the repos in the Gitstore to make this list comprehensive. Also sort out which projects are "components", which are "assembly tools" and which are "reference assemblies", and how they relate to one another]_

- **buildkit** (Go): Concurrent, cache-efficient, and Dockerfile-agnostic builder toolkit
- **swarmkit** (Go): A toolkit for orchestrating distributed systems at any scale. It includes primitives for node discovery, raft-based consensus, task scheduling and more.
- **sys** (Go): No description in GitHub but this package is used by runc
- **datakit** (OCaml): Connect processes into powerful data pipelines with a simple git-like filesystem interface
- **vpnkit** (OCaml): A toolkit for embedding VPN capabilities in your application
- **libnetwork** (Go): Networking for containers
- **hyperkit** (C): A toolkit for embedding hypervisor capabilities in your application

Previous projects that "spinned-off":

- **runc:** now at the [[Containers - Open Container Initiative (OCI)|Open Container Initiative (OCI)]]
- **containerd** and **notary:** now at the Cloud Native Computing Foundation (CNCF)
- **distribution:** now under the [distribution](https://github.com/distribution) gitstore
- **linuxkit:** now at the [linuxkit](https://github.com/linuxkit) gitstore