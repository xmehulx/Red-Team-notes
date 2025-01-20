---
tags:
  - windows
---
> It is available only if we have Active Directory Domain Services (AD DS) server role installed.

>Needs elevated privilege.

[Dsquery](https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/cc732952(v=ws.11)) can be utilized to find Active Directory objects.

# Example Usage
## User search
```powershell
PS > dsquery user
```
## Computer Search
```powershell
PS > dsquery computer
```
## Wildcard Search
We can use a [dsquery wildcard search](https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/cc754232(v=ws.11)) to view all objects in an OU, for example.
```powershell
PS > dsquery * "CN=Users,DC=INLANEFREIGHT,DC=LOCAL"
```
## Combine queries with [[Services/Directory & Identity Services/LDAP]] search filters
Use [LDAP Object Identifiers (OIDs)](https://ldap.com/ldap-oid-reference-guide/) with proper [User Account Control (UAC) attributes](https://docs.microsoft.com/en-us/troubleshoot/windows-server/identity/useraccountcontrol-manipulate-account-properties) 
- Users with `PASSWD_NOTREQD` flag set in the `userAccountControl` attribute:
```powershell
PS > dsquery * -filter "(&(objectCategory=person)(objectClass=user)(userAccountControl:1.2.840.113556.1.4.803:=32))" -attr distinguishedName userAccountControl
```
- All Domain Controllers in the current domain (limit 5):
```powershell
PS > dsquery * -filter "(userAccountControl:1.2.840.113556.1.4.803:=8192)" -limit 5 -attr sAMAccountName
```
- Disabled account's description
```powershell
PS > dsquery * -Filter "(&(objectCategory=Person)(objectClass=User)(userAccountControl:1.2.840.113556.1.4.803:=2))" -Attr samAccountName description
```


| OID                       | **Description**                                                                                                                                  |
| ------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------ |
| `1.2.840.113556.1.4.803`  | The bit value must match completely to meet the search requirements. Great for matching a singular attribute.                                    |
| `1.2.840.113556.1.4.804`  | The results should show any attribute match if any bit in the chain matches. This works in the case of an object having multiple attributes set. |
| `1.2.840.113556.1.4.1941` | Used to match filters that apply to the Distinguished Name of an object and will search through all ownership and membership entries.            |

