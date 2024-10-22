# **Cracking the SQL Interview**

# 100 Must-Know SQL Interview Questions

<br>

## 1. What is _SQL_ and what is it used for?

**SQL** (**Structured Query Language**) is a domain-specific, declarative programming language designed for managing relational databases. It is the primary language for tasks like data retrieval, data manipulation, and database administration.

### Core Components

- **DDL** (Data Definition Language): Used for defining and modifying the structure of the database.
- **DML** (Data Manipulation Language): Deals with adding, modifying, and removing data in the database.
- **DCL** (Data Control Language): Manages the permissions and access rights of the database.
- **TCL** (Transaction Control Language): Governs the transactional management of the database, such as commits or rollbacks.

### Common Database Management Tasks

**Data Retrieval and Reporting**: Retrieve and analyze data, generate reports, and build dashboards.

**Data Manipulation**: Insert, update, or delete records from tables. Powerful features like Joins and Subqueries enable complex operations.

**Data Integrity**: Ensure data conform to predefined rules. Techniques like foreign keys, constraints, and triggers help maintain the integrity of the data.

**Data Security**: Manage user access permissions and roles.

**Data Consistency**: Enforce ACID properties (Atomicity, Consistency, Isolation, Durability) in database transactions.

**Data Backups and Recovery**: Perform database backups and ensure data is restorable in case of loss.

**Data Normalization**: Design databases for efficient storage and reduce data redundancy.

**Indices and Performance Tuning**: Optimize queries for faster data retrieval.

**Replication and Sharding**: Advanced techniques for distributed systems.

### Basic SQL Commands

- **CREATE DATABASE**: Used to create a new database.
- **CREATE TABLE**: Defines a new table.
- **INSERT INTO**: Adds a new record into a table.
- **SELECT**: Retrieves data from one or more tables.
- **UPDATE**: Modifies existing records.
- **DELETE**: Removes records from a table.
- **ALTER TABLE**: Modifies an existing table (e.g., adds a new column, renames an existing column, etc.).
- **DROP TABLE**: Deletes a table (along with its data) from the database.
- **INDEX**: Adds an index to a table for better performance.
- **VIEW**: Creates a virtual table that can be used for data retrieval.
- **TRIGGER**: Triggers a specified action when a database event occurs.
- **PROCEDURE** and **FUNCTION**: Store database logic for reuse and to simplify complex operations.

### Code Example: Basic SQL Queries

Here is the SQL code:

```sql
-- Create a database
CREATE DATABASE Company;

-- Use Company database
USE Company;

-- Create tables
CREATE TABLE Department (
    DeptID INT PRIMARY KEY AUTO_INCREMENT,
    DeptName VARCHAR(50) NOT NULL
);

CREATE TABLE Employee (
    EmpID INT PRIMARY KEY AUTO_INCREMENT,
    EmpName VARCHAR(100) NOT NULL,
    EmpDeptID INT,
    FOREIGN KEY (EmpDeptID) REFERENCES Department(DeptID)
);

-- Insert data
INSERT INTO Department (DeptName) VALUES ('Engineering');
INSERT INTO Department (DeptName) VALUES ('Sales');

INSERT INTO Employee (EmpName, EmpDeptID) VALUES ('John Doe', 1);
INSERT INTO Employee (EmpName, EmpDeptID) VALUES ('Jane Smith', 2);

-- Select data from database
SELECT * FROM Department;
SELECT * FROM Employee;

-- Perform an inner join to combine data from two tables
SELECT Employee.EmpID, Employee.EmpName, Department.DeptName
FROM Employee
JOIN Department ON Employee.EmpDeptID = Department.DeptID;
```

<br>

## 2. Describe the difference between _SQL_ and _NoSQL_ databases.

**SQL** and **NoSQL** databases offer different paradigms, each designed to suit various types of data and data handling.

### Top-Level Differences

- **SQL**: Primarily designed for structured (structured, semi-structured) data — data conforming to a predefined schema.
- **NoSQL**: Suited for unstructured or semi-structured data that evolves gradually, thereby supporting flexible schemas.

- **SQL**: Employs SQL (Structured Query Language) for data modification and retrieval.
- **NoSQL**: Offers various APIs (like the document and key-value store interfaces) for data operations; the use of structured query languages can vary across different NoSQL implementations.

- **SQL**: Often provides ACID (Atomicity, Consistency, Isolation, Durability) compliance to ensure data integrity.
- **NoSQL**: Databases are oftentimes optimized for high performance and horizontal scalability, with potential trade-offs in consistency.

### Common NoSQL Database Types

#### Document Stores

- **Example**: MongoDB, Couchbase
- **Key Features**: Each record is a self-contained document, typically formatted as JSON. Relationship between documents is established through embedded documents or references.
  Example: Users and their blog posts could be encapsulated within a single document or linked via document references.

#### Key-Value Stores

- **Example**: Redis, Amazon DynamoDB
- **Key Features**: Data is stored as a collection of unique keys and their corresponding values. No inherent structure or schema is enforced, providing flexibility in data content.
  Example: Shopping cart items keyed by a user's ID.

#### Wide-Column Stores (Column Families)

- **Example**: Apache Cassandra, HBase
- **Key Features**: Data is grouped into column families, akin to tables in traditional databases. Each column family can possess a distinct set of columns, granting a high degree of schema flexibility.
  Example: User profiles, where certain users might have additional or unique attributes.

#### Graph Databases

- **Example**: Neo4j, JanusGraph
- **Key Features**: Tailored for data with complex relationships. Data entities are represented as nodes, and relationships between them are visualized as edges.
  Example: A social media platform could ensure efficient friend connections management.

### Data Modeling Differences

- **SQL**: Normalization is employed to minimize data redundancies and update anomalies.
- **NoSQL**: Data is often denormalized, packaging entities together to minimize the need for multiple queries.

### Auto-Incrementing IDs

- **SQL**: Often, each entry is assigned a unique auto-incrementing ID.
- **NoSQL**: The generation of unique IDs can be driven by external systems or even specific to individual documents within a collection.

### Handling Data Relationships

- **SQL**: Relationships between different tables are established using keys (e.g., primary, foreign).
- **NoSQL**: Relationships are handled either through embedded documents, referencing techniques, or as graph-like structures in dedicated graph databases.

### Transaction Support

- **SQL**: Transactions (a series of operations that execute as a single unit) are standard.
- **NoSQL**: The concept and features of transactions can be more varied based on the specific NoSQL implementation.

### Data Consistency Levels

- **SQL**: Traditionally ensures strong consistency across the database to maintain data integrity.
- **NoSQL**: Offers various consistency models, ranging from strong consistency to eventual consistency. This flexibility enables performance optimizations in distributed environments.

### Scalability

- **SQL**: Typically scales vertically, i.e., by upgrading hardware.
- **NoSQL**: Is often designed to scale horizontally, using commodity hardware across distributed systems.

### Data Flexibility

- **SQL**: Enforces a predefined, rigid schema, making it challenging to accommodate evolving data structures.
- **NoSQL**: Supports dynamic, ad-hoc schema updates for maximum flexibility.

### Data Integrity & Validation

- **SQL**: Often relies on constraints and strict data types to ensure data integrity and validity.
- **NoSQL**: Places greater emphasis on the application layer to manage data integrity and validation.
  <br>

## 3. What are the different types of _SQL commands_?

**SQL** commands fall into four primary categories: **Data Query Language** (DQL), **Data Definition Language** (DDL), **Data Manipulation Language** (DML), and **Data Control Language** (DCL).

### Data Query Language (DQL)

These commands focus on querying data within tables.

#### Keywords and Examples:

- **SELECT**: Retrieve data.
- **FROM**: Identify the source table.
- **WHERE**: Apply filtering conditions.
- **GROUP BY**: Group results based on specified fields.
- **HAVING**: Establish qualifying conditions for grouped data.
- **ORDER BY**: Arrange data based on one or more fields.
- **LIMIT**: Specify result count (sometimes replaces `SELECT TOP` for certain databases).
- **JOIN**: Bring together related data from multiple tables.

### Data Definition Language (DDL)

DDL commands are for managing the structure of the database, including tables and constraints.

#### Keywords and Examples:

- **CREATE TABLE**: Generate new tables.
- **ALTER TABLE**: Modify existing tables.
  - **ADD**, **DROP**: Incorporate or remove elements like columns, constraints, or properties.
- **CREATE INDEX**: Establish indexes to improve query performance.
- **DROP INDEX**: Remove existing indexes.
- **TRUNCATE TABLE**: Delete all rows from a table, but the table structure remains intact.
- **DROP TABLE**: Delete tables from the database.

### Data Manipulation Language (DML)

These commands are useful for handling data within tables.

#### Keywords and Examples:

- **INSERT INTO**: Add new rows of data.
  - **SELECT**: Copy data from another table or tables.
- **UPDATE**: Modify existing data in a table.
- **DELETE**: Remove rows of data from a table.

### Data Control Language (DCL)

DCL is all about managing the access and permissions to database objects.

#### Keywords and Examples:

- **GRANT**: Assign permission to specified users or roles for specific database objects.
- **REVOKE**: Withdraw or remove these permissions previously granted.
  <br>

## 4. Explain the purpose of the _SELECT_ statement.

The **SELECT** statement in SQL is fundamental to data retrieval and manipulation within relational databases. Its primary role is to precisely choose, transform, and organize data per specific business requirements.

### Key Components of the SELECT Statement

The **SELECT** statement typically comprises the following elements:

- **SELECT**: Identifies the columns or expressions to be included in the result set.
- **FROM**: Specifies the table(s) from which the data should be retrieved.
- **WHERE**: Introduces conditional statements to filter rows based on specific criteria.
- **GROUP BY**: Aggregates data for summary or statistical reporting.
- **HAVING**: Functions like **WHERE**, but operates on aggregated data.
- **ORDER BY**: Defines the sort order for result sets.
- **LIMIT** or **TOP**: Limits the number of rows returned.

### Practical Applications of SELECT

The robust design of the **SELECT** statement empowers data professionals across diverse functions, enabling:

- **Data Exploration**: Gaining insights through filtered views or aggregated summaries.
- **Data Transformation**: Creating new fields via operations such as concatenation or mathematical calculations.
- **Data Validation**: Verifying data against defined criteria.
- **Data Reporting**: Generating formatted outputs for business reporting needs.
- **Data Consolidation**: Bringing together information from multiple tables or databases.
- **Data Export**: Facilitating the transfer of query results to other systems or for data backup.

Beyond these functions, proper utilization of the other components ensures efficiency and consistency working with relational databases.

### SELECT Query Example

Here is the SQL code:

```sql
SELECT
    Orders.OrderID,
    Customers.CustomerName,
    Orders.OrderDate,
    OrderDetails.UnitPrice,
    OrderDetails.Quantity,
    Products.ProductName,
    Employees.LastName
FROM
    ((Orders
    INNER JOIN Customers ON Orders.CustomerID = Customers.CustomerID)
    INNER JOIN Employees ON Orders.EmployeeID = Employees.EmployeeID)
    INNER JOIN OrderDetails ON Orders.OrderID = OrderDetails.OrderID
```

<br>

## 5. What is the difference between _WHERE_ and _HAVING_ clauses?

**WHERE** and **HAVING** clauses are both used in SQL queries to filter data, but they operate in distinct ways.

### WHERE Clause

The `WHERE` clause is primarily used to filter records before they are grouped or aggregated. It's typically employed with non-aggregated fields or raw data.

### HAVING Clause

Conversely, the `HAVING` clause filters data **after** the grouping step, often in conjunction with aggregate functions like `SUM` or `COUNT`. This makes it useful for setting group-level conditions.
<br>

## 6. Define what a _JOIN_ is in SQL and list its types.

**Join** operations in SQL are responsible for combining rows from multiple tables, primarily based on related columns that are established using a foreign key relationship.

The **three common types of joins** in SQL are:

- **Inner Join**
- **Outer Join**
  - Left Outer Join
  - Right Outer Join
  - Full Outer Join
- **Cross Join**
- **Self Join**

### Inner Join

Inner Join only returns rows where there is a match in both tables for the specified column(s).

**Visual Representation**:

```
Table1:        Table2:            Result (Inner Join):

A   B         B    C               A    B    C
-   -         -    -               -    -    -
1  aa         aa   20              1   aa   20
2  bb         bb   30              2   bb   30
3  cc         cc   40
```

**SQL Query**:

```sql
SELECT Table1.A, Table1.B, Table2.C
FROM Table1
INNER JOIN Table2 ON Table1.B = Table2.B;
```

### Outer Join

Outer Joins—whether left, right or full—include all records from one table (the "left" or the "right" table") and matched existing records from the other table. Unmatched records are filled with NULL values for missing columns from the other table.

#### Left Outer Join

Left Outer Join (or simply Left Join) returns all records from the "left" table and the matched records from the "right" table.

**Visual Representation**:

```
Table1:        Table2:            Result (Left Outer Join):

A   B         B    C               A    B    C
-   -         -    -               -    -    -
1  aa         aa   20              1   aa   20
2  bb         bb   30              2   bb   30
3  cc         NULL  NULL            3   cc  NULL
```

**SQL Query**:

```sql
SELECT Table1.A, Table1.B, Table2.C
FROM Table1
LEFT JOIN Table2 ON Table1.B = Table2.B;
```

#### Right Outer Join

Right Outer Join (or Right Join) returns all records from the "right" table and the matched records from the "left" table.

**Visual Representation**:

```
Table1:        Table2:            Result (Right Outer Join):

A   B         B    C               A    B    C
-   -         -    -               -    -    -
1  aa         aa   20              1   aa   20
2  bb         bb   30              2   bb   30
NULL NULL      cc   40             NULL NULL  40
```

**SQL Query**:

```sql
SELECT Table1.A, Table1.B, Table2.C
FROM Table1
RIGHT JOIN Table2 ON Table1.B = Table2.B;
```

#### Full Outer Join

Full Outer Join (or Full Join) returns all records when there is a match in either the left or the right table.

**Visual Representation**:

```
Table1:        Table2:            Result (Full Outer Join):

A   B         B    C               A    B    C
-   -         -    -               -    -    -
1  aa         aa   20              1   aa   20
2  bb         bb   30              2   bb   30
3  cc         NULL  NULL            3   cc  NULL
NULL NULL      cc   40            NULL NULL  40
```

**SQL Query**:

```sql
SELECT COALESCE(Table1.A, Table2.A) AS A, Table1.B, Table2.C
FROM Table1
FULL JOIN Table2 ON Table1.B = Table2.B;
```

### Cross Join

A Cross Join, also known as a Cartesian Join, produces a result set that is the cartesian product of the two input sets. It will generate every possible combination of rows from both tables.

**Visual Representation**:

```
Table1:        Table2:            Result (Cross Join):

A   B         C    D               A    B    C    D
-   -         -    -               -    -    -    -
1  aa        20    X               1   aa   20   X
2  bb        30    Y               1   aa   30   Y
3  cc        40    Z               1   aa   40   Z
                                    2   bb   20   X
                                    2   bb   30   Y
                                    2   bb   40   Z
                                    3   cc   20   X
                                    3   cc   30   Y
                                    3   cc   40   Z
```

**SQL Query**:

```sql
SELECT Table1.*, Table2.*
FROM Table1
CROSS JOIN Table2;
```

### Self Join

A Self Join is when a table is joined with itself. This is used when a table has a relationship with itself, typically when it has a parent-child relationship.

**Visual Representation**:

```
Employee:                              Result (Self Join):

EmpID   Name       ManagerID       EmpID  Name     ManagerID
-       -           -               -       -          -
1      John         3               1     John        3
2      Amy          3               2     Amy         3
3      Chris       NULL              3    Chris      NULL
4      Lisa        2                4     Lisa        2
5      Mike        2                5     Mike        2
```

**SQL Query**:

```sql
SELECT E1.EmpID, E1.Name, E1.ManagerID
FROM Employee AS E1
LEFT JOIN Employee AS E2 ON E1.ManagerID = E2.EmpID;
```

<br>

## 7. What is a _primary key_ in a database?

A **primary key** in a database is a unique identifier for each record in a table.

### Key Characteristics

- **Uniqueness**: Each value in the primary key column is unique, distinguishing every record.

- **Non-Nullity**: The primary key cannot be null, ensuring data integrity.

- **Stability**: It generally does not change throughout the record's lifetime, promoting consistency.

### Data Integrity Benefits

- **Entity Distinctness**: Enforces that each record in the table represents a unique entity.

- **Association Control**: Helps manage relationships across tables and ensures referential integrity in foreign keys.

### Performance Advantages

- **Efficient Indexing**: Primary keys are often auto-indexed, making data retrieval faster.

- **Optimized Joins**: When the primary key links to a foreign key, query performance improves for related tables.

### Industry Best Practice

- **Pick a Natural Key**: Whenever possible, choose existing data values that are unique and stable.

- **Keep It Simple**: Single-column primary keys are easier to manage.

- **Avoid Data in Column Attributes**: Using data can lead to bloat, adds complexity, and can be restrictive.

- **Avoid Data Sensitivity**: Decrease potential risks associated with sensitive data by separating it from keys.

- **Evaluate Multi-Column Keys Carefully**: Identify and justify the need for such complexity.

### Code Example: Declaring a Primary Key

Here is the SQL code:

```sql
CREATE TABLE Students (
    student_id INT PRIMARY KEY,
    grade_level INT,
    first_name VARCHAR(50),
    last_name VARCHAR(50)
);
```

<br>

## 8. Explain what a _foreign key_ is and how it is used.

A **foreign key** (FK) is a column or a set of columns in a table that uniquely identifies a row or a set of rows in another table. It establishes a relationship between two tables, often referred to as the **parent table** and the **child table**.

### Key Functions of a Foreign Key

- **Data Integrity**: Assures that each entry in the referencing table has a corresponding record in the referenced table, ensuring the data's accuracy and reliability.
- **Relationship Mapping**: Defines logical connections between tables that can be used to retrieve related data.

- **Action Propagation**: Specify what action should be taken in the child table when a matching record in the parent table is created, updated, or deleted.

- **Cascade Control**: Allows operations like deletion or updates to propagate to related tables, maintaining data consistency.

### Foreign Key Constraints

The database ensures the following with foreign key constraints:

- **Uniqueness**: The referencing column or combination of columns in the child table is unique.
- **Consistency**: Each foreign key in the child table either matches a corresponding primary key or unique key in the parent table or contains a null value.

### Use Cases and Best Practices

- **Data Integrity and Consistency**: FKs ensure that references between tables are valid and up-to-date. For instance, a sales entry references a valid product ID and a customer ID.

- **Relationship Representation**: FKs depict relationships between tables, such as 'One-to-Many' (e.g., one department in a company can have multiple employees) or 'Many-to-Many' (like in associative entities).

- **Querying Simplification**: They aid in performing joined operations to retrieve related data, abstracting away complex data relationships.

### Code Example: Creating a Foreign Key Relationship

Here is the SQL code:

```sql
-- Create the parent (referenced) table first
CREATE TABLE departments (
    id INT PRIMARY KEY,
    name VARCHAR(100)
);

-- Add a foreign key reference to the child table
CREATE TABLE employees (
    id INT PRIMARY KEY,
    name VARCHAR(100),
    department_id INT,
    FOREIGN KEY (department_id) REFERENCES departments(id)
);
```

<br>

## 9. How can you prevent _SQL injections_?

**SQL injection** occurs when untrusted data is mixed with SQL commands. To prevent these attacks, use **parameterized queries** and input validation.

Here are specific methods to guard against SQL injection:

### Parameterized Queries

- **Description**: Also known as a prepared statement, it separates SQL code from user input, rendering direct command injection impossible.
- **Code Example**:

  - Java (JDBC):

  ```java
  String query = "SELECT * FROM users WHERE username = ? AND password = ?";
  PreparedStatement ps = con.prepareStatement(query);
  ps.setString(1, username);
  ps.setString(2, password);
  ResultSet rs = ps.executeQuery();
  ```

  - Python (MySQL):

  ```python
  cursor.execute("SELECT * FROM users WHERE username = %s AND password = %s", (username, password))
  ```

- **Benefits**:
  - Improved security.
  - Reliability across different databases.
  - No need for manual escaping.

### Stored Procedures

- **Description**: Allows the database to pre-compile and store your SQL code, providing a layer of abstraction between user input and database operations.

- **Code Example**:

  - With MySQL:
    - Procedure definition:

  ```sql
  CREATE PROCEDURE login(IN p_username VARCHAR(50), IN p_password VARCHAR(50))
  BEGIN
    SELECT * FROM users WHERE username = p_username AND password = p_password;
  END
  ```

  - Calling the procedure:

  ```python
  cursor.callproc('login', (username, password))
  ```

- **Advantages**:
  - Reduction of code redundancy.
  - Allows for granular permissions.
  - Can improve performance through query plan caching.

### Input Validation

- **Description**: Examine user-supplied data to ensure it meets specific criteria before allowing it in a query.

- **Code Example**:
  Using regex:

  ```python
  if not re.match("^[A-Za-z0-9_-]*$", username):
      print("Invalid username format")
  ```

- **Drawbacks**:
  - Not a standalone method for preventing SQL injection.
  - Might introduce false positives, limiting the user's input freedom.

### Code Filtering

- **Description**: Sanitize incoming data based on its type, like strings or numbers. This approach works best in conjunction with other methods.

- **Code Example**:
  In Python:

  ```python
  username = re.sub("[^a-zA-Z0-9_-]", "", username)
  ```

- **Considerations**:
  - Still necessitates additional measures for robust security.
  - Can restrict legitimate user input.
    <br>

## 10. What is _normalization_? Explain with examples.

**Normalization** is a database design method, refining table structures to reduce data redundancy and improve data integrity. It is a multi-step process, divided into five normal forms (1NF, 2NF, 3NF, BCNF, 4NF), each with specific rules.

### Normalization in Action

Let's consider a simplistic "Customer Invoices" scenario, starting from an unnormalized state:

#### Unnormalized Table (0NF)

| ID  | Name | Invoice No. | Invoice Date | Item No. | Description | Quantity | Unit Price |
| --- | ---- | ----------- | ------------ | -------- | ----------- | -------- | ---------- |
|     |      |             |              |          |             |          |            |

In this initial state, all data is stored in a single table without structural cohesion. Each record is a mix of customer and invoice information. This can lead to data redundancy and anomalies.

#### First Normal Form (1NF)

To reach 1NF, ensure **all cells are atomic**, meaning they hold single values. Make separate tables for related groups of data. In our example, let's separate customer details from invoices and address multiple items on a single invoice.

##### Customer Details Table

| ID  | Name |
| --- | ---- |
|     |      |

##### Invoices Table

| Invoice No. | Customer_ID | Invoice Date |
| ----------- | ----------- | ------------ |
|             |             |              |

##### Items Table

| Invoice No. | Item No. | Description | Quantity | Unit Price |
| ----------- | -------- | ----------- | -------- | ---------- |

Now, each table focuses on specific data, unique to 1NF.

1NF is crucial for efficient database operations, especially for tasks like reporting and maintenance.

#### Second Normal Form (2NF)

To achieve 2NF, consider the context of a complete data entry. **Each non-key column should be dependent on the whole primary key**.

In our example, the Items table already satisfies 2NF, as all non-key columns, like `Description` and `Unit Price`, depend on the entire primary key, formed by `Invoice No.` and `Item No.` together.

#### Third Normal Form (3NF)

For 3NF compliance, **there should be no transitive dependencies**. Non-key columns should rely only on the primary key.

The Invoices table requires further refinement:

##### Updated Invoices Table

| Invoice No. | Customer_ID | Invoice Date |
| ----------- | ----------- | ------------ |
|             |             |              |

Here, `Customer_ID` is the sole attribute associated with the customer.

### Practical Implications

- Higher normal forms provide **stronger** data integrity but might be harder to maintain during regular data operations.
- Consider your specific application needs when determining the target normal form.

### Real-World Usage

- Many databases aim for 3NF.
- In scenarios requiring exhaustive data integrity, 4NF, and sometimes beyond, are appropriate.

### Code Example: Implementing 3NF

Here is the SQL code:

```sql
-- Create Customer and Invoices Table
CREATE TABLE Customers (
    ID INT PRIMARY KEY,
    Name VARCHAR(50)
);

CREATE TABLE Invoices (
    InvoiceNo INT PRIMARY KEY,
    Customer_ID INT,
    InvoiceDate DATE,
    FOREIGN KEY (Customer_ID) REFERENCES Customers(ID)
);

-- Create Items Table
CREATE TABLE Items (
    InvoiceNo INT,
    ItemNo INT,
    Description VARCHAR(100),
    Quantity INT,
    UnitPrice DECIMAL(10,2),
    PRIMARY KEY (InvoiceNo, ItemNo),
    FOREIGN KEY (InvoiceNo) REFERENCES Invoices(InvoiceNo)
);
```

This code demonstrates the specified 3NF structure with distinct tables for Customer, Invoices, and Items, ensuring data integrity during operations.
<br>

## 11. Describe the concept of _denormalization_ and when you would use it.

**Denormalization** involves optimizing database performance by reducing redundancy at the cost of some data integrity.

### Common Techniques for Denormalization

1. **Flattening Relationships**:

   - Combining related tables to minimize joins.
   - Example: `Order` and `Product` tables are merged, eliminating the many-to-many relationship.

2. **Aggregating Data**:

   - Precomputing derived values to minimize costly calculations.
   - Example: a `Sales_Total` column in an `Order` table.

3. **Adding Additional Redundant Data**:
   - Replicating data from one table in another to reduce the need for joins.
   - Example: The `Customer` and `Sales` tables can both have a `Country` column, even though the country is indirectly linked through the `Customer` table.

### Common Use Cases

- **Reporting and Analytics**:

  - Companies often need to run complex reports that span numerous tables.
  - Denormalization can flatten these tables, making the reporting process more efficient.

- **High-Volume Transaction Systems**:

  - In systems where data consistency can be relaxed momentarily, denormalization can speed up operations.
  - It's commonly seen in e-commerce sites where a brief delay in updating the sales figures might be acceptable for faster checkouts and improved user experience.

- **Read-Mostly Applications**:

  - Systems that are heavy on data reads and relatively light on writes can benefit from denormalization.

- **Search- and Query-Intensive Applications**:

  - For example, search engines often store data in a denormalized format to enhance retrieval speed.

- **Partitioning Data**:
  - In distributed systems like Hadoop or NoSQL databases, data is often stored redundantly across multiple nodes for enhanced performance.

### Considerations and Trade-offs

- **Performance vs. Consistency**:

  - Denormalization can boost performance but at the expense of data consistency.

- **Maintenance Challenges**:

  - Redundant data must be managed consistently, which can pose challenges.

- **Operational Simplicity**:

  - Sometimes, having a simple, denormalized structure can outweigh the benefits of granularity and normalization.

- **Query Flexibility**:
  - A normalized structure can be more flexible for ad-hoc queries and schema changes. Denormalized structures might require more effort to adapt to such changes.
    <br>

## 12. What are _indexes_ and how can they improve query performance?

**Indexes** are essential in SQL to accelerate queries by providing quick data lookups.

### How Do Indexes Improve Performance?

- **Faster Data Retrieval**: Think of an index like a book's table of contents, which leads you right to the desired section.

- **Sorted Data Access**: With data logically ordered, lookups are more efficient.

- **Reduces Disk I/O**: Queries may read fewer data pages when using an index.

- **Enhances Joins**: Indexes help optimize join conditions, particularly in larger tables.

- **Aggregates and Uniques**: They can swiftly resolve aggregate functions and enforce data uniqueness.

### Index Types

- **B-Tree**: Standard for most databases, arranges data in a balanced tree structure.
- **Hash**: Direct lookup based on a hash of the indexed column.
- **Bitmap**: Best used for columns with a low cardinality.
- **R-Tree**: Optimized for spatial data, such as maps.

Different databases may offer additional specialized index types.

### When to Use Carefully

Excessive or unnecessary indexing can:

- **Consume Resources**: Indexes require disk space and upkeep during data modifications.
- **Slow Down Writes**: Each write operation might trigger updates to associated indexes.

### Best Practices

1. **Appropriate Index Count**: Identify crucial columns and refrain from over-indexing.
2. **Monitor and Refactor**: Regularly assess index performance and refine or remove redundant ones.
3. **Consistency**: Ensure all queries access data in a consistent manner to take full advantage of indexes.
4. **Data Type Consideration**: Certain data types are better suited for indexing than others.

### Types of Keys

- **Primary Key**: Uniquely identifies each record in a table.
- **Foreign Key**: Establishes a link between tables, enforcing referential integrity.
- **Compound Key**: Combines two or more columns to form a unique identifier.
  <br>

## 13. Explain the purpose of the _GROUP BY_ clause.

The **GROUP BY** clause in SQL serves to consolidate data and perform operations across groups of records.

### Key Functions

- **Data Aggregation**: Collapses rows into summary data.
- **Filtering**: Provides filtering criteria for groups.
- **Calculated Fields**: Allows computation on group-level data.

### Usage Examples

Consider a `Sales` table with the following columns: `Product`, `Region`, and `Amount`.

#### Data Aggregation

For data aggregation, we use aggregate functions such as `SUM`, `AVG`, `COUNT`, `MIN`, or `MAX`.

The query below calculates total sales by region:

```sql
SELECT Region, SUM(Amount) AS TotalSales
FROM Sales
GROUP BY Region;
```

#### Filtering

The **GROUP BY** clause can include conditional statements. For example, to count only those sales that exceed $100 in amount:

```sql
SELECT Region, COUNT(Amount) AS SalesAbove100
FROM Sales
WHERE Amount > 100
GROUP BY Region;
```

#### Calculated Fields

You can compute derived values for groups. For instance, to find what proportion each product contributes to the overall sales in a region, use this query:

```sql
SELECT Region, Product, SUM(Amount) / (SELECT SUM(Amount) FROM Sales WHERE Region = s.Region) AS RelativeContribution
FROM Sales s
GROUP BY Region, Product;
```

### Performance Considerations

Efficient database design aims to balance query performance with storage requirements. Aggregating data during retrieval can optimize performance, especially when dealing with huge datasets.

It's essential to verify these calculations for accuracy, as improper data handling can lead to skewed results.
<br>

## 14. What is a _subquery_, and when would you use one?

**Subqueries** are embedded SQL select statements that provide inputs for an outer query. They can perform various tasks, such as filtering and aggregate computations. **Subqueries** can also be useful for complex join conditions, self-joins, and more.

### Common Subquery Types

#### Scalar Subquery

A **Scalar Subquery** returns a single value. They're frequently used for comparisons—like `>`, `=`, or `IN`.

Examples:

- Getting the **maximum** value:

  - `SELECT col1 FROM table1 WHERE col1 = (SELECT MAX(col1) FROM table1);`

- Checking existence:

  - `SELECT col1, col2 FROM table1 WHERE col1 = (SELECT col1 FROM table2 WHERE condition);`

- Using **aggregates**:
  - `SELECT col1 FROM table1 WHERE col1 = (SELECT SUM(col2) FROM table2);`

#### Table Subquery

A **Table Subquery** is like a temporary table. It returns rows and columns and can be treated as a regular table for further processing.

Examples:

- Filtering data:

  - `SELECT * FROM table1 WHERE col1 IN (SELECT col1 FROM table2 WHERE condition);`

- Data deduplication:
  - `SELECT DISTINCT col1 FROM table1 WHERE condition1 AND col1 IN (SELECT col1 FROM table2 WHERE condition2);`

### Advantages of Using Subqueries

- **Simplicity**: They offer cleaner syntax, especially for complex queries.

- **Structured Data**: Subqueries can ensure that intermediate data is properly processed, making them ideal for multi-step tasks.

- **Reduced Code Duplication**: By encapsulating certain logic within a subquery, you can avoid repetitive code.

- **Dynamic Filtering**: The data returned by a subquery can dynamically influence the scope of the outer query.

- **Milestone Calculations**: For long and complex queries, subqueries can provide clarity and help break down the logic into manageable parts.

### Limitations and Optimization

- **Performance**: Subqueries can sometimes be less efficient. Advanced databases like Oracle, SQL Server, and PostgreSQL offer optimizations, but it's essential to monitor query performance.

- **Versatility**: While subqueries are powerful, they can be less flexible in some scenarios compared to other advanced features like Common Table Expressions (CTEs) and Window Functions.

- **Understanding and Debugging**: Nested logic might make a stored procedure or more advanced techniques like CTEs easier to follow and troubleshoot.

### Code Example: Using Subqueries

Here is the SQL code:

```sql
-- Assuming you have table1 and table2

-- Scalar Subquery Example
SELECT col1
FROM table1
WHERE col1 = (SELECT MAX(col1) FROM table1);

-- Table Subquery Example
SELECT col1, col2
FROM table1
WHERE col1 = (SELECT col1 FROM table2 WHERE condition);
```

<br>

## 15. Describe the functions of the _ORDER BY_ clause.

The **ORDER BY** clause in SQL serves to sort the result set based on specified columns, in either ascending (**ASC**, default) or descending (**DESC**) order. It's often used in conjunction with various SQL statements like **SELECT** or **UNION** to enhance result presentation.

### Key Features

- **Column-Specific Sorting**: You can designate one or more columns as the basis for sorting. For multiple columns, the order of precedence is from left to right.
- **ASC and DESC Directives**: These allow for both ascending and descending sorting. If neither is specified, it defaults to ascending.

### Use Cases

- **Top-N Queries**: Selecting a specific number of top or bottom records can be accomplished using **ORDER BY** along with **LIMIT** or **OFFSET**.
- **Trends Identification**: With **ORDER BY**, you can identify trends or patterns in your data, such as ranking by sales volume or time-based sequences.

- **Improved Data Presentation**: By sorting records in a logical order, you can enhance the visual appeal and comprehension of your data representations.

### Code Example: Order by Multiple Columns and Limit Results

Let's say you have a "sales" table with columns `product_name`, `sale_date`, and `units_sold`. You want to fetch the top 3 products that sold the most units on a specific date, sorted by units sold (in descending order) and product name (in ascending order).

Here is the SQL query:

```sql
SELECT product_name, sale_date, units_sold
FROM sales
WHERE sale_date = '2022-01-15'
ORDER BY units_sold DESC, product_name ASC
LIMIT 3;
```

The expected result will show the top 3 products with the highest units sold on the given date. If two products have the same number of units sold, they will be sorted in alphabetical order by their names.

### SQL Server Specific: Order by Column Position

In **SQL Server**, you can also use the column position in the ORDER BY clause. For example, instead of using column names, you can use 1 for the first column, 2 for the second, and so on. This syntax:

```sql
SELECT product_name, sale_date, units_sold
FROM sales
WHERE sale_date = '2022-01-15'
ORDER BY 3 DESC, 1 ASC
LIMIT 3;
```

performs the same operation as the previous example.

### MySQL Specific: Random Order

In **MySQL**, you can reorder the results in a random sequence. This can be useful, for instance, in a quiz app to randomize the order of questions. The **ORDER BY** clause with the **RAND()** function looks like this:

```sql
SELECT product_name
FROM products
ORDER BY RAND()
LIMIT 1;
```

<br>


## A comprehensive guide before your next interview

**1. SQL was developed as an integral part of**

SQL - Structured Query Language is a domain-specific language used in programming and designed for managing data held in a relational database management system (RDBMS), or for stream processing in a relational data stream management system (RDSMS).

Although SQL is an ANSI (American National Standards Institute) standard, there are different versions of the SQL language.

However, to be compliant with the ANSI standard, they all support at least the major commands (such as SELECT, UPDATE, DELETE, INSERT, WHERE) in a similar manner.

Note: Most of the SQL database programs also have their own proprietary extensions in addition to the SQL standard!

What Can SQL do?

-   SQL can execute queries against a database
-   SQL can retrieve data from a database
-   SQL can insert records in a database
-   SQL can update records in a database
-   SQL can delete records from a database
-   SQL can create new databases
-   SQL can create new tables in a database
-   SQL can create stored procedures in a database
-   SQL can create views in a database
-   SQL can set permissions on tables, procedures, and views

**2. What does SQL stand for?**

SQL stands for Structured Query Language.
SQL lets you access and manipulate databases.
SQL is an ANSI (American National Standards Institute) standard.

**3. SQL is (referring to it as a Programming Language)**

**SQL is a declarative language in which the expected result or operation is given without the specific details about how to accomplish the task.** The steps required to execute SQL statements are handled transparently by the SQL database. Sometimes **SQL is characterized as non-procedural** because procedural languages generally require the details of the operations to be specified, such as opening and closing tables, loading and searching indexes, or flushing buffers and writing data to filesystems. Therefore, SQL is considered to be designed at a higher conceptual level of operation than procedural languages because the lower level logical and physical operations aren't specified and are determined by the SQL engine or server process that executes it.

**4. Which of the following is NOT a SQL command? (SELECT, REMOVE, UPDATE, INSERT)**

REMOVE is not a valid SQL command.  

**Some of The Most Important SQL Commands**

```sql
SELECT - extracts data from a database
UPDATE - updates data in a database
DELETE - deletes data from a database
INSERT INTO - inserts new data into a database
CREATE DATABASE - creates a new database
ALTER DATABASE - modifies a database
CREATE TABLE - creates a new table
ALTER TABLE - modifies a table
DROP TABLE - deletes a table
CREATE INDEX - creates an index (search key)
DROP INDEX - deletes an index
```


**5. What is MySQL?**

MySQL is the most popular Open Source SQL database management system developed, distributed, and supported by Oracle Corporation.

**Another common SQL interview question regarding MySQL may come in a different form**

“**What is the difference between SQL and MySQL?**” or **Difference between SQL and MySQL:**

SQL is a structured query language that is used for manipulating and accessing the relational database, on the other hand, MySQL itself is a relational database that uses SQL as the standard database language.


**6. What are some properties of PL/SQL?**

PL/SQL is a combination of SQL along with the procedural features of programming languages. It was developed by Oracle Corporation in the early 90's to enhance the capabilities of SQL. PL/SQL is one of three key programming languages embedded in the Oracle Database, along with SQL itself and Java.

Another common SQL interview question regarding PL/SQL may come in a different form:

##### “What is the difference between SQL and PL/SQL? or Difference between SQL and PL/SQL:

SQL is a Structured Query Language used to issue a single query or execute a single insert/update/delete.

- PL-SQL is a programming language SQL, used to write full programs using variables, loops,operators etc. to carry out multiple selects/inserts/updates/deletes.

- SQL may be considered as the source of data for our reports, web pages and screens.

- PL/SQL can be considered as the application language similar to Java or PHP. It might be the language used to build, format and display those reports, web pages and screens.

- SQL is a data oriented language used to select and manipulate sets of data.

- PL/SQL is a procedural language used to create applications.

#### **SQL vs. PL-SQL**

- SQL is used to write queries, DDL and DML statements.
- PL/SQL is used to write program blocks, functions, procedures triggers,and packages.
- SQL is executed one statement at a time.
- PL/SQL is executed as a block of code.
- SQL is declarative, i.e., it tells the database what to do but not how to do it. Whereas, PL/SQL is procedural, i.e., it tells the database how to do things.
- SQL can be embedded within a PL/SQL program. But PL/SQL cant be embedded within a SQL statement.

**7. What are the possible values for the BOOLEAN data field in MySQL?**

MySQL uses TINYINT(1) data type to represent boolean values. A value of zero is considered false . Non-zero values are considered true .

**8. What data type would you choose if you wanted to store the distance (rounded to the nearest mile)?**

```sql
INTEGER (or INT )   
```

**9. Which are valid SQL keywords (statements & clauses)**

- SELECT - extracts data from a database
- FROM - clause is used to specify the tables to extract data from
- WHERE - clause is used to extract only those records that fulfill a specified condition.
- GROUP BY - is often used with aggregate functions (COUNT , MAX , MIN , SUM , AVG ) to group the result-set by one or more columns.
- HAVING - clause was added to SQL because the WHERE keyword could not be used with aggregate functions.
- ORDER BY - keyword is used to sort the result-set in ascending or descending order.
- UPDATE - updates data in a database
- DELETE - deletes data from a database
- INSERT INTO - inserts new data into a database
- CREATE DATABASE - creates a new database
- ALTER DATABASE - modifies a database
- CREATE TABLE - creates a new table
- ALTER TABLE - modifies a table
- DROP TABLE - deletes a table
- CREATE INDEX - creates an index (search key)
- DROP INDEX - deletes an index


**10. Which of the following are valid SQL comments?**

Comments are used to explain sections of SQL statements, or to prevent execution of SQL statements.

Single line comments start with -- .

Any text between -- and the end of the line will be ignored (will not be executed).

The following example uses a single-line comment as an explanation:

```sql
--Select all:
SELECT * FROM Customers;
```

Multi-line comments start with /* and end with */ .

Any text between /* and */ will be ignored.

The following example uses a multi-line comment as an explanation:

```sql
/*Select all the columns
of all the records
in the Customers table:*/
SELECT * FROM Customers;
```

The following example uses a multi-line comment to ignore many statements:

```sql
/*SELECT * FROM Customers;
SELECT * FROM Products;*/
SELECT * FROM Suppliers;
```

**11. Which SQL statement is used to extract data from a database?**

A SELECT statement retrieves zero or more rows from one or more database tables or database views.

As SQL is a declarative programming language, SELECT queries specify a result set, but do not specify how to calculate it.

The SELECT statement has many optional clauses:

- **WHERE** specifies which rows to retrieve.

- **GROUP BY** groups rows sharing a property so that an aggregate function can be applied to each group.

- **HAVING** selects among the groups defined by the GROUP BY clause.

- **ORDER BY** specifies an order in which to return the rows.

- **AS** provides an alias which can be used to temporarily rename tables or columns.  

**12. How to select all records from the table 'Products'?**

SELECT * FROM Products;

If you want to select all the fields available in the table, use the * syntax as this:

SELECT * FROM Products;

**13. Can we rename a column in the output of SQL query?**

Yes, using AS .

SQL aliases are used to give a column in a table, a temporary name.

Aliases are often used to make column names more readable.

An alias only exists for the duration of the query.

Alias Column Syntax:

-   SELECT column_name AS alias_name
-   FROM table_name;

**14. With SQL, how do you select a column named "FirstName" from a table named "Customers"?**

SELECT FirstName FROM Customers;

The SELECT statement is used to select data from a database.

The data returned is stored in a result table, called the result-set.

**SELECT Syntax**

```sql
SELECT column1, column2, ...
FROM table_name;
```

Here, column1, column2, ... are the field names of the table you want to select data from. If you want to select all the fields available in the table, use the following syntax:

SELECT * FROM table_name;

**15. What does the SQL FROM clause do?**

The SQL FROM clause is used to list the tables and any joins required for the SQL statement.

**16. Which SQL statement is used to return only different values?**

The SELECT DISTINCT statement is used to return only distinct (different) values.

Inside a table, a column often contains many duplicate values; and sometimes you only want to list the different (distinct) values.

**SELECT DISTINCT Syntax**  

```sql
SELECT DISTINCT column1, column2, ...
FROM table_name;
```


**17. Consider the following schema ADDRESSES (id, street_name, number, city, state) Which of the following query would display the distinct cities in the ADDRESSES table?**

SELECT DISTINCT city FROM addresses;

(same theory applies as for the previous question)

**18. With SQL, how do you select all the records from a table named "Customers" where the value of the column "FirstName" is "John"?**

SELECT * FROM Customers WHERE FirstName='John';

The WHERE clause is used to filter records.

The WHERE clause is used to extract only those records that fulfill a specified condition.

SQL requires single quotes around text values (most database systems will also allow double quotes).

**19. The OR operator displays a record if ANY conditions listed are true. The AND operator displays a record if ALL of the conditions listed are true.**

The WHERE clause can be combined with AND , OR , and NOT operators.

The AND and OR operators are used to filter records based on more than one condition:

The AND operator displays a record if all the conditions separated by AND are TRUE .

The OR operator displays a record if any of the conditions separated by OR are TRUE .

The NOT operator displays a record if the condition(s) is NOT TRUE .

**AND Syntax**

```sql
SELECT column1, column2, ...
FROM table_name
WHERE condition1 AND condition2 AND condition3 …;
```

**OR Syntax**

```sql
SELECT column1, column2, ...
FROM table_name
WHERE condition1 OR condition2 OR condition3 ...;
```

**NOT Syntax**

```sql
SELECT column1, column2, ...
FROM table_name
WHERE NOT condition;
```


**20. Which of the following SQL statements has correct syntax?**

SELECT * FROM Table1 WHERE Column1 >= 100

SQL Comparison Operators

- '=' Equal to

- '>' Greater than

- '<' Less than

- '>=' Greater than or equal to

- '<=' Less than or equal to

- '≠' Not equal to

**21. With SQL, how do you select all the records from a table named "Customers" where the "FirstName" is "John" and the "LastName" is "Jackson"?**

```sql
SELECT * FROM Customers WHERE FirstName='John' AND LastName='Jackson'
```

Same answers as for the previous 2 questions.

You must use the AND operator that displays a record if all the conditions separated by AND are TRUE and the = (‘equal’) comparison operator.

**22. How to select random 10 rows from a table?**

The easiest way to generate random rows in MySQL is to use the ORDER BY RAND() clause.

```sql
SELECT * FROM tbl ORDER BY RAND() LIMIT 10;
```

This can work fine for small tables. However, for big table, it will have a serious performance problem as in order to generate the list of random rows, MySQL need to assign random number to each row and then sort them.

Even if you want only 10 random rows from a set of 100k rows, MySQL need to sort all the 100k rows and then, extract only 10 of them.

**23. Examine the following code. What will the value of price be if the statement finds a NULL value? SELECT name, ISNULL(price, 50) FROM PRODUCTS**

What is a NULL Value?

A field with a NULL value is a field with no value.

If a field in a table is optional, it is possible to insert a new record or update a record without adding a value to this field. Then, the field will be saved with a NULL value.

Note: It is very important to understand that a NULL value is different from a zero value or a field that contains spaces. A field with a NULL value is one that has been left blank during record creation!

**How can you return a default value for a NULL?**

**MySQL**

The MySQL IFNULL() function lets you return an alternative value if an expression is NULL:

```sql
SELECT ProductName, UnitPrice * (UnitsInStock + IFNULL(UnitsOnOrder, 0))
FROM Products
```

or we can use the COALESCE() function, like this:

```sql
SELECT ProductName, UnitPrice * (UnitsInStock + COALESCE(UnitsOnOrder, 0))
FROM Products
```

**SQL Server**

The **SQL Server** ISNULL() function lets you return an alternative value when an expression is NULL :

-   SELECT ProductName, UnitPrice * (UnitsInStock + ISNULL(UnitsOnOrder, 0))
-   FROM Products

**MS Access**

The MS Access IsNull() function returns TRUE (-1) if the expression is a null value, otherwise FALSE (0) :

-   SELECT ProductName, UnitPrice * (UnitsInStock + IIF(IsNull(UnitsOnOrder), 0, UnitsOnOrder))
-   FROM Products

**Oracle**

The Oracle NVL() function achieves the same result:

-   SELECT ProductName, UnitPrice * (UnitsInStock + NVL(UnitsOnOrder, 0))
-   FROM Products  

**24. Which operator is used to search for a specified text pattern in a column?**

The LIKE operator is used in a WHERE clause to search for a specified pattern in a column.

There are two wildcards used in conjunction with the LIKE operator:

% - The percent sign represents zero, one, or multiple characters

_ - The underscore represents a single character

Examples:

-   WHERE CustomerName LIKE 'a%' -- Finds any values that starts with  "a"
-   WHERE CustomerName LIKE '%a' -- Finds any values that ends with  "a"
-   WHERE CustomerName LIKE '%or%' -- Finds any values that have "or"  in any position
-   WHERE CustomerName LIKE '_r%' -- Finds any values that have "r"  in the second position
-   WHERE CustomerName LIKE 'a_%_%' -- Finds any values that starts with  "a"  and are at least 3 characters in length
-   WHERE ContactName LIKE 'a%o' -- Finds any values that starts with  "a"  and ends with  "o"

**25. How to write a query to show the details of a student from Students table whose FirstName starts with 'K'?**

SELECT * FROM Students WHERE FirstName LIKE 'K%'.

Explanation from previous question applies.

**26. Which operator is used to select values within a range?**

The BETWEEN operator selects values within a given range. The values can be numbers, text, or dates.

The BETWEEN operator is inclusive: begin and end values are included.

**BETWEEN Syntax**

```sql
SELECT column_name(s)
FROM table_name
WHERE column_name BETWEEN value1 AND value2;
```

**27. Which of the following SQL statements is correct?**

SELECT * FROM Sales WHERE Date BETWEEN '01/12/2017' AND '01/01/2018'  

Explanation from previous question applies.

**28. With SQL, how do you select all the records from a table named "Customers" where the "LastName" is alphabetically between (and including) "Brooks" and "Gray"?**

```sql
SELECT * FROM Customers WHERE LastName BETWEEN 'Brooks' AND 'Gray'  
```

Explanation from previous question applies.  

**29. The 'IN' SQL keyword…**

The IN operator allows you to specify multiple values in a WHERE clause.

The IN operator is a shorthand for multiple OR conditions.

**IN Syntax**

```sql
SELECT column_name(s)
FROM table_name
WHERE column_name IN (value1, value2, ...);
```

or:

```sql
SELECT column_name(s)
FROM table_name
WHERE column_name IN (SELECT STATEMENT); --subquery
```

**30. What does UPPER function do?**

The UPPER() function converts a string to upper-case.

**31. What function to use to round a number to the smallest integer value that is greater than or equal to a number?**

The CEILING() function returns the smallest integer value that is greater than or equal to the specified number.

The CEIL() function is a synonym for the CEILING() function and also returns the smallest integer value that is greater than or equal to a number.

Example:

SELECT CEILING(25.50); -- returns 26  

**32. How to get current date in MySQL (without time)?**

The CURDATE() function returns the current date. This function returns the current date as a YYYY-MM-DD format if used in a string context, and as a YYYYMMDD format if used in a numeric context.

The CURRENT_DATE() function is a synonym for the CURDATE() function.

**33. Which SQL keyword is used to retrieve a maximum value?**

The MAX() function returns the largest value of the selected column.

**MAX() Syntax**

```sql
SELECT MAX(column_name)
FROM table_name
WHERE condition;
```

**34. Which SQL functions is used to count the number of results?**

The COUNT() function returns the number of rows that matches a specified criteria.

**COUNT() Syntax**

```sql
SELECT COUNT(column_name)
FROM table_name
WHERE condition;
```


**35. Which of the following are Aggregate Functions?**

SQL Aggregate functions are:

- **COUNT** counts how many rows are in a particular column.

- **SUM** adds together all the values in a particular column.

- **MIN** and **MAX** return the lowest and highest values in a particular column, respectively.

- **AVG** calculates the average of a group of selected values.

**36. With SQL, how can you return the number of records in the "Customers" table?**

**SELECT COUNT(*) FROM Customers**

Same answer as for the previous question.

**37. Which 2 SQL keywords specify the sorting direction of the result set retrieved with ORDER BY clause?**

**ASC | DESC**

The ORDER BY keyword is used to sort the result-set in ascending or descending order.

**ORDER BY Syntax**

```sql
SELECT column1, column2, ...
FROM table_name
ORDER BY column1, column2, ... ASC|DESC;
```


**38. If you don't specify ASC or DESC for an ORDER BY clause, the following is used by default:**

The ORDER BY keyword sorts the records in ascending order by default. To sort the records in descending order, use the DESC keyword.

**39. Suppose a Students table has two columns, name and mark. How to get name and mark of top three students?**

SELECT name, mark FROM Students ORDER BY mark DESC LIMIT 3;  

You have to use ORDER BY to order students by their marks and then select just the first 3 records.

The ORDER BY keyword sorts the records in ascending order by default. To sort the records in descending order, use the DESC keyword.

**The SQL SELECT TOP Clause**

The **SELECT TOP** clause is used to specify the number of records to return.

The SELECT TOP clause is useful on large tables with thousands of records. Returning a large number of records can impact on performance.

Not all database systems support the SELECT TOP clause.**MySQL supports the** LIMIT clause to select a limited number of records, while **Oracle uses** ROWNUM .

**40. With SQL, how can you return all the records from a table named "Customers" sorted descending by "FirstName"?**

```sql
SELECT * FROM Customers ORDER BY FirstName DESC;
```

Same answers as for the previous 2 questions apply.

**41. Which of the following SQL statements is correct?**

_SELECT name, COUNT(name) FROM customers GROUP BY name_

SQL aggregate functions like COUNT , AVG , and SUM have something in common: they all aggregate across the entire table. But what if you want to aggregate only part of a table? For example, you might want to count the customers having the same name. In situations like this, you’d need to use the GROUP BY clause. GROUP BY allows you to separate data into groups, which can be aggregated independently of one another.

**42. What is the difference between HAVING clause and WHERE clause?**

Both specify a search condition but HAVING clause is used only with the SELECT statement and typically used with GROUP BY clause.

If GROUP BY clause is not used then HAVING behaves like WHERE clause only.

Here are some other differences:

- HAVING filters records that work on summarized GROUP BY results.

- HAVING applies to summarized group records, whereas WHERE applies to individual records.

- Only the groups that meet the HAVING criteria will be returned.

- HAVING requires that a GROUP BY clause is present.

- WHERE and HAVING can be in the same query.

**43. What is JOIN used for?**

A JOIN clause is used to combine rows from two or more tables, based on a related column between them.

**44. What is the most common type of join?**

The most common type of join used in day to day queries is INNER JOIN .

**45. What are different JOINS used in SQL?**

There are several types of joins:

**SQL INNER JOIN Keyword**

The INNER JOIN keyword selects records that have matching values in both tables.

**SQL LEFT JOIN Keyword**

The LEFT JOIN keyword returns all records from the left table (table1), and the matched records from the right table (table2). The result is NULL from the right side, if there is no match.

**SQL RIGHT JOIN Keyword**

The RIGHT JOIN keyword returns all records from the right table (table2), and the matched records from the left table (table1). The result is NULL from the left side, when there is no match.

**SQL FULL OUTER JOIN Keyword**

The FULL OUTER JOIN keyword return all records when there is a match in either left (table1) or right (table2) table records.

Note: FULL OUTER JOIN can potentially return very large result-sets!

**SQL Self JOIN**

A self JOIN is a regular join, but the table is joined with itself.

**SQL CROSS JOIN**

The SQL CROSS JOIN produces a result set which is the number of rows in the first table multiplied by the number of rows in the second table if no WHERE clause is used along with CROSS JOIN .This kind of result is called as Cartesian Product.

If WHERE clause is used with CROSS JOIN , it functions like an INNER JOIN .

**46. Which of the following is NOT TRUE about the ON clause?**

ON clause is used to specify conditions or specify columns to JOIN . ON clause makes the query easy to understand. ON clause allows 3 way (or more) joins.

**47. Assume that Table A is joined to Table B. An inner join:**

**Displays rows (records) only when the values of the Key in table A and the foreign key in table B are equal.**

The INNER JOIN keyword selects records that have matching values in both tables.

**48. Can you join 3 tables together with inner join:**

Yes. You can join multiple tables with inner join.

For example, for a Faculty table the lookup tables might be Division, with DivisionID as the PK, Country, with CountryID as the PK, and Nationality, with NationalityID as the PK. To join Faculty to the Division, Country, and Nationality tables, the fields DivisionID, CountryID and NationalityID would need to be foreign keys in the Faculty table.  

The SQL to join them would then be:

```sql
SELECT <fieldlist> FROM Faculty AS f
INNER JOIN Division AS d ON d.FacultyID = f.FacultyID
INNER JOIN Country AS c ON c.FacultyID = f.FacultyID
INNER JOIN Nationality AS n ON n.FacultyID = f.FacultyID
```


**49. Can you join a table to itself?**

Yes. The operation is called self join.

**Self JOIN Syntax**

```sql
SELECT column_name(s)
FROM table1 T1, table1 T2
WHERE condition;
```

**50. Which of the following is true about Cartesian Products?**

**A Cartesian product is formed when a join condition is omitted.**

The SQL CROSS JOIN produces a result set which is the number of rows in the first table multiplied by the number of rows in the second table if no WHERE clause is used along with CROSS JOIN .This kind of result is called as Cartesian Product.

If WHERE clause is used with CROSS JOIN , it functions like an INNER JOIN .

**CROSS JOIN Syntax**  

```sql
SELECT *
FROM table1
CROSS JOIN table2;
```

**51. In relational algebra the INTERSECTION of two sets (set A and Set B) corresponds to**

**A AND B**

The SQL INTERSECT clause/operator is used to combine two SELECT statements, but returns rows only from the first SELECT statement that are identical to a row in the second SELECT statement. This means INTERSECT returns only common rows returned by the two SELECT statements.

**INTERSECT Syntax**

```sql
SELECT column1 [, column2 ]
FROM table1 [, table2 ]
[WHERE condition]
INTERSECT
SELECT column1 [, column2 ]
FROM table1 [, table2 ]
[WHERE condition]
```


**52. In relational algebra the UNION of two sets (set A and Set B) corresponds to**

**A OR B**

The SQL UNION clause/operator is used to combine the results of two or more SELECT statements without returning any duplicate rows.

To use this UNION clause, each SELECT statement must have

The same number of columns selected

The same number of column expressions

The same data type and

Have them in the same order

But they need not have to be in the same length.

**Union Syntax**

```sql
SELECT column1 [, column2 ]
FROM table1 [, table2 ]
[WHERE condition]
UNION
SELECT column1 [, column2 ]
FROM table1 [, table2 ]
[WHERE condition]
```

**53. What is the difference between UNION and UNION ALL?**

**UNION** – returns all distinct rows selected by either query

**UNION ALL** – returns all rows selected by either query, including all duplicates.

**54. Having a list of Customer Names that searched for product 'X' and a list of customer Names that bought the product 'X'. What set operator would you use to get only those who are interested but did not bought product 'X' yet?**

**MINUS**

The **SQL** MINUS operator is used to return all rows in the first SELECT statement that are not returned by the second SELECT statement. Each SELECT statement will define a dataset. The MINUS operator will retrieve all records from the first dataset and then remove from the results all records from the second dataset.

**MINUS Syntax**

```sql
SELECT expression1, expression2, ... expression_n
FROM tables
[WHERE conditions]
MINUS
SELECT expression1, expression2, ... expression_n
FROM tables
[WHERE conditions];
```


**55. One (or more) select statement whose return values are used in filtering conditions of the main query is called**

**Subquery.**

A subquery is a SQL query nested inside a main query.

A subquery may occur in :

```sql
A SELECT clause
A FROM clause
A WHERE clause
```

A subquery is usually added within the WHERE clause of another SQL SELECT statement.

You can use the comparison operators, such as > , < , or = . The comparison operator can also be a multiple-row operator, such as IN , ANY , or ALL .

A subquery is also called an inner query or inner select, while the statement containing a subquery is also called an outer query or outer select.

The inner query executes first before its parent query so that the results of an inner query can be passed to the outer query.

**56. Subqueries can be nested in...**

The subquery can be nested inside a SELECT , INSERT , UPDATE , or DELETE statement or inside another subquery.  

**57. What row comparison operators can be used with a subquery?**

You can use the comparison operators, such as > , < , or = . The comparison operator can also be a multiple-row operator, such as IN , ANY , or ALL .

**58. A subquery that is used inside a subquery is called a**

Subquery within another subquery is called as **Nested Subquery**.

**59. A subquery that uses a correlation name from the outer query is called a**

If the output of a subquery is depending on column values of the parent query table then the query is called **Correlated Subquery**.

**60. What is Case Function?**

The CASE function lets you evaluate conditions and return a value when the first condition is met (like an IF-THEN-ELSE statement).

**CASE Syntax**

```sql
CASE expression
WHEN condition1 THEN result1
WHEN condition2 THEN result2
...
WHEN conditionN THEN resultN
ELSE result
END  
```

**61. What are different types of statements supported by SQL?**

**DDL** (Data Definition Language): It is used to define the database structure such as tables. It includes three statements such as Create, Alter, and Drop.

**DML** (Data Manipulation Language): These statements are used to manipulate the data in records. Commonly used DML statements are Select, Insert, Update, and Delete.

Note: Some people prefer to assign the SELECT statement to a category of its own called: DQL. Data Query Language.

**DCL** (Data Control Language): These statements are used to set privileges such as Grant and Revoke database access permission to the specific user.

**62. Which of the following SQL statements are DDL**

**DDL statements include:**

- ALTER Statements

- CREATE Statements

- DROP Statements

- TRUNCATE TABLE

**63. DML includes the following SQL statements**

**DML statements are:**

```sql
SELECT
INSERT
UPDATE
DELETE
```


**64. Grant and Revoke commands are under**

**DCL (Data Control Language)**

GRANT : Used to provide any user access privileges or other privileges for the database.

REVOKE : Used to take back permissions from any user.

**65. Which of the following is not a DML statement?**

Same answer as for the previous questions.

COMMIT is used to persist changes performed during a TRANSACTION.

**66. Which SQL statement is used to insert new data in a database?**

The INSERT INTO statement is used to insert new records in a table.

**67. When inserting data in a table do you have to specify the list of columns you are inserting values for?**

**INSERT INTO Syntax**

It is possible to write the INSERT INTO statement in two ways.

The first way specifies both the column names and the values to be inserted:

```sql
INSERT INTO table_name (column1, column2, column3, ...)
VALUES (value1, value2, value3, ...);
```

If you are adding values for all the columns of the table, you do not need to specify the column names in the SQL query. However, make sure the order of the values is in the same order as the columns in the table. The INSERT INTO syntax would be as follows:

```sql
INSERT INTO table_name
VALUES (value1, value2, value3, ...);
```

**68. With SQL, how can you insert a new record into the "Customers" table?**

Same answer as for the previous question.

**69. With SQL, how can you insert "Hawkins" as the "LastName" in the "Customers" table?**

Same answer as for the previous question.

**70. How do you create a temporary table in MySQL?**

To create a temporary table, you just need to add the TEMPORARY keyword to the CREATE TABLE statement.

Example:

```sql
CREATE TEMPORARY TABLE top10customers
SELECT customer.fname, customer.lname
FROM customers
```

-   /* all the conditions to fecth the top 10 customers */

**71. Which SQL statement is used to update data in a database?**

The UPDATE statement is used to modify the existing records in a table.

**UPDATE Syntax**

```
-   UPDATE table_name
-   SET column1 = value1, column2 = value2, ...
-   WHERE condition;
```

**72. What is the keyword is used in an UPDATE query to modify the existing value?**

The answer is SET .

**UPDATE Syntax**

```sql
UPDATE table_name
SET column1 = value1, column2 = value2, ...
WHERE condition;
```

**73. How can you change "Jackson" into "Hawkins" in the "LastName" column in the Customer table?**

```sql
UPDATE Customers SET LastName='Hawkins' WHERE LastName='Jackson'  
```

Same explanation as for the previous question.

**74. Which SQL statement is used to delete data from a database?**

The DELETE statement is used to delete existing records in a table.

**DELETE Syntax**

```sql
DELETE FROM table_name
WHERE condition;
```


**75. With SQL, how can you delete the records where the "FirstName" is "John" in the Customers Table?**

DELETE FROM Customers WHERE FirstName = 'John'  

**76. The FROM SQL keyword is used to**

Specify the table we are are selecting or deleting from.

**DELETE Syntax**

```sql
DELETE FROM table_name
WHERE condition;
```

**UPDATE Syntax**

```sql
UPDATE table_name
SET column1 = value1, column2 = value2, ...
WHERE condition;
```

**INSERT Syntax**

```sql
INSERT INTO table_name (column1, column2, column3, ...)
VALUES (value1, value2, value3, ...);
```

**SELECT Syntax**

```sql
SELECT column1, column2, ...
FROM table_name;
```

**77. What is the difference between DELETE and TRUNCATE?**

The basic difference in both is DELETE is DML command and TRUNCATE is DDL.

DELETE is used to delete a specific row from the table whereas TRUNCATE is used to remove all rows from the table

We can use DELETE with WHERE clause but cannot use TRUNCATE with it.

**78. Which SQL statement is used to create a table in a database?**

The CREATE TABLE statement is used to create a new table in a database.

**Syntax**

```sql
CREATE TABLE table_name (
column1 datatype,
column2 datatype,
column3 datatype,
....
);
```

**79. What is Collation in SQL?**

A collation is a set of rules that defines how to compare and sort character strings.

**Detailed Explanation:**

A character set is a set of symbols and encodings. A collation is a set of rules for comparing characters in a character set. Let's make the distinction clear with an example of an imaginary character set.

Suppose that we have an alphabet with four letters: A, B, a, b. We give each letter a number: A = 0, B = 1, a = 2, b = 3. The letter A is a symbol, the number 0 is the encoding for A, and the combination of all four letters and their encodings is a character set.

Suppose that we want to compare two string values, A and B. The simplest way to do this is to look at the encodings: 0 for A and 1 for B. Because 0 is less than 1, we say A is less than B. What we've just done is apply a collation to our character set. The collation is a set of rules (only one rule in this case): “compare the encodings.” We call this simplest of all possible collations a binary collation.

But what if we want to say that the lowercase and uppercase letters are equivalent? Then we would have at least two rules: (1) treat the lowercase letters a and b as equivalent to A and B; (2) then compare the encodings. We call this a case-insensitive collation. It is a little more complex than a binary collation.

In real life, most character sets have many characters: not just A and B but whole alphabets, sometimes multiple alphabets or eastern writing systems with thousands of characters, along with many special symbols and punctuation marks. Also in real life, most collations have many rules, not just for whether to distinguish lettercase, but also for whether to distinguish accents (an “accent” is a mark attached to a character as in German Ö), and for multiple-character mappings (such as the rule that Ö = OE in one of the two German collations).

_Source: dev.mysql.com_

**80. What is true about an AUTO_INCREMENT column in SQL?**

AUTO_INCREMENT allows a unique number to be generated automatically when a new record is inserted into a table. Often this is the primary key field that we would like to be created automatically every time a new record is inserted.

**Syntax for MySQL**

The following SQL statement defines the "ID" column to be an auto-increment primary key field in the "Persons" table:

```sql
CREATE TABLE Persons (
ID int NOT NULL AUTO_INCREMENT,
LastName varchar(255) NOT NULL,
FirstName varchar(255),
Age int,
PRIMARY KEY (ID)
);
```

**Syntax for SQL Server**

The following SQL statement defines the "ID" column to be an auto-increment primary key field in the "Persons" table:

```sql
CREATE TABLE Persons (
ID int IDENTITY(1,1) PRIMARY KEY,
LastName varchar(255) NOT NULL,
FirstName varchar(255),
Age int
);
```

**Syntax for Oracle**

In Oracle the code is a little bit more tricky.

You will have to create an auto-increment field with the sequence object (this object generates a number sequence).

Use the following **CREATE SEQUENCE syntax:**

```sql
CREATE SEQUENCE seq_person
MINVALUE 1
START WITH 1
INCREMENT BY 1
CACHE 10;
```

The code above creates a sequence object called seq_person, that starts with 1 and will increment by 1. It will also cache up to 10 values for performance. The cache option specifies how many sequence values will be stored in memory for faster access.

To insert a new record into the "Persons" table, we will have to use the nextval function (this function retrieves the next value from seq_person sequence):

```sql
INSERT INTO Persons (ID,FirstName,LastName)
VALUES (seq_person.nextval,'Lars','Monsen');
```

The SQL statement above would insert a new record into the "Persons" table. The "ID" column would be assigned the next number from the seq_person sequence. The "FirstName" column would be set to "Lars" and the "LastName" column would be set to "Monsen".

**81. What are valid constraints in MySQL?**

SQL constraints are used to specify rules for the data in a table.

Constraints are used to limit the type of data that can go into a table. This ensures the accuracy and reliability of the data in the table. If there is any violation between the constraint and the data action, the action is aborted.

Constraints can be column level or table level. Column level constraints apply to a column, and table level constraints apply to the whole table.

The following constraints are commonly used in SQL:

- NOT NULL - Ensures that a column cannot have a NULL value

- UNIQUE - Ensures that all values in a column are different

- PRIMARY KEY - A combination of a NOT NULL and UNIQUE . Uniquely identifies each row in a table

- FOREIGN KEY - Uniquely identifies a row/record in another table

- CHECK - Ensures that all values in a column satisfies a specific condition

- DEFAULT - Sets a default value for a column when no value is specified

- INDEX - Used to create and retrieve data from the database very quickly

  

**82. Which of the following is NOT TRUE about constraints?**

NOT NULL - Ensures that a column cannot have a NULL value

UNIQUE - Ensures that all values in a column are different

PRIMARY KEY - A combination of a NOT NULL and UNIQUE . Uniquely identifies each row in a table

FOREIGN KEY - Uniquely identifies a row/record in another table

  

**83. The NOT NULL constraint enforces a column to not accept null values.**

NOT NULL - Ensures that a column cannot have a NULL value.

**84. An unique (non-key) field**

The UNIQUE constraint ensures that all values in a column are different.

 

**85. What is CHECK Constraint?**

The CHECK constraint is used to limit the value range that can be placed in a column.

If you define a CHECK constraint on a single column it allows only certain values for this column.

If you define a CHECK constraint on a table it can limit the values in certain columns based on values in other columns in the row.

**SQL CHECK on CREATE TABLE**

The following SQL creates a CHECK constraint on the "Age" column when the "Persons" table is created. The CHECK constraint ensures that you can not have any person below 18 years:

**MySQL:**

```sql
CREATE TABLE Persons (
ID int NOT NULL,
LastName varchar(255) NOT NULL,
FirstName varchar(255),
Age int,
CHECK (Age>=18)
);
```

**SQL Server / Oracle / MS Access:**

-   CREATE TABLE Persons (
-   ID int NOT NULL,
-   LastName varchar(255) NOT NULL,
-   FirstName varchar(255),
-   Age int CHECK (Age>=18)
-   );

**86. What is the difference between UNIQUE and PRIMARY KEY constraints?**

UNIQUE vs PRIMARY KEY

Both the UNIQUE and PRIMARY KEY constraints provide a guarantee for uniqueness for a column or set of columns.

A PRIMARY KEY constraint automatically has a UNIQUE constraint.

However, you can have many UNIQUE constraints per table, but only one PRIMARY KEY constraint per table.

**87. What does SQL DROP TABLE clause do?**

The DROP TABLE statement is used to drop an existing table in a database.

**Syntax**

```sql
DROP TABLE table_name;
```


**88. What is the difference between DROP and TRUNCATE?**

TRUNCATE removes all rows from the table which cannot be retrieved back, DROP removes the entire table from the database and it cannot be retrieved back.  

**89. How do you add a 'order_date' column to a table called 'order'?**

ALTER TABLE order ADD order_date DATE

The ALTER TABLE statement is used to add, delete, or modify columns in an existing table.

The ALTER TABLE statement is also used to add and drop various constraints on an existing table.

**ALTER TABLE - ADD Column**

To add a column in a table, use the following syntax:

```sql
ALTER TABLE table_name
ADD column_name datatype;
```

**90. What the correct syntax to rename column 'Address' to 'Addr' in 'Customer' table?**

ALTER TABLE Customer CHANGE Address Addr varchar(50);

ALTER TABLE - CHANGE

You rename a column using the ALTER TABLE and CHANGE commands together to change an existing column.

**Syntax**

```sql
ALTER TABLE table_name
CHANGE oldname newname datatype 
```


**91. Consider the following schema ADDRESSES (id, street_name, number, city, state) Which code snippet will alter the table ADDRESSES and delete the column named CITY?**

ALTER TABLE addresses DROP COLUMN city;

ALTER TABLE - DROP COLUMN

To delete a column in a table, use the following syntax (notice that some database systems don't allow deleting a column):

```sql
ALTER TABLE table_name
DROP COLUMN column_name;
```

**92. What are Indexes in SQL?**

Indexes are used to retrieve data from the database very fast. The users cannot see the indexes, they are just used to speed up searches/queries.

Note: Updating a table with indexes takes more time than updating a table without (because the indexes also need an update). So, only create indexes on columns that will be frequently searched against.

**CREATE INDEX Syntax**

Creates an index on a table. Duplicate values are allowed:

```sql
CREATE INDEX index_name
ON table_name (column1, column2, ...)
```


**93. What is the difference between clustered and non-clustered indexes? Which of the following statements are true?**

One table can have only one clustered index but multiple nonclustered indexes.

Clustered indexes can be read rapidly rather than non-clustered indexes.

Clustered indexes store data physically in the table or view and non-clustered indexes do not store data in table as it has separate structure from data row.

**94. The statement for assigning access privileges is**

SQL GRANT and REVOKE commands are used to implement privileges in SQL multiple user environments. The administrator of the database can grant or revoke privileges to or from users of database object like SELECT , INSERT , UPDATE , DELETE , ALL etc.

GRANT Command: This command is used provide database access to user apart from an administrator.

**Syntax**

```sql
GRANT privilege_name
ON object_name
TO {user_name|PUBLIC|role_name}
[WITH GRANT OPTION];
```

In above syntax WITH GRANT OPTIONS indicates that the user can grant the access to another user too.

REVOKE Command: This command is used provide database deny or remove access to database objects.

**Syntax**

```sql
REVOKE privilege_name
ON object_name
FROM {user_name|PUBLIC|role_name}
```


**95. How many types of Privileges are available in SQL?**

There are two types of privileges used in SQL, such as

**System Privilege**: System privileges deal with an object of a particular type and specifies the right to perform one or more actions on it which include Admin allows a user to perform administrative tasks, ALTER ANY INDEX, ALTER ANY CACHE GROUP CREATE/ALTER/DELETE TABLE, CREATE/ALTER/DELETE VIEW etc.

**Object Privilege**: This allows to perform actions on an object or object of another user(s) viz. table, view, indexes etc. Some of the object privileges are EXECUTE, INSERT, UPDATE, DELETE, SELECT, FLUSH, LOAD, INDEX, REFERENCES etc.

**96. List the various privileges that a user can grant to another user?**

Same answers as for the previous question

**97. What does the term 'locking' refer to?**

Locking is a process preventing users from reading data being changed by other users, and prevents concurrent users from changing the same data at the same time.  

**98. What are a transaction's main controls?**

COMMIT command is used to permanently save any transaction into the database.

When we use any DML command like INSERT , UPDATE or DELETE , the changes made by these commands are not permanent, until the current session is closed, the changes made by these commands can be rolled back.

To avoid that, we use the COMMIT command to mark the changes as permanent.

Following is commit command syntax:

```sql
COMMIT;
```

ROLLBACK command

This command restores the database to last committed state. It is also used with SAVEPOINT command to jump to a savepoint in an ongoing transaction.

If we have used the UPDATE command to make some changes into the database, and realise that those changes were not required, then we can use the ROLLBACK command to rollback those changes, if they were not committed using the COMMIT command.

Following is rollback command syntax:

- _ROLLBACK TO savepoint_name;_

- SAVEPOINT command

- SAVEPOINT command is used to temporarily save a transaction so that you can rollback to that point whenever required.  

Following is savepoint command syntax:

- _SAVEPOINT savepoint_name;_

In short, using this command we can name the different states of our data in any table and then rollback to that state using the ROLLBACK command whenever required.

**99. A transaction completes its execution is said to be**

Committed**.**

**100. Consider the following code: START TRANSACTION /*transaction body*/ COMMIT; ROLLBACK; What does Rollback do?**

It does nothing. Once a transaction has executed commit, its effects can no longer be undone by rollback.  

**101. What are valid properties of the transaction?**

The characteristics of these four properties as defined by Reuter and Härder are as follows:

**Atomicity**

​	Atomicity requires that each transaction be "all or nothing": if one part of the transaction fails, then the entire transaction fails, and the database state is left unchanged. An atomic system must guarantee atomicity in each and every situation, including power failures, errors and crashes. To the outside world, a committed transaction appears (by its effects on the database) to be indivisible ("atomic"), and an aborted transaction does not happen.

**Consistency**

​	The consistency property ensures that any transaction will bring the database from one valid state to another. Any data written to the database must be valid according to all defined rules, including constraints, cascades, triggers, and any combination thereof. This does not guarantee correctness of the transaction in all ways the application programmer might have wanted (that is the responsibility of application-level code), but merely that any programming errors cannot result in the violation of any defined rules.

**Isolation**

​	The isolation property ensures that the concurrent execution of transactions results in a system state that would be obtained if transactions were executed sequentially, i.e., one after the other. Providing isolation is the main goal of concurrency control. Depending on the concurrency control method (i.e., if it uses strict - as opposed to relaxed - serializability), the effects of an incomplete transaction might not even be visible to another transaction.

**Durability**

​	The durability property ensures that once a transaction has been committed, it will remain so, even in the event of power loss, crashes, or errors. In a relational database, for instance, once a group of SQL statements execute, the results need to be stored permanently (even if the database crashes immediately thereafter). To defend against power loss, transactions (or their effects) must be recorded in a non-volatile memory.

 

**102. What happens if autocommit is enabled?**

If autocommit mode is enabled, each SQL statement forms a single transaction on its own. By default, most of DBMS start the session for each new connection with autocommit enabled, so they do a commit after each SQL statement if that statement did not return an error. If a statement returns an error, the commit or rollback behavior depends on the error.

**103. What is the default isolation level used in MySQL?**

**REPEATABLE READ**

**104. In MySQL autocommit is enabled by default for each session.**

**TRUE**

**105. Which of these is also known as a virtual table in MySQL?**

**A view.**

In SQL, a view is a virtual table based on the result-set of an SQL statement.

A view contains rows and columns, just like a real table. The fields in a view are fields from one or more real tables in the database.

You can add SQL functions, WHERE , and JOIN statements to a view and present the data as if the data were coming from one single table.

**CREATE VIEW Syntax**

```sql
CREATE VIEW view_name AS
SELECT column1, column2, ...
FROM table_name
WHERE condition;
```


**106. Are views updatable using INSERT, DELETE or UPDATE?**

The SQL UPDATE VIEW command can be used to modify the data of a view.

All views are not updatable. So, UPDATE command is not applicable to all views. An updatable view is one which allows performing a UPDATE command on itself without affecting any other table.

When can a view be updated?

1. The view is defined based on one and only one table.

2. The view must include the PRIMARY KEY of the table based upon which the view has been created.

3. The view should not have any field made out of aggregate functions.

4. The view must not have any DISTINCT clause in its definition.

5. The view must not have any GROUP BY or HAVING clause in its definition.

6. The view must not have any SUBQUERIES in its definitions.

7. If the view you want to update is based upon another view, the later should be updatable.

8. Any of the selected output fields (of the view) must not use constants, strings or value expressions.  

**107. What are the advantages of Views?**

Simplify queries: You can use database view to hide the complexity of underlying tables to the end-users and external applications

All changes performed by SQL statements are executed only after a COMMIT command.

A database view helps limit data access to specific users. You may not want a subset of sensitive data can be queryable by all users

**108. Does a View contain data?**

A view does not contain any data of its own, but is like a window through which data from other tables can be viewed and changed.

The answer depends on the type of view. In case of normal view, the answer is NO it only contains query based on a base table but in case of materialized view, YES it does contain data and for the updated data in the base table, it needs to be refreshed.

**109. What SQL statement do you have to use to remove a view?**

You can delete a view with the DROP VIEW command.

**SQL DROP VIEW Syntax**

_DROP VIEW view_name;_

**110. Can a View based on another View?**

Yes, A View is based on another View.  

**111. Inside a stored procedure you to iterate over a set of rows returned by a query using a**

A CURSOR is a database object which is used to manipulate data in a row-to-row manner.

Cursor follows steps as given below:

-   Declare Cursor
-   Open Cursor
-   Retrieve row from the Cursor
-   Process the row
-   Close Cursor
-   Deallocate Cursor  

**112. How do you call the process of finding a good strategy for processing a query?**

Query optimization is a function of many relational database management systems. The query optimizer attempts to determine the most efficient way to execute a given query by considering the possible query plans.


**113. What is a trigger? or  There are triggers for…* or  A trigger is applied to**

Triggers in SQL is kind of stored procedures used to create a response to a specific action performed on the table. You can invoke triggers explicitly on the table in the database.

Triggers are, in fact, written to be executed in response to any of the following events:

- A database manipulation (DML) statement (DELETE , INSERT , or UPDATE )

- A database definition (DDL) statement (CREATE , ALTER , or DROP ).

- A database operation (SERVERERROR , LOGON , LOGOFF , STARTUP , or SHUTDOWN ).

Action and Event are two main components of SQL triggers when certain actions are performed the event occurs in response to that action.

**Syntax**

```sql
CREATE TRIGGER name {BEFORE|AFTER} (event [OR..]}
ON table_name [FOR [EACH] {ROW|STATEMENT}]
EXECUTE PROCEDURE functionname {arguments}
```

**114. A special kind of a stored procedure that executes in response to certain action on the table like insertion, deletion or updation of data is called**

**Trigger.**


**115. What is SQL Injection?**

- SQL injection is a code injection technique that might destroy your database.

- SQL injection is one of the most common web hacking techniques.

- SQL injection is the placement of malicious code in SQL statements, via web page input.

  

**_Resources_**

[www.wikipedia.org](http://www.wikipedia.org/)

[www.w3schools.com](http://www.w3schools.com/)

[www.freefeast.info](https://freefeast.info/)
