---
tags:
  - linux
  - dns
  - MicrosoftAD
---
[adidnsdump](https://github.com/dirkjanm/adidnsdump) enumerates all DNS records in a domain using a valid domain user account.
```shell-session
$ adidnsdump -u inlanefreight\\forend ldap://<DC-IP>
```
# Useful Flags
-`-r`: attempt to resolve unknown records by performing an `A` query.