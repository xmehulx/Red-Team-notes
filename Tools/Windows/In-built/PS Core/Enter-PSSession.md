Starts an interactive session with a remote computer.
# Kerberos "Double-Hop"
- [[klist]] can confirm if any forwardable credentials are stored. 
1. If not, we can register a new session configuration using the [[Register-PSSessionConfiguration]] cmdlet.
```powershell
PS > Register-PSSessionConfiguration -Name <Session-Name> -RunAsCredential inlanefreight\backupadm
```
1. Restart WinRM session with `Restart-Service WinRM`. This will kick us out, so we need to start a new PSSession using the named registered session we set up
```powershell
PS > Enter-PSSession -ComputerName DEV01 -Credential INLANEFREIGHT\backupadm -ConfigurationName <Session-Name>
```