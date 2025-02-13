---
tags:
  - windows
  - reversible-encryption
  - powershell
---
# Example Usage
## Discover Modules
```powershell
PS C:\htb> Get-Module
```
## Load `ActiveDirectory` Module
```powershell
PS > Import-Module ActiveDirectory
PS > Get-Module
```
## [Get-ADDomain](https://docs.microsoft.com/en-us/powershell/module/activedirectory/get-addomain?view=windowsserver2022-ps)
Provides info like domain SID, domain functional level, any child domains, and more.
```powershell
PS > Get-ADDomain
```
## [Get-ADUser](https://docs.microsoft.com/en-us/powershell/module/activedirectory/get-aduser?view=windowsserver2022-ps)
We want to filter for AD accounts with the `ServicePrincipalName` property populated as they may be susceptible to a Kerberoasting attack.
```powershell
PS > Get-ADUser -Filter {ServicePrincipalName -ne "$null"} -Properties ServicePrincipalName
```
- Check for accounts with reversible encryption enabled:
```powershell
PS > Get-ADUser -Filter 'userAccountControl -band 128' -Properties userAccountControl
```
## [Get-ADTrust](https://docs.microsoft.com/en-us/powershell/module/activedirectory/get-adtrust?view=windowsserver2022-ps)
This verifies domain trust relationships, also determines if they are trusts within our forest or with domains in other forests, the type of trust, the direction of the trust, and the name of the domain the relationship is with. Could be advantageous for child-to-parent trust relationships and attacking across forest trusts.
```powershell
PS > Get-ADTrust -Filter *
```
## [Get-ADGroup](https://docs.microsoft.com/en-us/powershell/module/activedirectory/get-adgroup?view=windowsserver2022-ps)
```powershell
PS > Get-ADGroup -Filter * | select name
PS > Get-ADGroup -Identity "Backup Operators"
PS > Get-ADGroup -Identity "Enterprise Admins" -Server "INLANEFREIGHT.LOCAL"
```
## [Get-ADGroupMember](https://docs.microsoft.com/en-us/powershell/module/activedirectory/get-adgroupmember?view=windowsserver2022-ps)
```powershell
PS > Get-ADGroupMember -Identity "Backup Operators"
```
