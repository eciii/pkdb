**Facts**:

- If a file has a data size of `fds` bytes (fDS = `fds`) and the underlying file system has a file system block size of `fsbs` bytes (fsBS = `fsbs`) then the number of blocks needed to store the file's data in that file system is `ceil(fds/fsbs)`. A sparse file is a _file system's file_ that uses strictly less blocks than `ceil(fds/fsbs)`.
- Some file systems have a mechanism to identify which blocks of a given file are filled only with zeros. This allows them to avoid allocating storage for those blocks.
- File systems that support sparse files might _allow discretionary partial sparsiness_ or _enforce full sparsiness_:
	- Most file systems implement discretionary partial sparsiness. This happens when the clients of the file system are allowed to write large chunks of contiguous zeros and the file system doesn't proactively check if the allocation of blocks containing those chunks can be skipped. In this case a file system file can be partially sparse, that is but the number of "skipped" blocks for that file could potentially be higher.
	- A file system could also enforce full sparsiness. This would happen if the file system proactively checks if the allocation of a block can be skipped. In such file system there is never a file's data block filled only with zeros.
- Note that the sparse nature of a file (i.e whether a file is sparse or not) depends on the _content_ of the file _and_ on the _file system_ where the file is stored. If the file system allows discretionary partial sparsiness, the sparse nature of a file is not discrete (is the file sparse or not) but continous (how sparse is the file).

**About sparse files in Proxmox and Ceph**

- https://www.mail-archive.com/pve-devel@pve.proxmox.com/msg16349.html (is the mailing list post with the zeroinit QEMU patch by Proxmox)
- https://pve-devel.pve.proxmox.narkive.com/LOoCl4a8/qemu-drive-mirror-with-zeroinit-and-ceph-rbd-not-working (mentions that the zeroinit patch uses the `SEEK_DATA`/`SEEK_HOLE` options of `lseek(2)`)
- https://www.man7.org/linux/man-pages/man2/lseek.2.html (is the man page of `lseek` that mentions the `SEEK_DATA`/`SEEK_HOLE` options)
- https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/7/html/storage_administration_guide/ch-nfs (mentions that NFS supports the `SEEK_DATA`/`SEEK_HOLE` options of `lseek(2)` only on version 4.2)