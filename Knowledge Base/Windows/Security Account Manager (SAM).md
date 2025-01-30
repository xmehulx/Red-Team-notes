---
tags:
  - windows
  - dcsync
---
Database file that stores users' passwords. Can be used to authenticate local and remote users.

Windows systems can be assigned to either a workgroup or domain during setup. If assigned to a workgroup, it handles the SAM database locally and stores all existing users locally in this database. However, if joined to a domain, the Domain Controller (`DC`) must validate the credentials from the [[Active Directory|AD]] database (`ntds.dit`), which is stored in `%SystemRoot%\ntds.dit`.

# #SAM-registry-hive

If the system is not connected to a domain, we can extract these offline to crack.

|Registry Hive|Description|
|---|---|
|`hklm\sam`|Contains the hashes associated with local account passwords. We will need the hashes so we can crack them and get the user account passwords in cleartext.|
|`hklm\system`|Contains the system bootkey, which is used to encrypt the SAM database. We will need the bootkey to decrypt the SAM database.|
|`hklm\security`|Contains cached credentials for domain accounts. We may benefit from having this on a domain-joined Windows target.|

# #syskey
System key utility (Syskey) protects the SAM database. But if