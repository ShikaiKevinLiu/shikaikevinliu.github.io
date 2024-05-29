---
title: SQL Syntax Fundamentals
category: 数据库
date: 2024-05-04
tag:
  - 数据库基础
  - SQL
series: ["Databse and SQL"]
series_order: 2

---

## Basic Concepts

### Database Terminology

- `Database`- A container for storing organized data, usually in the form of one or more files.
- `Table`- A structured list of data of a particular type.
- `Schema`- Information about the layout and characteristics of a database and its tables. The schema defines how data is stored in the tables, what type of data is stored, how data is broken down, and how different parts are named. Both databases and tables have schemas.
- `Column`- A field in a table. All tables consist of one or more columns.
- `Row`- A record in a table.
- `Primary Key`- One column (or a set of columns) whose values uniquely identify each row in the table.

### SQL (Structured Query Language) Syntax

#### SQL Syntax Structure

![](https://oss.javaguide.cn/p3-juejin/cb684d4c75fc430e92aaee226069c7da~tplv-k3u1fbpfcp-zoom-1.png)

SQL Syntax Structure includes:
- `Clause` - A component of statements and queries. (In some cases, these are optional.)
Expression - Can produce any scalar value, or a database table made of columns and rows.
- `Predicate` - Specifies conditions for evaluating SQL three-valued logic (3VL) (true/false/unknown) or boolean truth values, and limits the effects of statements and queries, or alters program flow.
- `Query` - Retrieves data based on specific criteria. This is an important component of SQL.
- `Statement` - Can permanently affect the schema and data, or control database transactions, program flow, connections, sessions, or diagnostics.

#### Comments on SQL Syntax

- SQL statements are case-insensitive
- Multiple SQL statements must be separated by a semicolon (;).
- When processing SQL statements, all spaces are ignored.


### SQL Categories

#### Data Definition Language（DDL）

DDL is responsible for defining data structures and database objects.

DDL core commands are `CREATE`、`ALTER`、`DROP`.

#### Data Manipulation Language（DML）

DML is used for database operations to manage and access(read/write) data within the database.

DML core commands are `INSERT`、`UPDATE`、`DELETE`、`SELECT`.

#### Transaction Control Language（TCL）

TCL is used to manage transactions within a database. It is used to manage changes made by DML statements and allows statements to be grouped into logical transactions.

TCL core commands are `COMMIT`、`ROLLBACK`。

#### Data Control Language（DCL）
DCL is a type of command that can control access rights to data. It can control specific user accounts' permissions for database objects such as tables, views, stored procedures, and user-defined functions.

DCL core commands are `GRANT`、`REVOKE`。

**Let's first introduce the usage of DML statements. The main function of DML is to read and write to the database, implementing operations such as Creating, deleting, updating, and reading.**

## CRUD

### Creating

`INSERT INTO` statement is used to insert new records into a table.

**Insert a whole row**

```sql
# insert one row
INSERT INTO user
VALUES (10, 'root', 'root', 'xxxx@163.com');
# insert multiple row
INSERT INTO user
VALUES (10, 'root', 'root', 'xxxx@163.com'), (12, 'user1', 'user1', 'xxxx@163.com'), (18, 'user2', 'user2', 'xxxx@163.com');
```

**Insert partial columns of the table**

```sql
INSERT INTO user(username, password, email)
VALUES ('admin', 'admin', 'xxxx@163.com');
```

**Insert data from another query**

```sql
INSERT INTO user(username)
SELECT name
FROM account;
```

### Updating

`UPDATE` statement is used to update records in a table

```sql
UPDATE user
SET username='robot', password='robot'
WHERE username = 'root';
```

### Deleting

- `DELETE` statement is used to delete records in a table

```sql
DELETE FROM user
WHERE username = 'robot';
```

### Query Data

`SELECT` statement is used to query data from the database.


`DISTINCT` is used to return unique different values. It applies to all columns, meaning that it considers rows identical only if all column values are the same.

`LIMIT` restricts the number of rows returned. It have two parameters; the first parameter is the starting row, beginning at 0, and the second parameter is the total number of rows to return.

**Query Single Column**

```sql
SELECT prod_name
FROM products;
```

**Query Multiple Columns**

```sql
SELECT prod_id, prod_name, prod_price
FROM products;
```

**Query All Columns**

```sql
SELECT *
FROM products;
```

**Query Unique Values**

```sql
SELECT DISTINCT
vend_id FROM products;
```

**Limit number of rows**

```sql
-- return first 5 rows
SELECT * FROM mytable LIMIT 5;
SELECT * FROM mytable LIMIT 0, 5;
-- return row 3-5
SELECT * FROM mytable LIMIT 2, 3;
```

## Sorting

`ORDER BY` is used to sort the result set by one or more columns. 

When sorting by multiple columns in `ORDER BY`, the column sorted first is placed in front, and the column sorted later is placed behind. Additionally, different columns can have different sorting rules.

```sql
SELECT * FROM products
ORDER BY prod_price DESC, prod_name ASC;
```

## Grouping
**`Group By`**：

- `GROUP BY` clause groups records into summary rows.
- `GROUP BY` returns one record for each group.
- `GROUP BY` often involves aggregation such as count, max, sum, avg, etc.
- `GROUP BY` can group by one or more columns.
- After sorting by the grouping fields, `ORDER BY` can be used to sort by the aggregated fields.

**Grouping**

```sql
SELECT cust_name, COUNT(cust_address) AS addr_num
FROM Customers GROUP BY cust_name;
```

**Grouping, then Sorting**

```sql
SELECT cust_name, COUNT(cust_address) AS addr_num
FROM Customers GROUP BY cust_name
ORDER BY cust_name DESC;
```

**`Having`**：

- `having` is to filter the results after aggregation.
- `having` are usually used together with `group by`
- `where` and `having` can also be used together.

**Example of WHERE and HAVING**

```sql
SELECT cust_name, COUNT(*) AS num
FROM Customers
WHERE cust_email IS NOT NULL
GROUP BY cust_name
HAVING COUNT(*) >= 1;
```

**`having` vs `where`**：

- `WHERE`: Filters specified rows and **cannot** be followed by aggregate functions (grouping functions). `WHERE` is used before `GROUP BY`.
- `HAVING`: Filters groups and is generally used with `GROUP BY`, cannot be used alone. `HAVING` is used after `GROUP BY`.

## Subquery
A subquery is an SQL query nested within a larger query, also known as an inner query or inner select.

A subquery can be embedded in `SELECT`, `INSERT`, `UPDATE`, and `DELETE` statements and can be used with operators such as `=`, `<`, `>`, `IN`, `BETWEEN`, and `EXISTS`.

Subquery is usually after `WHERE` and `FROM`：

- When used in the `WHERE` clause, a subquery can return single-row single-column, multi-row single-column, or single-row multi-column data, depending on the operator. The subquery must return a value that can serve as a condition for the `WHERE` clause.
- When used in the `FROM` clause, a subquery generally returns multi-row multi-column data, essentially returning a temporary table that adheres to the rule that what follows `FROM` should be a table. This approach enables multi-table join queries.。

The basic syntax for a subquery used in the `WHERE` clause is as follows:

```sql
SELECT column_name [, column_name ]
FROM   table1 [, table2 ]
WHERE  column_name operator
    (SELECT column_name [, column_name ]
    FROM table1 [, table2 ]
    [WHERE])
```

- The subquery must be enclosed in parentheses `( )`.
- `operator` represents the operator used in the `WHERE` clause.

The basic syntax for a subquery used in the `FROM` clause is as follows:

```sql
SELECT column_name [, column_name ]
FROM (SELECT column_name [, column_name ]
      FROM table1 [, table2 ]
      [WHERE]) AS temp_table_name
WHERE  condition
```

Since the subquery in the `FROM` clause returns a result set equivalent to a temporary table, the `AS` keyword is used to name this temporary table.

### WHERE

- `WHERE` is to filter records and shrink the size of data
- `WHERE` is followed by a true/false boolean evaluation
- `WHERE` can be used together with `SELECT`，`UPDATE` 和 `DELETE`
- below are comparision operators can be used inside `WHERE` clause
  - =, <>, >, <, >=, <=, BETWEEN, LIKE, IN

**`SELECT` in `WHERE` clause**

```SQL
SELECT * FROM Customers
WHERE cust_name = 'Kids Place';
```

**`UPDATE` in `WHERE` clause**

```SQL
UPDATE Customers
SET cust_name = 'Jack Jones'
WHERE cust_name = 'Kids Place';
```

**`DELETE` in `WHERE` clause**

```SQL
DELETE FROM Customers
WHERE cust_name = 'Kids Place';
```
