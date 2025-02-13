# #sid-abuse
The [sidHistory](https://docs.microsoft.com/en-us/windows/win32/adschema/a-sidhistory) attribute is used in migration scenarios. If a user in one domain is migrated to another, a new account is created in the second one. The original user's SID is added to the new user's SID history, ensuring that the user can still access resources in the original domain. 

If the SID of a Domain Admin is added to the SID History, then this account will be able to perform DCSync and create a [Golden Ticket](https://attack.mitre.org/techniques/T1558/001/) or a Kerberos ticket-granting ticket (TGT), which will allow for us to authenticate as any account in the domain of our choosing for further persistence.
# Requirements
To perform this attack after compromising a child domain, we need the following:
- The KRBTGT hash for the child domain
- The SID for the child domain
- The name of a target user in the child domain (does not need to exist!)
- The FQDN of the child domain.
- The SID of the Enterprise Admins group of the root domain.

With this data collected, the attack can be performed with Mimikatz.