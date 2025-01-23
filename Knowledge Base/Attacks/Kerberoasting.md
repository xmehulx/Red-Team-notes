---
tags:
  - MicrosoftAD
  - kerberos
---
Generates:
	[4769](https://docs.microsoft.com/en-us/windows/security/threat-protection/auditing/event-4769): A Kerberos service ticket was requested
	[4770](https://docs.microsoft.com/en-us/windows/security/threat-protection/auditing/event-4770): A Kerberos service ticket was renewed

#kerberoasting is a lateral movement/privilege escalation technique in AD environments, targetting [Service Principal Names (SPN)](https://docs.microsoft.com/en-us/windows/win32/ad/service-principal-names) accounts. SPNs are unique identifiers that [[Knowledge Base/Protocols/Kerberos|Kerberos]] uses to map a service instance to a service account in whose context the service is running. Any domain user can request a Kerberos ticket for any service account in the same domain, and also across forest trusts (if authentication is permitted across the trust boundary).

Kerberoasting tools typically request `RC4 encryption` when performing the attack and initiating TGS-REQ requests as they are easier to crack.
# Requirements
1. An account's cleartext password/NTLM hash
2. Shell under domain user account
OR
1. `SYSTEM` shell on a domain-joined host
# The Attack
Depending on our position in a network, this attack can be performed in multiple ways:
- From a non-domain joined Linux host using valid domain user credentials.
- From a domain-joined Linux host as root after retrieving the keytab file.
- From a domain-joined Windows host authenticated as a domain user.
- From a domain-joined Windows host with a shell in the context of a domain account.
- As SYSTEM on a domain-joined Windows host.
- From a non-domain joined Windows host using [runas](https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/cc771525(v=ws.11)) /netonly.

Several tools can be utilized to perform the attack:

- Impacket’s [GetUserSPNs.py](https://github.com/SecureAuthCorp/impacket/blob/master/examples/GetUserSPNs.py) from a non-domain joined Linux host.
- A combination of the built-in setspn.exe Windows binary, PowerShell, and Mimikatz.
- From Windows, utilizing tools such as PowerView, [Rubeus](https://github.com/GhostPack/Rubeus), and other PowerShell scripts.

# Mitigation & Detection
- For non-managed service accounts, set a long and complex password or passphrase that does not appear in any word list.
- Recommended to use [Managed Service Accounts (MSA)](https://techcommunity.microsoft.com/t5/ask-the-directory-services-team/managed-service-accounts-understanding-implementing-best/ba-p/397009), and [Group Managed Service Accounts (gMSA)](https://docs.microsoft.com/en-us/windows-server/security/group-managed-service-accounts/group-managed-service-accounts-overview), which use very complex and rotated passwords (like machine accounts)
- Accounts set up with [[LAPS]].
- Restricting the use of the `RC4`, particularly for Kerberos requests by service accounts.
- Domain Admins and other highly privileged accounts should not be used as SPN accounts (if SPN accounts must exist in the environment).

When Kerberoasting is occurring in the environment, an abnormal number of `TGS-REQ` and `TGS-REP` requests and responses occurs, signaling the use of automated Kerberoasting tools. Domain controllers can be configured to log Kerberos TGS ticket requests by selecting [Audit Kerberos Service Ticket Operations](https://docs.microsoft.com/en-us/windows/security/threat-protection/auditing/audit-kerberos-service-ticket-operations) within Group Policy.