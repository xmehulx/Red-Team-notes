---
tags:
  - windows
  - dcsync
---
Port:
	TCP 135

The **Directory Replication Service (DRS) Remote Protocol** is an [[RPC]] and management of data in [Active Directory](https://learn.microsoft.com/en-us/openspecs/windows_protocols/ms-drsr/e5c2026b-f732-4c9d-9d60-b945c0ab54eb#gt_e467d927-17bf-49c9-98d1-96ddf61ddd90).
# DCSync Attack
To perform this attack, you must have control over an account that has the rights to perform domain replication (a user with the Replicating Directory Changes and Replicating Directory Changes All permissions set). Domain/Enterprise Admins and default domain administrators have this right by default.
## 1. Confirm User's Group Membersip
Can use [[PowerView#[Get-DomainUser](https //powersploit.readthedocs.io/en/latest/Recon/Get-DomainUser/)|Powerview's Get-DomainUser]] to get SID:   
```powershell
PS > Get-DomainUser -Identity adunn | select samaccountname,objectsid,memberof,useraccountcontrol | fl

samaccountname     : adunn
objectsid          : S-1-5-21-3842939050-3880317879-2865463114-1164
memberof           : {CN=VPN Users,OU=Security Groups,OU=Corp,DC=INLANEFREIGHT,DC=LOCAL, CN=Shared Calendar
                     Read,OU=Security Groups,OU=Corp,DC=INLANEFREIGHT,DC=LOCAL, CN=Printer Access,OU=Security
                     Groups,OU=Corp,DC=INLANEFREIGHT,DC=LOCAL, CN=File Share H Drive,OU=Security
                     Groups,OU=Corp,DC=INLANEFREIGHT,DC=LOCAL...}
useraccountcontrol : NORMAL_ACCOUNT, DONT_EXPIRE_PASSWORD
```
## 2. Check for Replication Rights
Can use [[PowerView#[Get-DomainObjectACL](https //powersploit.readthedocs.io/en/latest/Recon/Get-DomainObjectAcl/)|Get-DomainObjectACL]] to list ACLs for the user:
```powershell
PS > $sid= "S-1-5-21-3842939050-3880317879-2865463114-1164"
PS > Get-DomainObjectAcl "DC=inlanefreight,DC=local" -ResolveGUIDs | ? { ($_.ObjectAceType -match 'Replication-Get')} | ?{$_.SecurityIdentifier -match $sid} | select AceQualifier, ObjectDN, ActiveDirectoryRights,SecurityIdentifier,ObjectAceType | fl

... SNIP ...
AceQualifier          : AccessAllowed
ObjectDN              : DC=INLANEFREIGHT,DC=LOCAL
ActiveDirectoryRights : ExtendedRight
SecurityIdentifier    : S-1-5-21-3842939050-3880317879-2865463114-1164
ObjectAceType         : DS-Replication-Get-Changes-In-Filtered-Set

AceQualifier          : AccessAllowed
ObjectDN              : DC=INLANEFREIGHT,DC=LOCAL
ActiveDirectoryRights : ExtendedRight
SecurityIdentifier    : S-1-5-21-3842939050-3880317879-2865463114-1164
ObjectAceType         : DS-Replication-Get-Changes

AceQualifier          : AccessAllowed
ObjectDN              : DC=INLANEFREIGHT,DC=LOCAL
ActiveDirectoryRights : ExtendedRight
SecurityIdentifier    : S-1-5-21-3842939050-3880317879-2865463114-1164
ObjectAceType         : DS-Replication-Get-Changes-All
```
## 3. Extract NTLM Hashes and [[Services/Directory & Identity Services/Kerberos|Kerberos]] Keys
We can use [[SecretsDump]] or [[Mimikatz#DCSync|Mimikatz]] for this