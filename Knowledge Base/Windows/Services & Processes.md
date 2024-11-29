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

# Services
## #dpapi 
The Data Protection APIs or [DPAPI](https://docs.microsoft.com/en-us/dotnet/standard/security/how-to-use-data-protection) are used to encrypt and decrypt DPAPI data blobs on a per-user basis for Windows OS features and various third-party applications. Some applications that use DPAPI and for what:

|Applications|Use of DPAPI|
|---|---|
|`Internet Explorer`|Password form auto-completion data (username and password for saved sites).|
|`Google Chrome`|Password form auto-completion data (username and password for saved sites).|
|`Outlook`|Passwords for email accounts.|
|`Remote Desktop Connection`|Saved credentials for connections to remote machines.|
|`Credential Manager`|Saved credentials for accessing shared resources, joining Wireless networks, VPNs and more.|
The DPAPI masterkey can be used to decrypt the secrets associated with different applications using DPAPI and result in the capturing of credentials for various accounts