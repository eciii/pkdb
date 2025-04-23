
**Fact:** Each Hashicorp product used to have a separate website for the corresponding community version (e.g `packer.io` for Packer and `vagrantup.com` for Vagrant). However, after the IBM acquisition, each one of those websites now redirect to the docsite of the corresponding product.

**Fact:** There is only one big fat gitstore for all Hashicorp products: https://github.com/hashicorp.

**Fact:** The docsites of all Hashicorp products are all located under https://developer.hashicorp.com.

---
### Packer

Website: https://www.packer.io (redirects to the docsite)
Gitrepo: https://github.com/hashicorp/packer
Docsite: https://developer.hashicorp.com/packer

Note that there are also [several others](https://github.com/orgs/hashicorp/repositories?q=packer) Packer-related gitrepos in the same gitstore.

**Integration with the Virt stack**

Packer has an official plugin for QEMU: https://developer.hashicorp.com/packer/integrations/hashicorp/qemu. Note that this plugin uses QEMU directly instead of using it through libvirt.

---
### Vagrant

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