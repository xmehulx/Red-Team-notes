[Netsh](https://docs.microsoft.com/en-us/windows-server/networking/technologies/netsh/netsh-contexts) can help with the network configuration of a particular Windows system and networking related tasks such as:
- Finding routes
- Viewing the firewall configuration
- Adding proxies
- Creating port forwarding rules
# Port Forwarding
If the initial compromised host is a Windows system connected to deeper networks, we can use this system as pivot to dive deeper in the network. ![[88.webp]]
```cmd-session
> netsh.exe interface portproxy add v4tov4 listenport=8080 listenaddress=10.129.15.150 connectport=3389 connectaddress=172.16.5.25

> netsh.exe interface portproxy show v4tov4
```
Now we can simply `xfreerdp` into remoter Windows server through `10.129.15.150:8080`

# Firewall Checks
```powershell
PS > netsh advfirewall show allprofiles
```