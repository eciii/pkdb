### Red Hat Satellite

The Red Hat Satellite project is the integration of several open source projects that are mostly promoted and maintained by Red Hat:

- **Foreman**: The main project. It provides:
	- The Web UI
	- A plugin system
	- _Provisioning and Configuration Management_ functionality
- **Katello**: A Foreman plugin. It provides/integrates:
	- _Content and Patch Management_ functionality using **Pulp**
	- _Subscription and Entitlement Management_ functionality using **Candlepin**

The [documentation of Red Hat Satellite](https://docs.redhat.com/en/documentation/red_hat_satellite) should apply to all these projects as well.

---
### Foreman

Website: https://theforeman.org
Gitstore: https://github.com/theforeman

Documentation:

The current Foreman documentation is at https://theforeman.org/manuals/latest. However, there is an ongoing effort to use the Red Hat Satellite documentation as the basis of a new official documentation under https://docs.theforeman.org (see [here](https://community.theforeman.org/t/making-docs-theforeman-org-the-primary-documentation-source) for more information).

---
### Katello

Website: http://www.katello.org
Gitstore: https://github.com/Katello
Documentation: currently under https://docs.theforeman.org

**Links**

- [Livestream: Introduction to Katello](https://www.youtube.com/watch?v=kWbfU_1zseU)

---
### Pulp

- Website: https://pulpproject.org
- Gitstore: https://github.com/pulp

---
### Candlepin

- Website: https://www.candlepinproject.org
- Gitstore: https://github.com/candlepin

Note that the `subscription-manager` tool is part of this project and have its own gitrepo at https://github.com/candlepin/subscription-manager.