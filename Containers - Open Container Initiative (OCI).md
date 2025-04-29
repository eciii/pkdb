- Website: https://opencontainers.org/
- Gitstore: https://github.com/opencontainers

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