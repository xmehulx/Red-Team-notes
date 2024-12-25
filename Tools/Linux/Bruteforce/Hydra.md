---
tags:
  - basic-http-auth
---
# Important Flags
- `-u`: Try all users for 1 password first
- `-f`: Stop after first success
# Bruteforcing Protocols
## HTTP
### Basic HTTP Authentication
```shell-session
$ hydra -C /path/to/creds <IP> -s <PORT> http-get /
```
### HTTP POST authentication
Use `-U` flag to learn more about `http-post-form` syntax.
```bash
"/login.php:[user parameter]=^USER^&[password parameter]=^PASS^:F=<form name='login'"
```
## SMB/FTP/SSH/POP3/SMTP
>**NOTE:** Only SMBv1 supported!
```shell-session
$ hydra -f <smb/ftp/ssh/pop3/smtp>://<IP> -l <user> -P </path/to/password> -t 4
```
>Don't use `t>4` for SSH as it gets dropped

