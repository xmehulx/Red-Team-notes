## IPMI2 RAKP HMAC-SHA1
```bash
$ hashcat -m 7300 ipmi.txt -a 3 ?1?1?1?1?1?1?1?1 -1 ?d?u
```

## Generating Rule-based Wordlist
```shell-session
$ hashcat --force password.list -r custom.rule --stdout | sort -u > mut_password.list
```
