```shell-session
$ mysql -u <user> -p [IP address]
```

>**!!** When port forwarding to use MySQL, use `-h 127.0.0.1` to force the system to use TCP socket instead of UNIX. This will use the TCP forwarded port, for example with `ssh -L`.

