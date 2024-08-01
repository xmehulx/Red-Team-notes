Port: TCP 139, 445
# Dangerous Settings
| **Setting**                 | **Description**                                                     |
| --------------------------- | ------------------------------------------------------------------- |
| `browseable = yes`          | Allow listing available shares in the current share?                |
| `read only = no`            | Forbid the creation and modification of files?                      |
| `writable = yes`            | Allow users to create and modify files?                             |
| `guest ok = yes`            | Allow connecting to the service without using a password?           |
| `enable privileges = yes`   | Honor privileges assigned to specific SID?                          |
| `create mask = 0777`        | What permissions must be assigned to the newly created files?       |
| `directory mask = 0777`     | What permissions must be assigned to the newly created directories? |
| `logon script = script.sh`  | What script needs to be executed on the user's login?               |
| `magic script = script.sh`  | Which script should be executed when the script gets closed?        |
| `magic output = script.out` | Where the output of the magic script needs to be stored?            |
# Tools
- [[SMBMap]]
```shell-session
$ smbmap -H 10.129.14.128
```
- [[nxc]]
```shell-session
$ nxc smb 10.10.10.1 -u '' -p ''
```
- [[CrackMapExec]]
```shell-session
$ crackmapexec smb 10.129.14.128 --shares -u '' -p ''
```
- [[SMBclient]]