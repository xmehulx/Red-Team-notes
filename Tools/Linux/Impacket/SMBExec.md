A similar approach to [[SMBExec]] without using [RemComSvc](https://github.com/kavika13/RemCom). The technique's implementation goes one step further, with instantiating a local SMB server to receive the output of the commands. This is useful when the target machine does NOT have a writeable share available.
```shell-session
$ impacket-smbexec <USER>:'<PASSWORD>'@<IP>
```