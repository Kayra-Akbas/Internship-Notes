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



