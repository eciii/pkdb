[VMware](https://www.vmware.com/) is a company that offers multiple products for virtualization and cloud computing. One of the most notable products of VMware is **vSphere**, a _software suite_ that provides the required software to run and manage virtualized infrastructure. vSphere is essentially composed of two pieces of software:

* **ESXi**: The actual type 1 hypervisor to be installed in the physical machines.
* **vCenter Server**: A piece of software, provided as a [virtual appliance](https://en.wikipedia.org/wiki/Virtual_appliance), that can be used to manage all ESXi hosts in one or more datacenters from one central place. It also provides an abstraction layer that enables features that can only be implemented with the  aggregation of multiple ESXi hosts, such as clustering, high availability, etc.

Although the vSphere software suite is not free, it is possible to use a free version of just the ESXi hypervisor (with limited features).

### APIs

Automatization of management tasks can be done using one of the two APIs exposed by vCenter Server (only for the paid version; the free ESXi version doesn't expose these APIs):

* The older [vSphere Web Services API](https://developer.vmware.com/apis/1192/vsphere) (SOAP/WSDL based). Can be accessed using either the [pyvmomi](https://github.com/vmware/pyvmomi) or [govmomi](https://github.com/vmware/govmomi) libraries. It is very mature and still widely used.
* The newer [vSphere Automation API](https://developer.vmware.com/apis/vsphere-automation/latest/) (JSON/REST based). It still has many limitations (see [here](https://mist.io/blog/2020-05-27-vmware-please-improve-vspheres-restful-api)).

### VMware Tools

VMware offers a software suite called **VMware Tools** ([docs](https://docs.vmware.com/en/VMware-Tools/index.html)) that is meant to be installed in guest OSes to improve the interaction with them. Previously it was provided as a closed-source ISO but eventually VMware open-sourced the Linux version of it under the name **open-vm-tools**.

The open-vm-tools project seems to be hosted both at [GitHub](https://github.com/vmware/open-vm-tools) and at [SourceForge](http://sourceforge.net/projects/open-vm-tools/). Nowadays open-vm-tools is available on the default repositories of virtually any mainstream Linux distribution and is automatically installed by most installers when a VMware hypervisor is detected. In Debian the `open-vm-tools` package is responsible for two systemd services:

- `vgauth.service` which executes the binary `/usr/bin/VGAuthService`. This service provides support for SAML based authentication for vSphere Guest Operations.
- `open-vm-tools.service` which executes the binary `/usr/bin/vmtoolsd`. This service uses a plug-in architecture to implement several features to improve the interaction with guest OSes.

### Experiments with `govc`

`govc` is a tool shipped with [govmomi](https://github.com/vmware/govmomi/tree/main)

* I downloaded `govc` into a directory
* In that directory I also created a convenience script called `govc.sh` that calls the `govc` binary with the right parameters
* The last things I was experimenting with was to get the lest version of a ESXi host in order to know the maximum compatibility that I could assign to a VM during creation:

```
# List the ESXi hosts together with their version
$ ./govc.sh find -type h \
> | sort \
> | while IFS='' read l; do
>     v="$(
>         ./govc.sh host.info -host="$l" -json \
>         | jq '.HostSystems[].Summary.Config.Product.Version'
>     )"
>     echo "$l;$v"
>   done
  
# List all resources in vSphere (the HostSystem ones are the ones I was
# interested in as they are the actual ESXi hosts)
$ ./govc.sh find -l | sed 's/ \+/;/' | sort -k2,2 -t';' | column -ts';'
```