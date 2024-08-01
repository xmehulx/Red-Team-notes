Port: TCP/UDP 3389
Script: `rdp*`
_Note: `--packet-trace`_ for better inspection

>**!!** RDP cookies (`mstshash=nmap`) used by Nmap to interact with the RDP server can be identified by `threat hunters`,Endpoint Detection and Response (`EDR`), etc.

>\> Windows system don't insist but they might accept inadequate encryption via [RDP Security](https://docs.microsoft.com/en-us/openspecs/windows_protocols/ms-rdpbcgr/8e8b2cca-c1fa-456c-8ecb-a82fc60b2322).
>\> Self-signed certificates only warn the client at user-end

# Tools
- [[RDP-Sec-checks]](https://github.com/CiscoCXSecurity/rdp-sec-check) by Cisco CX Security Labs
- [[rdesktop]]
- [[xfreerdp]]
```shell-session
$ xfreerdp /u:'cry0l1t3' /p:'P455w0rd!' /v:10.129.201.248
```