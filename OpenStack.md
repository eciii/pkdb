_This is a summary of the talk [Is OpenStack still needed in 2023?](https://fosdem.org/2023/schedule/event/vai_openstack_still_needed/) given at FOSDEM 2023_

- OpenStack started in 2010 when VMware and AWS dominated the private and public infrastructure sectors. Back then the solutions provided by VMware and AWS were mainly devoted to provide virtualization services.
- It had a "viral" growth that resulted in a lot of "scope creep" (everyone wanted to add their own use-cases to it). OpenStack itself didn't have a clear understanding of who their own users were (the people providing infrastructure or the people building stuff on top of that infrastructure).
- Eventually virtualization and OpenStack stopped being trendy and the new hot thing became containerization and Docker/Kubernetes. This led to the common perception that OpenStack was "dead". But in reality OpenStack continued a steady growth and is consistently used by a lot of big technology companies around the world.
- This allowed OpenStack to redefine its place, purpose and scope as the layer that provides IaaS, located between Linux (as the hypervisor) and Kubernetes (as the Container Orchestrator). This is what they call the **LOKI** stack (**L**inux + **O**penStack + **K**ubernetes = **I**nfrastructure).
- To advance this new purpose the OpenStack Foundation changed its name to **OpenInfra Foundation** ([website](https://openinfra.dev/)) and expanded its mission to support the development and adoption of open infrastructure globally.
- There are two major aspects that will play a big role in the relevance of private clouds and OpenStack in the future:
  - Cost. There is a [new study](https://a16z.com/2021/05/27/cost-of-cloud-paradox-market-cap-cloud-lifecycle-scale-growth-repatriation-optimization/) that shows how public clouds can become a heavy burden in comparison with private clouds.
  - Compliance and Digital Sovereignty (especially in the European Union).

---

Some technical aspects:

- The initial (and maybe most fundamental) components of OpenStack were (and are) **Nova**, which handles the "compute" aspect, and **Swift**, which handles the storage aspect.