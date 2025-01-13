---
tags:
  - linux
---
# Example Usage
```shell-session
$ ldapsearch -H ldap://<IP> -x -s base namingcontexts
```
## Anonymous Bind
Null Session
```shell-session
$ ldapsearch -h <IP> -x -b "DC=INLANEFREIGHT,DC=LOCAL" -s sub "*" | grep -m 1 -B 10 pwdHistoryLength
```