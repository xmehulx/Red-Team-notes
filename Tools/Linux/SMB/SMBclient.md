# Connect to a Share
## Using Username and Password
```shell-session
$ smbclient -U <user> \\\\<IP>\\<share>
```
## Read Shares Anonymously
```shell-session
$ smbclient -N -L //<IP>/<READABLE-SHARE>
$ smbclient //<IP>/<share> -U ''
```
## Using Kerberos Ticket
If ccache is present and loaded, we can read shares using this ticket.
```shell-session
# cp /tmp/krb5cc_file .
# export KRB5CCNAME=/full/path/to/krb5cc_file
# klist
# smbclient //dc01/<USER> -k -c ls -no-pass
```