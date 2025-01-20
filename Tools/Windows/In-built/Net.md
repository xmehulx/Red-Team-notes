---
tags:
  - windows
  - password-policy
  - non-evasive
  - evasive
---
>**!! NOTE:** Usually monitored by EDR Solutions. Perform with `net1` to circumvent!
# Account
>This command is only used on local computer and can't be used on domain controller.

The "[Net]() Accounts" command is used to set the policy settings on local computer, such as Account policies and password policies. 
```cmd-session
> net accounts
```
Can give us password policy info
#### Table of Useful Net Commands

| **Command**            | **Description**                         |
| ---------------------- | --------------------------------------- |
| `net accounts`         | Information about password requirements |
| `net accounts /domain` | Password and lockout policy             |

# [Use](https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/gg651155(v=ws.11))
`net use` connects/disconnects a computer to/from a shared resource or displays information about computer connections after mapping it to a drive letter.
### Null Session
```cmd-session
> net use \\DC01\ipc$ "" /u:""
```

| **Command**                                     | **Description**                                                                                                              |
| ----------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------- |
| `net use x: \computer\share`                    | Mount the share locally                                                                                                      |
# [Group](https://learn.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/cc754051(v=ws.11))
Adds, displays, or modifies global groups in domains.

| **Command**                              | **Description**                         |
| ---------------------------------------- | --------------------------------------- |
| `net group /domain`                      | Information about domain groups         |
| `net group "Domain Admins" /domain`      | List users with domain admin privileges |
| `net group "domain computers" /domain`   | List of PCs connected to the domain     |
| `net group "Domain Controllers" /domain` | List PC accounts of domains controllers |
| `net group <domain_group_name> /domain`  | User that belongs to the group          |
| `net groups /domain`                     | List of domain groups                   |
# [Local Group](https://learn.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/cc725622(v=ws.11))
Adds, displays, or modifies local groups.

| **Command**                                     | **Description**                                                                                                              |
| ----------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------- |
| `net localgroup`                                | All available groups                                                                                                         |
| `net localgroup administrators /domain`         | List users that belong to the administrators group inside the domain (the group `Domain Admins` is included here by default) |
| `net localgroup Administrators`                 | Information about a group (admins)                                                                                           |
| `net localgroup administrators [username] /add` | Add user to administrators                                                                                                   |
# [Share](https://learn.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh750728(v=ws.11))
Manages shared resources. Used without parameters, **net share** displays information about all of the resources that are shared on the local computer. For each resource, the device name(s) or pathname(s) and a descriptive comment are displayed.

| **Command**                          | **Description**                                |
| ------------------------------------ | ---------------------------------------------- |
| `net share`                          | Check current shares                           |
# [User](https://learn.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/cc771865(v=ws.11))
Adds or modifies user accounts, or displays user account information.

| **Command**                          | **Description**                                |
| ------------------------------------ | ---------------------------------------------- |
| `net user <ACCOUNT_NAME> /domain`    | Get information about a user within the domain |
| `net user /domain`                   | List all users of the domain                   |
| `net user %username%`                | Information about the current user             |
# [View](https://learn.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/cc771865(v=ws.11))
Displays a list of domains, computers, or resources that are being shared by the specified computer.

| **Command**                                     | **Description**                                                                                                              |
| ----------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------- |
| `net view`                                      | Get a list of computers                                                                                                      |
| `net view /all /domain[:domainname]`            | Shares on the domains                                                                                                        |
| `net view \computer /ALL`                       | List shares of a computer                                                                                                    |
| `net view /domain`                              | List of PCs of the domain                                                                                                    |