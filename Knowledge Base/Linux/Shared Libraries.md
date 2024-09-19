# Static Libraries
## Static Link in Go
```shell-session
$ CGO_ENABLED=0 go build -o chisel ./path_to_chisel
```
## Static Link in C
```shell-session
$ gcc -static -o myprogram myprogram.c
```
# Dynamically Linked Shared Object Libraries
# Tools
- `ldd`
- `objdump`
- `readelf`

Statically link libraries to share across systems without dependency issues.
