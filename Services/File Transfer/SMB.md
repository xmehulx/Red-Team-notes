Port: TCP 139(with NetBIOS), 445 (on TCP/IP)
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
- If guest mode allowed, can try to bruteforce RIDs with [[nxc#Bruteforce RIDs|nxc]] or [[CrackMapExec#Bruteforce RIDs|crackmapexec]].

# Windows
## Commands
- [[net use]] (CMD)
```cmd-session
> net use n: \\<IP>\<share> [/user:plaintext Password123]
The command completed successfully.
```
- [[New-PSDrive]] (Powershell)
![[File Transfer#Windows Client#New-PSDrive|Connect to SMB server]]
# Linux
UNIX machines can browse and mount both SMB and Samba shares.
```shell-session
$ sudo mount -t cifs -o username=<USERNAME>,password=<PASSWORD>,domain=. //192.168.220.129/Finance <MOUNT FOLDER>
```
Credential file can be used with `-o credentials=/path/credentialfile`, with the credential file structured as below:
```txt
username=plaintext
password=Password123
domain=.
```
## RCE using SMB
- [[PsExec|Impacket-PSExec]]
- [[SMBExec|Impacket-SMBExec]]
- [[atexac|Impacket-atexac]]
- [[CrackMapExec#SMBExec|CME-SMBExec]]
- [[Tools/Linux/Frameworks/Metasploit|Metasploit]]
## Tools
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
```shell-session
$ smbclient -U <user> \\\\<IP>\\<share>
```
- [[Enum4Linux]]
```shell-session
$ ./enum4linux-ng.py 10.10.11.45 -A -C
```