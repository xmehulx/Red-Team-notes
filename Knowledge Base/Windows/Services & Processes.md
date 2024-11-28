# Processes
## Autorun Processes
```powershell
> reg query HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Run
```
## Important Processes
### #lsass
**Local Security Authority Subsystem Service** stores credentials that have active logon sessions\
It uses #msv (or WDigest in older systems) authentication package. LSASS caches `passwords`, `ekeys`, `tickets`, and `pins` associated with Kerberos

Upon initial logon, LSASS will:
- Cache credentials locally in memory
- Create [access tokens](https://docs.microsoft.com/en-us/windows/win32/secauthz/access-tokens)
- Enforce security policies
- Write to Windows [security log](https://docs.microsoft.com/en-us/windows/win32/eventlog/event-logging-security)
# Permission
## #service_change_config
This grants the caller the right to change the executable file that the system runs.

# Properties
## BINARY_PATH_NAME ( #unquoted-service-path)
