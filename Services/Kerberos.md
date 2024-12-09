Can try `AS-Rep Roasting` attack if any user has `"Do not require Kerberos preauthentication"` 

# Linux
A Linux machine not connected to #MicrosoftAD could use Kerberos tickets in scripts or to authenticate to the network. It is not a requirement to be joined to the domain to use Kerberos tickets from a Linux machine.

In most cases, tickets are stored as [ccache files](https://web.mit.edu/kerberos/krb5-1.12/doc/basic/ccache_def.html), in the location defined in the environment variable `KRB5CCNAME`.

A [keytab](https://kb.iu.edu/d/aumh) is a file containing pairs of Kerberos principals and encrypted keys (derived from the Kerberos password), which can be used to authenticate to remote systems using Kerberos without a password. But 

>**Note:** Modern Windows domains (functional level 2008 and above) use AES encryption by default in normal Kerberos exchanges. If we use a rc4_hmac (NTLM) hash in a Kerberos exchange instead of an aes256_cts_hmac_sha1 (or aes128) key, it may be detected as an "encryption downgrade."
# Tools
- [[Kerbrute]]
- [[DNSchef]] - Point DNS for Kerberos lookups to our server