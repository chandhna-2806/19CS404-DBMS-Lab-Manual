# Experiment 10: PL/SQL – Triggers

## AIM
To write and execute PL/SQL trigger programs for automating actions in response to specific table events like INSERT, UPDATE, or DELETE.

---

## THEORY

A **trigger** is a stored PL/SQL block that is automatically executed or fired when a specified event occurs on a table or view. Triggers can be used for enforcing business rules, auditing changes, or automatic updates.

### Types of Triggers:
- **Before Trigger**: Executes before the operation (INSERT, UPDATE, DELETE).
- **After Trigger**: Executes after the operation.
- **Row-level Trigger**: Executes for each affected row.
- **Statement-level Trigger**: Executes once for the triggering statement.

**Basic Syntax:**
```sql
CREATE OR REPLACE TRIGGER trigger_name
BEFORE|AFTER INSERT|UPDATE|DELETE ON table_name
[FOR EACH ROW]
BEGIN
   -- trigger logic
END;
```

## 1. Write a trigger to log every insertion into a table.
**Steps:**
- Create two tables: `employees` (for storing data) and `employee_log` (for logging the inserts).
- Write an **AFTER INSERT** trigger on the `employees` table to log the new data into the `employee_log` table.

**Expected Output:**
- A new entry is added to the `employee_log` table each time a new record is inserted into the `employees` table.

**Program**
```
SET SERVEROUTPUT ON;

-- 1. Setup: Create both tables
DROP TABLE employee_log;
DROP TABLE employees;

CREATE TABLE employees (
    emp_id NUMBER PRIMARY KEY,
    emp_name VARCHAR2(50),
    designation VARCHAR2(50)
);

CREATE TABLE employee_log (
    log_id NUMBER GENERATED ALWAYS AS IDENTITY PRIMARY KEY,
    emp_id NUMBER,
    action_type VARCHAR2(50),
    log_date TIMESTAMP
);

-- 2. Create the Trigger
CREATE OR REPLACE TRIGGER trg_employee_insert
AFTER INSERT ON employees
FOR EACH ROW
BEGIN
    INSERT INTO employee_log (emp_id, action_type, log_date)
    VALUES (:NEW.emp_id, 'INSERTED NEW RECORD', SYSTIMESTAMP);
    
    DBMS_OUTPUT.PUT_LINE('Trigger Fired: Log entry created for Emp ID ' || :NEW.emp_id);
END;
/

-- 3. Test: Insert a record and view the log
INSERT INTO employees (emp_id, emp_name, designation) 
VALUES (201, 'John Doe', 'Software Engineer');

SELECT * FROM employee_log;
```
**Output**

<img width="423" height="363" alt="image" src="https://github.com/user-attachments/assets/ce67c2c6-259e-4e8e-bcef-b2b57e5230f2" />


---

## 2. Write a trigger to prevent deletion of records from a sensitive table.
**Steps:**
- Write a **BEFORE DELETE** trigger on the `sensitive_data` table.
- Use `RAISE_APPLICATION_ERROR` to prevent deletion and issue a custom error message.

**Expected Output:**
- If an attempt is made to delete a record from `sensitive_data`, an error message is raised, e.g., `ERROR: Deletion not allowed on this table.`

**Program**
```
SET SERVEROUTPUT ON;

-- 1. Setup: Create the sensitive table
DROP TABLE sensitive_data;

CREATE TABLE sensitive_data (
    id NUMBER PRIMARY KEY,
    secret_info VARCHAR2(100)
);

-- 2. Insert sample data to try and delete later
INSERT INTO sensitive_data VALUES (1, 'Top Secret Project Alpha');
INSERT INTO sensitive_data VALUES (2, 'Confidential Financials 2026');

-- 3. Create the BEFORE DELETE Trigger
CREATE OR REPLACE TRIGGER trg_prevent_delete
BEFORE DELETE ON sensitive_data
FOR EACH ROW
BEGIN
    -- Raise a custom error (Error codes must be between -20000 and -20999)
    RAISE_APPLICATION_ERROR(-20001, 'ERROR: Deletion not allowed on this sensitive table.');
END;
/

-- 4. Test: Attempt to delete a record
-- This will fail and display the custom error message defined above
DELETE FROM sensitive_data WHERE id = 1;
```

**Output**

<img width="437" height="301" alt="image" src="https://github.com/user-attachments/assets/c9ac111a-56ab-4e13-b933-75d52ea0f38c" />


---

## 3. Write a trigger to automatically update a `last_modified` timestamp.
**Steps:**
- Add a `last_modified` column to the `products` table.
- Write a **BEFORE UPDATE** trigger on the `products` table to set the `last_modified` column to the current timestamp whenever an update occurs.

**Expected Output:**
- The `last_modified` column in the `products` table is updated automatically to the current date and time when any record is updated.

**Program**
```
SET SERVEROUTPUT ON;

-- 1. Setup: Create the products table with last_modified column
DROP TABLE products;

CREATE TABLE products (
    product_id NUMBER PRIMARY KEY,
    product_name VARCHAR2(50),
    price NUMBER(10, 2),
    last_modified TIMESTAMP
);

-- 2. Insert sample data (last_modified will initially be NULL)
INSERT INTO products (product_id, product_name, price) 
VALUES (501, 'Gaming Laptop', 1200.00);

-- 3. Create the BEFORE UPDATE Trigger
CREATE OR REPLACE TRIGGER trg_update_timestamp
BEFORE UPDATE ON products
FOR EACH ROW
BEGIN
    -- Automatically set the column to the current system timestamp
    :NEW.last_modified := SYSTIMESTAMP;
    
    DBMS_OUTPUT.PUT_LINE('Trigger Fired: last_modified updated for Product ID ' || :OLD.product_id);
END;
/

-- 4. Test: Update the price of the product
UPDATE products 
SET price = 1150.00 
WHERE product_id = 501;

-- 5. Verify: Check if the last_modified column was updated automatically
SELECT product_id, product_name, price, last_modified FROM products;
```
**Output**
<img width="438" height="306" alt="image" src="https://github.com/user-attachments/assets/dc42a55b-ef8b-42c4-8284-5642d5c090a8" />


---

## 4. Write a trigger to keep track of the number of updates made to a table.
**Steps:**
- Create an `audit_log` table with a counter column.
- Write an **AFTER UPDATE** trigger on the `customer_orders` table to increment the counter in the `audit_log` table every time a record is updated.

**Expected Output:**
- The `audit_log` table will maintain a count of how many updates have been made to the `customer_orders` table.

**Program**
```
SET SERVEROUTPUT ON;

-- 1. Setup: Create the tables
DROP TABLE customer_orders;
DROP TABLE audit_log;

CREATE TABLE customer_orders (
    order_id NUMBER PRIMARY KEY,
    customer_name VARCHAR2(50),
    status VARCHAR2(20)
);

CREATE TABLE audit_log (
    table_name VARCHAR2(50),
    update_count NUMBER DEFAULT 0
);

-- 2. Initialize the counter for the table
INSERT INTO audit_log (table_name, update_count) VALUES ('CUSTOMER_ORDERS', 0);

-- 3. Insert sample data into orders
INSERT INTO customer_orders VALUES (1, 'Alice', 'Pending');
INSERT INTO customer_orders VALUES (2, 'Bob', 'Pending');

-- 4. Create the AFTER UPDATE Trigger
CREATE OR REPLACE TRIGGER trg_track_updates
AFTER UPDATE ON customer_orders
FOR EACH ROW
BEGIN
    -- Increment the global counter in the audit_log table
    UPDATE audit_log 
    SET update_count = update_count + 1
    WHERE table_name = 'CUSTOMER_ORDERS';
    
    DBMS_OUTPUT.PUT_LINE('Trigger Fired: Global update counter incremented.');
END;
/

-- 5. Test: Perform two separate updates
UPDATE customer_orders SET status = 'Shipped' WHERE order_id = 1;
UPDATE customer_orders SET status = 'Cancelled' WHERE order_id = 2;

-- 6. Verify: Check the final count
SELECT * FROM audit_log;
```
**Output**
<img width="437" height="285" alt="image" src="https://github.com/user-attachments/assets/7ab41786-10e9-4db0-9370-37a7b057c48b" />


---

## 5. Write a trigger that checks a condition before allowing insertion into a table.
**Steps:**
- Write a **BEFORE INSERT** trigger on the `employees` table to check if the inserted salary meets a specific condition (e.g., salary must be greater than 3000).
- If the condition is not met, raise an error to prevent the insert.

**Expected Output:**
- If the inserted salary in the `employees` table is below the condition (e.g., salary < 3000), the insert operation is blocked, and an error message is raised, such as: `ERROR: Salary below minimum threshold.`

**Program**
```
SET SERVEROUTPUT ON;

-- 1. Setup: Create the employees table
DROP TABLE employees;

CREATE TABLE employees (
    emp_id NUMBER PRIMARY KEY,
    emp_name VARCHAR2(50),
    salary NUMBER(10, 2)
);

-- 2. Create the BEFORE INSERT Trigger
CREATE OR REPLACE TRIGGER trg_check_salary
BEFORE INSERT ON employees
FOR EACH ROW
BEGIN
    -- Check if the new salary is less than 3000
    IF :NEW.salary < 3000 THEN
        -- Raise a custom error to block the insertion
        RAISE_APPLICATION_ERROR(-20002, 'ERROR: Salary below minimum threshold (3000).');
    END IF;
    
    DBMS_OUTPUT.PUT_LINE('Trigger Fired: Salary validation passed for ' || :NEW.emp_name);
END;
/

-- 3. Test Case 1: Attempt to insert with salary < 3000 (Should Fail)
INSERT INTO employees VALUES (101, 'Low Pay User', 2500);

-- 4. Test Case 2: Attempt to insert with salary >= 3000 (Should Pass)
INSERT INTO employees VALUES (102, 'Fair Pay User', 3500);

-- 5. Verify the table content
SELECT * FROM employees;
```
**Output**
<img width="678" height="316" alt="image" src="https://github.com/user-attachments/assets/aa9e6d0c-5de1-4ae4-92c9-91b9fe0331c9" />


## RESULT
Thus, the PL/SQL trigger programs were written and executed successfully.
