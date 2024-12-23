Port: 110/143 or 993/995 (+TLS)
> - Unencrypted by default.
> - Check if [[SNMP]] present. They might be managing something.
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

## Navigation
```IMAP
<seq> LOGIN <USER> <PASS>
<seq> LIST "" *
<seq> SELECT <INBOX>
<seq> STATUS <INBOX> (MESSAGES)
<seq> fetch <#>:<#> (BODY[HEADER]) 
<seq> fetch <#> (BODY[n])
```

## Tools
- [[Evolution]]