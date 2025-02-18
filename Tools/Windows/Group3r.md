[Group3r](https://github.com/Group3r/Group3r) is a tool purpose-built to find vulnerabilities in AD associated Group Policy. It must be run from a domain-joined host with a domain user (administrator is not necessary), or in the context of a domain user (i.e., using `runas /netonly`).
```cmd-session
> group3r.exe -f <filepath-name.log> 
```