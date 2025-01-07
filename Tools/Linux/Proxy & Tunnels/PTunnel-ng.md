---
tags:
  - icmp
---
>NOTE: ICMP must be permitted by firewall.
>Note: Consider the versions of GLIBC, make sure it matches with the one on the target.

ICMP tunneling encapsulate its traffic within the ping echo request and send it to an external server. The external server can validate this traffic and send an appropriate response. [ptunnel-ng](https://github.com/utoni/ptunnel-ng) is useful for data exfiltration and creating pivot tunnels to an external server.
# Installing PTunnel-ng
## Building with Autogen.sh
1. Clone repo and run `autogen.sh`:
```shell-session
$ sudo ./autogen.sh 
```
## Alternative approach of building a static binary
```shell-session
$ sudo apt install automake autoconf -y
$ cd ptunnel-ng/
$ sed -i '$s/.*/LDFLAGS=-static "${NEW_WD}\/configure" --enable-static $@ \&\& make clean \&\& make -j${BUILDJOBS:-4} all/' autogen.sh
$ ./autogen.sh
```
# Example Usage
## Port forwarding
1. [[File Transfer|Transfer]] complete `ptunnel-ng/` to pivot host and run it as server:
```shell-session
ubuntu@WEB01:~/ptunnel-ng/src$ sudo ./ptunnel-ng -r<PIVOT-IP> -R22
```
2. Connect to server from attack host (**through local port 2222**):
```shell-session
$ sudo ./ptunnel-ng -p<PIVOT-IP> -l2222 -r<PIVOT-IP> -R22
```
- Once the tunnel is established, we can connect to target SSH via `localhost:2222`
```shell-session
$ ssh -p 2222 -l ubuntu 127.0.0.1
```

>We should see traffic statistics and session logs through the tunnel.
## Reverse Port Forwarding
We could even use this tunnel and SSH to perform dynamic port forwarding to allow us to use proxychains in various ways.
```shell-session
$ ssh -D 9050 -p 2222 -l ubuntu 127.0.0.1
```
And then use proxychains with Nmap to scan targets on the internal network (172.16.5.x). Based on our discoveries, we can attempt to connect to the target.
```shell-session
$ proxychains nmap -sV -sT 172.16.5.19 -p3389
```