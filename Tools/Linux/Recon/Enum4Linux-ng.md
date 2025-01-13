---
tags:
  - linux
  - password-policy
---
[Enum4Linux-ng](https://github.com/cddmp/enum4linux-ng) is a rewrite of [[Enum4Linux]] in Python, with additional features such as YAML or JSON file export for further processing, colored output, etc.
# Example Usage
```shell-session
$ enum4linux-ng 172.16.5.5 -oA ilfreight
```
## Useful Flags
- `-P`: Extract Password policy
- `-oA`: Save output in all JSON and YAML format