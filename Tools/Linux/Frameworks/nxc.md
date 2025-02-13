---
tags:
  - linux
  - smb
  - gpp
---
Updated version for [[CrackMapExec]]
# LDAP
``` shell-session
$ nxc ldap 10.10.11.24 -d ghost.htb -u '' --use-kcache --bloodhound --kdcHost DC01.ghost.htb --dns-server 10.10.11.24 -c All
```
# SMB
```shell-session
$ nxc smb <IP> -u <username> -p <password>
```
### Bruteforce Username/Passwords
```shell-session
$ nxc smb <IP> -u </path/to/username> -p </path/to/passwords>
```
### Enumerate Users
#### Using Anonymous Session
```shell-session
$ nxc smb 172.16.5.5 --users
```
#### Using Valid Credential
```shell-session
$ sudo nxc smb <IP> -u htb-student -p Academy_student_AD! --users
```
### Password Spraying
```shell-session
$ sudo nxc smb <IP> -u <USERS.LST> -p <PASS> | grep +
```
### Hash Spraying
```shell-session
$ sudo crackmapexec smb --local-auth <IP>/23 -u administrator -H <NT-HASH> | grep +
```
#non-evasive
## Useful Flags and Modules
- `--continue-on-success`: To keep on bruteforcing after a successful login.
- `--local-auth`: If the target is a non-domain joined system.
- `--users`: enumerate users. Also provides `badpwdcount` to see failed password attempts
- `--groups`: enumerate groups with `membercount`
- `--loggedon-users`
- `--share[s] <SHARE>`: List shares, or a specific share
- `spider_plus`: digs through each readable share and lists all readable files
- `gpp_autologin`: Searches the DC for registry.xml to find autologon information and returns the username and password.
- `gpp_password`: Retrieves the plaintext password and other information for accounts pushed through GPP.
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