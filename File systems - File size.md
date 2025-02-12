**Facts**:

- A **storage device** stores information in blocks of a predefined size in bytes. This size is called the **storage device's Block Size (sdBS)**. Early storage devices used a sdBS of 512 bytes but nowadays the usual sdBS is 4096 bytes.
- The sdBS also represent the optimal block size for data transfers to the storage device, that is, for a data transfer to be optimal it must be performed in chunks whose size is a multiple of the sdBS.

**Facts**:

- **File systems** organize data in **files**.
- A file represent a piece of information in bytes (the **file's data**). The **file's Data Size (fDS)** is the amount of bytes of the file's data. This number is independent of the file system where the file is stored.
- File systems store files in structures called **inodes** (see a graphical representation of an inode [here](https://thecustomizewindows.cachefly.net/wp-content/uploads/2023/11/Why-is-Inode-Used-in-Linux.png)). The inode stores the **file's metadata** in one contiguous chunk while the file's data is spread in blocks. The size of those blocks in bytes is the **file system's Block Size (fsBS)**.

**Fact**: If a file system wants to make optimal use of a storage device the fsBS must be equal to the sdBS.

**Fact**: The **file's Allocated Size (fAS)** is the total size of all the (storage device) blocks used by the underlying storage device to store the (file system) data blocks used by the file system to store the file's data. The fAS can be given in units of bytes or (storage device) blocks.

**Facts**:

- The `stat` system call in Linux returns a structure that has three fields related to size:
	- `st_size`: the fDS.
	- `st_blocks`: according to `man`, _"the number of blocks allocated to the file, in 512-byte units (this may be smaller than `st_size`/512 when the file has holes)"._ This seems to correspond to the fAS.
	- `st_blksize`: according to `man`, _"the "preferred" block size for efficient filesystem I/O"._ This seems to correspond to the sdBS.
- Since the `st_blocks` field is provided in 512-byte block size units, the `stat` command reports the allocated size of a file (fAS) in blocks of such size. However it also reports the `st_blksize` field (`IO Block` in the output).
- Weirdly enough some Unix/Linux tools like `du` and `ls -s` report by default the allocated size of a file in blocks of size 1024 bytes. It seems that these tools just convert the `st_blocks` field reported by `stat` to such block size (i.e no new information is provided with respect to `stat`).
- In general the three size fields reported by `stat` are consistent if the **stat size consistency (ssc) equation** holds: `st_blocks = ceil(st_size/st_blksize) * st_blksize/512`
- There is a way to find the underlying sdBS by using only the `st_blocks` field reported by `stat` (I'll call it the **1-byte-file trick**). It is enough to write a one-byte file (e.g using `printf "x" > x`) and then checking the amount of blocks reported by `stat`, say `n`. Then the sdBS would be `n*512`.
- For NFS file systems the ssc equation doesn't hold. This is because the value reported by `st_blksize` on NFS file systems correspond to the `wsize` NFS mount option and not to the sdBS of the underlying storage device. On the other hand the block size given by the 1-byte-file trick seems to agree with the underlying sdBS.