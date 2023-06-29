**Software**

- **MinIO** ([Website](https://min.io/), [Source Code](https://github.com/minio/minio)): Object storage solution written in Go that offers an S3-compatible API and a GUI management console (in Typescript). The same project also maintains a CLI tool (also in Go) called ''mc'' ([Source Code](https://github.com/minio/mc)) to interact with any S3-compatible API.
- **rclone** ([Website](https://rclone.org/), [Source Code](https://github.com/rclone/rclone)): CLI tool written in Go to manage files on cloud storage. It can backup/restore, sync/migrate, mount and analyze files across many cloud storage providers.

[Amazon S3 Official Documentation](https://docs.aws.amazon.com/AmazonS3/latest/userguide/Welcome.html). Some interesting parts:

- [Object Keys](https://docs.aws.amazon.com/AmazonS3/latest/userguide/object-keys.html)
- [Object integrity checks](https://docs.aws.amazon.com/AmazonS3/latest/userguide/checking-object-integrity.html)