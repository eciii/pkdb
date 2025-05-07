
Website: https://www.vagrantup.com (redirects to the docsite)
Gitrepo: https://github.com/hashicorp/vagrant
Docsite: https://developer.hashicorp.com/vagrant

Note that there are also [several others](https://github.com/orgs/hashicorp/repositories?q=vagrant) Vagrant-related gitrepos in the same gitstore.

**Integration with the Virt stack**

Vagrant has no official plugin to integrate with the Virt stack but this can be achieved by using the third-party Vagrant-Libvirt plugin:

Website: https://vagrant-libvirt.github.io/vagrant-libvirt
Gitstore: https://github.com/vagrant-libvirt
Docsite: (same as website)

**Links**

- [Article](https://fedoramagazine.org/vagrant-qemukvm-fedora-devops-sysadmin/) in Fedora Magazine: Installing and running Vagrant using qemu-kvm
- [GitHub repository](https://github.com/patrickdlee/vagrant-examples) with Vagrant examples

---

**Most important sub-commands**

- **help**: Shows the help for a subcommand
- **version**: Prints current and latest Vagrant version

User-level:

- **cloud**: Interact with the Vagrant Cloud
- **plugin**: Manage plugins
- **box**: Manage boxes
- **global-status**: List the status of all Vagrant environments for this user

Environment-level:

- Related to the Vagrantfile:
	- **init**: Initialize a new Vagrant environment by creating a Vagrantfile
	- **validate**: Validate the Vagrantfile
- Query or change the status of the environment:
	- **status**: Show the status of the vagrant environment
	- **up**: Start and provision the vagrant environment
	- **suspend**: Suspend the machines in the vagrant environment
	- **resume**: Resume the suspended machines in the vagrant environment
	- **halt**: Shut down the machines in the vagrant environment
	- **reload**: Shut down and start (without provisioning)
	- **destroy**: Stop the machines and delete all traces of the vagrant environment
- Communicate with the environment via some protocol:
	- **ssh**: Connects to machine via SSH
	- **ssh-config**: Outputs OpenSSH valid configuration to connect to the machine
	- **rdp**: Connects to machine via RDP
	- **powershell**: Connects to machine via powershell remoting
	- **winrm**: Executes commands on a machine via WinRM
	- **winrm-config**: Outputs WinRM configuration to connect to the machine
- Pre-defined tasks that interact with the enviroment:
	- **port**: Displays information about guest port mappings
	- **provision**: Provisions the vagrant machines
	- **upload**: Upload to machine via communicator
	- **snapshot**: Manages snapshots of the machines

