# Types of Databases
Databases can be broadly categorized into:
## 1. **Relational Databases (RDBMS)**
- **Uses SQL** for querying and communication.
- **Structure**: Data is stored in **tables** with **rows** and **columns**, governed by a **schema**.
- **Relationships**: Tables are linked using **keys** (primary/foreign), forming a **schema** that enables efficient data access.
### Key Concepts:
- **Schema**: Blueprint that defines structure, relationships, and constraints.
- **Tables/Entities**: Represent data objects (e.g., Customers, Products, Orders).
- **Keys**:
    - **Primary Key** – unique identifier in a table.
    - **Foreign Key** – links records across tables.
### Example:
- `users` table: `id`, `username`, `first_name`, `last_name`
- `posts` table: `id`, `user_id`, `date`, `content`
- `user_id` in `posts` links to `id` in `users` to associate posts with users.
![Database schema diagram showing 'users' table with columns: id, username, first_name, last_name, and 'posts' table with columns: id, user_id, date, content. Arrows indicate relationships between user_id in 'posts' and id in 'users'.](https://academy.hackthebox.com/storage/modules/75/web_apps_relational_db.jpg)
### Advantages:
- Highly structured and organized.
- Great for **complex queries** and **transaction management**.
- Ensures **data consistency** and **integrity**.
### Common RDBMS Tools:
- MySQL
- PostgreSQL
- SQL Server
- Oracle
- Microsoft Access

---
## 2. Non-Relational Databases (NoSQL)
- **Does not use SQL**, tables, rows, or strict schemas.
- Optimized for **scalability**, **flexibility**, and **unstructured/semi-structured data**.
### Types of NoSQL Models:
1. **Key-Value Store**: Each item is stored as a key and value pair (e.g., Redis).
2. **Document-Based**: Stores data in documents (e.g., JSON, BSON) (e.g., MongoDB).
3. **Wide-Column Store**: Data stored in columns (e.g., Cassandra).
4. **Graph Database**: Focused on relationships between data (e.g., Neo4j).

### 📄 Example (Key-Value, in JSON):

```json
{
  "100001": {
    "date": "01-01-2021",
    "content": "Welcome to this web application."
  },
  "100002": {
    "date": "02-01-2021",
    "content": "This is the first post on this web app."
  }
}
```
![Table of posts with entries: id 100001, date 01-01-2021, content 'Welcome to this web application'; id 100002, date 02-01-2021, content 'This is the first post on this web app'; id 100003, date 02-01-2021, content 'Reminder: Tomorrow is the...'. Keys and values are indicated](https://academy.hackthebox.com/storage/modules/75/web_apps_non-relational_db.jpg)
### ✅ Advantages:
- Highly **scalable** (horizontal scaling).
- Ideal for **big data**, **real-time applications**, and **dynamic schemas**.
- Easier to model **complex, hierarchical, or changing data**.
### Common NoSQL Tools:
- MongoDB
- Redis
- Cassandra
- Neo4j

---

## Comparison Table

|Feature|Relational (SQL)|Non-Relational (NoSQL)|
|---|---|---|
|Query Language|SQL|Varies (JSON, APIs, etc.)|
|Structure|Table-based (schema)|Document, Key-Value, etc.|
|Data Integrity|High|Moderate|
|Scalability|Vertical (traditionally)|Horizontal|
|Flexibility|Low|High|
|Examples|MySQL, PostgreSQL|MongoDB, Redis|
