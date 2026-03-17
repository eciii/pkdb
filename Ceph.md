
**Design goals**

Ceph is a _storage system_ that was designed to:

- be free and open source
- run on commodity hardware
- be unified, i.e. provide object, block and file storage on a single cluster

**Architecture**

Ceph is designed with a _client/cluster architecture_:

- The cluster is called _RADOS_. It is the foundational layer on which Ceph is built.
- The clients are the applications that use _librados_, the library provided by Ceph to interact with the RADOS cluster.

Ceph provides three _high-level storage services_ already built on top of librados:

- RGW for object storage
- RBD for block storage
- CephFS for file storage

In practice applications never use librados directly. They use instead one (or more) of the three high-level storage services provided by Ceph.

## The RADOS cluster

The RADOS cluster is the foundational layer on which Ceph is built. As a cluster, it is composed of 3 types of nodes:

- Monitors (`ceph-mon`)
	- ~ 3-7 per cluster
	- they are the "guardians" and "spokepersons" of the cluster state, which is "protected" using the Paxos (consensus) algorithm
	- as spokepersons of the cluster state they are
		- the central authority for authentication and policy
		- the coordination point for all other components in the cluster
- Manager (`ceph-mgr`)
	- 1 active, 1+ standby per cluster
	- aggregates real-time metrics
	- management host
- OSDs (`ceph-osd`)
	- 10s to 1000s per cluster
	- handle IO requests to storage devices (one per OSD)
	- cooperatively peer to replicate and rebalance data

**RADOS Objects**

RADOS objects are the fundamental unit of storage in a RADOS cluster. All data written to the cluster is first split into RADOS objects. Structurally RADOS objects have:

- a _name_ with 10s of characters (e.g. `rbd_header.10171e72d03d`)
- optionally up to 10s of _attributes_ that are up to 100s of bytes each (e.g. `version=12`)
- actual data that can be either:
	- _byte data_ of up to 10s of megabytes
	- _key/value data (omap)_ with up to 10000s of items of up to 10s of kilobytes each

**Pools**

RADOS objects live in named _pools_. Pools are logical containers for RADOS objects and usually reflect a technical use-case, e.g. VM images or user's home directories.

Pools can hold petabytes of data, which translates to bazillions of objects.

Pools are uniformly partitioned into a fixed number of _placement groups_. This number range form the 100s to the 1000s and is determined by the administrator of the cluster on a pool by pool basis.

- Each placement group is replicated a number of times according to the pool's replication policy
- Each replica is fully stored in an OSD. The OSD are chosen (pseudo)randomly according to the pool's placement policy.
- Each OSD can end up holding 10s to 100s of placement group replicas.

Pools also get individual policies (?).



Librados reads/writes data from/to OSDs directly. Since there are 10s to 1000s of OSDs in a single cluster there must be a way to find out where to read/write the data. For that librados uses _calculated placement_:

- librados gets a description of the cluster and its policies, _the cluster map_.
- 




