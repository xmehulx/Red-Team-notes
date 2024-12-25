---
tags:
  - zone-transfer
---
Port 53
BIND file format

>**!!** Some servers can detect and block excessive DNS queries. Check for #waf-presence and use proper rate limits. 

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
# Subdomain Enumeration
## Subdomain Bruteforcing

```bash
for sub in $(cat subdomain.lst);do dig $sub.inlanefreight.htb @10.129.14.128 | grep -v ';\|SOA' | sed -r '/^\s*$/d' | grep $sub | tee -a subdomains.txt; done

ns.inlanefreight.htb.   604800  IN      A       10.129.34.136
mail1.inlanefreight.htb. 604800 IN      A       10.129.18.201
app.inlanefreight.htb.  604800  IN      A       10.129.18.15
```
## Domain Takeover
A DNS's canonical name (`CNAME`) record is used to map different domains to a parent domain. Many organizations use third-party services like AWS, GitHub, Akamai, Fastly, and other CDNs to host their content. In this case, they usually create a subdomain and make it point to those services. For example,

```TXT
sub.target.com.   60   IN   CNAME   anotherdomain.com
```

The domain name `sub.target.com` uses a CNAME record to another domain (i.e., `anotherdomain.com`). If `anotherdomain.com` expires and we register it for ourselves, we would have complete control over `sub.target.com` since the `target.com`'s DNS server has the above `CNAME` record.

After finding subdomains, using `nslookup` or `host`, we can enumerate the `CNAME` records for them.
```shell-session
$ host support.inlanefreight.com
support.inlanefreight.com is an alias for inlanefreight.s3.amazonaws.com
```
The [can-i-take-over-xyz](https://github.com/EdOverflow/can-i-take-over-xyz) repository shows whether the target services are vulnerable to a subdomain takeover and provides guidelines on assessing the vulnerability.
## Tools
- [[Fierce]]
- [[Subbrute]]
- [[Subfinder]]
- [[Sublist3r]]
# DNS Spoofing
## Local DNS Cache Poisoning
## Tools
- [[Ettercap]]
- [[Bettercap]]
# Tools
- [[Dig]]
- [[DNSenum]]
- [[whois]]
- [[pureDNS]]
