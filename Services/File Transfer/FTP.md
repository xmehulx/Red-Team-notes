# Dangerous Settings
| **Setting**                    | **Description**                                                                    |
| ------------------------------ | ---------------------------------------------------------------------------------- |
| `anonymous_enable=YES`         | Allowing anonymous login?                                                          |
| `anon_upload_enable=YES`       | Allowing anonymous to upload files?                                                |
| `anon_mkdir_write_enable=YES`  | Allowing anonymous to create new directories?                                      |
| `no_anon_password=YES`         | Do not ask anonymous for password?                                                 |
| `anon_root=/home/username/ftp` | Directory for anonymous.                                                           |
| `write_enable=YES`             | Allow the usage of FTP commands: STOR, DELE, RNFR, RNTO, MKD, RMD, APPE, and SITE? |

```shell-session
$ wget -m --no-passive ftp://anonymous:anonymous@10.129.14.136
```

# FTP Bounce Attack
Use Nmap's `-b` flag to use FTP servers to deliver outbound traffic to another device on the network.
```shell-session
$ nmap -Pn -v -n -p80 -b anonymous:<PASSWORD>@<INTERMEDIATE-IP> <TARGET-IP>
```
## Tools
- [[Medusa]]![[Medusa#Bruteforce FTP]]