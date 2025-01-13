---
tags:
  - windows
  - password-policy
---
>This command is only used on local computer and can't be used on domain controller.

The "[Net]() Accounts" command is used to set the policy settings on local computer, such as Account policies and password policies. 
## Account
```cmd-session
> net accounts
```
Can give us password policy info
## Use
[net use](https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/gg651155(v=ws.11)) connects/disconnectes a computer to/from a shared resource or displays information about computer connections after mapping it to a drive letter.
## Example Usage
### Null Session
```cmd-session
> net use \\DC01\ipc$ "" /u:""
```