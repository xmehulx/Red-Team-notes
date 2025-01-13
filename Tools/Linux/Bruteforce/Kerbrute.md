[Kerbrute](https://github.com/ropnop/kerbrute) is a stealthier option for domain account enumeration. It takes advantage of the fact that Kerberos pre-authentication failures often will **not** trigger logs or alerts.

# Example Usage
```shell-session
$ kerbrute userenum -d <DOMAIN-NAME> --dc <DC-IP> <USER.LST> -o <OUTPUT.FILE>
```