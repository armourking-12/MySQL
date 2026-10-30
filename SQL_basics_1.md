# SQL Basics – Database, Tables & Queries

This repository contains my SQL practice, covering database creation, table creation, inserting records, filtering, aggregate functions, grouping, sorting, and retrieving top records using MySQL.

## 📚 Topics Covered

1. CREATE DATABASE
2. CREATE TABLE
3. INSERT INTO
4. SELECT
5. WHERE
6. COUNT()
7. AVG()
8. SUM()
9. GROUP BY
10. HAVING
11. ORDER BY
12. LIMIT
13. TRUNCATE TABLE

---

## 1. Create Database

**Command:**

```sql
CREATE DATABASE IF NOT EXISTS college;

USE college;
```

**Explanation:**
- `CREATE DATABASE` creates a new database.
- `IF NOT EXISTS` prevents an error if the database already exists.
- `USE` selects the database.

---

## 2. Create Table

**Command:**

```sql
CREATE TABLE IF NOT EXISTS CUSTOMERS (
    ID INT PRIMARY KEY,
    NAME VARCHAR(50),
    COUNTRY VARCHAR(20),
    SCORE INT
);
```

**Table Structure:**

| Column | Data Type | Description |
|---|---|---|
| ID | INT | Customer ID (Primary Key) |
| NAME | VARCHAR(50) | Customer Name |
| COUNTRY | VARCHAR(20) | Customer Country |
| SCORE | INT | Customer Score |

---

## 3. Insert Records

**Command:**

```sql
INSERT INTO CUSTOMERS
(ID, NAME, COUNTRY, SCORE)
VALUES
(1, 'VISHAL', 'INDIA', 1000),
(2, 'AMAN', 'INDIA', 100),
(3, 'Kelly', 'CANADA', 900),
(4, 'Madam', 'CANADA', 50),
(5, 'Course', 'AUSTRALIA', 800),
(6, 'Surgy', 'AUSTRALIA', 150),
(7, 'Laila', 'CHINA', 1200),
(8, 'Kiu', 'CHINA', 300),
(9, 'George', 'RUSSIA', 1000),
(10, 'Hamilton', 'RUSSIA', 120),
(11, 'Frank', 'RUSSIA', 0);
```

---

## 4. Display All Records

**Command:**

```sql
SELECT * FROM CUSTOMERS;
```

**Output:**

| ID | NAME | COUNTRY | SCORE |
|---:|---|---|---:|
| 1 | VISHAL | INDIA | 1000 |
| 2 | AMAN | INDIA | 100 |
| 3 | Kelly | CANADA | 900 |
| 4 | Madam | CANADA | 50 |
| 5 | Course | AUSTRALIA | 800 |
| 6 | Surgy | AUSTRALIA | 150 |
| 7 | Laila | CHINA | 1200 |
| 8 | Kiu | CHINA | 300 |
| 9 | George | RUSSIA | 1000 |
| 10 | Hamilton | RUSSIA | 120 |
| 11 | Frank | RUSSIA | 0 |

---

## 5. WHERE Clause

**Goal:** Display customers from India only.

**Command:**

```sql
SELECT *
FROM CUSTOMERS
WHERE COUNTRY = 'INDIA';
```

**Output:**

| ID | NAME | COUNTRY | SCORE |
|---:|---|---|---:|
| 1 | VISHAL | INDIA | 1000 |
| 2 | AMAN | INDIA | 100 |

**Explanation:**

`WHERE` filters records based on a specified condition.

---

## 6. WHERE with Comparison Operator

**Goal:** Find customers whose score is greater than 500.

**Command:**

```sql
SELECT *
FROM CUSTOMERS
WHERE SCORE > 500;
```

**Output:**

| ID | NAME | COUNTRY | SCORE |
|---:|---|---|---:|
| 1 | VISHAL | INDIA | 1000 |
| 3 | Kelly | CANADA | 900 |
| 5 | Course | AUSTRALIA | 800 |
| 7 | Laila | CHINA | 1200 |
| 9 | George | RUSSIA | 1000 |

---

## 7. COUNT() Function

**Goal:** Count the total number of customers.

**Command:**

```sql
SELECT COUNT(*) AS total_customers
FROM CUSTOMERS;
```

**Output:**

| total_customers |
|---:|
| 11 |

**Explanation:**

`COUNT()` returns the number of records.

---

## 8. AVG() Function

**Goal:** Calculate the average score of all customers.

**Command:**

```sql
SELECT AVG(SCORE) AS average_score
FROM CUSTOMERS;
```

**Output:**

| average_score |
|---:|
| 510.91 |

**Explanation:**

`AVG()` calculates the average value of a numeric column.

---

## 9. GROUP BY Clause

**Goal:** Calculate the total score and total customers for each country.

**Command:**

```sql
SELECT
    COUNTRY,
    SUM(SCORE) AS total_score,
    COUNT(ID) AS total_customers
FROM CUSTOMERS
WHERE SCORE != 0
GROUP BY COUNTRY;
```

**Output:**

| COUNTRY | total_score | total_customers |
|---|---:|---:|
| INDIA | 1100 | 2 |
| CANADA | 950 | 2 |
| AUSTRALIA | 950 | 2 |
| CHINA | 1500 | 2 |
| RUSSIA | 1120 | 2 |

**Explanation:**

- `SUM()` adds all scores.
- `COUNT()` counts records.
- `WHERE SCORE != 0` excludes customers with zero score.
- `GROUP BY COUNTRY` creates separate groups for each country.

---

## 10. HAVING Clause

**Goal:** Display countries whose total score is greater than 1200.

**Command:**

```sql
SELECT
    COUNTRY AS customer_country,
    SUM(SCORE) AS total_score,
    COUNT(ID) AS total_customers
FROM CUSTOMERS
WHERE SCORE != 0
GROUP BY COUNTRY
HAVING SUM(SCORE) > 1200;
```

**Output:**

| customer_country | total_score | total_customers |
|---|---:|---:|
| CHINA | 1500 | 2 |

**Explanation:**

- `GROUP BY` creates groups for each country.
- `HAVING` filters groups after aggregation.
- China's total score = 1200 + 300 = 1500.
- Since 1500 is greater than 1200, only China is returned.

### Difference Between WHERE and HAVING

| WHERE | HAVING |
|---|---|
| Filters individual records | Filters grouped results |
| Applied before GROUP BY | Applied after GROUP BY |
| Generally used for row conditions | Commonly used with aggregate functions |

---

## 11. ORDER BY Clause

**Goal:** Sort customers by score from highest to lowest.

**Command:**

```sql
SELECT *
FROM CUSTOMERS
ORDER BY SCORE DESC;
```

**Output:**

| ID | NAME | COUNTRY | SCORE |
|---:|---|---|---:|
| 7 | Laila | CHINA | 1200 |
| 1 | VISHAL | INDIA | 1000 |
| 9 | George | RUSSIA | 1000 |
| 3 | Kelly | CANADA | 900 |
| 5 | Course | AUSTRALIA | 800 |
| 8 | Kiu | CHINA | 300 |
| 6 | Surgy | AUSTRALIA | 150 |
| 10 | Hamilton | RUSSIA | 120 |
| 2 | AMAN | INDIA | 100 |
| 4 | Madam | CANADA | 50 |
| 11 | Frank | RUSSIA | 0 |

**Explanation:**

- `ASC` sorts in ascending order.
- `DESC` sorts in descending order.

---

## 12. LIMIT Clause – Top 3 Customers

**Goal:** Find the three customers with the highest scores.

**Command:**

```sql
SELECT *
FROM CUSTOMERS
ORDER BY SCORE DESC
LIMIT 3;
```

**Output:**

| ID | NAME | COUNTRY | SCORE |
|---:|---|---|---:|
| 7 | Laila | CHINA | 1200 |
| 1 | VISHAL | INDIA | 1000 |
| 9 | George | RUSSIA | 1000 |

**Explanation:**

- `ORDER BY SCORE DESC` sorts scores from highest to lowest.
- `LIMIT 3` returns only the first three records.

---

## 13. TRUNCATE TABLE

**Command:**

```sql
TRUNCATE TABLE CUSTOMERS;
```

**Explanation:**

1. Deletes all records from the table.
2. Keeps the table structure intact.
3. In MySQL, TRUNCATE is a DDL operation and causes an implicit commit, so it cannot normally be rolled back.

**Warning:** Execute carefully because all records will be deleted.

---

## SQL Commands Quick Revision

| SQL Command | Purpose |
|---|---|
| CREATE DATABASE | Creates a database |
| USE | Selects a database |
| CREATE TABLE | Creates a table |
| INSERT INTO | Inserts records |
| SELECT | Retrieves records |
| WHERE | Filters records |
| COUNT() | Counts records |
| SUM() | Calculates total |
| AVG() | Calculates average |
| GROUP BY | Groups records |
| HAVING | Filters groups |
| ORDER BY | Sorts records |
| LIMIT | Restricts output rows |
| TRUNCATE | Deletes all table records |

---

## Technologies Used

- MySQL
- SQL
- MySQL Workbench

## Purpose of This Repository

This repository documents my SQL learning journey through practical queries and examples. It is intended to strengthen my understanding of database management and SQL fundamentals.

**Learning SQL one query at a time!** 🚀
