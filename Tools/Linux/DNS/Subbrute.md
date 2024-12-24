[Subbrute](https://github.com/TheRook/subbrute) allows us to use self-defined resolvers and perform pure DNS brute-forcing attacks during internal penetration tests on hosts that do not have Internet access.
```shell-session
$ echo "ns1.inlanefreight.com" > ./resolvers.txt
$ ./subbrute inlanefreight.com -s ./names.txt -r ./resolvers.txt
```