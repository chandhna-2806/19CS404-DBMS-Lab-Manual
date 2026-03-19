# EXPERIMENT 3: DML Commands
## NAME : A. Chandhna
## REGISTRATION NUMBER : 212224040051


## AIM
To study and implement DML (Data Manipulation Language) commands.

## THEORY

### 1. INSERT INTO
Used to add records into a relation.
These are three type of INSERT INTO queries which are as
A)Inserting a single record
**Syntax (Single Row):**
```sql
INSERT INTO table_name (field_1, field_2, ...) VALUES (value_1, value_2, ...);
```
**Syntax (Multiple Rows):**
```sql
INSERT INTO table_name (field_1, field_2, ...) VALUES
(value_1, value_2, ...),
(value_3, value_4, ...);
```
**Syntax (Insert from another table):**
```sql
INSERT INTO table_name SELECT * FROM other_table WHERE condition;
```
### 2. UPDATE
Used to modify records in a relation.
Syntax:
```sql
UPDATE table_name SET column1 = value1, column2 = value2 WHERE condition;
```
### 3. DELETE
Used to delete records from a relation.
**Syntax (All rows):**
```sql
DELETE FROM table_name;
```
**Syntax (Specific condition):**
```sql
DELETE FROM table_name WHERE condition;
```
### 4. SELECT
Used to retrieve records from a table.
**Syntax:**
```sql
SELECT column1, column2 FROM table_name WHERE condition;
```
**Question 1**
--
For products with a profit % less than 30% of selling price, update the selling price to provide a profit margin of 35% over cost price of the product in the products table.

PRODUCTS TABLE

name               type
-----------------  ---------------
product_id         INT
product_name       VARCHAR(100)
category           VARCHAR(50)
cost_price         DECIMAL(10,2)
sell_price         DECIMAL(10,2)
reorder_lvl        INT
quantity           INT
supplier_id        INT

```sql
UPDATE products
SET sell_price = cost_price * 135/100
WHERE ((sell_price - cost_price) / sell_price) * 100 < 30;
```

**Output:**

<img width="700" height="500" alt="Screenshot (14)" src="https://github.com/user-attachments/assets/ddf307e6-6fa8-4488-aed4-1faec189af5d" />

**Question 2**
---
Write a SQL statement to Update the address to '58 Lakeview, Magnolia' where supplier ID is 5 in the suppliers table.

Suppliers Table 

name               type
-----------------  ---------------
supplier_id        INT
supplier_name      VARCHAR(100)
contact_person     VARCHAR(100)
phone_number       VARCHAR(20)
email              VARCHAR(100)
address            VARCHAR(250)

```sql
UPDATE suppliers
SET address = '58 Lakeview, Magnolia'
WHERE supplier_id = 5;
```

**Output:**

<img width="700" height="500" alt="Screenshot (15)" src="https://github.com/user-attachments/assets/29063afe-4a7d-4f11-99d8-bcde52d18521" />

**Question 3**
---
Write a SQL query to reduce the reorder level by 30% where cost price is more than 50 and quantity in stock is less than 100 in the products table.

Products Table 

name          type       
----------    ---------- 
product_id     INT PRIMARY KEY        
product_name   VARCHAR(10) 
category       VARCHAR(50) 
cost_price     DECIMAL(10) 
sell_price     DECIMAL(10) 
reorder_lvl    INT        
quantity       INT        
supplier_id    INT  

```sql
UPDATE products
SET reorder_lvl = reorder_lvl * 70 / 100
WHERE cost_price > 50
  AND quantity < 100;
```

**Output:**

<img width="700" height="500" alt="Screenshot (16)" src="https://github.com/user-attachments/assets/e8d86cb0-5ac8-4d5d-9097-0b2601e66e9c" />

**Question 4**
---
Update the reorder level to 40 pieces for all products belonging to the 'Grocery' category in the products table.

PRODUCTS TABLE

name               type
-----------------  ---------------
product_id         INT
product_name       VARCHAR(100)
category           VARCHAR(50)
cost_price         DECIMAL(10,2)
sell_price         DECIMAL(10,2)
reorder_lvl        INT
quantity           INT
supplier_id        INT

```sql
UPDATE products
SET reorder_lvl = 40
WHERE category = 'Grocery';
```

**Output:**

<img width="700" height="500" alt="Screenshot (17)" src="https://github.com/user-attachments/assets/bb39145f-e4ca-49ba-a3f9-54b90e2387a7" />

**Question 5**
---
Write a SQL statement to Increase the salary by 500 and email as 'updated' for employees with job ID 'SA_REP' and commission percentage greater than 0.15

Employees table

---------------
employee_id
first_name
last_name
email
phone_number
hire_date
job_id
salary
commission_pct
manager_id
department_id

```sql
UPDATE employees
SET salary = salary + 500,
    email = 'updated'
WHERE job_id = 'SA_REP'
  AND commission_pct > 0.15;
```

**Output:**

<img width="700" height="500" alt="Screenshot (18)" src="https://github.com/user-attachments/assets/0aeb5ac1-050c-4df6-afe6-e7e905cc44a5" />

**Question 6**
---
Write a SQL query to Delete customers from 'customer' table where 'CUST_NAME' has exactly 6 characters.
```sql
DELETE FROM customer
WHERE LENGTH(CUST_NAME) = 6;
```

**Output:**

<img width="700" height="500" alt="Screenshot (19)" src="https://github.com/user-attachments/assets/5a054ae3-29d8-4820-b530-a28186228380" />

**Question 7**
---
Write a SQL query to remove rows from the table 'customer' with the following condition -

1. 'cust_city' should begin with the letter 'L',
```sql
DELETE FROM customer
WHERE cust_city LIKE 'L%';
```

**Output:**

<img width="700" height="500" alt="Screenshot (20)" src="https://github.com/user-attachments/assets/1a3c549c-71cc-4c34-acd9-4098cc5e000f" />

**Question 8**
---
Write a SQL query to Delete All Doctors with a NULL Last Name

Sample table: Doctors

attributes : doctor_id, first_name, last_name, specialization
For example:

Test	Result
SELECT * FROM doctors;
doctor_id   first_name  last_name   specialization
----------  ----------  ----------  --------------
1           John        Smith       Cardiology
2           Emily       Johnson     Orthopedics
3           Michael     Brown       Pediatrics
4           Febin                   Cardiology
doctor_id   first_name  last_name   specialization
----------  ----------  ----------  --------------
1           John        Smith       Cardiology
2           Emily       Johnson     Orthopedics
3           Michael     Brown       Pediatrics


```sql
DELETE FROM doctors
WHERE last_name IS NULL;
```

**Output:**

<img width="700" height="500" alt="Screenshot (21)" src="https://github.com/user-attachments/assets/2691f511-343e-4562-af3a-8c01857d45f5" />

**Question 9**
---
Write a SQL query to Delete customers whose 'GRADE' is greater than 2 and have a 'PAYMENT_AMT' less than the average 'PAYMENT_AMT' for all customers, or whose 'OUTSTANDING_AMT' is greater than 8000:

Sample table: Customer

+-----------+-------------+-------------+--------------+--------------+-------+-------------+-------------+-------------+---------------+--------------+------------+  
|CUST_CODE  | CUST_NAME   | CUST_CITY   | WORKING_AREA | CUST_COUNTRY | GRADE | OPENING_AMT | RECEIVE_AMT | PAYMENT_AMT |OUTSTANDING_AMT| PHONE_NO     | AGENT_CODE |
+-----------+-------------+-------------+--------------+--------------+-------+-------------+-------------+-------------+---------------+--------------+------------+
| C00013    | Holmes      | London      | London       | UK           |     2 |     6000.00 |     5000.00 |     7000.00 |       4000.00 | BBBBBBB      | A003       |
| C00001    | Micheal     | New York    | New York     | USA          |     2 |     3000.00 |     5000.00 |     2000.00 |       6000.00 | CCCCCCC      | A008       |
| C00020    | Albert      | New York    | New York     | USA          |     3 |     5000.00 |     7000.00 |     6000.00 |       6000.00 | BBBBSBB      | A008      

```sql
DELETE FROM customer
WHERE (grade > 2
       AND payment_amt < (SELECT AVG(payment_amt) FROM customer))
   OR outstanding_amt > 8000;
```

**Output:**

<img width="700" height="500" alt="Screenshot (23)" src="https://github.com/user-attachments/assets/3c86a700-7387-451f-9030-9465363642f3" />


**Question 10**
---
Show the categoryName and description from the categories table sorted by categoryName.

name                     type
---------------       ---------------
CategoryID           INTEGER
CategoryName     VARCHAR(25)
Description          VARCHAR(255)

```sql
SELECT CategoryName, Description
FROM categories
ORDER BY CategoryName;
```

**Output:**

<img width="700" height="500" alt="Screenshot (24)" src="https://github.com/user-attachments/assets/e1fe7ada-bdf4-495f-bdb3-0a87c28f6ace" />

## RESULT
Thus, the SQL queries to implement DML commands have been executed successfully.
