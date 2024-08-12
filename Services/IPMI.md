---
tags:
  - Critical
---
Port: UDP 623
Script: ipmi-version

Used when: 
- Before the OS has booted to modify BIOS settings
- When the host is fully powered down
- Access to a host after a system failure

Components:
- Baseboard Management Controller (BMC) - A micro-controller and essential component of an IPMI
- Intelligent Chassis Management Bus (ICMB) - An interface that permits communication from one chassis to another
- Intelligent Platform Management Bus (IPMB) - extends the BMC
- IPMI Memory - stores things such as the system event log, repository store data, and more
- Communications Interfaces - local system interfaces, serial and LAN interfaces, ICMB and PCI Management Bus

## Default Creds
|Product|Username|Password|
|---|---|---|
|Dell iDRAC|root|calvin|
|HP iLO|Administrator|randomized 8-character string consisting of numbers and uppercase letters|
|Supermicro IPMI|ADMIN|ADMIN|

## Attacks
Exploit [flaw](http://fish2.com/ipmi/remote-pw-cracking.html) in the RAKP protocol in IPMI 2.0 to grab client's password hash during authentication with [[Tools/Frameworks/Metasploit#IPMI Hash Dump]]. Use with [[Hashcat#IPMI2 RAKP HMAC-SHA1]].
