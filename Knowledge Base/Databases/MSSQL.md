Port: TCP 1433, UDP 1434, TCP 2433 (**hidden**)
Default ran as NT SERVICE\\MSSQLSERVER,  without encryption when connecting.
# Authentication Mechanisms
`MSSQL` supports two [authentication modes](https://docs.microsoft.com/en-us/sql/connect/ado-net/sql/authentication-sql-server); users can be created in Windows or in the SQL Server:

| **Authentication Type**       | **Description**                                                                                                                                                                                                                                                                          |
| ----------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `Windows authentication mode` | The default, `integrated` security since SQL Server security model is tightly integrated with Windows/AD. Specific Windows user and group accounts are trusted to log in to SQL Server. Windows users who have already been authenticated do not have to present additional credentials. |
| `Mixed mode`                  | Mixed mode supports authentication by Windows/AD accounts and SQL Server. Username and password pairs are maintained within SQL Server.                                                                                                                                                  |
# Default Database

| Name       | Description                                                                                                                                                                                            |
| ---------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| `master`   | Tracks all system information for an SQL server instance                                                                                                                                               |
| `model`    | Template database that acts as a structure for every new database created. Any setting changed in the model database will be reflected in any new database created after changes to the model database |
| `msdb`     | The SQL Server Agent uses this database to schedule jobs & alerts                                                                                                                                      |
| `tempdb`   | Stores temporary objects                                                                                                                                                                               |
| `resource` | Read-only database containing system objects included with SQL server                                                                                                                                  |

![[mssql.webp]]
Windows Authentication means underlying Windows OS will process the login request and use either the local SAM database or the domain controller (hosting Active Directory) before allowing connectivity to the DBMS. Using #MicrosoftAD can be ideal, but if an account is compromised, it could lead to privilege escalation and lateral movement across a Windows domain environment.
# Execute Commands
`MSSQL` has a [extended stored procedures](https://docs.microsoft.com/en-us/sql/relational-databases/extended-stored-procedures-programming/database-engine-extended-stored-procedures-programming?view=sql-server-ver15) called [xp_cmdshell](https://docs.microsoft.com/en-us/sql/relational-databases/system-stored-procedures/xp-cmdshell-transact-sql?view=sql-server-ver15) which allow us to execute system commands using SQL. Keep in mind the following about `xp_cmdshell`:
- `xp_cmdshell` is disabled by default and can be enabled/disabled by using the [Policy-Based Management](https://docs.microsoft.com/en-us/sql/relational-databases/security/surface-area-configuration) or by executing [sp_configure](https://docs.microsoft.com/en-us/sql/database-engine/configure-windows/xp-cmdshell-server-configuration-option)
- The Windows process spawned by `xp_cmdshell` has the same security rights as the SQL Server service account
- `xp_cmdshell` operates synchronously. Control is not returned to the caller until the command-shell command is completed
# Write Local Files
To write files using `MSSQL`, we need to enable [Ole Automation Procedures](https://docs.microsoft.com/en-us/sql/database-engine/configure-windows/ole-automation-procedures-server-configuration-option), which requires admin privileges, and then execute some stored procedures to create the file:

# Linked Servers
[linked servers](https://docs.microsoft.com/en-us/sql/relational-databases/linked-servers/create-linked-servers-sql-server-database-engine) are typically configured to enable the database engine to execute a Transact-SQL statement that includes tables in another instance of SQL Server, or another database product such as Oracle.