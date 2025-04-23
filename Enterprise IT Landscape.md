**Links**

- [Diagram](https://commons.wikimedia.org/wiki/File:Wikipedia_webrequest_2022.png) of MediaWiki web request flow
- Wikimedia's Grafana instance: https://grafana.wikimedia.org
- Wikimedia Tech Blog: https://techblog.wikimedia.org

---

OS: Linux

Cloud:

- See _Virtualization (ecosystem)_
- Kubernetes

Virtual Networking (a.k.a SDN (software-defined networking)):

- Open vSwitch (OvS)
- OVN (Open Virtual Network)

Distributed Storage:

- SAN + Ceph

LB/HA, Proxy:
- Nginx (for simpler setups?)
- HAProxy (for advanced setups)
- Linux Virtual Server (LVS; for very advanced Layer 4 LB/HA; no proxy functionality)
- Envoy (for cloud infrastructure; CNCF graduated)

DNS:
- BIND (for the Internet)
- dnsmasq (for LANs)
- CoreDNS (for cloud infrastructure; CNCF graduated)

Firewall:
- nftables (Linux subsystem)

Cache:
- Varnish
- Apache Traffic Server (ATS)
- Squid

Event/Message streaming:
- Kafka
- NATS

IAM:

- FreeIPA

Monitoring:

- Icinga2

---

IT Infrastructure Lifecycle Management (ITILM):

- Provisioning / Initial Configuration
- Maintenance:
	- Reconfiguration
	- Updates/Upgrades
	- Backups
- Monitoring
- Disposal

Automation of ITILM tasks (on-demand, scheduled, event-driven):
- Ansible (pros: very general, very powerful, with a simple interface; cons: cumbersome and inflexible for advanced use-cases)
- Puppet, Chef, Salt (see [here](https://www.redhat.com/en/topics/automation/understanding-ansible-vs-terraform-puppet-chef-and-salt); they all seem mature automation solutions)
- Terraform/OpenTofu (specialized in provisioning)
- Apache Airflow (looks interesting for advanced use-cases, but supports only scheduled workflows)
- Foreman (hmmm doesn't feel right; depends on Puppet; seems to be kind of a Puppet frontend)