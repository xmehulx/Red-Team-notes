[Windapsearch](https://github.com/ropnop/windapsearch) is a Python script to enumerate users, groups, and computers from a Windows domain by utilizing LDAP queries.
# Example Usage
## Enumerate Users
Can use anonymous access:
```shell-session
$ ./windapsearch.py --dc-ip <IP> -u "" -U
```
## Enumerate Domain Admins
```shell-session
$ python3 windapsearch.py --dc-ip <IP> -u <USER>@<DOMAIN> -p <PASS> --da
```
# Useful Flags
- `--da`: enumerate domain admins group members.
- `-PU`: find privileged users (performs recursive search for users with nested group membership). 