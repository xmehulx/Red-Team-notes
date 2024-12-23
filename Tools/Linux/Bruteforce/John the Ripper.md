# SSH keys
```shell-session
$ ssh2john <id_rsa> > <mediary_file>
$ john --wordlist=word.list <mediary_file>
```

# Cracking Files
| **Tool**                | **Description**                               |
| ----------------------- | --------------------------------------------- |
| `pdf2john`              | Converts PDF documents for John               |
| `ssh2john`              | Converts SSH private keys for John            |
| `mscash2john`           | Converts MS Cash hashes for John              |
| `keychain2john`         | Converts OS X keychain files for John         |
| `rar2john`              | Converts RAR archives for John                |
| `pfx2john`              | Converts PKCS#12 files for John               |
| `truecrypt_volume2john` | Converts TrueCrypt volumes for John           |
| `keepass2john`          | Converts KeePass databases for John           |
| `vncpcap2john`          | Converts VNC PCAP files for John              |
| `putty2john`            | Converts PuTTY private keys for John          |
| `zip2john`              | Converts ZIP archives for John                |
| `hccap2john`            | Converts WPA/WPA2 handshake captures for John |
| `office2john`           | Converts MS Office documents for John         |
| `wpa2john`              | Converts WPA/WPA2 handshakes for John         |
| `bitlocker2john`        | Converts Bitlocker encrypted drives for John  |
# Modules
## `bitlocker2john`
### 1. Get image of the encrypted memory device
If you already have the encrypted `.vhd`, skip to step 2.
```shell-session
$ sudo dd if=/dev/disk2 of=/path/to/imageEncrypted conv=noerror,sync
```
### 2. Extract the hash and Attack
```shell-session
$ bitlocker2john -i /path/to/imageEncrypted
$ john --format=bitlocker-opencl --wordlist=wordlist <HASH-1>
```
>It gives 4 different hashes, the first one being the Bitlocker password,

If the device was encrypted using the User Password authentication method, bitlocker2john prints those 2 hashes:

- $bitlocker\$0\$… : it starts the User Password fast attack mode (see [User Password Section](https://openwall.info/wiki/john/OpenCL-BitLocker#User-Password-authentication-method "john:OpenCL-BitLocker ↵"))    
- $bitlocker\$1\$… : it starts the User Password attack mode with MAC verification (slower execution, no false positives)    
In any case, `bitlocker2john` prints 2 extra hashes:
- $bitlocker\$2\$… : it starts the Recovery Password fast attack mode (see [Recovery Password Section](https://openwall.info/wiki/john/OpenCL-BitLocker#Recovery-Password-authentication-method "john:OpenCL-BitLocker ↵"))
- $bitlocker\$3\$… : it starts the Recovery Password attack mode with MAC verification (slower execution, no false positives)

[[Hashcat#Bitlocker Cracking|Hashcat]] can also be used for this.

>Post cracking, mount the drive in Windows (requires administrator privileges) or Linux.