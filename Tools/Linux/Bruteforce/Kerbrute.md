---
tags:
  - linux
  - asrep-roasting
---
[Kerbrute](https://github.com/ropnop/kerbrute) is a stealthier option for domain account enumeration. It takes advantage of the fact that [Kerberos Pre-Authentication](https://ldapwiki.com/wiki/Wiki.jsp?page=Kerberos%20Pre-Authentication) failures often will **not** trigger logs or alerts.

# Example Usage
## Enumerate Users
```shell-session
$ kerbrute userenum -d <DOMAIN-NAME> --dc <DC-IP> <USER.LST> -o <OUTPUT.FILE>
```
>This generates Windows code [4768](https://docs.microsoft.com/en-us/windows/security/threat-protection/auditing/event-4768) only if [Kerberos event logging](https://docs.microsoft.com/en-us/troubleshoot/windows-server/identity/enable-kerberos-event-logging) is enabled via Group Policy.

This will also automatically retrieve AS-REP for any user that do not require Kerberos pre-authentication.
## Password Spray
```shell-session
$ kerbrute passwordspray -d <DOMAIN> --dc <IP> <USERS.LST> <PASS>
```
