1. Edit the `/etc/ettercap/etter.dns` file to map the target domain name (e.g., `inlanefreight.com`) that we want to spoof and the attacker's IP address (e.g., `192.168.1.110`) that we want to redirect a user to:
```shell-session
$ cat /etc/ettercap/etter.dns

inlanefreight.com      A   192.168.1.110
*.inlanefreight.com    A   192.168.1.110
```
2. Start the `Ettercap` tool and scan for live hosts within the network by navigating to `Hosts > Scan for Hosts`. Once completed, add the target IP address (e.g., `192.168.152.129`) to Target1 and add a default gateway IP (e.g., `192.168.152.2`) to Target2. ![[target.webp]]
3. Activate `dns_spoof` attack by navigating to `Plugins > Manage Plugins`. This sends the target machine with fake DNS responses that will resolve `inlanefreight.com` to IP address `192.168.1.110`:![[etter_plug.webp]]
4. This should redirect `inlanefreight.com` users to our fake page hosted on `192.168.1.110`. And even pings should resolve to our IP address.