---
tags:
  - basic-http-auth
---
# Basic HTTP Authentication
```shell-session
$ hydra -C /path/to/creds <IP> -s <PORT> http-get /
```

# HTTP POST authentication
Use `-U` flag to learn more about `http-post-form` syntax.
```bash
"/login.php:[user parameter]=^USER^&[password parameter]=^PASS^:F=<form name='login'"
```
