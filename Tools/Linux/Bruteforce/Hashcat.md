## Generating Rule-based Wordlist
```shell-session
$ hashcat --force password.list -r custom.rule --stdout | sort -u > mut_password.list
```

## IPMI2 RAKP HMAC-SHA1
```bash
$ hashcat -m 7300 ipmi.txt -a 3 ?1?1?1?1?1?1?1?1 -1 ?d?u
```

- NTLM (**-m 1000**)
If we got SAM dump, extract hashes in the format `uid:rid:lmhash:nthash` and save `nthash`
## Bitlocker Cracking
```shell-session
$ hashcat -m 22100 <BITLOCKER#1.HASH> <WORDLIST.txt> -o backup.cracked
```