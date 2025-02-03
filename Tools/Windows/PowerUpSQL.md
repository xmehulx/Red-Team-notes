# Enumerate MSSQL Instances
```powershell
PS >  Get-SQLInstanceDomain
```
# Run query
```powershell
PS >  Get-SQLQuery -Verbose -Instance "172.16.5.150,1433" -username "inlanefreight\damundsen" -password "SQL1234!" -query 'Select @@version'
```
or directly authenticate using [[MSSQLclient]]