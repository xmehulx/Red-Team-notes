# Processes
## Autorun Processes
```powershell
> reg query HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Run
```
## Important Processes
### #lsass
**Local Security Authority Subsystem Service** stores credentials that have active logon sessions\
It uses #msv (or WDigest in older systems) authentication package. LSASS caches `passwords`, `ekeys`, `tickets`, and `pins` associated with Kerberos
# Permission
## #service_change_config
This grants the caller the right to change the executable file that the system runs.

# Properties
## BINARY_PATH_NAME ( #unquoted-service-path)
