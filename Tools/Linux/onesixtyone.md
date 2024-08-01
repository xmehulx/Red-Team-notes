`Onesixtyone` can be used to brute-force the names of the community strings since they can be named arbitrarily by the administrator. 

Community strings can be bound to any source.
```bash
$ onesixtyone -c /opt/useful/SecLists/Discovery/SNMP/snmp.txt 10.129.14.128

Scanning 1 hosts, 3220 communities
10.129.14.128 [public] Linux htb 5.11.0-37-generic #41~20.04.2-Ubuntu SMP Fri Sep 24 09:06:38 UTC 2021 x86_64
```