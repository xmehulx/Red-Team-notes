Updated version for [[crackmapexec]]
# LDAP
``` shell-session
$ nxc ldap 10.10.11.24 -d ghost.htb -u '' --use-kcache --bloodhound --kdcHost DC01.ghost.htb --dns-server 10.10.11.24 -c All
```
# SMB
```shell-session
$ nxc smb <IP> -u <username> -p <password> --shares
```
## Useful Flags
- `--continue-on-success`: To keep on bruteforcing after a successful login.
- `--local-auth`: If the target is a non-domain joined system.
## List Shares Anonymously
```shell-session
$ nxc smb <IP> -u 'admin' -p '' --shares
```

# Bruteforce
## SMB
### Bruteforce RIDs
```shell-session
$ nxc smb <IP> -u 'guest' -p '' --rid-brute
```
### Bruteforce Username/Passwords
```shell-session
$ nxc smb <IP> -u </path/to/username> -p </path/to/passwords>
```