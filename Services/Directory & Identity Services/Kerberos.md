Can try `AS-Rep Roasting` attack if any user has `"Do not require Kerberos preauthentication"` 

>**Note:** Modern Windows domains (functional level 2008 and above) use AES encryption by default in normal Kerberos exchanges. If we use a rc4_hmac (NTLM) hash in a Kerberos exchange instead of an aes256_cts_hmac_sha1 (or aes128) key, it may be detected as an "encryption downgrade."
# Linux
[[realm]], [[winbind]], [[sssd]], [[klist]], [[kinit]] can be used to check if the Linux host is joined to an AD.

- Find keytab files
```shell-session
$ find / -name *keytab* -ls 2>/dev/null
```

## Impersonating with `keytab`
We can use [[kinit]] to impersonate
```shell-session
$ klist
$ kinit carlos@INLANEFREIGHT.HTB -k -t /opt/specialfiles/carlos.keytab
```
>**Note:** To keep the ticket from current session, before importing the keytab, save a copy of the `ccache` file present in the environment variable `KRB5CCNAME`.
## Keytab Extract
To gain access to the system we can crack the account's password by extracting the hashes from `keytab` file using [[KeyTabExtract]]

## Impersonating with `ccache`
For this we can simply set the path of the ccache key as our `KRB5CCNAME` value: 
```shell-session
$ cp /tmp/krb5cc_file . 
$ export KRB5CCNAME=/full/path/to/krb5cc_file
```

![[SMBclient#Using Kerberos Ticket|SMBclient]]

## Tools
- [[Kerbrute]]
- [[DNSchef]] - Point DNS for Kerberos lookups to our server
- [[WMIExec]]


>**Note:** If you are using Impacket tools from a Linux machine connected to the domain, note that some Linux Active Directory implementations use the FILE: prefix in the KRB5CCNAME variable. If this is the case, we need to modify the variable only to include the path to the `ccache` file.