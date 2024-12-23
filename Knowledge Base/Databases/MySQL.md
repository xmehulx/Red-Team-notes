Port: 3306
Attacks: #SQLinjection
# Default Database
- `mysql` - is the system database that contains tables that store information required by the MySQL server
- `information_schema` - provides access to database metadata
- `performance_schema` - a feature for monitoring MySQL Server execution at a low level
- `sys` - a set of objects for DBAs and developers to interpret data collected by the Performance Schema

A global system variable `secure_file_priv` may be set as follows:

- Empty: the variable has no effect, which is not a secure setting.
- Directory Name: the server limits import and export operations to work only with files in that directory. The directory must exist; the server does not create it.
- `NULL`: the server disables import and export operations.