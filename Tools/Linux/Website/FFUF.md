```shell-session
$ ffuf -w /usr/share/wordlists/amass/subdomains.lst -u https://napper.htb -H "HOST: FUZZ.napper.htb" -fl 187
```
`-fl` to filter out non-matching results of length 187 (to only show matching results)

# Fuzz with a request packet
```shell-session
$ ffuf -request <request_file> -request-proto http -mode clusterbomb -w wordlist1:<FUZZ1> -w wordlist2:<FUZZ2> -mc 200
```
