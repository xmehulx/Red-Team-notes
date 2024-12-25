Port: 
	110 (POP3 unencrypted)
	143 (IMAP4 unencrypted)
	993 (POP3 encrypted)
	995 (IMAP4 encrypted +TLS)
> - Unencrypted by default.
> - Check if [[SNMP]] present. They might be managing something.

Nowadays, most organizations use 3rd-party cloud services.
```TXT
hackthebox.eu mail is handled by 1 aspmx.l.google.com.
microsoft.com mail is handled by 10 microsoft-com.mail.protection.outlook.com.
plaintext.do.           7076    IN      MX      50 mx3.zoho.com.
inlanefreight.com.      300     IN      MX      10 mail1.inlanefreight.com.
```
# POP3
>Servers **usually** deletes email after getting downloaded on a client, but can be configured to not do so.

# Misconfiguration
We can use the `POP3` protocol to enumerate users depending on the service implementation.
## `User`
To check if a user exists or not.
```shell-session
$ telnet 10.10.110.20 110

+OK POP3 Server ready

USER julio
-ERR

USER john
+OK
```
## Dangerous Setting
|**Setting**|**Description**|
|---|---|
|`auth_debug`|Enables all authentication debug logging.|
|`auth_debug_passwords`|This setting adjusts log verbosity, the submitted passwords, and the scheme gets logged.|
|`auth_verbose`|Logs unsuccessful authentication attempts and their reasons.|
|`auth_verbose_passwords`|Passwords used for authentication are logged and can also be truncated.|
|`auth_anonymous_username`|This specifies the username to be used when logging in with the ANONYMOUS SASL mechanism.|
### Curl
```bash
curl -k 'imaps://10.129.14.128' --user user:p4ssw0rd -v
```
Unencrypted
### OpenSSL
```bash
openssl s_client -connect 10.129.14.128:[pop3s/imaps]
```

# Navigation
## IMAPS
```IMAP
<seq> LOGIN <USER> <PASS>
<seq> LIST "" *
<seq> SELECT <INBOX>
<seq> STATUS <INBOX> (MESSAGES)
<seq> fetch <#>:<#> (BODY[HEADER]) 
<seq> fetch <#> (BODY[n])
```
## POP3
```POP3
USER <username>
PASS <password>
STAT
LIST
RETR <#>
QUIT
```
## Tools
- [[Dig]]
```shell-session
$ dig mx inlanefreight.com | grep "MX" | grep -v ";"
```
- [[Host]]
```shell-session
$ host -t MX hackthebox.eu
$ host -t A mail1.inlanefreight.htb.
```
- [[Evolution]]