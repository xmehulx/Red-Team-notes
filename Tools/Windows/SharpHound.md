---
tags:
  - windows
  - c-sharp
---
[SharpHound collector](https://github.com/BloodHoundAD/BloodHound/tree/master/Collectors) (aka `ingestor`) written in C# is part of [[BloodHound]] used for collecting data from AD such as users, groups, computers, group membership, GPOs, ACLs, domain trusts, local admin access, user sessions, computer and user properties, RDP access, WinRM access, etc.
# Example Usage
```powershell
PS > .\SharpHound.exe --help
PS > .\SharpHound.exe -c All --zipfilename ILFREIGHT
```