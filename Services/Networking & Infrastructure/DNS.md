---
tags:
  - zone-transfer
---
Port 53
BIND file format

> **!!** Some servers can detect and block excessive DNS queries. Check for #waf-presence and use proper rate limits. 

### Dangerous Settings
| Option            | Description                                                                    |
| ----------------- | ------------------------------------------------------------------------------ |
| `allow-query`     | Defines which hosts are allowed to send requests to the DNS server.            |
| `allow-recursion` | Defines which hosts are allowed to send recursive requests to the DNS server.  |
| `allow-transfer`  | Defines which hosts are allowed to receive zone transfers from the DNS server. |
| `zone-statistics` | Collects statistical data of zones.                                            |

### DNS Server Version
DNS server's version using a class CHAOS query and type TXT. However, this entry must exist on the DNS server.

```bash
$ dig CH TXT version.bind 10.129.120.85
```

### DNS Zone Transfer
```bash
$ dig axfr internal.inlanefreight.htb @10.129.33.148
```

## Subdomain Bruteforcing

```bash
for sub in $(cat subdomain.lst);do dig $sub.inlanefreight.htb @10.129.14.128 | grep -v ';\|SOA' | sed -r '/^\s*$/d' | grep $sub | tee -a subdomains.txt; done

ns.inlanefreight.htb.   604800  IN      A       10.129.34.136
mail1.inlanefreight.htb. 604800 IN      A       10.129.18.201
app.inlanefreight.htb.  604800  IN      A       10.129.18.15
```

# Tools
- [[Dig]]
- [[DNSenum]]
- [[whois]]
- [[pureDNS]]