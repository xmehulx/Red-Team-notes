# Connect to a Share
## Using Username and Password
```shell-session
$ smbclient -U <user> \\\\<IP>\\<share>
```
## Read Share Anonymously
```shell-session
$ smbclient //<IP>/<READABLE-SHARE> -N
$ smbclient //<IP>/<share> -U 'abc'
```
## Using Kerberos Ticket
If ccache is present and loaded, we can read shares using this ticket.
```shell-session
# cp /tmp/krb5cc_file .
# export KRB5CCNAME=/root/krb5cc_copied_file
# klist
# smbclient //dc01/<USER> -k -c ls -no-pass
```