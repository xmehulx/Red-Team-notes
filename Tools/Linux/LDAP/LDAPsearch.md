---
tags:
  - linux
---
# Example Usage
```shell-session
$ ldapsearch -H ldap://<IP> -x -s base namingcontexts
```
## Anonymous Bind
### Password Policy
Null Session can be helpful to find password policy
```shell-session
$ ldapsearch -h <IP> -x -b "DC=INLANEFREIGHT,DC=LOCAL" -s sub "*" | grep -m 1 -B 10 pwdHistoryLength
```
### Enumerate users
```shell-session
$ ldapsearch -h 172.16.5.5 -x -b "DC=INLANEFREIGHT,DC=LOCAL" -s sub "(&(objectclass=user))"  | grep sAMAccountName: | cut -f2 -d" "
```