---
tags:
  - linux
  - password-policy
---
The [original tool](https://github.com/CiscoCXSecurity/enum4linux) was written in Perl and [[Enum4Linux-ng]] rewritten in Python. It is built around the [Samba suite of tools](https://www.samba.org/samba/docs/current/man-html/samba.7.html) [[nmblookup]], [[Net]], [[RPCclient]] and [[SMBclient]] to use for enumeration of windows hosts and domains.

# Example Usage
```shell-session
$ enum4linux <IP>
```
## Useful Flags
- `-P`: Password policies