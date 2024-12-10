# #PtT 
# Windows
Tickets are processed and stored by the #lsass. The tickets that end with `$` correspond to the computer account, which needs a ticket to interact with the Active Directory. User tickets have the user's name, followed by an `@` that separates the service name and the domain, for example: `[randomvalue]-username@service-domain.local.kirbi`.

>**Note:** If you pick a ticket with the service krbtgt, it corresponds to the TGT of that account.

>**Note:** At the time of writing, using Mimikatz version 2.2.0 20220919, if we run "`sekurlsa::ekeys`" it presents all hashes as `des_cbc_md4` on some Windows 10 versions. Exported tickets (`sekurlsa::tickets /export`) do not work correctly due to the wrong encryption. It is possible to use these hashes to generate new tickets or use Rubeus to export tickets in base64 format.
# Linux
A Linux machine not connected to #MicrosoftAD could use Kerberos tickets in scripts or to authenticate to the network. It is not a requirement to be joined to the domain to use Kerberos tickets from a Linux machine.

In most cases, tickets are stored as [ccache files](https://web.mit.edu/kerberos/krb5-1.12/doc/basic/ccache_def.html), in the location defined in the environment variable `KRB5CCNAME`.

A [keytab](https://kb.iu.edu/d/aumh) is a file containing pairs of Kerberos principals and encrypted keys (derived from the Kerberos password), which can be used to authenticate to remote systems using Kerberos without a password. But when changing passwords, all the keytab files must be recreated. Keytabs commonly allow scripts 