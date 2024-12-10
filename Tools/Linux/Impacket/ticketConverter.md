---
tags:
  - PtT
---
It is used to used to convert a `ccache file` in Windows or a `kirbi file` in a Linux machine to be used for authentication
```shell-session
$ impacket-ticketConverter krb5cc_647401106_I8I133 <USER.kirbi>
```
- In Windows:
```cmd-session
> .\Rubeus.exe ptt /ticket:c:\tools\user.kirbi
```