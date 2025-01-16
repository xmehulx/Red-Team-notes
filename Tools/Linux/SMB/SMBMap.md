```shell-session
$ smbmap -H 10.129.14.128 [-u <USER> -p <PASSWORD>] [-d <DOMAIN>]
```
# Useful Flags
- `-r/-R <SHARE> [--dir-only]`: Recursively browse share \[only directories\]
- `--download "<FILE>"`
- `--upload <FILE> "<LOCATION\FILENAME>"`