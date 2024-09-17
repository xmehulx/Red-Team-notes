Database file that stores users' passwords. Can be used to authenticate local and remote users.

Windows systems can be assigned to either a workgroup or domain during setup. If assigned to a workgroup, it handles the SAM database locally and stores all existing users locally in this database. However, if joined to a domain, the Domain Controller (`DC`) must validate the credentials from the [[Active Directory|AD]] database (`ntds.dit`), which is stored in `%SystemRoot%\ntds.dit`.

