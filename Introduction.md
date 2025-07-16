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
