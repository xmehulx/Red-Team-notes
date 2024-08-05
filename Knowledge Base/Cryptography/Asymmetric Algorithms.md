# #rsa 
# #self-signed-certificates
```
$ openssl req -x509 -out server.pem -keyout server.pem -newkey <key> -nodes -sha256 -subj '/CN=server'
```
Possible \<keys>:
- rsa:2048
- rsa:4096
- ec:<(openssl ecparam -name secp256r1)
- ec:<(openssl ecparam -name secp384r1)
- etc
