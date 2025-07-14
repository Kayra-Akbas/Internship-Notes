# Internship Notes

## Day 1 - 🐘 Basic SQL Practice: CUSTOMERS Table

A beginner-friendly summary of the SQL commands I practiced using a simple `CUSTOMERS` table. Includes table creation, inserting data, querying, updating, deleting, filtering, and more.

---

### Creating Table

CREATE TABLE CUSTOMERS (
    ID INT NOT NULL,
    NAME VARCHAR(20) NOT NULL,
    AGE INT NOT NULL,
    ADDRESS CHAR(25),
    SALARY DECIMAL(18, 2),
    EMAIL VARCHAR(50),
    PRIMARY KEY (ID)
);

---

### Inserting Data

INSERT INTO CUSTOMERS (ID, NAME, AGE, ADDRESS, SALARY, EMAIL) VALUES (1, 'Alice', 30, '123 Main St', 50000.00, 'alice@example.com');

INSERT INTO CUSTOMERS (ID, NAME, AGE, ADDRESS, SALARY, EMAIL) VALUES (2, 'Bob', 25, '456 Oak Ave', 42000.50, NULL);

INSERT INTO CUSTOMERS (ID, NAME, AGE, ADDRESS, SALARY, EMAIL) VALUES (3, 'Charlie', 28, '789 Pine Rd', 61000.75, 'charlie@mail.com');

INSERT INTO CUSTOMERS (ID, NAME, AGE, ADDRESS, SALARY, EMAIL) VALUES (4, 'Diana', 35, '321 Maple Dr', 72000.00, 'diana@example.com');

INSERT INTO CUSTOMERS (ID, NAME, AGE, ADDRESS, SALARY, EMAIL) VALUES (5, 'Ethan', 40, '789 Elm Street', 88000.00, 'ethan@mail.com');

---

### Query Selection

**Selecting All Rows**

SELECT * FROM CUSTOMERS;

**Selecting Specific Columns**

SELECT NAME, SALARY FROM CUSTOMERS;

SELECT EMAIL FROM CUSTOMERS;

**Filtering With Conditions**

SELECT * FROM CUSTOMERS WHERE AGE > 30;

SELECT * FROM CUSTOMERS WHERE EMAIL IS NULL;

SELECT * FROM CUSTOMERS WHERE EMAIL IS NOT NULL;

**Combined Filtering**

SELECT NAME, EMAIL FROM CUSTOMERS WHERE AGE >= 30 AND EMAIL IS NOT NULL;

**Sorted Results**

SELECT * FROM CUSTOMERS ORDER BY SALARY DESC;

---

### Updating Records

UPDATE CUSTOMERS SET SALARY = 75000.00, EMAIL = 'alice@update.com' WHERE ID = 1;

---

### Deleting Record

DELETE FROM CUSTOMERS WHERE ID = 3;

---

### Grouping and Aggregation

SELECT COUNT(*) AS TotalCustomers FROM CUSTOMERS;

SELECT AGE, AVG(SALARY) AS AverageSalary FROM CUSTOMERS GROUP BY AGE;

---

### Altering Table

ALTER TABLE CUSTOMERS ADD EMAIL VARCHAR(50);

---
