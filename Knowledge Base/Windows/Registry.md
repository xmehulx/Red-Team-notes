# #AlwaysInstallElevated
**If** these 2 registers are **enabled** (value is **0x1**), then users of any privilege can **install** (execute) `*.msi` files as NT AUTHORITY\\SYSTEM.

```powershell
PS > reg query HKCU\SOFTWARE\Policies\Microsoft\Windows\Installer /v AlwaysInstallElevated
PS > reg query HKLM\SOFTWARE\Policies\Microsoft\Windows\Installer /v AlwaysInstallElevated
```

# Autorun Processes
Check registry to find any autorun applications:
```powershell
> reg query HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Run
```
# Permissions
