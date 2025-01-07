---
tags:
  - rdp
  - windows
  - linux
---
>RDP tunnels have slow performance. Switching `Performance` to `Modem` in Experience tab in `mstsc` might help.
>Turn off "Real-Time Protection" to load the `.dll`

[SocksOverRDP](https://github.com/nccgroup/SocksOverRDP) is a tool that uses `Dynamic Virtual Channels` (`DVC`) from the Remote Desktop Service feature of Windows. DVC is responsible for tunneling packets over the RDP connection. We can use `SocksOverRDP` to tunnel our custom packets and then proxy through it. We can use the tool [[Proxifier]] Portable as our proxy server.
# Example Usage
>Connecting to a remotest Windows system; making initial Windows pivot, the base, the remoter Windows, the pivot to, connect to Remotest Windows system.

Download correct [SocksOverRDP binary](https://github.com/nccgroup/SocksOverRDP/releases), then:
1. [[File Transfer|Transfer]] `SocksOverRDPx64.zip` to the pivot Windows system and load the `.dll`:
```cmd-session
> regsvr32.exe SocksOverRDP-Plugin.dll
```
>Needs `administrator` privilege to load it completely. Otherwise, `.dll` gets loaded but can't be called.
2. From pivot, connect to remoter `172.16.5.19` using `mstsc.exe`. We should receive a prompt that the SocksOverRDP plugin is enabled, and it listens on `127.0.0.1:1080`
3. [[File Transfer|Transfer]] `SocksOverRDPx64.zip` or just the `SocksOverRDP-Server.exe` to `172.16.5.19`. We can then start `SocksOverRDP-Server.exe` with admin privileges (can run without admin as well).
- On initial pivot system(now base Windows), we should see our SOCKS listener on `127.0.0.1:1080`
4. [[File Transfer|Transfer]] Proxifier Portable to the pivot WIndows, and configure it to forward all our packets to `127.0.0.1:1080`
- With Proxifier configured and running, we can start `mstsc.exe`, and it will use Proxifier to pivot all our traffic via `127.0.0.1:1080`, which will tunnel it over RDP to `172.16.5.19`, which will then route it to `172.16.6.155` using `SocksOverRDP-server.exe`.