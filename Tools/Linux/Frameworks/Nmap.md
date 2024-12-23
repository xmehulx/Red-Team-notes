When port scanning with Nmap, there are three basic scan types. These are:

- TCP Connect Scans (`-sT`)
	If slow, maybe firewall/IDS? check for #waf-presence 
- SYN "Half-open" Scans (`-sS`)
- UDP Scans (`-sU`)

Additionally there are several less common port scan types:

- TCP Null Scans (`-sN`)
- TCP FIN Scans (`-sF`)
- TCP Xmas Scans (`-sX`)

## SYN "Half-open" Scans (`-sS`)
SYN scans are **often not logged** by applications listening on open ports, as standard practice is to log a connection once it's been fully established. Again, this plays into the idea of SYN scans being stealthy.

SYN scans are **significantly faster** than a standard TCP Connect scan.

>SYN scans are the default scans _if run with sudo permissions_. Otherwise it defaults to the TCP Connect scan.

| Flags                | Description                                          |
| -------------------- | ---------------------------------------------------- |
| `-Pn`                | Disables ICMP Echo requests.                         |
| `-n`                 | Disables DNS resolution.                             |
| `--disable-arp-ping` | Disables ARP ping.                                   |
| `--packet-trace`     | Shows all packets sent and received.                 |
| `--reason`           | Displays the reason a port is in a particular state. |
| `-sV`                | Performs a service scan.                             |

> If filtered port, try `--packet-trace -n --disable-arp-ping -Pn` and if _ICMP unreachable_ then mostly firewall/IDS rejecting the packet.

## UDP Scans (`-sU`)
Only responses when configured to do so. 

Try `-Pn -n --disable-arp-ping --packet-trace --reason`. Only if ICMP unreachable, it is closed, for all else it is _open|filtered_. 

>Nmap usually sends completely empty requests -- just raw UDP packets. But tries to send well formed packets for known UDP ports.

### Generate XML report

```bash
$ nmap ... -oX target.xml
$ xsltproc target.xml -o target.html
```

## Firewall and IDS/IPS Evasion
> Use VPSes to prevent ISP blacklisting.

1. Packet Fragmentation
2. Decoys
3. ACK scan
4. DNS source port
	If port is susceptible to scans from different port, `netcat` via that port can help grabbing banners.

### Connect to Filtered Port
```shell
$ ncat -nv --source-port 53 10.129.2.28 50000
```

### NSE
Categories of libraries include (but not limited to):
- `safe`:- Won't affect the target
- `intrusive`:- Not safe: likely to affect the target  
- `vuln`:- Scan for vulnerabilities
- `exploit`:- Attempt to exploit a vulnerability
- `auth`:- Attempt to bypass authentication for running services (e.g. Log into an FTP server anonymously)
- `brute`:- Attempt to bruteforce credentials for running services
- `discovery`:- Attempt to query running services for further information about the network (e.g. query an SNMP server).

```bash
$ nmap -p 80 --script http-put --script-args http-put.url='/dav/shell.php',http-put.file='./shell.php'
```


# [[Services/Services]]
