# Cracking Files
## OpenSSL Encrypted Files
```shell-session
$ file GZIP.gzip 
GZIP.gzip: openssl enc'd data with salted password
$ for i in $(cat rockyou.txt);do openssl enc -aes-256-cbc -d -in GZIP.gzip -k $i 2>/dev/null| tar xz;done
```

## Protected Files
For protected files like SSH keys, doc files, ZIP archives, etc, [[John the Ripper]] can be used.