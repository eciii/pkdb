**Software**

- **MinIO** ([Website](https://min.io/), [Source Code](https://github.com/minio/minio)): Object storage solution written in Go that offers an S3-compatible API and a GUI management console (in Typescript). The same project also maintains a CLI tool (also in Go) called ''mc'' ([Source Code](https://github.com/minio/mc)) to interact with any S3-compatible API.
- **rclone** ([Website](https://rclone.org/), [Source Code](https://github.com/rclone/rclone)): CLI tool written in Go to manage files on cloud storage. It can backup/restore, sync/migrate, mount and analyze files across many cloud storage providers.

[Amazon S3 Official Documentation](https://docs.aws.amazon.com/AmazonS3/latest/userguide/Welcome.html). Some interesting parts:

- [Object Keys](https://docs.aws.amazon.com/AmazonS3/latest/userguide/object-keys.html)
- [Object integrity checks](https://docs.aws.amazon.com/AmazonS3/latest/userguide/checking-object-integrity.html)

---
### Concepts (from the official Amazon S3 documentation)

**Storage classes:** From low-latency/high-cost for frequently accessed data to high-latency/low-cost for infrequently accessed data.

**Object lifecycle:** Set of rules that define actions that Amazon S3 applies to a group of objects. 

- There are two types of actions:
  - Transition actions: These actions define when objects transition to another storage class.
  - Expiration actions: These actions define when objects expire. Amazon S3 deletes expired objects on your behalf.

**Object lock:** Settings that prevent objects from being deleted or overwritten.

- There are two types of locks:
  - Retention period: Specifies a fixed period of time during which an object remains locked.
  - Legal hold: Provides the same protection as a retention period, but it has no expiration date. Instead, a legal hold remains in place until you explicitly remove it.
- Object Lock works only in versioned buckets.
- Object locks apply only to individual object versions. An object lock don't prevent new versions of the object from being created.
- An object version can have both a retention period and a legal hold, one but not the other, or neither.
- When you lock an object version, Amazon S3 stores the lock information in the metadata for that object version.

**Bucket replication:** Enables automatic, _asynchronous_ copying of objects across Amazon S3 buckets.

- You can replicate objects to a single destination bucket or to multiple destination buckets.
- The destination buckets can be in different AWS Regions or within the same Region as the source bucket.

**S3 Batch Operations:** Perform large-scale batch operations on lists of Amazon S3 objects that you specify.

- Amazon S3 tracks progress, sends notifications, and stores a detailed completion report of all actions, providing a fully managed, auditable, and serverless experience.
- You can use S3 Batch Operations through the AWS Management Console, AWS CLI, Amazon SDKs, or REST API.