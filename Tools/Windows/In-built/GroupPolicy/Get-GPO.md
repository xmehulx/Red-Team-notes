Gets one GPO or all the GPOs in a domain.
```powershell
PS > Get-GPO -All | Select DisplayName
```
# Enumerate GPO rights
- Of a whole group:
```powershell
PS > $sid=Convert-NameToSid "Domain Users"
PS > Get-DomainGPO | Get-ObjectAcl | ?{$_.SecurityIdentifier -eq $sid}

ObjectDN              : CN={7CA9C789-14CE-46E3-A722-83F4097AF532}, CN=Policies,CN=System,DC=INLANEFREIGHT,DC=LOCAL
ObjectSID             :
ActiveDirectoryRights : ... WriteProperty, GenericExecute, WriteDacl, WriteOwner
... SNIP ...
SecurityIdentifier    : S-1-5-21-3842939050-3880317879-2865463114-513
AceType               : AccessAllowed
AceFlags              : ObjectInherit, ContainerInherit
IsInherited           : False
InheritanceFlags      : ContainerInherit, ObjectInherit
```
## Convert GPO GUID to Name
```powershell
PS > Get-GPO -Guid 7CA9C789-14CE-46E3-A722-83F4097AF532
```