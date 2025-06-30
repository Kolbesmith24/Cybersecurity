# Databases
Store various content and information related to the web application. This can be core web application assets like images and files, web application content like posts and updates, or user data like usernames and passwords. This allows web applications to easily and quickly store and retrieve data and enable dynamic content that is different for each user.
## Relational (SQL)
[Relational](https://en.wikipedia.org/wiki/Relational_database) (SQL) databases store their data in tables, rows, and columns. Each table can have unique keys, which can link tables together and create relationships between tables.

For example, we can have a `users` table in a relational database containing columns like `id`, `username`, `first_name`, `last_name`, and so on. The `id` can be used as the table key. Another table, `posts`, may contain posts made by all users, with columns like `id`, `user_id`, `date`, `content`, and so on.

![Database schema with 'users' table showing IDs, usernames, first and last names, and 'posts' table showing post IDs, user IDs, dates, and content.](https://academy.hackthebox.com/storage/modules/75/web_apps_relational_db.jpg)

We can link the `id` from the `users` table to the `user_id` in the `posts` table to easily retrieve the user details for each post, without having to store all user details with each post.

A table can have more than one key, as another column can be used as a key to link with another table.

`The relationship between tables within a database is called a Schema.`

Some of the most common relational databases include:

|Type|Description|
|---|---|
|[MySQL](https://en.wikipedia.org/wiki/MySQL)|The most commonly used database around the internet. It is an open-source database and can be used completely free of charge|
|[MSSQL](https://en.wikipedia.org/wiki/Microsoft_SQL_Server)|Microsoft's implementation of a relational database. Widely used with Windows Servers and IIS web servers|
|[Oracle](https://en.wikipedia.org/wiki/Oracle_Database)|A very reliable database for big businesses, and is frequently updated with innovative database solutions to make it faster and more reliable. It can be costly, even for big businesses|
|[PostgreSQL](https://en.wikipedia.org/wiki/PostgreSQL)|Another free and open-source relational database. It is designed to be easily extensible, enabling adding advanced new features without needing a major change to the initial database design|

Other common SQL databases include: `SQLite`, `MariaDB`, `Amazon Aurora`, and `Azure SQL`.
## Non-relational (NoSQL)
[non-relational database](https://en.wikipedia.org/wiki/NoSQL) does not use tables, rows, columns, primary keys, relationships, or schemas. Instead, a `NoSQL` database stores data using various storage models, depending on the type of data stored.

When dealing with datasets that are not very well defined and structured, a `NoSQL` database would be the best choice for storing our data.

There are 4 common storage models for `NoSQL` databases:

- Key-Value
- Document-Based
- Wide-Column
- Graph

Each of the above models has a different way of storing data.

For example, the `Key-Value` model usually stores data in `JSON` or `XML`, and has a key for each pair, storing all of its data as its value:

![Table of posts with IDs, dates, and content, showing key-value pairs](https://academy.hackthebox.com/storage/modules/75/web_apps_non-relational_db.jpg)

The above example can be represented using `JSON` as follows:

Code: json

```json
{
  "100001": {
    "date": "01-01-2021",
    "content": "Welcome to this web application."
  },
  "100002": {
    "date": "02-01-2021",
    "content": "This is the first post on this web app."
  },
  "100003": {
    "date": "02-01-2021",
    "content": "Reminder: Tomorrow is the ..."
  }
}
```

Some of the most common `NoSQL` databases include:

|Type|Description|
|---|---|
|[MongoDB](https://en.wikipedia.org/wiki/MongoDB)|The most common `NoSQL` database. It is free and open-source, uses the `Document-Based` model, and stores data in `JSON` objects|
|[ElasticSearch](https://en.wikipedia.org/wiki/Elasticsearch)|Another free and open-source `NoSQL` database. It is optimized for storing and analyzing huge datasets. As its name suggests, searching for data within this database is very fast and efficient|
|[Apache Cassandra](https://en.wikipedia.org/wiki/Apache_Cassandra)|Also free and open-source. It is very scalable and is optimized for gracefully handling faulty values|

Other common `NoSQL` databases include: `Redis`, `Neo4j`, `CouchDB`, and `Amazon DynamoDB`.
## Use in Web Applications
first, the database has to be installed and set up on the back end server, and once it is up and running, the web applications can start utilizing it to store and retrieve data.

For example, within a `PHP` web application, once `MySQL` is up and running, we can connect to the database server with:

Code: php

```php
$conn = new mysqli("localhost", "user", "pass");
```

Thus, we can create new databases and query the databases.