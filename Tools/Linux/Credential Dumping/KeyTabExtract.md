[KeyTabExtract](https://github.com/sosdave/KeyTabExtract) is a tool to extract information from 502-type `.keytab` files, which may be used to authenticate Linux boxes to Kerberos. This extracts information such as the realm, Service Principal, Encryption Type, and Hashes.

```shell-session
$ python3 /opt/keytabextract.py /opt/specialfiles/<USER.keytab>
```

These hashes can be used for #PtH or #PtT attacks