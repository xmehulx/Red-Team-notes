Updated version for crackmapexec
# LDAP
``` shell-session
$ nxc ldap 10.10.11.24 -d ghost.htb -u '' --use-kcache --bloodhound --kdcHost DC01.ghost.htb --dns-server 10.10.11.24 -c All
```
# SMB
```shell-session
$ nxc smb <IP> -u <username> -p <password> --shares
```
## List Shares Anonymously
```shell-session
$ nxc smb <IP> -u 'admin' -p '' --shares
```
## Bruteforce RIDs
```shell-session
$ crackmapexec smb <IP> -u 'guest' -p '' --rid-brute
```
