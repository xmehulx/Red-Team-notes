This utility lets us enter Transact-SQL statements, system procedures, and script files through various modes:
- At the command prompt.
- In Query Editor in SQLCMD mode.
- In a Windows script file.
- In an operating system (Cmd.exe) job step of a SQL Server Agent job.

```cmd-session
> sqlcmd -S <IP> -U <USER> -P <PASSWORD>
```