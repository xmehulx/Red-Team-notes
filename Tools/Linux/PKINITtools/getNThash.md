---
tags:
  - linux
---
If we have AS-REP key, we could request the NT hash for our target host/user by using Kerberos U2U to submit a TGS request with the [Privileged Attribute Certificate (PAC)](https://stealthbits.com/blog/what-is-the-kerberos-pac/) containing this NT hash. This can be decrypted with the AS-REP encryption key we obtained when requesting the TGT using [[getTGTPKINIT]].
```shell-session
$ getnthash.py -key <AS-REP-Key> INLANEFREIGHT.LOCAL/ACADEMY-EA-DC01$
```