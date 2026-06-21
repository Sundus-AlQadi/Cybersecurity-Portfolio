# SQL Injection Notes

## What is SQL Injection?
SQL Injection is a web security vulnerability that allows an attacker to interfere with the database queries made by an application.

## Possible Impact
- Viewing unauthorized data
- Modifying or deleting data
- Bypassing application logic
- In some cases, affecting the back-end infrastructure

## What I Learned
- SQL Injection happens when user input is inserted into SQL queries without proper validation or parameterization.
- It can affect confidentiality, integrity, and availability.
- Secure coding practices are needed to prevent it.

## Lab Practice

### Lab 01: WHERE Clause SQL Injection
In this lab, I practiced how SQL injection can modify a WHERE clause and change the query logic to reveal hidden data. The main lesson was that insecure handling of user input can allow unauthorized access to restricted information.

### Lab 02: Login Bypass
In this lab, I practiced how SQL injection can affect authentication logic. The main lesson was that insecure handling of login input can allow unauthorized access to privileged accounts. This showed why login forms must use parameterized queries and secure authentication checks.

### Lab 03: UNION-Based SQL Injection

UNION-based SQL injection is used when the results of the injected query are returned in the application's response.

The UNION operator combines the results of two SELECT queries. However, both SELECT queries must return the same number of columns.

A column is like a field in a database table. For example, a products table may contain columns such as:

```text
id | name | price
```
### Lab 04: Finding a Text-Compatible Column in UNION-Based SQL Injection

After determining the number of columns returned by the original query, the next step is to identify which column can display string data.

This is important because different database columns may have different data types, such as:

```text
id | name | price
```
### Lab 05: Retrieving Data from Other Tables

In this lab, I used the techniques from previous UNION-based SQL injection labs.

First, I confirmed that the original query returned two columns. Then, I confirmed that both columns were compatible with text data.

After that, I retrieved the username and password columns from the users table and used the administrator credentials to complete the lab.

#### Key Takeaway
UNION-based SQL injection can expose sensitive information from other database tables if user input is not handled securely and query results are displayed in the application's response.

### Lab 06: Retrieving Multiple Values in a Single Column

In UNION-based SQL injection, sometimes the original query returns multiple columns, but only one of them is compatible with string/text data.

When only one text-compatible column is available, multiple values can be combined into one output using string concatenation.

For example, instead of displaying username and password in two separate columns, they can be combined into one value:

```text
username~password
```
### Lab 07: Querying the Database Type and Version

SQL injection can be used to gather information about the database system itself. This is known as database enumeration.

Database enumeration may include identifying:

- Database type
- Database version
- Table names
- Column names

This information is useful because SQL syntax can differ between database systems such as MySQL, Microsoft SQL Server, PostgreSQL, and Oracle.

For MySQL and Microsoft SQL Server, the database version can be queried using:

```text
@@version
```

### Lab 08: Listing Database Contents on Non-Oracle Databases

When table and column names are unknown, database metadata can be queried to understand the database structure.

On many non-Oracle databases, `information_schema` stores metadata about tables and columns.

Useful metadata locations include:

```text
information_schema.tables
information_schema.columns
```

### Lab 09: Querying the Database Type and Version on Oracle

Different database systems use different syntax and metadata locations.

Oracle databases store version information in:

```text
v$version
```

The version details are stored in the:

```text
BANNER
```

column.

Oracle also commonly uses a special table called:

```text
dual
```

for standalone SELECT statements.

Database version information can be useful because different database systems support different SQL syntax, functions, tables, and metadata structures.

#### Key Takeaway

Database enumeration helps identify the database platform and version, which can guide later SQL injection techniques and payload construction.

### Lab 10: Listing Database Contents on Oracle

When table and column names are unknown, Oracle database metadata can be queried to understand the database structure.

Unlike many non-Oracle databases, Oracle does not use:

```text
information_schema
```

Instead, Oracle stores metadata in system tables such as:

```text
all_tables
all_tab_columns
```

Useful metadata locations include:

```text
all_tables
```

Used to retrieve table names.

```text
all_tab_columns
```

Used to retrieve column names for a specific table.

Oracle also commonly uses:

```text
dual
```

for standalone SELECT statements.

#### Key Takeaway

Database enumeration techniques vary between database systems.

In Oracle databases, metadata stored in `all_tables` and `all_tab_columns` can be used to discover table names, column names, and ultimately locate sensitive information.

### Lab 11: Blind SQL Injection with Conditional Responses

In some SQL injection vulnerabilities, query results are not displayed directly to the user.

Instead, the application may behave differently depending on whether a condition is true or false.

This is known as Blind SQL Injection.

In this lab, the application displayed a:

```text
Welcome back
