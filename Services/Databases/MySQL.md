Port: 3306
Attacks: #SQLinjection
Config file: `/etc/mysql/mysql.conf.d/mysqld.cnf` 
Nmap script: _mysql*_

```bash
cat /etc/mysql/mysql.conf.d/mysqld.cnf | grep -v "#" | sed -r '/^\s*$/d'
```
## Cheatsheet

```mysql
SHOW databases / tables;
USE <database>;
DESCRIBE <table>;
SELECT * FROM <table> WHERE <column> = "<string>";
$ mysqldump --host=10.0.0.27 [tablename]
select version();
```

### Dangerous Settings

| **Settings**       | **Description**                                                                                              |
| ------------------ | ------------------------------------------------------------------------------------------------------------ |
| `user`             | Sets which user the MySQL service will run as.                                                               |
| `password`         | Sets the password for the MySQL user.                                                                        |
| `admin_address`    | The IP address on which to listen for TCP/IP connections on the administrative network interface.            |
| `debug`            | This variable indicates the current debugging settings                                                       |
| `sql_warnings`     | This variable controls whether single-row INSERT statements produce an information string if warnings occur. |
| `secure_file_priv` | This variable is used to limit the effect of data import and export operations.                              |
> `system schema` (`sys`) and `information schema` (`information_schema`) are most important databases.


# Tools
- [[MySQL - tool]]