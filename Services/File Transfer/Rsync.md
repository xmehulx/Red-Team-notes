Port: 873, can piggyback SSH

Uses delta-transfer algorithm to send only file differences based on size and/or date modified during backups and mirroring.

# Tools
- [[Netcat]]
```bash
$ nc -nv 127.0.0.1 873

(UNKNOWN) [127.0.0.1] 873 (rsync) open
@RSYNCD: 31.0
@RSYNCD: 31.0
#list
dev            	Dev Tools
@RSYNCD: EXIT
```
- [[rsync-tool]] 
```bash
$ rsync -av --list-only rsync://127.0.0.1/<folder>

receiving incremental file list
drwxr-xr-x             48 2022/09/19 09:43:10 .
-rw-r--r--              0 2022/09/19 09:34:50 build.sh
-rw-r--r--              0 2022/09/19 09:36:02 secrets.yaml
drwx------             54 2022/09/19 09:43:10 .ssh

sent 25 bytes  received 221 bytes  492.00 bytes/sec
total size is 0  speedup is 0.00
```
Use `-e ssh` flag, or `-e "ssh -p2222"` if Rsync using SSH