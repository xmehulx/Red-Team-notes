![[Pentest process.png]]

| **Layer**                               | **Description**                                                                                        | **Information Categories**                                                                         |
| --------------------------------------- | ------------------------------------------------------------------------------------------------------ | -------------------------------------------------------------------------------------------------- |
| 1. [[#Internet Presence]]               | Identification of internet presence and externally accessible infrastructure.                          | Domains, Subins,doma vHosts, [[#ASN]], Netblocks, IP Addresses, Cloud Instances, Security Measures |
| 2. [[#Gateway]]                         | Identify the possible security measures to protect the company's external and internal infrastructure. | Firewalls, DMZ, IPS/IDS, EDR, Proxies, NAC, Network Segmentation, VPN, Cloudflare                  |
| 3. [[#Accessible Services and Systems]] | Identify accessible interfaces and services that are hosted externally or internally.                  | Service Type, Functionality, Configuration, Port, Version, Interface                               |
| `4. Processes`                          | Identify the internal processes, sources, and destinations associated with the services.               | PID, Processed Data, Tasks, Source, Destination                                                    |
| `5. Privileges`                         | Identification of the internal permissions and privileges to the accessible services.                  | Groups, Users, Permissions, Restrictions, Environment                                              |
| `6. OS Setup`                           | Identification of the internal components and systems setup.                                           | OS Type, Patch Level, Network config, OS Environment, Configuration files, sensitive private files |
# What to look for
| **Data Point**       | **Description**                                                                                                                                                                                                                                                                                                                                                                                                          |
| -------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| `IP Space`           | Valid [[#ASN]] for our target, netblocks in use for the organization's public-facing infrastructure, cloud presence and the hosting providers, DNS record entries, etc.                                                                                                                                                                                                                                                  |
| `Domain Information` | Based on IP data, DNS, and site registrations. Who administers the domain? Any subdomains tied to our target? Ant publicly accessible domain services present? (Mailservers, DNS, Websites, VPN portals, etc.) Can we determine what kind of defenses are in place? (SIEM, AV, IPS/IDS, etc.)                                                                                                                            |
| `Schema Format`      | Can we discover the organization's email accounts, AD usernames, and even password policies? Anything that will give us information to build a valid username list for password spraying, credential stuffing, brute forcing, etc.                                                                                                                                                                                       |
| `Data Disclosures`   | We will be looking for publicly accessible files (.pdf, .ppt, .docx, .xlsx, etc.) for any information that helps shed light on the target. For example, any published files that contain `intranet` site listings, user metadata, shares, or other critical software or hardware in the environment (credentials pushed to a public GitHub repo, the internal AD username format in the metadata of a PDF, for example.) |
| `Breach Data`        | Any publicly released usernames, passwords, or other critical information that can help an attacker gain a foothold.                                                                                                                                                                                                                                                                                                     |
# Where to look for
| **Resource**                   | **Examples**                                                                                                                                                                                                                                                                                                                                                                                                                             |
| ------------------------------ | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| [[#ASN]] / IP registrars       | [IANA](https://www.iana.org/), [arin](https://www.arin.net/) for searching the Americas, [RIPE](https://www.ripe.net/) for searching in Europe, [BGP Toolkit](https://bgp.he.net/)                                                                                                                                                                                                                                                       |
| Domain Registrars & DNS        | [Domaintools](https://www.domaintools.com/), [PTRArchive](http://ptrarchive.com/), [ICANN](https://lookup.icann.org/lookup), manual DNS record requests against the domain in question or against well known DNS servers, such as `8.8.8.8`.                                                                                                                                                                                             |
| Social Media                   | Searching Linkedin, Twitter, Facebook, your region's major social media sites, news articles, and any relevant info you can find about the organization.                                                                                                                                                                                                                                                                                 |
| Public-Facing Company Websites | Often, the public website for a corporation will have relevant info embedded. News articles, embedded documents, and the "About Us" and "Contact Us" pages can also be gold mines.                                                                                                                                                                                                                                                       |
| Cloud & Dev Storage Spaces     | [GitHub](https://github.com/), [AWS S3 buckets & Azure Blog storage containers](https://grayhatwarfare.com/), [Google searches using "Dorks"](https://www.exploit-db.com/google-hacking-database)                                                                                                                                                                                                                                        |
| [[#Breached Data]] Sources     | [HaveIBeenPwned](https://haveibeenpwned.com/) to determine if any corporate email accounts appear in public breach data, [Dehashed](https://www.dehashed.com/) to search for corporate emails with cleartext passwords or hashes we can try to crack offline. We can then try these passwords against any exposed login portals (Citrix, RDS, OWA, 0365, VPN, VMware Horizon, custom applications, etc.) that may use AD authentication. |

# OSINT
## Internet Presence
Refer [[HTTP-HTTPS]]
### ASN
Large organizations often self-host their infrastructure, and since they have such a large footprint, they will have their own ASN. But smaller organizations will often host their websites and other infrastructure in someone else's space (e.g. Cloudflare, Google Cloud, AWS, or Azure).
#### Tools
- [Hurricane Electric](http://he.net/)
- [BGP Toolkit](https://bgp.he.net/)
Americas:
- [IANA](https://www.iana.org/)
- [arin](https://www.arin.net/) 
Europe:
- [RIPE](https://www.ripe.net/)
### Tools
- [[theHarvester]]
- [[Recon-ng]]
- [[FinalRecon]]
- [Trufflehog](https://github.com/trufflesecurity/truffleHog)
- [Greyhat Warfare](https://buckets.grayhatwarfare.com/)
### Subdomain enumeration
- Wordlist
	#web-fuzzers 
	[[Services/Networking & Infrastructure/DNS#Subdomain Bruteforcing]]
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
### Breached Data
#### Tools
- [HaveIBeenPwned](https://haveibeenpwned.com/)
- [Dehashed](http://dehashed.com/)
# Gateway
## Firewalls
Check if firewall present for #waf-presence
## Tools
- [[WAFw00f]]
### [[Services/Networking & Infrastructure/DNS|DNS]] Records

## Tools
- [[Dig]]
```bash
$ dig any inlanefreight.com
```
- [domaintools](https://whois.domaintools.com/)
- [viewdns.info](https://viewdns.info/)
## [[Google Dorks]]
```google
> intext:$company-name inurl:$3rd-party-service
```

# Accessible Services and Systems
## [[Services]]
## Systems
## Tools
- [[Fping]]
- [[Wireshark]]
- [[Nmap]]
# BugBounty
Refer [[Bug Bounty Checklist]]