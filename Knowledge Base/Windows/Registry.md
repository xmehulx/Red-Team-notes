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

# Keys
## Installer
Location: `HKCU\SOFTWARE\Policies\Microsoft\Windows\`
Seen above, for #AlwaysInstallElevated privilege
## LSA
Location: `HKLM\System\CurrentControlSet\Control\`
## LocalAccountTokenFilterPolicy
Location: `HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System\`
[[UAC]] limits local users to perform remote administration operations. When the registry is 0, the built-in local admin account (RID-500, "Administrator") is the only local account allowed to perform remote administration tasks. Setting it to 1 will allow other local admins.

>**Note:** There is one exception, if the registry key `FilterAdministratorToken` (disabled by default) is enabled, the RID 500 account (even if it is renamed) is enrolled in UAC protection. This means that remote PTH will fail against the machine when using that account.