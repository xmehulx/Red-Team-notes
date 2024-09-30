# Connect to a Share
```shell-session
$ smbclient -U <user> \\\\<IP>\\<share>
```
## Read Shares Anonymously
```shell-session
$ smbclient //<IP>/<share> -U 'abc'
```

