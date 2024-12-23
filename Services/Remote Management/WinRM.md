Port: TCP 5985, 5986

Uses #soap and can execute remote code

Tools:
- [[TestWsMan]] cmdlet
- [[Evil-WinRM]]
```shell-session
$ evil-winrm -i 10.129.201.248 -u 'Cry0l1t3' -p 'P455w0rD!'
```