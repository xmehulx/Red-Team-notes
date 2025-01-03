>**!! NOTE:** This tool **only** works for pivoting over SSH and does not provide other options for pivoting over TOR or HTTPS proxy servers. 

[Sshuttle](https://github.com/sshuttle/sshuttle) is a Python tool which removes the need to configure proxychains. It can be useful for automating the execution of iptables and adding pivot rules for the remote host.

```shell-session
$ sudo sshuttle -r ubuntu@<UBUNTU-IP> <REMOTER-IP>/23 -v

```
This command creates an entry in our `iptables` to redirect all traffic to the 172.16.5.0/23 network through the pivot host.
# Useful Flags
- `-r`: connect to the remote machine with a username and password