Can be used for #PtH 
## Extract hash from SAM
```shell-session
$ pypykatz registry --sam sam system
```
## Dump LSASS
```shell-session
$ pypykatz lsa minidump /home/peter/Documents/lsass.dmp
```
## Dump #dpapi master key
Pypykatz can extract the DPAPI `masterkey` for the **logged-on user** whose data is present in LSASS process memory. 