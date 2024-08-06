![[Pentest process.png]]

| **Layer**                | **Description**                                                                                        | **Information Categories**                                                                         |
| ------------------------ | ------------------------------------------------------------------------------------------------------ | -------------------------------------------------------------------------------------------------- |
| `1. Internet Presence`   | Identification of internet presence and externally accessible infrastructure.                          | Domains, Subdomains, vHosts, ASN, Netblocks, IP Addresses, Cloud Instances, Security Measures      |
| `2. Gateway`             | Identify the possible security measures to protect the company's external and internal infrastructure. | Firewalls, DMZ, IPS/IDS, EDR, Proxies, NAC, Network Segmentation, VPN, Cloudflare                  |
| `3. Accessible Services` | Identify accessible interfaces and services that are hosted externally or internally.                  | Service Type, Functionality, Configuration, Port, Version, Interface                               |
| `4. Processes`           | Identify the internal processes, sources, and destinations associated with the services.               | PID, Processed Data, Tasks, Source, Destination                                                    |
| `5. Privileges`          | Identification of the internal permissions and privileges to the accessible services.                  | Groups, Users, Permissions, Restrictions, Environment                                              |
| `6. OS Setup`            | Identification of the internal components and systems setup.                                           | OS Type, Patch Level, Network config, OS Environment, Configuration files, sensitive private files |

# Internet Presence
Refer [[HTTP-HTTPS]]
## Tools
- [[theHarvester]]
- [[Recon-ng]]
- [[FinalRecon]]
### Subdomain enumeration
- Wordlist
	#web-fuzzers 
	[[Services/DNS#Subdomain Bruteforcing]]
- Tools
	[[DNSenum]]
	[[amass]]
	[[pureDNS]]

```shell
curl https://crt.sh/?q=inlanefreight.com&output=json | jq .

# Get only subdomains
curl -s https://crt.sh/\?q\=inlanefreight.com\&output\=json | jq . | grep name | cut -d":" -f2 | grep -v "CN=" | cut -d'"' -f2 | awk '{gsub(/\\n/,"\n");}1;' | sort -u

# Check if hosts directly accessible from the Internet and not hosted by third-party providers.
for i in $(cat subdomainlist); do host $i | grep "has address" | grep inlanefreight.com | cut -d" " -f1,4; done

#Shodan
for i in $(cat ip-addresses.txt); do shodan host $i; done
```
Can be further used with [[shodan]]

# Gateway
## Firewalls
Check if firewall present for #waf-presence
## Tools
- [[WAFw00f]]
# [[Services]]
## Tools
- [[Nmap]]
- [[Nikto]]

### [[Services/DNS]] Records

```bash
$ dig any inlanefreight.com
```

## Google Dorks
[[Google Dorks]]
```google
> intext:$company-name inurl:$3rd-party-service
```

# BugBounty
Refer [[Bug Bounty Checklist]]