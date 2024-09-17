# Comprehensive SQL Course Notes

## 1. Introduction to SQL

- Developed by IBM in 1970
- Declarative language: You specify what you want, not how to get it
- Two main sublanguages:
  - DDL (Data Definition Language): Define and modify schema
  - DML (Data Manipulation Language): Manipulate data, write queries
- RDBMS is responsible for efficient evaluation and choosing algorithms

## 2. SQL Data Types

### 2.1 Character Types
- CHAR(M): Fixed-length string, always uses M * 4 bytes (with utf8mb4)
- VARCHAR(M): Variable-length string, uses L + 1 bytes (L = actual length)
- Example:
  ```
  CHAR(10) 'CA' -> Stores 'CA        ' (8 spaces), uses 40 bytes
  VARCHAR(10) 'CA' -> Stores 'CA', uses 3 bytes
  ```

### 2.2 Numeric Types
- Integer types (with bytes):
  - TINYINT (1), SMALLINT (2), MEDIUMINT (3), INT (4), BIGINT (8)
- Fixed-point: DECIMAL(M,D)
  - M: total digits, D: decimal digits
  - Example: DECIMAL(9,2) 1234567.89 uses 5 bytes
- Floating-point: FLOAT (4 bytes), DOUBLE (8 bytes)
  - Less precise than DECIMAL for exact values

### 2.3 Date and Time Types
- Types (with bytes): DATE (3), TIME (3), DATETIME (8), TIMESTAMP (4), YEAR (1)
- TIMESTAMP handles time zones, DATETIME doesn't
- Example:
  ```sql
  CREATE TABLE timezone_test (
    `timestamp` TIMESTAMP,
    `datetime` DATETIME
  );
  -- Insert same time in UTC
  SET SESSION time_zone = '+00:00';
  INSERT INTO timezone_test VALUES ('2029-02-14 08:47', '2029-02-14 08:47');
  -- View in different time zone
  SET SESSION time_zone = '-05:00';
  SELECT * FROM timezone_test;
  -- Result:
  -- timestamp: 2029-02-14 03:47:00 (adjusted)
  -- datetime:  2029-02-14 08:47:00 (not adjusted)
  ```

### 2.4 Other Types
- ENUM: e.g., ENUM('Yes','No','Maybe')
  - Stored as integers (1, 2, 3) but displayed as strings
- SET: e.g., SET('Pepperoni','Mushrooms','Olives')
  - Stored as bit vector, can have multiple values
- Binary types: BINARY(M), VARBINARY(M)
- Large Object types:
  - BLOB: TINYBLOB, BLOB, MEDIUMBLOB, LONGBLOB
  - TEXT: TINYTEXT, TEXT, MEDIUMTEXT, LONGTEXT

## 3. SQL DDL (Data Definition Language)

### 3.1 CREATE TABLE
```sql
CREATE TABLE Sailors (
  sid INTEGER,
  sname CHAR(20),
  rating INTEGER,
  age REAL,
  PRIMARY KEY (sid)
);

CREATE TABLE Boats (
  bid INTEGER,
  bname CHAR(20),
  color CHAR(10),
  PRIMARY KEY (bid)
);

CREATE TABLE Reserves (
  sid INTEGER,
  bid INTEGER,
  day DATE,
  PRIMARY KEY (sid, bid, day),
  FOREIGN KEY (sid) REFERENCES Sailors(sid),
  FOREIGN KEY (bid) REFERENCES Boats(bid)
);
```

### 3.2 Constraints
- Primary Key: Uniquely identifies each record
  - Must be unique and not null
- Foreign Key: References primary key in another table
  - Ensures referential integrity
- UNIQUE: Ensures all values in a column are different
- Actions on foreign key violations:
  - ON DELETE/UPDATE CASCADE: Delete/update related records
  - ON DELETE/UPDATE SET NULL: Set foreign key to NULL
  - ON DELETE/UPDATE NO ACTION: Prevent deletion/update if referenced

## 4. SQL DML (Data Manipulation Language)

### 4.1 SELECT
Basic structure:
```sql
SELECT [DISTINCT] <column_list>
FROM <table>
[WHERE <condition>]
[GROUP BY <column_list>]
[HAVING <condition>]
[ORDER BY <column_list> [ASC|DESC]];
```
- DISTINCT removes duplicates
- ORDER BY sorts results (ASC by default)

### 4.2 INSERT
```sql
-- Method 1
INSERT INTO account VALUES ('Perry', 'A-768', 1200);
-- Method 2
INSERT INTO account(bname, acct_no, balance)
VALUES ('Perry', 'A-768', 1200);
-- Method 3 (using subquery)
INSERT INTO account
SELECT bname, lno, 200
FROM loan
WHERE bname = 'Kenmore';
```

### 4.3 UPDATE
```sql
UPDATE account
SET balance = 
  CASE
    WHEN balance <= 10000 THEN balance * 1.05
    ELSE balance * 1.06
  END;
```

### 4.4 DELETE
```sql
DELETE FROM account
WHERE bname IN (SELECT bname
                FROM branch
                WHERE bcity = 'Bkln');
```

## 5. Advanced SQL Concepts

### 5.1 Aggregates
- Functions: AVG, SUM, COUNT, MAX, MIN
- Used with GROUP BY clause
- Example:
  ```sql
  SELECT AVG(S.gpa)
  FROM students S
  WHERE S.dept = 'CS';
  ```

### 5.2 Subqueries
- Queries nested within other queries
- Can be used in SELECT, FROM, WHERE clauses

### 5.3 Joins
- Combine rows from two or more tables based on a related column

### 5.4 Views
- Virtual tables based on the result of an SQL statement

## 6. SQL Integrity Constraints

- Domain Constraints: Restrict the type of data that can be stored
- Referential Integrity: Ensures relationships between tables remain consistent
- Entity Integrity: Ensures each row in a table is uniquely identifiable

## 7. Handling NULL Values

- Represents missing or unknown information
- Requires special handling in queries and constraints

## 8. Indexing

- Improves query performance
- Various types: B-tree, Hash, etc.

## 9. Transactions

- ACID properties: Atomicity, Consistency, Isolation, Durability
- Ensures data integrity in multi-user environments

These notes cover the main concepts from your SQL slides in detail, providing examples where relevant. They should give you a solid understanding of the material without having to read through all the slides. Let me know if you need any clarification or want to focus on a specific area!
