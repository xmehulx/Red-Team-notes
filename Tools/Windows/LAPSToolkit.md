The [LAPSToolkit](https://github.com/leoloobeek/LAPSToolkit) greatly facilitates enumerating which domain users can read the LAPS password set for machines with LAPS installed and which machines do not have LAPS installed. One is parsing `ExtendedRights` for all computers with LAPS enabled which shows groups specifically delegated to read LAPS passwords, which are often users in protected groups. An account that has joined a computer to a domain receives `All Extended Rights` over that host, and this right gives the account the ability to read passwords. Enumeration may show a user account that can read the LAPS password on a host. This can help us target specific AD users who can read LAPS passwords.
# Functions
## `Find-LAPSDelegatedGroups`
```powershell
PS C:\htb> Find-LAPSDelegatedGroups

OrgUnit                                             Delegated Groups
-------                                             ----------------
OU=Servers,DC=INLANEFREIGHT,DC=LOCAL                INLANEFREIGHT\Domain Admins
OU=Servers,DC=INLANEFREIGHT,DC=LOCAL                INLANEFREIGHT\LAPS Admins
OU=Workstations,DC=INLANEFREIGHT,DC=LOCAL           INLANEFREIGHT\Domain Admins
OU=Workstations,DC=INLANEFREIGHT,DC=LOCAL           INLANEFREIGHT\LAPS Admins
... SNIP ...
```

## `Find-AdmPwdExtendedRights`
This checks the rights on each computer with LAPS enabled for any groups with read access and users with "All Extended Rights." Users with "All Extended Rights" can read LAPS passwords and may be less protected than users in delegated groups.
```powershell
PS > Find-AdmPwdExtendedRights

ComputerName                Identity                    Reason
------------                --------                    ------
EXCHG01.INLANEFREIGHT.LOCAL INLANEFREIGHT\Domain Admins Delegated
EXCHG01.INLANEFREIGHT.LOCAL INLANEFREIGHT\LAPS Admins   Delegated
... SNIP ...
```
## `Get-LAPSComputers`
It searches for computers that have LAPS enabled when passwords expire, and even the randomized passwords in cleartext if our user has access.
```powershell
> Get-LAPSComputers

ComputerName                Password       Expiration
------------                --------       ----------
DC01.INLANEFREIGHT.LOCAL    6DZ[+A/[]19d$F 08/26/2020 23:29:45
EXCHG01.INLANEFREIGHT.LOCAL oj+2A+[hHMMtj, 09/26/2020 00:51:30
... SNIP ...
```