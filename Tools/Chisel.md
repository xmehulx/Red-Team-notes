>Mindful of file size when transferring (could be **DETECTED!**). Refer 24:29 of [video](https://www.youtube.com/watch?v=Yp4oxoQIBAM&t=1469s) to shrink its size.
>By default, `chisel` directs to #SOCKS proxy on `localhost:1080`.

[Chisel](https://github.com/jpillora/chisel) is a TCP/UDP-based tunneling tool written in Go that uses HTTP to transport data that is secured using SSH. It can create a client-server tunnel connection in a firewall restricted environment.
# Example Usage
Example scenario for port forwarding.
1. Run `chisel server` on pivot host:
```shell-session
$ ./chisel server -p 1234 --socks5 -v 
```
This will forward all packets to **all networks accessible** to the pivot host.
2. Connect with `chisel client` on attacking machine:
```shell-session
$ ./chisel client -v <PIVOT-IP>:1234 socks
```
3. Modify #proxychains config file to forward packets to this port:
```shell-session
$ tail -1 /etc/proxychains.conf 
socks5 127.0.0.1 1080
```
- Now running `proxychains <COMMAND>` will transfer it to the remote system and run as if ran on that system primarily.