Port: 1521, 49159 (requires service name)
Config files: `$ORACLE_HOME/network/admin/tnsnames.ora` and `listener.ora`
Nmap script: oracle-sid-brute

Default cred> `dbsnmp:dbsnmp`
Default SIDs> XE, XEXDB

Client uses `tnsnames.ora` to resolve service names to network addresses. The listener process uses `listener.ora` to determine the services it should listen to and the behavior of the listener.

## Tools
- [[odat]].py
```bash
$ ./odat.py all -s 10.129.204.235
```
- sqlplus
```bash
$ sqlplus <user>/<pass>@10.129.204.235/<SID>
```
If the following error comes: `sqlplus: error while loading shared libraries: libsqlplus.so: cannot open shared object file: No such file or directory`, execute the below code
```bash
sudo sh -c "echo /usr/lib/oracle/12.2/client64/lib > /etc/ld.so.conf.d/oracle-instantclient.conf"; sudo ldconfig
```

