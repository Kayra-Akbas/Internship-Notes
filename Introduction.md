# Internship Notes

## Day 1 -  Basic SQL Practice: CUSTOMERS Table

A beginner-friendly summary of the SQL commands I practiced using a simple `CUSTOMERS` table. Includes table creation, inserting data, querying, updating, deleting, filtering, and more.

---

### Creating Table
```sql
CREATE TABLE CUSTOMERS (
    ID INT NOT NULL,
    NAME VARCHAR(20) NOT NULL,
    AGE INT NOT NULL,
    ADDRESS CHAR(25),
    SALARY DECIMAL(18, 2),
    EMAIL VARCHAR(50),
    PRIMARY KEY (ID)
);```

---

### Inserting Data
```sql
INSERT INTO CUSTOMERS (ID, NAME, AGE, ADDRESS, SALARY, EMAIL) VALUES (1, 'Alice', 30, '123 Main St', 50000.00, 'alice@example.com');

INSERT INTO CUSTOMERS (ID, NAME, AGE, ADDRESS, SALARY, EMAIL) VALUES (2, 'Bob', 25, '456 Oak Ave', 42000.50, NULL);

INSERT INTO CUSTOMERS (ID, NAME, AGE, ADDRESS, SALARY, EMAIL) VALUES (3, 'Charlie', 28, '789 Pine Rd', 61000.75, 'charlie@mail.com');

INSERT INTO CUSTOMERS (ID, NAME, AGE, ADDRESS, SALARY, EMAIL) VALUES (4, 'Diana', 35, '321 Maple Dr', 72000.00, 'diana@example.com');

INSERT INTO CUSTOMERS (ID, NAME, AGE, ADDRESS, SALARY, EMAIL) VALUES (5, 'Ethan', 40, '789 Elm Street', 88000.00, 'ethan@mail.com');
```
---

### Query Selection

**Selecting All Rows**
```sql
SELECT * FROM CUSTOMERS;
```
**Selecting Specific Columns**
```sql
SELECT NAME, SALARY FROM CUSTOMERS;

SELECT EMAIL FROM CUSTOMERS;
```
**Filtering With Conditions**
```sql
SELECT * FROM CUSTOMERS WHERE AGE > 30;

SELECT * FROM CUSTOMERS WHERE EMAIL IS NULL;

SELECT * FROM CUSTOMERS WHERE EMAIL IS NOT NULL;
```
**Combined Filtering**
```sql
SELECT NAME, EMAIL FROM CUSTOMERS WHERE AGE >= 30 AND EMAIL IS NOT NULL;
```
**Sorted Results**
```sql
SELECT * FROM CUSTOMERS ORDER BY SALARY DESC;
```
---

### Updating Records
```sql
UPDATE CUSTOMERS SET SALARY = 75000.00, EMAIL = 'alice@update.com' WHERE ID = 1;
```
---

### Deleting Record
```sql
DELETE FROM CUSTOMERS WHERE ID = 3;
```
---

### Grouping and Aggregation
```sql
SELECT COUNT(*) AS TotalCustomers FROM CUSTOMERS;

SELECT AGE, AVG(SALARY) AS AverageSalary FROM CUSTOMERS GROUP BY AGE;
```
---

### Altering Table
```sql
ALTER TABLE CUSTOMERS ADD EMAIL VARCHAR(50);
```
---
## Day 1 Part 2 -  Basic SQL Practice: Orders Table

### Creating Table
```sql
CREATE TABLE ORDERS (
    ORDER_ID INT IDENTITY(1,1) PRIMARY KEY,
    CUSTOMER_ID INT,
    PRODUCT VARCHAR(50),
    AMOUNT DECIMAL(10,2),
    ORDER_DATE DATE,
    FOREIGN KEY (CUSTOMER_ID) REFERENCES CUSTOMERS(ID) );
```
### Inserting Data
```sql
INSERT INTO ORDERS (CUSTOMER_ID, PRODUCT, AMOUNT, ORDER_DATE) VALUES
(1, 'Laptop', 1200.00, '2025-07-14'),
(2, 'Wireless Mouse', 25.00, '2025-07-14'),
(2, 'USB-C Hub', 45.00, '2025-07-15'),
(3, 'Noise Cancelling Headphones', 200.00, '2025-07-14'),
(3, 'External SSD', 150.00, '2025-07-16'),
(4, 'Monitor', 300.00, '2025-07-14'),
(5, 'Keyboard', 120.00, '2025-07-14'),
(5, 'Gaming Keyboard', 130.00, '2025-07-14'),
(5, 'Webcam', 80.00, '2025-07-16'),
(6, 'Smart Watch', 180.00, '2025-07-15'),
(6, 'Bluetooth Speaker', 60.00, '2025-07-16');
```
### Joining Tables 
```sql
SELECT O.ORDER_ID, C.NAME AS CustomerName, O.PRODUCT, O.AMOUNT, O.ORDER_DATE
FROM ORDERS O
JOIN CUSTOMERS C ON O.CUSTOMER_ID = C.ID;
```
### Aggregating Data
```sql
SELECT 
    C.NAME AS CustomerName,
    COUNT(O.ORDER_ID) AS NumberOfOrders,
    SUM(O.AMOUNT) AS TotalOrderedAmount
FROM CUSTOMERS C
LEFT JOIN ORDERS O ON C.ID = O.CUSTOMER_ID
GROUP BY C.NAME
ORDER BY TotalOrderedAmount DESC;
```
### Creating a View
```sql
CREATE VIEW CustomerOrders AS
SELECT
    C.NAME,
    O.PRODUCT,
    O.ORDER_DATE
FROM CUSTOMERS C
JOIN ORDERS O ON C.ID = O.CUSTOMER_ID;
```
**Using the View**
```sql
SELECT * FROM CustomerOrders;
```
#### Creating a Filtered Unique Index 
```sql
CREATE UNIQUE INDEX UC_Email_NotNull
ON CUSTOMERS (EMAIL)
WHERE EMAIL IS NOT NULL;
```
### Creating a Trigger to Log New 
**Step 1: Create the logging table**
```sql
CREATE TABLE ORDER_LOG (
    LOG_ID INT IDENTITY(1,1) PRIMARY KEY,
    ORDER_ID INT,
    MESSAGE VARCHAR(100),
    CREATED_AT DATETIME DEFAULT GETDATE()
```
**Step 2: Create the trigger**
```sql
CREATE TRIGGER LogNewOrder
ON ORDERS
AFTER INSERT
AS
BEGIN
    INSERT INTO ORDER_LOG (ORDER_ID, MESSAGE)
    SELECT ORDER_ID, 'New order inserted'
    FROM inserted;
END;
```
## Day 2 -  Basic SQL Practices #2:
A summary of the SQL commands I practiced using a simple CLIENTS table. Includes table creation, inserting data, selecting, querying, updating, deleting, filtering, and more.

## Creating Table
```sql
CREATE TABLE CLIENTS (
    ID INT NOT NULL,
    NAME VARCHAR(20) NOT NULL,
    AGE INT NOT NULL,
    ADDRESS CHAR(25),
    SALARY DECIMAL(18, 2),
    PRIMARY KEY (ID)
);
```
## Inserting Data
```sql
INSERT INTO CLIENTS (ID, NAME, AGE, ADDRESS, SALARY) VALUES
(1, 'Ramesh', 32, 'Ahmedabad', 2000.00),
(2, 'Khilan', 25, 'Delhi', 1500.00),
(3, 'Kaushik', 23, 'Kota', 2000.00),
(4, 'Chaitali', 25, 'Mumbai', 6500.00),
(5, 'Hardik', 27, 'Bhopal', 8500.00),
(6, 'Komal', 22, 'Hyderabad', 4500.00),
(7, 'Muffy', 24, 'Indore', 10000.00);
```
## Selecting Data
**Select all columns and rows**
```sql
SELECT * FROM CLIENTS;
```
**Select specific columns**
```sql
 SELECT ID, NAME, SALARY FROM CLIENTS;
```
**Select with concatenated columns and ordering**
```sql
SELECT CONCAT(NAME, ' ', AGE) AS DETAILS, ADDRESS
FROM CLIENTS
ORDER BY NAME;
```
**Creating Backup Table with SELECT INTO**
```sql
SELECT * INTO CLIENT_BACKUP FROM CLIENTS;
```
**Creating Filtered Table (Names Starting with 'K' or 'k')**
```sql
SELECT * INTO NameStartsWith_K
FROM CLIENTS
WHERE NAME LIKE 'k%' OR NAME LIKE 'K%';
```
## Updating Data
**Update address of a client by ID**
```sql
UPDATE CLIENTS
SET ADDRESS = 'Pune'
WHERE ID = 6;
```
**Update multiple columns for a client by name**
```sql
UPDATE CLIENTS
SET ADDRESS = 'Pune', SALARY = 1000.00
WHERE NAME = 'Ramesh';
```
**Increment age and salary for all clients**
```sql
UPDATE CLIENTS
SET AGE = AGE + 5,
    SALARY = SALARY + 3000;
```
## Deleting Data
**Delete a client by ID**
```sql
DELETE FROM CLIENTS WHERE ID = 6;
```
**Delete clients older than 25**
```sql
DELETE FROM CLIENTS WHERE AGE > 25;
```
## Sorting with Custom Order by Address
```sql
SELECT * FROM CLIENTS
ORDER BY
  CASE UPPER(ADDRESS)
    WHEN 'DELHI' THEN 1
    WHEN 'BHOPAL' THEN 2
    WHEN 'KOTA' THEN 3
    WHEN 'AHMEDABAD' THEN 4
    WHEN 'HYDERABAD' THEN 5
    ELSE 100
  END ASC,
  ADDRESS DESC;
```
## Orders Table and Operations (Related to Clients)
## Creating Orders Table
```sql
CREATE TABLE ORDERS (
    OID INT NOT NULL,
    DATE VARCHAR(20) NOT NULL,
    CLIENT_ID INT NOT NULL,
    AMOUNT DECIMAL(18, 2)
);
```
## Inserting Orders
```sql
INSERT INTO ORDERS VALUES
(102, '2009-10-08 00:00:00', 3, 3000.00),
(100, '2009-10-08 00:00:00', 3, 1500.00),
(101, '2009-11-20 00:00:00', 2, 1560.00),
(103, '2008-05-20 00:00:00', 4, 2060.00);
```
## Joining CLIENTS and ORDERS to Create CLIENT_ORDERS Table
```sql
SELECT CLIENTS.Name, ORDERS.CLIENT_ID
INTO CLIENT_ORDERS
FROM CLIENTS
LEFT JOIN ORDERS ON CLIENTS.ID = ORDERS.CLIENT_ID;
```
## Selecting from CLIENT_ORDERS
```sql
SELECT * FROM CLIENT_ORDERS;
```
## Deleting Orders and Clients with SALARY > 2000 
```sql
DELETE O
FROM ORDERS O
INNER JOIN CLIENTS C ON O.CLIENT_ID = C.ID
WHERE C.SALARY > 2000;
DELETE FROM CLIENTS WHERE SALARY > 2000;
```
## Day 2 Part 2 - Basic SQL Practice #2 
## Creating Views for the CLIENTS Table
```sql
CREATE VIEW CLIENTS_VIEW1 AS
SELECT * FROM CLIENTS;

CREATE VIEW CLIENTS_VIEW2 AS
SELECT * FROM CLIENTS;

CREATE VIEW CLIENTS_VIEW3 AS
SELECT * FROM CLIENTS;
```

## Updating Records via Views
**Update the age of client named 'Ramesh':**
```sql
UPDATE CLIENTS_VIEW
SET AGE = 35
WHERE NAME = 'Ramesh';
```
**Change address of client with ID = 6:**
```sql
UPDATE CLIENTS_VIEW
SET ADDRESS = 'Pune'
WHERE ID = 6;
```
**Update name and age for client with ID = 3:**
```sql
UPDATE CLIENTS_VIEW
SET NAME = 'Kaushik Ramanujan', AGE = 24
WHERE ID = 3;
```
**Increase age for all clients by 6:**
```sql
UPDATE CLIENTS_VIEW
SET AGE = AGE + 6;
```
## Deleting Records via Views
**Delete clients where age is 22:**
```sql
DELETE FROM CLIENTS_VIEW3
WHERE AGE = 22;
```
## Creating Filtered Views
**View only clients with salary greater than 3000:**
```sql
CREATE VIEW BUYERS_VIEW AS
SELECT * FROM CLIENTS
WHERE SALARY > 3000;
```
**View only clients aged 25 or older with update/insert restrictions:**
```sql
CREATE VIEW BUYE
CREATE VIEW MY_VIEW AS
SELECT NAME, AGE
FROM CLIENTS
WHERE AGE >= 25
WITH CHECK OPTION;
```
## Managing Views
**Drop views if they exist before recreating or deleting:**
```sql
IF OBJECT_ID('CLIENTS_VIEW1', 'V') IS NOT NULL
    DROP VIEW CLIENTS_VIEW1;

IF OBJECT_ID('CLIENTS_VIEW2', 'V') IS NOT NULL
    DROP VIEW CLIENTS_VIEW2;
```
**List all views in the database:**
```sql
SELECT TABLE_SCHEMA, TABLE_NAME
FROM INFORMATION_SCHEMA.VIEWS;
```
## Renaming Views
```sql
EXEC sp_rename 'CLIENTS_VIEW', 'VIEW_CLIENTS';
```
## Day 3  - Basic SQL Practice #3: Working with the CARS Table

## Table Creation
```sql
CREATE TABLE CARS (
   ID INT NOT NULL,
   Name VARCHAR(150),
   IsRed BIT
);
```
## Inserting Sample Data
```sql
INSERT INTO CARS (ID, Name, IsRed) VALUES (1, 'Toyota Corolla', 1);
INSERT INTO CARS (ID, Name, IsRed) VALUES (2, 'Honda Civic', 0);
INSERT INTO CARS (ID, Name, IsRed) VALUES (3, 'Ford Mustang', 1);
```
## Querying Data
**Select All Cars**
```sql
SELECT * FROM CARS;
```
**Select Only Red Cars**
```sql
SELECT * FROM CARS WHERE IsRed = 1;
```
## Trouble Shooting
**Message**
```sql
(0 rows affected)
```
**Cause: The query condition did not match any data in the table.**
## How To Fix
**Check what data exists**
```sql
SELECT * FROM CARS;
```
**If table is empty or has no red cars, insert some using:**
```sql
INSERT INTO CARS (ID, Name, IsRed) VALUES (4, 'Chevy Camaro', 1);
```
## Day 3 Part 2 - Basic SQL Practice #4 
##  Creating the CUSTOMERS Table
```sql
CREATE TABLE CUSTOMERS (
   ID INT NOT NULL,
   NAME VARCHAR(20) NOT NULL,
   AGE INT NOT NULL,
   ADDRESS CHAR(25),
   SALARY DECIMAL(18, 2),
   PRIMARY KEY (ID)
);
```
##  Inserting Data into CUSTOMERS
```sql
INSERT INTO CUSTOMERS VALUES
(1, 'Ramesh', 32, 'Ahmedabad', 2000.00),
(2, 'Khilan', 25, 'Delhi', 1500.00),
(3, 'Kaushik', 23, 'Kota', 2000.00),
(4, 'Chaitali', 25, 'Mumbai', 6500.00),
(5, 'Hardik', 27, 'Bhopal', 8500.00),
(6, 'Komal', 22, 'Hyderabad', 4500.00),
(7, 'Muffy', 24, 'Indore', 10000.00);
```
## Basic SELECT Queries
**Select customers with salary greater than 2000:**
```sql
SELECT ID, NAME, SALARY FROM CUSTOMERS WHERE SALARY > 2000;
```
**Select customers by multiple names:**
```sql
SELECT * FROM CUSTOMERS WHERE NAME IN ('Khilan', 'Hardik', 'Muffy');
```
**Select customers where name starts with 'k':**
```sql
SELECT * FROM CUSTOMERS WHERE NAME LIKE 'k%';
```
## Updating Records
**Increase salary of 'Ramesh' by 10,000:**
```sql
UPDATE CUSTOMERS SET SALARY = SALARY + 10000 WHERE NAME = 'Ramesh';
```
**Update age to 48 where age is NULL:**
```sql
UPDATE CUSTOMERS SET AGE = 48 WHERE AGE IS NULL;
```
**Increase salary by 5,000 where salary is not NULL:**
```sql
UPDATE CUSTOMERS SET SALARY = SALARY + 5000 WHERE SALARY IS NOT NULL;
```
**Conditional update of salary based on age:**
```sql
UPDATE CUSTOMERS
SET SALARY = CASE AGE
    WHEN 25 THEN 17000
    WHEN 32 THEN 25000
    ELSE 12000
END;
```
## Deleting Records
**Delete customers with age 25 or salary less than 2000:**
```sql
DELETE FROM CUSTOMERS WHERE AGE = 25 OR SALARY < 2000;
```
**Creating the ORDERS Table**
```sql
CREATE TABLE ORDERS (
   OID INT NOT NULL,
   DATE VARCHAR(20) NOT NULL,
   CUSTOMER_ID INT NOT NULL,
   AMOUNT DECIMAL(18, 2)
);
```
## Inserting Data into ORDERS
```sql
INSERT INTO ORDERS VALUES
(102, '2009-10-08 00:00:00', 3, 3000.00),
(100, '2009-10-08 00:00:00', 3, 1500.00),
(101, '2009-11-20 00:00:00', 2, 1560.00),
(103, '2008-05-20 00:00:00', 4, 2060.00)
```
## Using EXISTS and NOT EXISTS
**Select customers who have placed orders**
```sql
SELECT * FROM CUSTOMERS WHERE EXISTS (
   SELECT CUSTOMER_ID FROM ORDERS WHERE ORDERS.CUSTOMER_ID = CUSTOMERS.ID
);
```
**Select customers with no orders:**
```sql
SELECT * FROM CUSTOMERS WHERE NOT EXISTS (
   SELECT CUSTOMER_ID FROM ORDERS WHERE ORDERS.CUSTOMER_ID = CUSTOMERS.ID
);
```
## Aggregate Queries with GROUP BY and HAVING
**Count customers by age excluding age 22:**
```sql
SELECT COUNT(ID) AS CustomerCount, AGE FROM CUSTOMERS
WHERE AGE <> 22
GROUP BY AGE;
```
**Average salary by address, filtering addresses with average salary > 5240:**
```sql
SELECT ADDRESS, AVG(SALARY) AS AVG_SALARY FROM CUSTOMERS
GROUP BY ADDRESS
HAVING AVG(SALARY) > 5240;
```
## Conditional Logic with CASE
**Assign salary based on age using CASE in UPDATE:**
```sql
UPDATE CUSTOMERS
SET SALARY = CASE AGE
    WHEN 25 THEN 17000
    WHEN 32 THEN 25000
    ELSE 12000
END;
```
## Classify customers into salary groups and sum total salary: 
```sql
SELECT
   CASE 
      WHEN SALARY <= 4000 THEN 'Lowest paid'
      WHEN SALARY > 4000 AND SALARY <= 6500 THEN 'Average paid'
      ELSE 'Highest paid'
   END AS SALARY_STATUS,
   SUM(SALARY) AS Total
FROM CUSTOMERS
GROUP BY 
   CASE 
      WHEN SALARY <= 4000 THEN 'Lowest paid'
      WHEN SALARY > 4000 AND SALARY <= 6500 THEN 'Average paid'
      ELSE 'Highest paid'
   END;
```
**Assign designations based on age:**
```sql
SELECT NAME, ADDRESS,
   CASE
      WHEN AGE < 25 THEN 'Intern'
      WHEN AGE >= 25 AND AGE <= 27 THEN 'Associate Engineer'
      ELSE 'Senior Developer'
   END AS Designation
FROM CUSTOMERS
WHERE SALARY >= 2000;
```
## Day 3 Part 3 - SQL Practice Summary

## Creating the COURSES_PICKED Table
```sql
CREATE TABLE COURSES_PICKED(
   STUDENT_ID INT NOT NULL, 
   STUDENT_NAME VARCHAR(30) NOT NULL, 
   COURSE_NAME VARCHAR(30) NOT NULL
);
```
## Inserting Data into COURSES_PICKED
```sql
INSERT INTO COURSES_PICKED VALUES
(1, 'JOHN', 'ENGLISH'),
(2, 'ROBERT', 'COMPUTER SCIENCE'),
(3, 'SASHA', 'COMMUNICATIONS'),
(4, 'JULIAN', 'MATHEMATICS');
```
## Creating the EXTRA_COURSES_PICKED Table
```sql
CREATE TABLE EXTRA_COURSES_PICKED(
   STUDENT_ID INT NOT NULL, 
   STUDENT_NAME VARCHAR(30) NOT NULL, 
   COURSE_NAME VARCHAR(30) NOT NULL
);
```
## Inserting Data into EXTRA_COURSES_PICKED
```sql
(1, 'JOHN', 'PHYSICAL EDUCATION'),
(2, 'ROBERT', 'GYM'),
(3, 'SASHA', 'FILM'),
(4, 'JULIAN', 'MATHEMATICS');
```
## Combining Course Records Using UNION
```sql
SELECT * FROM COURSES_PICKED
UNION
SELECT * FROM EXTRA_COURSES_PICKED;
```
## Creating the STUDENTS Table
```sql
CREATE TABLE STUDENTS(
   ID INT NOT NULL, 
   NAME VARCHAR(20) NOT NULL, 
   SUBJECT VARCHAR(20) NOT NULL, 
   AGE INT NOT NULL, 
   HOBBY VARCHAR(20) NOT NULL, 
   PRIMARY KEY(ID));
```
## Inserting Data into STUDENTS
```sql
INSERT INTO STUDENTS VALUES
(1, 'Naina', 'Maths', 24, 'Cricket'),
(2, 'Varun', 'Physics', 26, 'Football'),
(3, 'Dev', 'Maths', 23, 'Cricket'),
(4, 'Priya', 'Physics', 25, 'Cricket'),
(5, 'Aditya', 'Chemistry', 21, 'Cricket'),
(6, 'Kalyan', 'Maths', 30, 'Football');
```
## Creating the STUDENTS_HOBBY Table
```sql
CREATE TABLE STUDENTS_HOBBY(
   ID INT NOT NULL, 
   NAME VARCHAR(20) NOT NULL, 
   HOBBY VARCHAR(20) NOT NULL, 
   AGE INT NOT NULL, 
   PRIMARY KEY(ID)
);
```
## Copying Hobby Data from STUDENTS to STUDENTS_HOBBY
```sql
INSERT INTO STUDENTS_HOBBY (ID, NAME, HOBBY, AGE)
SELECT ID, NAME, HOBBY, AGE FROM STUDENTS;
```
## Using INTERSECT to Find Matching Records
```sql
SELECT NAME, AGE, HOBBY FROM STUDENTS_HOBBY
INTERSECT
SELECT NAME, AGE, HOBBY FROM STUDENTS;
```
## Using EXCEPT to Find Differences
```sql
SELECT NAME, HOBBY, AGE FROM STUDENTS
WHERE HOBBY IN ('Cricket')
EXCEPT
SELECT NAME, HOBBY, AGE FROM STUDENTS_HOBBY
WHERE HOBBY IN ('Cricket');
```
## Self-Join to Compare Customer Salaries
```sql
SELECT 
   a.ID, 
   b.NAME AS EARNS_HIGHER, 
   a.NAME AS EARNS_LESS, 
   a.SALARY AS LOWER_SALARY
FROM CUSTOMERS a
JOIN CUSTOMERS b ON a.SALARY < b.SALARY
ORDER BY a.ID;
```
## Day 4 SQL Set Operations: INTERSECT and EXCEPT

### INTERSECT - Return Common Records in Both Tables

**Find students who like 'Cricket' in both STUDENTS and STUDENTS_HOBBY:**
```sql
SELECT NAME, AGE, HOBBY FROM STUDENTS_HOBBY
WHERE HOBBY IN ('Cricket')
INTERSECT
SELECT NAME, AGE, HOBBY FROM STUDENTS
WHERE HOBBY IN ('Cricket');
```
**Find students whose names start with 'v' and exist in both STUDENTS and STUDENTS_HOBBY:**
```sql
SELECT NAME, AGE, HOBBY FROM STUDENTS_HOBBY
WHERE NAME LIKE 'v%'
INTERSECT
SELECT NAME, AGE, HOBBY FROM STUDENTS
WHERE NAME LIKE 'v%';
```
## EXCEPT - Return Records in One Table But Not in the Other
**Students who are in STUDENTS but not in STUDENTS_HOBBY:**
```sql
SELECT NAME, HOBBY, AGE FROM STUDENTS
EXCEPT 	
SELECT NAME, HOBBY, AGE FROM STUDENTS_HOBBY;
```
**Students who like 'Cricket' only in STUDENTS (not in STUDENTS_HOBBY):**
```sql
SELECT NAME, HOBBY, AGE FROM STUDENTS
WHERE HOBBY IN ('Cricket')
EXCEPT
SELECT NAME, HOBBY, AGE FROM STUDENTS_HOBBY
WHERE HOBBY IN ('Cricket');
```
**Students with hobbies starting with 'F' in STUDENTS but not in STUDENTS_HOBBY:**
```sql
SELECT ID, NAME, HOBBY, AGE FROM STUDENTS
WHERE HOBBY LIKE 'F%'
EXCEPT
SELECT ID, NAME, HOBBY, AGE FROM STUDENTS_HOBBY
WHERE HOBBY LIKE 'F%';
```
## Day 5 SQL Joins and Table Operations
## Creating Tables
**Customers table:**
```sql
CREATE TABLE CUSTOMERS (
   ID INT NOT NULL PRIMARY KEY,
   NAME VARCHAR(20) NOT NULL,
   AGE INT NOT NULL,
   ADDRESS CHAR(25),
   SALARY DECIMAL(18, 2)
);
```
**Orders table:**
```sql
CREATE TABLE ORDERS (
   OID INT NOT NULL PRIMARY KEY,
   DATE DATE NOT NULL,
   CUSTOMER_ID INT NOT NULL,
   AMOUNT DECIMAL(18, 2),
   FOREIGN KEY (CUSTOMER_ID) REFERENCES CUSTOMERS(ID)
);
```
**Employee table:**
```sql
CREATE TABLE EMPLOYEE (
   EID INT NOT NULL PRIMARY KEY,
   EMPLOYEE_NAME VARCHAR(30) NOT NULL,
   SALES_MADE DECIMAL(18, 2)
);
```
**Order Range table:**
```sql
CREATE TABLE ORDER_RANGE (
   SNO INT NOT NULL PRIMARY KEY,
   ORDER_RANGE VARCHAR(20) NOT NULL
);
```
## Inserting Sample Data
```sql
INSERT INTO CUSTOMERS VALUES
(1, 'Ramesh', 32, 'Ahmedabad', 2000.00),
(2, 'Khilan', 25, 'Delhi', 1500.00),
(3, 'Kaushik', 23, 'Kota', 2000.00);

INSERT INTO ORDERS VALUES
(101, '2025-07-20', 1, 3000.00),
(102, '2025-07-21', 2, 1500.00);

INSERT INTO EMPLOYEE VALUES
(100, 'ALEKHYA', 3623),
(101, 'REVATHI', 1291);

INSERT INTO ORDER_RANGE VALUES
(1, '1-100'),
(2, '100-200'),
(3, '200-300');
```
## INNER JOIN - Retrieve Customers with Their Orders
```sql
SELECT ID, NAME, AMOUNT, DATE
FROM CUSTOMERS
INNER JOIN ORDERS ON CUSTOMERS.ID = ORDERS.CUSTOMER_ID;
```
## LEFT JOIN - All Customers with Orders (including customers without orders)
```sql
SELECT ID, NAME, AMOUNT, DATE
FROM CUSTOMERS
LEFT JOIN ORDERS ON CUSTOMERS.ID = ORDERS.CUSTOMER_ID;
```
## RIGHT JOIN - All Orders with Customer Info (including orders without customers)
```sql
SELECT ID, NAME, AMOUNT, DATE
FROM CUSTOMERS
RIGHT JOIN ORDERS ON CUSTOMERS.ID = ORDERS.CUSTOMER_ID;
```
## FULL JOIN - All Customers and Orders, matched where possible
```sql
SELECT ID, NAME, AMOUNT, DATE
FROM CUSTOMERS
FULL JOIN ORDERS ON CUSTOMERS.ID = ORDERS.CUSTOMER_ID;
```
## Filtering Orders with Amount Greater Than 2000
```sql
SELECT ID, NAME, DATE, AMOUNT
FROM CUSTOMERS
INNER JOIN ORDERS ON CUSTOMERS.ID = ORDERS.CUSTOMER_ID
WHERE ORDERS.AMOUNT > 2000.00;
```
## Self-Join to Compare Salaries Within the Same Table
```sql
SELECT a.ID, b.NAME AS EARNS_HIGHER, a.NAME AS EARNS_LESS, a.SALARY AS LOWER_SALARY
FROM CUSTOMERS a, CUSTOMERS b
WHERE a.SALARY < b.SALARY
ORDER BY a.SALARY;
```
## DELETE with JOIN in SQL Server Syntax
```sql
DELETE a
FROM CUSTOMERS AS a
INNER JOIN ORDERS AS b ON a.ID = b.CUSTOMER_ID
WHERE a.SALARY < 2000.00;
```
## SELECT All Records from ORDERS
```sql
SELECT * FROM ORDERS;
```
## Updating Customer Salary Based on Orders (SQL Server Syntax)
```sql
UPDATE C
SET C.SALARY = C.SALARY + 1000
FROM CUSTOMERS C
INNER JOIN ORDERS O ON C.ID = O.CUSTOMER_ID;
```
## Creating COURSES_PICKED Table
```sql
CREATE TABLE COURSES_PICKED (
   STUDENT_ID INT NOT NULL,
   STUDENT_NAME VARCHAR(30) NOT NULL,
   COURSE_NAME VARCHAR(30) NOT NULL
);
```
## Inserting Data into COURSES_PICKED
```sql
INSERT INTO COURSES_PICKED VALUES
(1, 'JOHN', 'ENGLISH'),
(2, 'ROBERT', 'COMPUTER SCIENCE'),
(3, 'SASHA', 'COMMUNICATIONS'),
(4, 'JULIAN', 'MATHEMATICS');
```
## UNION to Combine Courses Picked
```sql
SELECT * FROM COURSES_PICKED
UNION
SELECT * FROM EXTRA_COURSES_PICKED;
```
## Joining COURSES_PICKED and EXTRA_COURSES_PICKED
```sql
SELECT 
  c.STUDENT_ID, 
  c.STUDENT_NAME, 
  c.COURSE_NAME AS COURSE_PICKED, 
  e.EXTRA_COURSE_NAME AS EXTRA_COURSE_PICKED
FROM COURSES_PICKED c
JOIN EXTRA_COURSES_PICKED e
ON c.STUDENT_ID = e.STUDENT_ID;
```
## DAY 6

## DAY 7 BASIC SQL CODES WITH CHINHOOK DATEBASE
 ## Connecting to the Chinhook database**
 ```sql
  USE Chinook;
GO
```
## View All Customers
 ```sql
SELECT * FROM Customer;
 ```
## List All Tracks with Album and Artist Name
 ```sql
SELECT 
    Track.Name AS TrackName,
    Album.Title AS AlbumTitle,
    Artist.Name AS ArtistName
FROM Track
JOIN Album ON Track.AlbumId = Album.AlbumId
JOIN Artist ON Album.ArtistId = Artist.ArtistId;
 ```
##  Show All Tracks in the 'Rock' Genre
 ```sql
SELECT Track.Name, Genre.Name AS Genre
FROM Track
JOIN Genre ON Track.GenreId = Genre.GenreId
WHERE Genre.Name = 'Rock';
 ```
## List All Invoices with Customer Info
 ```sql
SELECT 
    Invoice.InvoiceId,
    Customer.FirstName,
    Customer.LastName,
    Invoice.Total,
    Invoice.InvoiceDate
FROM Invoice
JOIN Customer ON Invoice.CustomerId = Customer.CustomerId;
```
## Top 5 Most Expensive Tracks
 ```sql
SELECT Name, UnitPrice
FROM Track
ORDER BY UnitPrice DESC
LIMIT 5;
 ```
## Total Sales per Country
 ```sql
SELECT 
    BillingCountry,
    SUM(Total) AS TotalSales
FROM Invoice
GROUP BY BillingCountry
ORDER BY TotalSales DESC;
 ```
## List All Employees and Their Roles
```sql
SELECT FirstName, LastName, Title
FROM Employee;
```
## Tracks in a Specific Playlist (e.g., "Music")
```sql
SELECT 
    Playlist.Name AS PlaylistName,
    Track.Name AS TrackName
FROM Playlist
JOIN PlaylistTrack ON Playlist.PlaylistId = PlaylistTrack.PlaylistId
JOIN Track ON PlaylistTrack.TrackId = Track.TrackId
WHERE Playlist.Name = 'Music';
```
## Find All Albums by “AC/DC”
```sql
SELECT Album.Title
FROM Album
JOIN Artist ON Album.ArtistId = Artist.ArtistId
WHERE Artist.Name = 'AC/DC';
```
##  How Many Tracks in Each Genre?
```sql
SELECT Genre.Name, COUNT(*) AS TrackCount
FROM Track
JOIN Genre ON Track.GenreId = Genre.GenreId
GROUP BY Genre.Name
ORDER BY TrackCount DESC;
```
## Top 5 Customers by Total Purchase Amount
```sql
SELECT 
    Customer.CustomerId,
    Customer.FirstName + ' ' + Customer.LastName AS FullName,
    SUM(Invoice.Total) AS TotalSpent
FROM Invoice
JOIN Customer ON Invoice.CustomerId = Customer.CustomerId
GROUP BY Customer.CustomerId, Customer.FirstName, Customer.LastName
ORDER BY TotalSpent DESC
OFFSET 0 ROWS FETCH NEXT 5 ROWS ONLY;
```
##  Most Popular Genre (By Track Count Sold)
```sql
SELECT 
    Genre.Name AS Genre,
    COUNT(*) AS TracksSold
FROM InvoiceLine
JOIN Track ON InvoiceLine.TrackId = Track.TrackId
JOIN Genre ON Track.GenreId = Genre.GenreId
GROUP BY Genre.Name
ORDER BY TracksSold DESC;
```
## Monthly Sales Report (Invoice Totals by Month)
```sql
SELECT 
    FORMAT(InvoiceDate, 'yyyy-MM') AS Month,
    SUM(Total) AS MonthlySales
FROM Invoice
GROUP BY FORMAT(InvoiceDate, 'yyyy-MM')
ORDER BY Month;
```
##  List All Employees and Their Customers
```sql
SELECT 
    e.FirstName + ' ' + e.LastName AS EmployeeName,
    c.FirstName + ' ' + c.LastName AS CustomerName
FROM Employee e
JOIN Customer c ON e.EmployeeId = c.SupportRepId
ORDER BY e.LastName, c.LastName;
```
## Albums With More Than 10 Tracks
```sql
SELECT 
    Album.Title,
    COUNT(Track.TrackId) AS TrackCount
FROM Album
JOIN Track ON Album.AlbumId = Track.AlbumId
GROUP BY Album.Title
HAVING COUNT(Track.TrackId) > 10
ORDER BY TrackCount DESC;
```
## Customers Who Bought Tracks from AC/DC
```sql
SELECT DISTINCT 
    c.FirstName + ' ' + c.LastName AS CustomerName
FROM Customer c
JOIN Invoice i ON c.CustomerId = i.CustomerId
JOIN InvoiceLine il ON i.InvoiceId = il.InvoiceId
JOIN Track t ON il.TrackId = t.TrackId
JOIN Album a ON t.AlbumId = a.AlbumId
JOIN Artist ar ON a.ArtistId = ar.ArtistId
WHERE ar.Name = 'AC/DC';
```
##  Find Duplicate Customer Emails
```sql
SELECT Email, COUNT(*) AS Count
FROM Customer
GROUP BY Email
HAVING COUNT(*) > 1;
```
## Invoice Summary: Number of Items per Invoice 
```sql
SELECT 
    Invoice.InvoiceId,
    COUNT(InvoiceLine.InvoiceLineId) AS Items,
    SUM(InvoiceLine.UnitPrice * InvoiceLine.Quantity) AS Total
FROM Invoice
JOIN InvoiceLine ON Invoice.InvoiceId = InvoiceLine.InvoiceId
GROUP BY Invoice.InvoiceId
ORDER BY Total DESC;
```
## Which Media Type Sells the Most Tracks?
```sql
SELECT 
    MediaType.Name AS Format,
    COUNT(*) AS TracksSold
FROM InvoiceLine
JOIN Track ON InvoiceLine.TrackId = Track.TrackId
JOIN MediaType ON Track.MediaTypeId = MediaType.MediaTypeId
GROUP BY MediaType.Name
ORDER BY TracksSold DESC;
```
##  Average Spending Per Country
```sql
SELECT 
    BillingCountry,
    COUNT(DISTINCT CustomerId) AS Customers,
    SUM(Total) AS TotalSpent,
    AVG(Total) AS AvgInvoice,
    SUM(Total) / COUNT(DISTINCT CustomerId) AS AvgSpentPerCustomer
FROM Invoice
GROUP BY BillingCountry
ORDER BY TotalSpent DESC;
```
## Full Invoice Details for One Invoice
```sql
SELECT 
    i.InvoiceId,
    i.InvoiceDate,
    c.FirstName + ' ' + c.LastName AS CustomerName,
    t.Name AS TrackName,
    il.UnitPrice,
    il.Quantity,
    (il.UnitPrice * il.Quantity) AS LineTotal
FROM Invoice i
JOIN Customer c ON i.CustomerId = c.CustomerId
JOIN InvoiceLine il ON i.InvoiceId = il.InvoiceId
JOIN Track t ON il.TrackId = t.TrackId
WHERE i.InvoiceId = 1;
```
## Average Purchase Frequency per Customer
```sql
WITH PurchaseIntervals AS (
    SELECT 
        CustomerId,
        InvoiceDate,
        LAG(InvoiceDate) OVER (PARTITION BY CustomerId ORDER BY InvoiceDate) AS PreviousInvoiceDate
    FROM Invoice
)

SELECT 
    CustomerId,
    COUNT(*) AS InvoiceCount,
    AVG(DATEDIFF(DAY, PreviousInvoiceDate, InvoiceDate)) AS AvgDaysBetweenPurchases
FROM PurchaseIntervals
WHERE PreviousInvoiceDate IS NOT NULL
GROUP BY CustomerId;
```
##  US Customers and Their Total Purchases
```sql
SELECT 
    c.FirstName + ' ' + c.LastName AS CustomerName,
    SUM(i.Total) AS TotalSpent
FROM Customer c
JOIN Invoice i ON c.CustomerId = i.CustomerId
WHERE c.Country = 'USA'
GROUP BY c.FirstName, c.LastName
ORDER BY TotalSpent DESC;
```
## Top 3 Invoices by Total Amount
```sql
SELECT 
    InvoiceId,
    Total
FROM Invoice
ORDER BY Total DESC
OFFSET 0 ROWS FETCH NEXT 3 ROWS ONLY;
```
## Total Revenue per InvoiceLine
```sql
SELECT 
    InvoiceLineId,
    UnitPrice,
    Quantity,
    UnitPrice * Quantity AS LineTotal
FROM InvoiceLine;
```
## Employees Who Are Also Managers
```sql
SELECT 
    FirstName, LastName, Title
FROM Employee
WHERE EmployeeId IN (SELECT ReportsTo FROM Employee WHERE ReportsTo IS NOT NULL);
```
## Track Count per Playlist
```sql
SELECT 
    Playlist.Name,
    COUNT(PlaylistTrack.TrackId) AS TrackCount
FROM Playlist
JOIN PlaylistTrack ON Playlist.PlaylistId = PlaylistTrack.PlaylistId
GROUP BY Playlist.Name
ORDER BY TrackCount DESC;
```
## Tracks That Were Never Sold
```sql
SELECT *
FROM Track
WHERE TrackId NOT IN (SELECT DISTINCT TrackId FROM InvoiceLine);
```
## Customers Who Have Never Bought Anything
```sql
SELECT *
FROM Customer
WHERE CustomerId NOT IN (SELECT DISTINCT CustomerId FROM Invoice);
```
## List of Artists and How Many Albums They Have
```sql
SELECT 
    Artist.Name,
    COUNT(Album.AlbumId) AS AlbumCount
FROM Artist
JOIN Album ON Artist.ArtistId = Album.ArtistId
GROUP BY Artist.Name
ORDER BY AlbumCount DESC;
```
## See All Grunge Tracks
```sql
SELECT Track.Name AS TrackName, Artist.Name AS ArtistName, Album.Title AS AlbumTitle
FROM Track
JOIN Genre ON Track.GenreId = Genre.GenreId
JOIN Album ON Track.AlbumId = Album.AlbumId
JOIN Artist ON Album.ArtistId = Artist.ArtistId
WHERE Genre.Name = 'Grunge';
```
##  RIGHT JOIN – Invoices Without a Matching Customer
```sql
SELECT 
    c.CustomerId,
    i.InvoiceId,
    i.Total
FROM Customer c
RIGHT JOIN Invoice i ON c.CustomerId = i.CustomerId
WHERE c.CustomerId IS NULL;
```
## LEFT JOIN – Customers Without Invoices
```sql
SELECT 
    c.CustomerId,
    c.FirstName + ' ' + c.LastName AS CustomerName,
    i.InvoiceId
FROM Customer c
LEFT JOIN Invoice i ON c.CustomerId = i.CustomerId
WHERE i.InvoiceId IS NULL;
```
##  Tracks From Albums with Short Titles (< 20 chars) AND Metal Genre
```sql
SELECT 
    t.TrackId,
    t.Name AS TrackName,
    a.Title AS AlbumTitle,
    g.Name AS Genre
FROM Track t
JOIN Album a ON t.AlbumId = a.AlbumId
JOIN Genre g ON t.GenreId = g.GenreId
WHERE LEN(a.Title) < 20
  AND g.Name = 'Metal';
```
## DAY 8 SQL PRACTICES WITH DATABASE
## Selecting Top 3 Bands and Top Genres of the Customers Who Are Also Employee
```sql
WITH GenreSales AS (
    SELECT 
        e.EmployeeId,
        e.FirstName + ' ' + e.LastName AS EmployeeName,
        g.Name AS GenreName,
        COUNT(*) AS GenrePurchaseCount
    FROM Customer c
    JOIN Employee e ON c.SupportRepId = e.EmployeeId
    JOIN Invoice i ON c.CustomerId = i.CustomerId
    JOIN InvoiceLine il ON i.InvoiceId = il.InvoiceId
    JOIN Track t ON il.TrackId = t.TrackId
    JOIN Genre g ON t.GenreId = g.GenreId
    GROUP BY e.EmployeeId, e.FirstName, e.LastName, g.Name
),
RankedGenres AS (
    SELECT *,
        RANK() OVER (PARTITION BY EmployeeId ORDER BY GenrePurchaseCount DESC) AS GenreRank
    FROM GenreSales
),
ArtistSales AS (
    SELECT 
        e.EmployeeId,
        e.FirstName + ' ' + e.LastName AS EmployeeName,
        ar.Name AS ArtistName,
        COUNT(*) AS TracksPurchased
    FROM Customer c
    JOIN Employee e ON c.SupportRepId = e.EmployeeId
    JOIN Invoice i ON c.CustomerId = i.CustomerId
    JOIN InvoiceLine il ON i.InvoiceId = il.InvoiceId
    JOIN Track t ON il.TrackId = t.TrackId
    JOIN Album al ON t.AlbumId = al.AlbumId
    JOIN Artist ar ON al.ArtistId = ar.ArtistId
    GROUP BY e.EmployeeId, e.FirstName, e.LastName, ar.Name
),
RankedArtists AS (
    SELECT *,
        ROW_NUMBER() OVER (PARTITION BY EmployeeId ORDER BY TracksPurchased DESC) AS ArtistRank
    FROM ArtistSales
)

SELECT 
    g.EmployeeName,
    g.GenreName AS TopGenre,
    MAX(CASE WHEN a.ArtistRank = 1 THEN a.ArtistName END) AS TopArtist1,
    MAX(CASE WHEN a.ArtistRank = 2 THEN a.ArtistName END) AS TopArtist2,
    MAX(CASE WHEN a.ArtistRank = 3 THEN a.ArtistName END) AS TopArtist3
FROM RankedGenres g
LEFT JOIN RankedArtists a ON g.EmployeeId = a.EmployeeId
WHERE g.GenreRank = 1
GROUP BY g.EmployeeName, g.GenreName
ORDER BY g.EmployeeName;
```
## The Top 3 songs and Top Genre  Bought by  the Top 5 Customers and How Much They are Spending
```sql
WITH CustomerSpending AS (
    SELECT 
        c.CustomerId,
        c.FirstName + ' ' + c.LastName AS CustomerName,
        SUM(i.Total) AS TotalSpent
    FROM Customer c
    JOIN Invoice i ON c.CustomerId = i.CustomerId
    GROUP BY c.CustomerId, c.FirstName, c.LastName
),
TopCustomers AS (
    SELECT TOP 5 *
    FROM CustomerSpending
    ORDER BY TotalSpent DESC
),


TrackPurchases AS (
    SELECT 
        c.CustomerId,
        c.FirstName + ' ' + c.LastName AS CustomerName,
        t.Name AS TrackName,
        COUNT(*) AS TimesBought
    FROM Customer c
    JOIN Invoice i ON c.CustomerId = i.CustomerId
    JOIN InvoiceLine il ON i.InvoiceId = il.InvoiceId
    JOIN Track t ON il.TrackId = t.TrackId
    WHERE c.CustomerId IN (SELECT CustomerId FROM TopCustomers)
    GROUP BY c.CustomerId, c.FirstName, c.LastName, t.Name
),
RankedTracks AS (
    SELECT *,
        ROW_NUMBER() OVER (PARTITION BY CustomerId ORDER BY TimesBought DESC) AS TrackRank
    FROM TrackPurchases
),


GenrePurchases AS (
    SELECT 
        c.CustomerId,
        c.FirstName + ' ' + c.LastName AS CustomerName,
        g.Name AS GenreName,
        COUNT(*) AS GenreCount
    FROM Customer c
    JOIN Invoice i ON c.CustomerId = i.CustomerId
    JOIN InvoiceLine il ON i.InvoiceId = il.InvoiceId
    JOIN Track t ON il.TrackId = t.TrackId
    JOIN Genre g ON t.GenreId = g.GenreId
    WHERE c.CustomerId IN (SELECT CustomerId FROM TopCustomers)
    GROUP BY c.CustomerId, c.FirstName, c.LastName, g.Name
),
RankedGenres AS (
    SELECT *,
        ROW_NUMBER() OVER (PARTITION BY CustomerId ORDER BY GenreCount DESC) AS GenreRank
    FROM GenrePurchases
)

SELECT 
    tc.CustomerName,
    tc.TotalSpent,
    MAX(CASE WHEN rt.TrackRank = 1 THEN rt.TrackName END) AS TopTrack1,
    MAX(CASE WHEN rt.TrackRank = 2 THEN rt.TrackName END) AS TopTrack2,
    MAX(CASE WHEN rt.TrackRank = 3 THEN rt.TrackName END) AS TopTrack3,
    MAX(CASE WHEN rg.GenreRank = 1 THEN rg.GenreName END) AS TopGenre
FROM TopCustomers tc
LEFT JOIN RankedTracks rt ON tc.CustomerId = rt.CustomerId
LEFT JOIN RankedGenres rg ON tc.CustomerId = rg.CustomerId
GROUP BY tc.CustomerName, tc.TotalSpent
ORDER BY tc.TotalSpent DESC;
```
## Top Genre and Most Purchased Song by Country (Excluding One-Time Sales)
```sql
WITH GenreSalesByCountry AS (
    SELECT 
        c.Country,
        g.Name AS GenreName,
        COUNT(*) AS GenreSales
    FROM Customer c
    JOIN Invoice i ON c.CustomerId = i.CustomerId
    JOIN InvoiceLine il ON i.InvoiceId = il.InvoiceId
    JOIN Track t ON il.TrackId = t.TrackId
    JOIN Genre g ON t.GenreId = g.GenreId
    GROUP BY c.Country, g.Name
    HAVING COUNT(*) > 1  
),
RankedGenres AS (
    SELECT *,
        RANK() OVER (PARTITION BY Country ORDER BY GenreSales DESC) AS GenreRank
    FROM GenreSalesByCountry
),
TrackSalesByCountry AS (
    SELECT 
        c.Country,
        t.Name AS TrackName,
        COUNT(*) AS TrackSales
    FROM Customer c
    JOIN Invoice i ON c.CustomerId = i.CustomerId
    JOIN InvoiceLine il ON i.InvoiceId = il.InvoiceId
    JOIN Track t ON il.TrackId = t.TrackId
    GROUP BY c.Country, t.Name
    HAVING COUNT(*) > 1  
),
RankedTracks AS (
    SELECT *,
        RANK() OVER (PARTITION BY Country ORDER BY TrackSales DESC) AS TrackRank
    FROM TrackSalesByCountry
)
SELECT 
    g.Country,
    g.GenreName AS TopGenre,
    g.GenreSales,
    t.TrackName AS TopTrack,
    t.TrackSales
FROM RankedGenres g
JOIN RankedTracks t ON g.Country = t.Country
WHERE g.GenreRank = 1 AND t.TrackRank = 1
ORDER BY g.Country;
```
## Top 3 Albums Revenue for Top 5 Customers + Revenue Contribution
```sql
WITH CustomerSpending AS (
    SELECT 
        c.CustomerId,
        c.FirstName + ' ' + c.LastName AS CustomerName,
        SUM(i.Total) AS TotalSpent
    FROM Customer c
    JOIN Invoice i ON c.CustomerId = i.CustomerId
    GROUP BY c.CustomerId, c.FirstName, c.LastName
),
TopCustomers AS (
    SELECT TOP 5 *
    FROM CustomerSpending
    ORDER BY TotalSpent DESC
),
AlbumRevenue AS (
    SELECT 
        c.CustomerId,
        al.Title AS AlbumTitle,
        SUM(il.UnitPrice * il.Quantity) AS AlbumTotal
    FROM Customer c
    JOIN Invoice i ON c.CustomerId = i.CustomerId
    JOIN InvoiceLine il ON i.InvoiceId = il.InvoiceId
    JOIN Track t ON il.TrackId = t.TrackId
    JOIN Album al ON t.AlbumId = al.AlbumId
    WHERE c.CustomerId IN (SELECT CustomerId FROM TopCustomers)
    GROUP BY c.CustomerId, al.Title
),

RankedAlbums AS (
    SELECT 
        CustomerId,
        AlbumTitle,
        AlbumTotal,
        ROW_NUMBER() OVER (PARTITION BY CustomerId ORDER BY AlbumTotal DESC) AS AlbumRank
    FROM AlbumRevenue
),
TopAlbumRevenue AS (
    SELECT 
        CustomerId,
        MAX(CASE WHEN AlbumRank = 1 THEN AlbumTitle END) AS TopAlbum1,
        MAX(CASE WHEN AlbumRank = 1 THEN AlbumTotal END) AS TopAlbum1Revenue,
        MAX(CASE WHEN AlbumRank = 2 THEN AlbumTitle END) AS TopAlbum2,
        MAX(CASE WHEN AlbumRank = 2 THEN AlbumTotal END) AS TopAlbum2Revenue,
        MAX(CASE WHEN AlbumRank = 3 THEN AlbumTitle END) AS TopAlbum3,
        MAX(CASE WHEN AlbumRank = 3 THEN AlbumTotal END) AS TopAlbum3Revenue
    FROM RankedAlbums
    WHERE AlbumRank <= 3
    GROUP BY CustomerId
)
SELECT 
    tc.CustomerName,
    tc.TotalSpent,

    tar.TopAlbum1,
    tar.TopAlbum1Revenue,

    tar.TopAlbum2,
    tar.TopAlbum2Revenue,

    tar.TopAlbum3,
    tar.TopAlbum3Revenue,

    ISNULL(tar.TopAlbum1Revenue, 0) + ISNULL(tar.TopAlbum2Revenue, 0) + ISNULL(tar.TopAlbum3Revenue, 0) AS TotalFromTop3Albums,

    ROUND(
        100.0 * (ISNULL(tar.TopAlbum1Revenue, 0) + ISNULL(tar.TopAlbum2Revenue, 0) + ISNULL(tar.TopAlbum3Revenue, 0)) / tc.TotalSpent,
        2
    ) AS PercentOfRevenueFromTop3
FROM TopCustomers tc
LEFT JOIN TopAlbumRevenue tar ON tc.CustomerId = tar.CustomerId
ORDER BY tc.TotalSpent DESC;
```
## Top Artist per Country and Their Best-Earning Album in that Country
```sql
WITH ArtistRevenueByCountry AS (
    SELECT
        c.Country,
        ar.ArtistId,
        ar.Name AS ArtistName,
        SUM(il.UnitPrice * il.Quantity) AS TotalArtistRevenue
    FROM Customer c
    JOIN Invoice i ON c.CustomerId = i.CustomerId
    JOIN InvoiceLine il ON i.InvoiceId = il.InvoiceId
    JOIN Track t ON il.TrackId = t.TrackId
    JOIN Album al ON t.AlbumId = al.AlbumId
    JOIN Artist ar ON al.ArtistId = ar.ArtistId
    GROUP BY c.Country, ar.ArtistId, ar.Name
),
RankedArtists AS (
    SELECT *,
        RANK() OVER (PARTITION BY Country ORDER BY TotalArtistRevenue DESC) AS ArtistRank
    FROM ArtistRevenueByCountry
),

-- Step 2: Revenue per album per country (still linked to artist)
AlbumRevenueByCountry AS (
    SELECT
        c.Country,
        ar.ArtistId,
        ar.Name AS ArtistName,
        al.Title AS AlbumTitle,
        SUM(il.UnitPrice * il.Quantity) AS AlbumRevenue
    FROM Customer c
    JOIN Invoice i ON c.CustomerId = i.CustomerId
    JOIN InvoiceLine il ON i.InvoiceId = il.InvoiceId
    JOIN Track t ON il.TrackId = t.TrackId
    JOIN Album al ON t.AlbumId = al.AlbumId
    JOIN Artist ar ON al.ArtistId = ar.ArtistId
    GROUP BY c.Country, ar.ArtistId, ar.Name, al.Title
),
RankedAlbums AS (
    SELECT *,
        RANK() OVER (PARTITION BY Country, ArtistId ORDER BY AlbumRevenue DESC) AS AlbumRank
    FROM AlbumRevenueByCountry
)

-- Final output: Top artist per country + their best album
SELECT
    ra.Country,
    ra.ArtistName,
    ra.TotalArtistRevenue,
    alb.AlbumTitle AS TopAlbum,
    alb.AlbumRevenue AS TopAlbumRevenue
FROM RankedArtists ra
JOIN RankedAlbums alb 
    ON ra.Country = alb.Country 
    AND ra.ArtistId = alb.ArtistId
WHERE ra.ArtistRank = 1 AND alb.AlbumRank = 1
ORDER BY ra.Country;
```
## Top 10 Customers by Revenue and their % of the Total Revenue
```sql
WITH TotalRevenue AS (
    SELECT SUM(Total) AS GlobalRevenue
    FROM Invoice
),
CustomerSpending AS (
    SELECT 
        c.CustomerId,
        c.FirstName + ' ' + c.LastName AS CustomerName,
        SUM(i.Total) AS TotalSpent
    FROM Customer c
    JOIN Invoice i ON c.CustomerId = i.CustomerId
    GROUP BY c.CustomerId, c.FirstName, c.LastName
),
TopCustomers AS (
    SELECT TOP 10 *
    FROM CustomerSpending
    ORDER BY TotalSpent DESC
)
SELECT 
    tc.CustomerName,
    tc.TotalSpent,
    tr.GlobalRevenue,
    ROUND(100.0 * tc.TotalSpent / tr.GlobalRevenue, 2) AS PercentOfTotalRevenue
FROM TopCustomers tc
CROSS JOIN TotalRevenue tr
ORDER BY tc.TotalSpent DESC;
```
## Artists' Album Counts and Their Top-Earning Country Revenue Share
```sql

WITH AlbumCounts AS (
    SELECT 
        ar.ArtistId,
        ar.Name AS ArtistName,
        COUNT(al.AlbumId) AS AlbumCount
    FROM Artist ar
    JOIN Album al ON ar.ArtistId = al.ArtistId
    GROUP BY ar.ArtistId, ar.Name
),
ArtistRevenueByCountry AS (
    SELECT 
        ar.ArtistId,
        c.Country,
        SUM(il.UnitPrice * il.Quantity) AS TotalRevenue
    FROM Customer c
    JOIN Invoice i ON c.CustomerId = i.CustomerId
    JOIN InvoiceLine il ON i.InvoiceId = il.InvoiceId
    JOIN Track t ON il.TrackId = t.TrackId
    JOIN Album al ON t.AlbumId = al.AlbumId
    JOIN Artist ar ON al.ArtistId = ar.ArtistId
    GROUP BY ar.ArtistId, c.Country
),
TopCountryPerArtist AS (
    SELECT 
        ArtistId,
        Country,
        TotalRevenue,
        RANK() OVER (PARTITION BY ArtistId ORDER BY TotalRevenue DESC) AS CountryRank
    FROM ArtistRevenueByCountry
),
TotalArtistRevenue AS (
    SELECT 
        ar.ArtistId,
        SUM(il.UnitPrice * il.Quantity) AS TotalGlobalRevenue
    FROM InvoiceLine il
    JOIN Track t ON il.TrackId = t.TrackId
    JOIN Album al ON t.AlbumId = al.AlbumId
    JOIN Artist ar ON al.ArtistId = ar.ArtistId
    GROUP BY ar.ArtistId
)
SELECT 
    ac.ArtistName,
    ac.AlbumCount,
    tca.Country AS TopCountry,
    tca.TotalRevenue AS RevenueInTopCountry,
    tar.TotalGlobalRevenue,
    ROUND(100.0 * tca.TotalRevenue / tar.TotalGlobalRevenue, 2) AS PercentFromTopCountry
FROM AlbumCounts ac
LEFT JOIN (
    SELECT * FROM TopCountryPerArtist WHERE CountryRank = 1
) tca ON ac.ArtistId = tca.ArtistId
LEFT JOIN TotalArtistRevenue tar ON ac.ArtistId = tar.ArtistId
ORDER BY PercentFromTopCountry DESC;
```
## Artists' Album Counts and Their Top-Earning Country Revenue Share (Excluding Artists with No Sales)
```sql
WITH AlbumCounts AS (
    SELECT 
        ar.ArtistId,
        ar.Name AS ArtistName,
        COUNT(al.AlbumId) AS AlbumCount
    FROM Artist ar
    JOIN Album al ON ar.ArtistId = al.ArtistId
    GROUP BY ar.ArtistId, ar.Name
),
ArtistRevenueByCountry AS (
    SELECT 
        ar.ArtistId,
        c.Country,
        SUM(il.UnitPrice * il.Quantity) AS TotalRevenue
    FROM Customer c
    JOIN Invoice i ON c.CustomerId = i.CustomerId
    JOIN InvoiceLine il ON i.InvoiceId = il.InvoiceId
    JOIN Track t ON il.TrackId = t.TrackId
    JOIN Album al ON t.AlbumId = al.AlbumId
    JOIN Artist ar ON al.ArtistId = ar.ArtistId
    GROUP BY ar.ArtistId, c.Country
),
TopCountryPerArtist AS (
    SELECT 
        ArtistId,
        Country,
        TotalRevenue,
        RANK() OVER (PARTITION BY ArtistId ORDER BY TotalRevenue DESC) AS CountryRank
    FROM ArtistRevenueByCountry
),
TotalArtistRevenue AS (
    SELECT 
        ar.ArtistId,
        SUM(il.UnitPrice * il.Quantity) AS TotalGlobalRevenue
    FROM InvoiceLine il
    JOIN Track t ON il.TrackId = t.TrackId
    JOIN Album al ON t.AlbumId = al.AlbumId
    JOIN Artist ar ON al.ArtistId = ar.ArtistId
    GROUP BY ar.ArtistId
)
SELECT 
    ac.ArtistName,
    ac.AlbumCount,
    tca.Country AS TopCountry,
    tca.TotalRevenue AS RevenueInTopCountry,
    tar.TotalGlobalRevenue,
    ROUND(100.0 * tca.TotalRevenue / tar.TotalGlobalRevenue, 2) AS PercentFromTopCountry
FROM AlbumCounts ac
INNER JOIN (
    SELECT * FROM TopCountryPerArtist WHERE CountryRank = 1
) tca ON ac.ArtistId = tca.ArtistId
INNER JOIN TotalArtistRevenue tar ON ac.ArtistId = tar.ArtistId
ORDER BY PercentFromTopCountry DESC;
```
## Artists' Album Counts and Their Top-Earning Country Revenue Share (Excluding Artists with No Sales and Sorted by Album Counts)
```sql
WITH AlbumCounts AS (
    SELECT 
        ar.ArtistId,
        ar.Name AS ArtistName,
        COUNT(al.AlbumId) AS AlbumCount
    FROM Artist ar
    JOIN Album al ON ar.ArtistId = al.ArtistId
    GROUP BY ar.ArtistId, ar.Name
),
ArtistRevenueByCountry AS (
    SELECT 
        ar.ArtistId,
        c.Country,
        SUM(il.UnitPrice * il.Quantity) AS TotalRevenue
    FROM Customer c
    JOIN Invoice i ON c.CustomerId = i.CustomerId
    JOIN InvoiceLine il ON i.InvoiceId = il.InvoiceId
    JOIN Track t ON il.TrackId = t.TrackId
    JOIN Album al ON t.AlbumId = al.AlbumId
    JOIN Artist ar ON al.ArtistId = ar.ArtistId
    GROUP BY ar.ArtistId, c.Country
),
TopCountryPerArtist AS (
    SELECT 
        ArtistId,
        Country,
        TotalRevenue,
        RANK() OVER (PARTITION BY ArtistId ORDER BY TotalRevenue DESC) AS CountryRank
    FROM ArtistRevenueByCountry
),
TotalArtistRevenue AS (
    SELECT 
        ar.ArtistId,
        SUM(il.UnitPrice * il.Quantity) AS TotalGlobalRevenue
    FROM InvoiceLine il
    JOIN Track t ON il.TrackId = t.TrackId
    JOIN Album al ON t.AlbumId = al.AlbumId
    JOIN Artist ar ON al.ArtistId = ar.ArtistId
    GROUP BY ar.ArtistId
)
SELECT 
    ac.ArtistName,
    ac.AlbumCount,
    tca.Country AS TopCountry,
    tca.TotalRevenue AS RevenueInTopCountry,
    tar.TotalGlobalRevenue,
    ROUND(100.0 * tca.TotalRevenue / tar.TotalGlobalRevenue, 2) AS PercentFromTopCountry
FROM AlbumCounts ac
INNER JOIN (
    SELECT * FROM TopCountryPerArtist WHERE CountryRank = 1
) tca ON ac.ArtistId = tca.ArtistId
INNER JOIN TotalArtistRevenue tar ON ac.ArtistId = tar.ArtistId
ORDER BY ac.AlbumCount DESC, PercentFromTopCountry DESC;
```
## Artist Sales Summary: Album Counts, Revenue by Country, Top Genre, and Top Track's Album Sales
```sql
WITH AlbumCounts AS (
    SELECT 
        ar.ArtistId,
        ar.Name AS ArtistName,
        COUNT(al.AlbumId) AS AlbumCount
    FROM Artist ar
    JOIN Album al ON ar.ArtistId = al.ArtistId
    GROUP BY ar.ArtistId, ar.Name
),

ArtistRevenueByCountry AS (
    SELECT 
        ar.ArtistId,
        c.Country,
        SUM(il.UnitPrice * il.Quantity) AS TotalRevenue
    FROM Customer c
    JOIN Invoice i ON c.CustomerId = i.CustomerId
    JOIN InvoiceLine il ON i.InvoiceId = il.InvoiceId
    JOIN Track t ON il.TrackId = t.TrackId
    JOIN Album al ON t.AlbumId = al.AlbumId
    JOIN Artist ar ON al.ArtistId = ar.ArtistId
    GROUP BY ar.ArtistId, c.Country
),

TopCountryPerArtist AS (
    SELECT 
        ArtistId,
        Country,
        TotalRevenue,
        RANK() OVER (PARTITION BY ArtistId ORDER BY TotalRevenue DESC) AS CountryRank
    FROM ArtistRevenueByCountry
),

TotalArtistRevenue AS (
    SELECT 
        ar.ArtistId,
        SUM(il.UnitPrice * il.Quantity) AS TotalGlobalRevenue
    FROM InvoiceLine il
    JOIN Track t ON il.TrackId = t.TrackId
    JOIN Album al ON t.AlbumId = al.AlbumId
    JOIN Artist ar ON al.ArtistId = ar.ArtistId
    GROUP BY ar.ArtistId
),

ArtistGenreSales AS (
    SELECT 
        ar.ArtistId,
        g.Name AS GenreName,
        SUM(il.UnitPrice * il.Quantity) AS GenreRevenue,
        RANK() OVER (PARTITION BY ar.ArtistId ORDER BY SUM(il.UnitPrice * il.Quantity) DESC) AS GenreRank
    FROM InvoiceLine il
    JOIN Track t ON il.TrackId = t.TrackId
    JOIN Genre g ON t.GenreId = g.GenreId
    JOIN Album al ON t.AlbumId = al.AlbumId
    JOIN Artist ar ON al.ArtistId = ar.ArtistId
    GROUP BY ar.ArtistId, g.Name
),
TopTrackWithAlbum AS (
    SELECT TOP 1 WITH TIES
        ar.ArtistId,
        t.TrackId,
        t.Name AS TrackName,
        al.AlbumId
    FROM InvoiceLine il
    JOIN Track t ON il.TrackId = t.TrackId
    JOIN Album al ON t.AlbumId = al.AlbumId
    JOIN Artist ar ON al.ArtistId = ar.ArtistId
    GROUP BY ar.ArtistId, t.TrackId, t.Name, al.AlbumId
    ORDER BY SUM(il.Quantity) DESC
),
AlbumSalesQuantity AS (
    SELECT 
        tta.ArtistId,
        tta.TrackName,
        tta.AlbumId,
        SUM(il.Quantity) AS AlbumQuantity
    FROM TopTrackWithAlbum tta
    JOIN InvoiceLine il ON il.TrackId IN (
        SELECT TrackId FROM Track WHERE AlbumId = tta.AlbumId
    )
    GROUP BY tta.ArtistId, tta.TrackName, tta.AlbumId
)

SELECT 
    ac.ArtistName,
    ac.AlbumCount,
    tca.Country AS TopCountry,
    tca.TotalRevenue AS RevenueInTopCountry,
    tar.TotalGlobalRevenue,
    ROUND(100.0 * tca.TotalRevenue / tar.TotalGlobalRevenue, 2) AS PercentFromTopCountry,
    ags.GenreName AS TopGenre,
    ISNULL(asq.TrackName, 'No Tracks Sold') AS TopTrack,
    ISNULL(asq.AlbumQuantity, 0) AS AlbumSaleQuantityOfTopTrack
FROM AlbumCounts ac
INNER JOIN (
    SELECT * FROM TopCountryPerArtist WHERE CountryRank = 1
) tca ON ac.ArtistId = tca.ArtistId
INNER JOIN TotalArtistRevenue tar ON ac.ArtistId = tar.ArtistId
INNER JOIN (
    SELECT ArtistId, GenreName FROM ArtistGenreSales WHERE GenreRank = 1
) ags ON ac.ArtistId = ags.ArtistId
LEFT JOIN AlbumSalesQuantity asq ON ac.ArtistId = asq.ArtistId
ORDER BY ac.AlbumCount DESC, PercentFromTopCountry DESC;
```
## Artist Summary: Album Counts, Track and Genre Diversity, Revenue by Top Country, and Top Track’s Album Sales
```sql
WITH AlbumCounts AS (
    SELECT 
        ar.ArtistId,
        ar.Name AS ArtistName,
        COUNT(al.AlbumId) AS AlbumCount
    FROM Artist ar
    JOIN Album al ON ar.ArtistId = al.ArtistId
    GROUP BY ar.ArtistId, ar.Name
),

ArtistRevenueByCountry AS (
    SELECT 
        ar.ArtistId,
        c.Country,
        SUM(il.UnitPrice * il.Quantity) AS TotalRevenue
    FROM Customer c
    JOIN Invoice i ON c.CustomerId = i.CustomerId
    JOIN InvoiceLine il ON i.InvoiceId = il.InvoiceId
    JOIN Track t ON il.TrackId = t.TrackId
    JOIN Album al ON t.AlbumId = al.AlbumId
    JOIN Artist ar ON al.ArtistId = ar.ArtistId
    GROUP BY ar.ArtistId, c.Country
),

TopCountryPerArtist AS (
    SELECT 
        ArtistId,
        Country,
        TotalRevenue,
        RANK() OVER (PARTITION BY ArtistId ORDER BY TotalRevenue DESC) AS CountryRank
    FROM ArtistRevenueByCountry
),

TotalArtistRevenue AS (
    SELECT 
        ar.ArtistId,
        SUM(il.UnitPrice * il.Quantity) AS TotalGlobalRevenue
    FROM InvoiceLine il
    JOIN Track t ON il.TrackId = t.TrackId
    JOIN Album al ON t.AlbumId = al.AlbumId
    JOIN Artist ar ON al.ArtistId = ar.ArtistId
    GROUP BY ar.ArtistId
),

ArtistGenreSales AS (
    SELECT 
        ar.ArtistId,
        g.Name AS GenreName,
        SUM(il.UnitPrice * il.Quantity) AS GenreRevenue,
        RANK() OVER (PARTITION BY ar.ArtistId ORDER BY SUM(il.UnitPrice * il.Quantity) DESC) AS GenreRank
    FROM InvoiceLine il
    JOIN Track t ON il.TrackId = t.TrackId
    JOIN Genre g ON t.GenreId = g.GenreId
    JOIN Album al ON t.AlbumId = al.AlbumId
    JOIN Artist ar ON al.ArtistId = ar.ArtistId
    GROUP BY ar.ArtistId, g.Name
),

TopTrackWithAlbum AS (
    SELECT TOP 1 WITH TIES
        ar.ArtistId,
        t.TrackId,
        t.Name AS TrackName,
        al.AlbumId
    FROM InvoiceLine il
    JOIN Track t ON il.TrackId = t.TrackId
    JOIN Album al ON t.AlbumId = al.AlbumId
    JOIN Artist ar ON al.ArtistId = ar.ArtistId
    GROUP BY ar.ArtistId, t.TrackId, t.Name, al.AlbumId
    ORDER BY SUM(il.Quantity) DESC
),

AlbumSalesQuantity AS (
    SELECT 
        tta.ArtistId,
        tta.TrackName,
        tta.AlbumId,
        SUM(il.Quantity) AS AlbumQuantity
    FROM TopTrackWithAlbum tta
    JOIN InvoiceLine il ON il.TrackId IN (
        SELECT TrackId FROM Track WHERE AlbumId = tta.AlbumId
    )
    GROUP BY tta.ArtistId, tta.TrackName, tta.AlbumId
),

-- Fix here: rank album sales quantity per artist, keep only top 1 per artist
AlbumSalesQuantityRanked AS (
    SELECT *,
        ROW_NUMBER() OVER (PARTITION BY ArtistId ORDER BY AlbumQuantity DESC) AS rn
    FROM AlbumSalesQuantity
),

ArtistTrackStats AS (
    SELECT 
        ar.ArtistId,
        COUNT(t.TrackId) AS TotalTracks,
        COUNT(DISTINCT g.GenreId) AS GenreVariety
    FROM Artist ar
    JOIN Album al ON ar.ArtistId = al.ArtistId
    JOIN Track t ON al.AlbumId = t.AlbumId
    JOIN Genre g ON t.GenreId = g.GenreId
    GROUP BY ar.ArtistId
)

SELECT 
    ac.ArtistName,
    ac.AlbumCount,
    ats.TotalTracks,
    ats.GenreVariety,
    tca.Country AS TopCountry,
    tca.TotalRevenue AS RevenueInTopCountry,
    tar.TotalGlobalRevenue,
    ROUND(100.0 * tca.TotalRevenue / tar.TotalGlobalRevenue, 2) AS PercentFromTopCountry,
    ags.GenreName AS TopGenre,
    ISNULL(asq.TrackName, 'No Tracks Sold') AS TopTrack,
    ISNULL(asq.AlbumQuantity, 0) AS AlbumSaleQuantityOfTopTrack
FROM AlbumCounts ac
INNER JOIN (
    SELECT * FROM TopCountryPerArtist WHERE CountryRank = 1
) tca ON ac.ArtistId = tca.ArtistId
INNER JOIN TotalArtistRevenue tar ON ac.ArtistId = tar.ArtistId
INNER JOIN (
    SELECT ArtistId, GenreName FROM ArtistGenreSales WHERE GenreRank = 1
) ags ON ac.ArtistId = ags.ArtistId
LEFT JOIN AlbumSalesQuantityRanked asq ON ac.ArtistId = asq.ArtistId AND asq.rn = 1
LEFT JOIN ArtistTrackStats ats ON ac.ArtistId = ats.ArtistId
ORDER BY ac.AlbumCount DESC, PercentFromTopCountry DESC;
```
## "Analysis of Artist Album Counts, Track Lengths, Top Genres, and Revenue Distribution by Country with Top Tracks and Album Sales"
```sql
WITH AlbumCounts AS (
    SELECT 
        ar.ArtistId,
        ar.Name AS ArtistName,
        COUNT(al.AlbumId) AS AlbumCount
    FROM Artist ar
    JOIN Album al ON ar.ArtistId = al.ArtistId
    GROUP BY ar.ArtistId, ar.Name
),

ArtistRevenueByCountry AS (
    SELECT 
        ar.ArtistId,
        c.Country,
        SUM(il.UnitPrice * il.Quantity) AS TotalRevenue
    FROM Customer c
    JOIN Invoice i ON c.CustomerId = i.CustomerId
    JOIN InvoiceLine il ON i.InvoiceId = il.InvoiceId
    JOIN Track t ON il.TrackId = t.TrackId
    JOIN Album al ON t.AlbumId = al.AlbumId
    JOIN Artist ar ON al.ArtistId = ar.ArtistId
    GROUP BY ar.ArtistId, c.Country
),

TopCountryPerArtist AS (
    SELECT 
        ArtistId,
        Country,
        TotalRevenue,
        RANK() OVER (PARTITION BY ArtistId ORDER BY TotalRevenue DESC) AS CountryRank
    FROM ArtistRevenueByCountry
),

TotalArtistRevenue AS (
    SELECT 
        ar.ArtistId,
        SUM(il.UnitPrice * il.Quantity) AS TotalGlobalRevenue
    FROM InvoiceLine il
    JOIN Track t ON il.TrackId = t.TrackId
    JOIN Album al ON t.AlbumId = al.AlbumId
    JOIN Artist ar ON al.ArtistId = ar.ArtistId
    GROUP BY ar.ArtistId
),

ArtistGenreSales AS (
    SELECT 
        ar.ArtistId,
        g.Name AS GenreName,
        SUM(il.UnitPrice * il.Quantity) AS GenreRevenue,
        RANK() OVER (PARTITION BY ar.ArtistId ORDER BY SUM(il.UnitPrice * il.Quantity) DESC) AS GenreRank
    FROM InvoiceLine il
    JOIN Track t ON il.TrackId = t.TrackId
    JOIN Genre g ON t.GenreId = g.GenreId
    JOIN Album al ON t.AlbumId = al.AlbumId
    JOIN Artist ar ON al.ArtistId = ar.ArtistId
    GROUP BY ar.ArtistId, g.Name
),

TopTrackWithAlbum AS (
    SELECT TOP 1 WITH TIES
        ar.ArtistId,
        t.TrackId,
        t.Name AS TrackName,
        al.AlbumId
    FROM InvoiceLine il
    JOIN Track t ON il.TrackId = t.TrackId
    JOIN Album al ON t.AlbumId = al.AlbumId
    JOIN Artist ar ON al.ArtistId = ar.ArtistId
    GROUP BY ar.ArtistId, t.TrackId, t.Name, al.AlbumId
    ORDER BY SUM(il.Quantity) DESC
),

AlbumSalesQuantity AS (
    SELECT 
        tta.ArtistId,
        tta.TrackName,
        tta.AlbumId,
        SUM(il.Quantity) AS AlbumQuantity
    FROM TopTrackWithAlbum tta
    JOIN InvoiceLine il ON il.TrackId IN (
        SELECT TrackId FROM Track WHERE AlbumId = tta.AlbumId
    )
    GROUP BY tta.ArtistId, tta.TrackName, tta.AlbumId
),

AlbumSalesQuantityRanked AS (
    SELECT *,
        ROW_NUMBER() OVER (PARTITION BY ArtistId ORDER BY AlbumQuantity DESC) AS rn
    FROM AlbumSalesQuantity
),

ArtistTrackStats AS (
    SELECT 
        ar.ArtistId,
        COUNT(t.TrackId) AS TotalTracks,
        COUNT(DISTINCT g.GenreId) AS GenreVariety,
        AVG(CAST(t.Milliseconds AS FLOAT)) AS AvgTrackLengthMs,
        MIN(CAST(t.Milliseconds AS FLOAT)) AS MinTrackLengthMs,
        MAX(CAST(t.Milliseconds AS FLOAT)) AS MaxTrackLengthMs
    FROM Artist ar
    JOIN Album al ON ar.ArtistId = al.ArtistId
    JOIN Track t ON al.AlbumId = t.AlbumId
    JOIN Genre g ON t.GenreId = g.GenreId
    GROUP BY ar.ArtistId
)

SELECT 
    ac.ArtistName,
    ac.AlbumCount,
    ats.TotalTracks,
    ats.GenreVariety,
    ROUND(ats.AvgTrackLengthMs / 60000, 2) AS AvgTrackLengthMinutes,
    ROUND(ats.MinTrackLengthMs / 60000, 2) AS MinTrackLengthMinutes,
    ROUND(ats.MaxTrackLengthMs / 60000, 2) AS MaxTrackLengthMinutes,
    tca.Country AS TopCountry,
    tca.TotalRevenue AS RevenueInTopCountry,
    tar.TotalGlobalRevenue,
    ROUND(100.0 * tca.TotalRevenue / tar.TotalGlobalRevenue, 2) AS PercentFromTopCountry,
    ags.GenreName AS TopGenre,
    ISNULL(asq.TrackName, 'No Tracks Sold') AS TopTrack,
    ISNULL(asq.AlbumQuantity, 0) AS AlbumSaleQuantityOfTopTrack
FROM AlbumCounts ac
INNER JOIN (
    SELECT * FROM TopCountryPerArtist WHERE CountryRank = 1
) tca ON ac.ArtistId = tca.ArtistId
INNER JOIN TotalArtistRevenue tar ON ac.ArtistId = tar.ArtistId
INNER JOIN (
    SELECT ArtistId, GenreName FROM ArtistGenreSales WHERE GenreRank = 1
) ags ON ac.ArtistId = ags.ArtistId
LEFT JOIN AlbumSalesQuantityRanked asq ON ac.ArtistId = asq.ArtistId AND asq.rn = 1
LEFT JOIN ArtistTrackStats ats ON ac.ArtistId = ats.ArtistId
ORDER BY ac.AlbumCount DESC, PercentFromTopCountry DESC;
```
## DAY 9 SQL SCENARIOS USING CHINHOOK
## Top Artists by Revenue in Each Genre
```sql
WITH ArtistGenreRevenue AS (
    SELECT 
        g.GenreId,
        g.Name AS GenreName,
        ar.ArtistId,
        ar.Name AS ArtistName,
        SUM(il.UnitPrice * il.Quantity) AS TotalRevenue,
        SUM(il.Quantity) AS TotalTracksSold
    FROM InvoiceLine il
    JOIN Track t ON il.TrackId = t.TrackId
    JOIN Genre g ON t.GenreId = g.GenreId
    JOIN Album al ON t.AlbumId = al.AlbumId
    JOIN Artist ar ON al.ArtistId = ar.ArtistId
    GROUP BY g.GenreId, g.Name, ar.ArtistId, ar.Name
),
TopArtistPerGenre AS (
    SELECT *,
        RANK() OVER (PARTITION BY GenreId ORDER BY TotalRevenue DESC) AS GenreArtistRank
    FROM ArtistGenreRevenue
),
TopTrackPerArtistGenre AS (
    SELECT 
        g.GenreId,
        ar.ArtistId,
        t.TrackId,
        t.Name AS TrackName,
        mt.Name AS MediaType,
        SUM(il.Quantity) AS TrackQuantity,
        RANK() OVER (PARTITION BY g.GenreId, ar.ArtistId ORDER BY SUM(il.Quantity) DESC) AS TrackRank
    FROM InvoiceLine il
    JOIN Track t ON il.TrackId = t.TrackId
    JOIN MediaType mt ON t.MediaTypeId = mt.MediaTypeId
    JOIN Genre g ON t.GenreId = g.GenreId
    JOIN Album al ON t.AlbumId = al.AlbumId
    JOIN Artist ar ON al.ArtistId = ar.ArtistId
    GROUP BY g.GenreId, ar.ArtistId, t.TrackId, t.Name, mt.Name
)

SELECT 
    tg.GenreName,
    tg.ArtistName AS TopArtist,
    tg.TotalRevenue,
    tg.TotalTracksSold,
    ISNULL(tt.TrackName, 'No track') AS TopTrack,
    ISNULL(tt.MediaType, 'Unknown') AS MediaType
FROM TopArtistPerGenre tg
LEFT JOIN TopTrackPerArtistGenre tt 
    ON tg.GenreId = tt.GenreId AND tg.ArtistId = tt.ArtistId AND tt.TrackRank = 1
WHERE tg.GenreArtistRank = 1
ORDER BY tg.TotalRevenue DESC;
```
## Most Loyal Customers by Artist
```sql
WITH CustomerArtistSales AS (
    SELECT 
        ar.ArtistId,
        ar.Name AS ArtistName,
        c.CustomerId,
        CONCAT(c.FirstName, ' ', c.LastName) AS CustomerName,
        c.Country,
        COUNT(il.InvoiceLineId) AS TracksBought,
        SUM(il.UnitPrice * il.Quantity) AS TotalSpent
    FROM InvoiceLine il
    JOIN Invoice i ON il.InvoiceId = i.InvoiceId
    JOIN Customer c ON i.CustomerId = c.CustomerId
    JOIN Track t ON il.TrackId = t.TrackId
    JOIN Album al ON t.AlbumId = al.AlbumId
    JOIN Artist ar ON al.ArtistId = ar.ArtistId
    GROUP BY ar.ArtistId, ar.Name, c.CustomerId, c.FirstName, c.LastName, c.Country
),
RankedCustomerArtistSales AS (
    SELECT *,
        RANK() OVER (PARTITION BY ArtistId ORDER BY TracksBought DESC, TotalSpent DESC) AS CustomerRank
    FROM CustomerArtistSales
),
ArtistTopGenre AS (
    SELECT 
        ar.ArtistId,
        g.Name AS GenreName,
        SUM(il.UnitPrice * il.Quantity) AS GenreRevenue,
        RANK() OVER (PARTITION BY ar.ArtistId ORDER BY SUM(il.UnitPrice * il.Quantity) DESC) AS GenreRank
    FROM InvoiceLine il
    JOIN Track t ON il.TrackId = t.TrackId
    JOIN Genre g ON t.GenreId = g.GenreId
    JOIN Album al ON t.AlbumId = al.AlbumId
    JOIN Artist ar ON al.ArtistId = ar.ArtistId
    GROUP BY ar.ArtistId, g.Name
)

SELECT 
    rcas.ArtistName,
    rcas.CustomerName,
    rcas.Country,
    rcas.TracksBought,
    ROUND(rcas.TotalSpent, 2) AS TotalSpent,
    atg.GenreName AS TopGenre
FROM RankedCustomerArtistSales rcas
LEFT JOIN ArtistTopGenre atg ON rcas.ArtistId = atg.ArtistId AND atg.GenreRank = 1
WHERE rcas.CustomerRank = 1
ORDER BY rcas.ArtistName;

```
## DAY 10 SQL SCENARIOS USING CHINHOOK

## Media Type Popularity Across Genres
```sql
WITH GenreMediaSales AS (
    SELECT 
        g.GenreId,
        g.Name AS GenreName,
        mt.MediaTypeId,
        mt.Name AS MediaTypeName,
        SUM(il.Quantity) AS TotalTracksSold
    FROM InvoiceLine il
    JOIN Track t ON il.TrackId = t.TrackId
    JOIN Genre g ON t.GenreId = g.GenreId
    JOIN MediaType mt ON t.MediaTypeId = mt.MediaTypeId
    GROUP BY g.GenreId, g.Name, mt.MediaTypeId, mt.Name
),
TotalGenreSales AS (
    SELECT 
        GenreId,
        SUM(TotalTracksSold) AS GenreTotal
    FROM GenreMediaSales
    GROUP BY GenreId
),
RankedGenreMedia AS (
    SELECT 
        gms.GenreId,
        gms.GenreName,
        gms.MediaTypeName,
        gms.TotalTracksSold,
        tg.GenreTotal,
        ROUND(100.0 * gms.TotalTracksSold / tg.GenreTotal, 2) AS PercentOfGenre,
        RANK() OVER (PARTITION BY gms.GenreId ORDER BY gms.TotalTracksSold DESC) AS MediaRank
    FROM GenreMediaSales gms
    JOIN TotalGenreSales tg ON gms.GenreId = tg.GenreId
)

SELECT 
    GenreName,
    MediaTypeName AS MostPopularMediaType,
    TotalTracksSold,
    PercentOfGenre
FROM RankedGenreMedia
WHERE MediaRank = 1
ORDER BY PercentOfGenre DESC;
```
## Country-Specific Genre Preferences
```sql
WITH CountryGenreSales AS (
    SELECT 
        c.Country,
        g.GenreId,
        g.Name AS GenreName,
        SUM(il.Quantity) AS TracksSold,
        SUM(il.UnitPrice * il.Quantity) AS Revenue
    FROM Invoice i
    JOIN Customer c ON i.CustomerId = c.CustomerId
    JOIN InvoiceLine il ON i.InvoiceId = il.InvoiceId
    JOIN Track t ON il.TrackId = t.TrackId
    JOIN Genre g ON t.GenreId = g.GenreId
    GROUP BY c.Country, g.GenreId, g.Name
),

CountryTotalSales AS (
    SELECT 
        Country,
        SUM(Revenue) AS TotalCountryRevenue
    FROM CountryGenreSales
    GROUP BY Country
),

RankedGenrePerCountry AS (
    SELECT 
        cgs.Country,
        cgs.GenreName,
        cgs.TracksSold,
        cgs.Revenue,
        cts.TotalCountryRevenue,
        ROUND(100.0 * cgs.Revenue / cts.TotalCountryRevenue, 2) AS PercentOfCountryRevenue,
        RANK() OVER (PARTITION BY cgs.Country ORDER BY cgs.Revenue DESC) AS GenreRank
    FROM CountryGenreSales cgs
    JOIN CountryTotalSales cts ON cgs.Country = cts.Country
)

SELECT 
    Country,
    GenreName AS TopGenre,
    TracksSold,
    ROUND(Revenue, 2) AS Revenue,
    PercentOfCountryRevenue
FROM RankedGenrePerCountry
WHERE GenreRank = 1
ORDER BY Revenue DESC;
```
## Best-Selling Albums by Country
```sql
WITH AlbumSalesByCountry AS (
    SELECT 
        c.Country,
        al.AlbumId,
        al.Title AS AlbumTitle,
        ar.Name AS ArtistName,
        SUM(il.UnitPrice * il.Quantity) AS Revenue
    FROM InvoiceLine il
    JOIN Invoice i ON il.InvoiceId = i.InvoiceId
    JOIN Customer c ON i.CustomerId = c.CustomerId
    JOIN Track t ON il.TrackId = t.TrackId
    JOIN Album al ON t.AlbumId = al.AlbumId
    JOIN Artist ar ON al.ArtistId = ar.ArtistId
    GROUP BY c.Country, al.AlbumId, al.Title, ar.Name
),
TotalCountryRevenue AS (
    SELECT 
        Country,
        SUM(Revenue) AS TotalRevenue
    FROM AlbumSalesByCountry
    GROUP BY Country
),
RankedAlbums AS (
    SELECT 
        ascb.Country,
        ascb.AlbumTitle,
        ascb.ArtistName,
        ascb.Revenue,
        tcr.TotalRevenue,
        ROUND(100.0 * ascb.Revenue / tcr.TotalRevenue, 2) AS PercentOfCountryRevenue,
        RANK() OVER (PARTITION BY ascb.Country ORDER BY ascb.Revenue DESC) AS AlbumRank
    FROM AlbumSalesByCountry ascb
    JOIN TotalCountryRevenue tcr ON ascb.Country = tcr.Country
)

SELECT 
    Country,
    AlbumTitle AS BestSellingAlbum,
    ArtistName,
    ROUND(Revenue, 2) AS Revenue,
    PercentOfCountryRevenue
FROM RankedAlbums
WHERE AlbumRank = 1
ORDER BY Revenue DESC;
```
## Best-Selling Music Albums by Country (Filtered by Media Type)"
```sql
WITH AlbumSalesByCountry AS (
    SELECT 
        c.Country,
        al.AlbumId,
        al.Title AS AlbumTitle,
        ar.Name AS ArtistName,
        SUM(il.UnitPrice * il.Quantity) AS Revenue
    FROM InvoiceLine il
    JOIN Invoice i ON il.InvoiceId = i.InvoiceId
    JOIN Customer c ON i.CustomerId = c.CustomerId
    JOIN Track t ON il.TrackId = t.TrackId
    JOIN MediaType mt ON t.MediaTypeId = mt.MediaTypeId
    JOIN Album al ON t.AlbumId = al.AlbumId
    JOIN Artist ar ON al.ArtistId = ar.ArtistId
    WHERE mt.Name IN ('MPEG audio file', 'AAC audio file', 'Protected AAC audio file')
    GROUP BY c.Country, al.AlbumId, al.Title, ar.Name
),

TotalCountryRevenue AS (
    SELECT 
        Country,
        SUM(Revenue) AS TotalRevenue
    FROM AlbumSalesByCountry
    GROUP BY Country
),

RankedAlbums AS (
    SELECT 
        ascb.Country,
        ascb.AlbumTitle,
        ascb.ArtistName,
        ascb.Revenue,
        tcr.TotalRevenue,
        ROUND(100.0 * ascb.Revenue / tcr.TotalRevenue, 2) AS PercentOfCountryRevenue,
        RANK() OVER (PARTITION BY ascb.Country ORDER BY ascb.Revenue DESC) AS AlbumRank
    FROM AlbumSalesByCountry ascb
    JOIN TotalCountryRevenue tcr ON ascb.Country = tcr.Country
)

SELECT 
    Country,
    AlbumTitle AS BestSellingAlbum,
    ArtistName,
    ROUND(Revenue, 2) AS Revenue,
    PercentOfCountryRevenue
FROM RankedAlbums
WHERE AlbumRank = 1
ORDER BY Revenue DESC;
```
##  Customers Who Only Buy One Genre
```sql
WITH CustomerGenreCount AS (
    SELECT 
        c.CustomerId,
        c.FirstName + ' ' + c.LastName AS CustomerName,
        c.Country,
        g.Name AS GenreName,
        COUNT(il.InvoiceLineId) AS TracksBought,
        SUM(il.UnitPrice * il.Quantity) AS TotalSpent
    FROM Customer c
    JOIN Invoice i ON c.CustomerId = i.CustomerId
    JOIN InvoiceLine il ON i.InvoiceId = il.InvoiceId
    JOIN Track t ON il.TrackId = t.TrackId
    JOIN Genre g ON t.GenreId = g.GenreId
    GROUP BY c.CustomerId, c.FirstName, c.LastName, c.Country, g.Name
),
SingleGenreCustomers AS (
    SELECT 
        CustomerId
    FROM CustomerGenreCount
    GROUP BY CustomerId
    HAVING COUNT(DISTINCT GenreName) = 1
)

SELECT 
    cgc.CustomerName,
    cgc.Country,
    cgc.GenreName,
    cgc.TracksBought,
    ROUND(cgc.TotalSpent, 2) AS TotalSpent
FROM CustomerGenreCount cgc
JOIN SingleGenreCustomers sgc ON cgc.CustomerId = sgc.CustomerId
ORDER BY cgc.TotalSpent DESC;
```
## High Spenders by Genre (Top 3 Customers Per Genre)
For each music genre, show the top 3 customers who spent the most money buying tracks from that genre.

```sql
WITH GenreSpending AS (
    SELECT
        g.Name AS GenreName,
        c.CustomerId,
        c.FirstName + ' ' + c.LastName AS CustomerName,
        c.Country,
        SUM(il.UnitPrice * il.Quantity) AS TotalSpent
    FROM Customer c
    JOIN Invoice i ON c.CustomerId = i.CustomerId
    JOIN InvoiceLine il ON i.InvoiceId = il.InvoiceId
    JOIN Track t ON il.TrackId = t.TrackId
    JOIN Genre g ON t.GenreId = g.GenreId
    GROUP BY g.Name, c.CustomerId, c.FirstName, c.LastName, c.Country
),
RankedSpenders AS (
    SELECT *,
        RANK() OVER (
            PARTITION BY GenreName
            ORDER BY TotalSpent DESC
        ) AS GenreRank
    FROM GenreSpending
)

SELECT
    GenreName,
    CustomerName,
    Country,
    TotalSpent,
    GenreRank
FROM RankedSpenders
WHERE GenreRank <= 3
ORDER BY GenreName, GenreRank;
```

## Find the Top 5 Customers by Total Spending
```sql
SELECT TOP 5
    c.FirstName + ' ' + c.LastName AS CustomerName,
    c.Country,
    COUNT(i.InvoiceId) AS TotalInvoices,
    SUM(i.Total) AS TotalSpent
FROM Customer c
JOIN Invoice i ON c.CustomerId = i.CustomerId
GROUP BY c.CustomerId, c.FirstName, c.LastName, c.Country
ORDER BY TotalSpent DESC;
```
## DAY 11 SQL Practices Using Chinhook
## Filtering to genres with at least 10 customers
```sql
WITH GenreSpending AS (
    SELECT
        g.GenreId,
        g.Name AS GenreName,
        c.CustomerId,
        c.FirstName + ' ' + c.LastName AS CustomerName,
        c.Country,
        SUM(il.UnitPrice * il.Quantity) AS TotalSpent
    FROM Customer c
    JOIN Invoice i ON c.CustomerId = i.CustomerId
    JOIN InvoiceLine il ON i.InvoiceId = il.InvoiceId
    JOIN Track t ON il.TrackId = t.TrackId
    JOIN Genre g ON t.GenreId = g.GenreId
    GROUP BY g.GenreId, g.Name, c.CustomerId, c.FirstName, c.LastName, c.Country
),

GenresWithEnoughCustomers AS (
    SELECT
        GenreId
    FROM GenreSpending
    GROUP BY GenreId
    HAVING COUNT(DISTINCT CustomerId) >= 10
),

FilteredGenreSpending AS (
    SELECT gs.*
    FROM GenreSpending gs
    JOIN GenresWithEnoughCustomers g10
        ON gs.GenreId = g10.GenreId
),

RankedSpenders AS (
    SELECT *,
        RANK() OVER (
            PARTITION BY GenreName
            ORDER BY TotalSpent DESC
        ) AS GenreRank
    FROM FilteredGenreSpending
)

SELECT
    GenreName,
    CustomerName,
    Country,
    TotalSpent,
    GenreRank
FROM RankedSpenders
WHERE GenreRank <= 3
ORDER BY GenreName, GenreRank;
```
## Top Media Type Used by Customers
```sql
WITH MediaUsage AS (
    SELECT 
        c.CustomerId,
        c.FirstName + ' ' + c.LastName AS CustomerName,
        mt.Name AS MediaType,
        COUNT(il.InvoiceLineId) AS PurchaseCount
    FROM dbo.Customer c
    JOIN dbo.Invoice i ON c.CustomerId = i.CustomerId
    JOIN dbo.InvoiceLine il ON i.InvoiceId = il.InvoiceId
    JOIN dbo.Track t ON il.TrackId = t.TrackId
    JOIN dbo.MediaType mt ON t.MediaTypeId = mt.MediaTypeId
    GROUP BY c.CustomerId, c.FirstName, c.LastName, mt.Name
),
RankedMedia AS (
    SELECT *,
        RANK() OVER (
            PARTITION BY CustomerId
            ORDER BY PurchaseCount DESC
        ) AS MediaRank
    FROM MediaUsage
)
SELECT 
    CustomerName,
    MediaType,
    PurchaseCount
FROM RankedMedia
WHERE MediaRank = 1
ORDER BY CustomerName;
```
## DAY 12 SQL PRACTICES
##  Customer Spending by Genre (Over $10)
```sql
SELECT 
    c.CustomerId,
    c.FirstName + ' ' + c.LastName AS CustomerName,
    g.Name AS Genre,
    ROUND(SUM(il.UnitPrice * il.Quantity), 2) AS TotalSpent
FROM Customer c
JOIN Invoice i ON c.CustomerId = i.CustomerId
JOIN InvoiceLine il ON i.InvoiceId = il.InvoiceId
JOIN Track t ON il.TrackId = t.TrackId
JOIN Genre g ON t.GenreId = g.GenreId
GROUP BY c.CustomerId, c.FirstName, c.LastName, g.Name
HAVING SUM(il.UnitPrice * il.Quantity) > 10
ORDER BY TotalSpent DESC;
```
##  Customer Count per Country (Only Countries with > 1 Customer)
```sql
SELECT 
    Country,
    COUNT(*) AS NumberOfCustomers
FROM Customer
GROUP BY Country
HAVING COUNT(*) > 1
ORDER BY NumberOfCustomers DESC;
```
## Albums with More Than 10 Tracks
```sql
SELECT 
    al.Title AS AlbumTitle,
    ar.Name AS Artist,
    COUNT(t.TrackId) AS NumberOfTracks
FROM Album al
JOIN Artist ar ON al.ArtistId = ar.ArtistId
JOIN Track t ON al.AlbumId = t.AlbumId
GROUP BY al.AlbumId, al.Title, ar.Name
HAVING COUNT(t.TrackId) > 10
ORDER BY NumberOfTracks DESC;
```
## Most Popular Media Types by Total Quantity Sold
```sql
SELECT 
    mt.Name AS MediaType,
    SUM(il.Quantity) AS TotalSold
FROM InvoiceLine il
JOIN Track t ON il.TrackId = t.TrackId
JOIN MediaType mt ON t.MediaTypeId = mt.MediaTypeId
GROUP BY mt.Name
ORDER BY TotalSold DESC;
```
## DAY 13 SQL with Chinhook
