---
tags: []
---
# #MicrosoftAD 
#### AdminSDHolder
The [AdminSDHolder](https://docs.microsoft.com/en-us/windows-server/identity/ad-ds/plan/security-best-practices/appendix-c--protected-accounts-and-groups-in-active-directory) object is used to manage ACLs for members of built-in groups in AD marked as privileged.

#### adminCount
The [adminCount](https://docs.microsoft.com/en-us/windows/win32/adschema/a-admincount) attribute determines whether or not the SDProp process protects a user. If the value is set to `0` or not specified, the user is not protected. If the attribute value is set to `value`, the user is protected.


# ADFS
## Endpoints
- `/adfs/ls`: responsible for handling browser-based authentication flows
- `/adfs/services/trust/2005/usernamemixed`: Same as above, in legacy.

## Authentication
If sent in `multipart/form-data`, try to see if wildcard(\*) creds accepted.
## Tools
- `Get-AdfsEndpoint`

# #password-policy
The default password policy when a new domain is created is as follows, and there have been plenty of organizations that never changed this policy:

|Policy|Default Value|
|---|---|
|Enforce password history|24 days|
|Maximum password age|42 days|
|Minimum password age|1 day|
|Minimum password length|7|
|Password must meet complexity requirements|Enabled|
|Store passwords using reversible encryption|Disabled|
|Account lockout duration|Not set|
|Account lockout threshold|0|
|Reset account lockout counter after|Not set|
