# #CT-logs
Can unveil subdomains associated with old or expired certificates hosting outdated software or configurations.
# Tools
- [crt.sh](https://crt.sh/)
```shell-session
$ curl -s "https://crt.sh/?q=facebook.com&output=json" | jq -r '.[]
 | select(.name_value | contains("dev")) | .name_value' | sort -u
 
*.dev.facebook.com
*.newdev.facebook.com
*.secure.dev.facebook.com
dev.facebook.com
```
- [Censys](https://search.censys.io/)

![[merkle-tree.svg]]
