### Overview

**vSphere** is the most notable product of **VMware**. It is a _software suite_ that provides the required software to run and manage virtualized infrastructure. vSphere is essentially composed of two pieces of software:

* **ESXi**: The actual type 1 hypervisor to be installed in the physical machines. This is a completely custom OS with a micro-kernel architecture.
* **vCenter Server**: A piece of software, provided as a _virtual appliance_, that can be used to manage all ESXi hosts in one or more datacenters from one central place. It also provides an abstraction layer that enables features that can only be implemented with the  aggregation of multiple ESXi hosts, such as clustering, high availability, etc.

### APIs

Automation of management tasks can be done using one of the two APIs exposed by vCenter Server (only for the paid version; the free ESXi version doesn't expose these APIs):

* The older [vSphere Web Services API](https://developer.vmware.com/apis/1192/vsphere) (SOAP/WSDL based). Can be accessed using either the [pyvmomi](https://github.com/vmware/pyvmomi) or [govmomi](https://github.com/vmware/govmomi) libraries. It is very mature and still widely used.
* The newer [vSphere Automation API](https://developer.vmware.com/apis/vsphere-automation/latest/) (JSON/REST based). It still has many limitations (see [here](https://mist.io/blog/2020-05-27-vmware-please-improve-vspheres-restful-api)).

`govc` is a tool shipped with govmomi that can be very handy for automation.

### VMware Tools

VMware offers a software suite called **VMware Tools** ([docs](https://docs.vmware.com/en/VMware-Tools/index.html)) that is meant to be installed in guest OSes to improve their interaction with vSphere. Previously it was provided as a closed-source ISO but eventually VMware open-sourced the Linux version of it under the name **open-vm-tools**.

The open-vm-tools project seems to be hosted both at [GitHub](https://github.com/vmware/open-vm-tools) and at [SourceForge](http://sourceforge.net/projects/open-vm-tools/). Nowadays open-vm-tools is available on the default repositories of virtually any mainstream Linux distribution and is automatically installed by most installers when a VMware hypervisor is detected. In Debian the `open-vm-tools` package is responsible for two systemd services:

- `vgauth.service` which executes the binary `/usr/bin/VGAuthService`. This service provides support for SAML based authentication for vSphere Guest Operations.
- `open-vm-tools.service` which executes the binary `/usr/bin/vmtoolsd`. This service uses a plug-in architecture to implement several features to improve the interaction with guest OSes.

### vSphere Networking

- [Official documentation](https://docs.vmware.com/en/VMware-vSphere/8.0/vsphere-networking/GUID-35B40B0B-0C13-43B2-BC85-18C9C91BE2D4.html) for _vSphere Networking_.
- Very good [blog post](https://fastreroute.com/vsphere-esxi-networking-guide-standard-switches/).
- About multi-NIC vMotion:
	- [This section](https://docs.vmware.com/en/VMware-vSphere/8.0/vsphere-vcenter-esxi-management/GUID-30FAA00F-D5F3-475D-820E-5D45517AC18E.html) of the official documentation for _vCenter Server and Host Management_.
	- [This KB article](https://knowledge.broadcom.com/external/article?legacyId=2007467).
	- [This blog post](https://frankdenneman.nl/2012/12/18/designing-your-vmotion-network/) by Frank Denneman.
- About Multihoming:
	- [This section](https://docs.vmware.com/en/VMware-vSphere/8.0/vsphere-networking/GUID-D4191320-209E-4CB5-A709-C8741E713348.html) of the official documentation.
	- [This KB article](https://knowledge.broadcom.com/external/article?legacyId=2010877).