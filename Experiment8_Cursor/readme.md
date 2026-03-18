# Experiment 8: PL/SQL Cursor Programs

## AIM
To write and execute PL/SQL programs using cursors and exception handling to manage runtime errors effectively and display appropriate messages.

## THEORY

In PL/SQL, cursors are used to handle query result sets row-by-row. 

There are two types of cursors:

- Implicit Cursors: Automatically created by PL/SQL for single-row queries.
- Explicit Cursors: Declared and controlled by the programmer for multi-row queries.

Types of Explicit Cursors:

1. Simple Cursor: Basic cursor to iterate over multiple rows.

2. Parameterized Cursor: Accepts parameters to filter the result dynamically.

3. Cursor FOR Loop: Simplifies cursor operations (open, fetch, close).

4. %ROWTYPE Cursor: Fetches entire row into a record using %ROWTYPE.

5. Cursor with FOR UPDATE: Used for row-level locking and updating the rows while looping.

**Syntax:**
```sql
DECLARE 
   <declarations section> 
BEGIN 
   <executable command(s)>
EXCEPTION 
   <exception handling> 
END;
```

### Basic Components of PL/SQL Block:

- DECLARE: Section to declare variables and constants.
- BEGIN: The execution section that contains PL/SQL statements.
- EXCEPTION: Handles errors or exceptions that occur in the program.
- END: Marks the end of the PL/SQL block.

**Exception Handling**

PL/SQL provides a robust mechanism to handle runtime errors using exception handling blocks. When an error occurs during execution, control is passed to the EXCEPTION section, where specific or general errors can be handled gracefully.

### Components of Exception Handling:
- Predefined Exceptions: Automatically raised by PL/SQL for common errors (e.g., NO_DATA_FOUND, TOO_MANY_ROWS, ZERO_DIVIDE).
- User-defined Exceptions: Declared explicitly in the declaration section using the EXCEPTION keyword.
- WHEN OTHERS: A generic handler for all exceptions not handled explicitly.

```sql
BEGIN
   -- Statements
EXCEPTION
   WHEN exception_name THEN
      -- Handling code
   WHEN OTHERS THEN
      -- Handling for unknown errors
END;
```

### **Question 1: Simple Cursor with Exception Handling**

**Write a PL/SQL program using a simple cursor to fetch employee names and designations from the `employees` table. Implement exception handling for the following cases:**

1. **NO_DATA_FOUND**: When no rows are fetched.
2. **OTHERS**: Any other unexpected errors during execution.

**Steps:**

- Create an `employees` table with fields `emp_id`, `emp_name`, and `designation`.
- Insert some sample data into the table.
- Use a simple cursor to fetch and display employee names and designations.
- Implement exception handling to catch the relevant exceptions and display appropriate messages.

**Output:**  
The program should display the employee details or an error message.

**Program**
```
-- 1. Create the employees table
CREATE TABLE employees (
    emp_id NUMBER PRIMARY KEY,
    emp_name VARCHAR2(50),
    designation VARCHAR2(50)
);

-- 2. Insert sample records
INSERT INTO employees VALUES (101, 'Alice Johnson', 'Manager');
INSERT INTO employees VALUES (102, 'Bob Smith', 'Developer');
INSERT INTO employees VALUES (103, 'Charlie Davis', 'Analyst');

SET SERVEROUTPUT ON;

DECLARE
    -- Variables to hold data from the cursor
    v_name employees.emp_name%TYPE;
    v_desig employees.designation%TYPE;

    -- Define the cursor to fetch employee details
    CURSOR emp_cursor IS 
        SELECT emp_name, designation FROM employees;

BEGIN
    -- Open the cursor and start fetching
    OPEN emp_cursor;
    
    LOOP
        FETCH emp_cursor INTO v_name, v_desig;
        
        -- Exit the loop when no more rows exist
        EXIT WHEN emp_cursor%NOTFOUND;
        
        -- Display the output
        DBMS_OUTPUT.PUT_LINE('Employee: ' || v_name || ' | Designation: ' || v_desig);
    END LOOP;

    -- Custom check: if no rows were ever fetched, raise an exception
    IF emp_cursor%ROWCOUNT = 0 THEN
        RAISE NO_DATA_FOUND;
    END IF;

    CLOSE emp_cursor;

EXCEPTION
    -- Handle the specific case where no rows were found
    WHEN NO_DATA_FOUND THEN
        DBMS_OUTPUT.PUT_LINE('Error: No employee records found.');
    
    -- Handle any other unexpected errors
    WHEN OTHERS THEN
        DBMS_OUTPUT.PUT_LINE('An error occurred: ' || SQLERRM);
        IF emp_cursor%ISOPEN THEN
            CLOSE emp_cursor;
        END IF;
END;
/
```

**Output**

<img width="649" height="398" alt="image" src="https://github.com/user-attachments/assets/2d2114e5-1eff-40ff-b123-b9dff1e00f88" />

<img width="422" height="355" alt="image" src="https://github.com/user-attachments/assets/790be060-5521-4dae-bfc2-b2920f87aa45" />


---

### **Question 2: Parameterized Cursor with Exception Handling**

**Write a PL/SQL program using a parameterized cursor to retrieve and display employees with a salary in a given range. Implement exception handling for the following errors:**

1. **NO_DATA_FOUND**: When no employees meet the salary criteria.
2. **OTHERS**: For any unexpected errors during the execution.

**Steps:**

- Modify the `employees` table by adding a `salary` column.
- Insert sample salary values for the employees.
- Use a parameterized cursor to accept a salary range as input and fetch employees within that range.
- Implement exception handling to catch and display relevant error messages.

**Output:**  
The program should display the employee details within the specified salary range or an error message if no data is found.

**Program**
```
-- 1. Add the salary column to your existing table
ALTER TABLE employees ADD salary NUMBER(10, 2);

-- 2. Update existing rows with sample salaries
UPDATE employees SET salary = 50000 WHERE emp_id = 101;
UPDATE employees SET salary = 75000 WHERE emp_id = 102;
UPDATE employees SET salary = 40000 WHERE emp_id = 103;

-- 3. Add a new row to test different ranges
INSERT INTO employees VALUES (104, 'Diana Prince', 'Director', 95000);

SET SERVEROUTPUT ON;

DECLARE
    -- Step 1: Declare the Parameterized Cursor
    CURSOR emp_salary_cursor (p_min NUMBER, p_max NUMBER) IS
        SELECT emp_name, designation, salary 
        FROM employees 
        WHERE salary BETWEEN p_min AND p_max;

    -- Record variable to hold cursor row data
    v_emp_record emp_salary_cursor%ROWTYPE;
    
    -- Variables for the input range
    v_min_sal NUMBER := 60000;
    v_max_sal NUMBER := 100000;
    v_found BOOLEAN := FALSE;

BEGIN
    DBMS_OUTPUT.PUT_LINE('Searching for employees with salary between ' || v_min_sal || ' and ' || v_max_sal);
    DBMS_OUTPUT.PUT_LINE('---------------------------------------------------------');

    -- Step 2: Open cursor with parameters
    OPEN emp_salary_cursor(v_min_sal, v_max_sal);
    
    LOOP
        FETCH emp_salary_cursor INTO v_emp_record;
        EXIT WHEN emp_salary_cursor%NOTFOUND;
        
        v_found := TRUE;
        DBMS_OUTPUT.PUT_LINE('Name: ' || v_emp_record.emp_name || 
                             ' | Post: ' || v_emp_record.designation || 
                             ' | Salary: ' || v_emp_record.salary);
    END LOOP;

    -- Step 3: Handle the NO_DATA_FOUND case
    IF NOT v_found THEN
        RAISE NO_DATA_FOUND;
    END IF;

    CLOSE emp_salary_cursor;

EXCEPTION
    -- Catch specific and general exceptions
    WHEN NO_DATA_FOUND THEN
        DBMS_OUTPUT.PUT_LINE('Error: No employees found in the specified salary range.');
    WHEN OTHERS THEN
        DBMS_OUTPUT.PUT_LINE('An unexpected error occurred: ' || SQLERRM);
        IF emp_salary_cursor%ISOPEN THEN
            CLOSE emp_salary_cursor;
        END IF;
END;
/

```
**Output**

<img width="422" height="354" alt="image" src="https://github.com/user-attachments/assets/f3bea8e1-88ca-41f5-8381-1b0121a7f555" />

<img width="425" height="359" alt="image" src="https://github.com/user-attachments/assets/3e1e0ddd-95eb-4d7e-bcf9-683f88444c24" />


---

### **Question 3: Cursor FOR Loop with Exception Handling**

**Write a PL/SQL program using a cursor FOR loop to retrieve and display all employee names and their department numbers from the `employees` table. Implement exception handling for the following cases:**

1. **NO_DATA_FOUND**: If no employees are found in the database.
2. **OTHERS**: For any other unexpected errors.

**Steps:**

- Modify the `employees` table by adding a `dept_no` column.
- Insert sample department numbers for employees.
- Use a cursor FOR loop to fetch and display employee names along with their department numbers.
- Implement exception handling to catch the relevant exceptions.

**Output:**  
The program should display employee names with their department numbers or the appropriate error message if no data is found.

**Program**
```
-- 1. Add the dept_no column
ALTER TABLE employees ADD dept_no NUMBER(3);

-- 2. Update existing rows with department numbers
UPDATE employees SET dept_no = 10 WHERE emp_id = 101;
UPDATE employees SET dept_no = 20 WHERE emp_id = 102;
UPDATE employees SET dept_no = 10 WHERE emp_id = 103;
UPDATE employees SET dept_no = 30 WHERE emp_id = 104;

SET SERVEROUTPUT ON;

DECLARE
    -- Step 1: Declare the cursor
    CURSOR emp_dept_cursor IS
        SELECT emp_name, dept_no FROM employees;

    -- Variable to track if any rows were found
    v_counter NUMBER := 0;

BEGIN
    DBMS_OUTPUT.PUT_LINE('Employee Name      | Dept Number');
    DBMS_OUTPUT.PUT_LINE('---------------------------------');

    -- Step 2: Use a Cursor FOR Loop
    -- 'emp_rec' is implicitly declared as the record variable
    FOR emp_rec IN emp_dept_cursor LOOP
        DBMS_OUTPUT.PUT_LINE(RPAD(emp_rec.emp_name, 18) || ' | ' || emp_rec.dept_no);
        v_counter := v_counter + 1;
    END LOOP;

    -- Step 3: Check if data was found to handle NO_DATA_FOUND
    IF v_counter = 0 THEN
        RAISE NO_DATA_FOUND;
    END IF;

EXCEPTION
    -- Step 4: Exception Handling
    WHEN NO_DATA_FOUND THEN
        DBMS_OUTPUT.PUT_LINE('Error: No employee records found in the database.');
    WHEN OTHERS THEN
        DBMS_OUTPUT.PUT_LINE('An unexpected error occurred: ' || SQLERRM);
END;
/
```

**Output**

<img width="420" height="367" alt="image" src="https://github.com/user-attachments/assets/aff27d66-862e-4d6b-952b-122b351f2fdf" />

<img width="420" height="360" alt="image" src="https://github.com/user-attachments/assets/cce6cd6d-88ca-4263-a723-c6ca99190801" />



---

### **Question 4: Cursor with `%ROWTYPE` and Exception Handling**

**Write a PL/SQL program that uses a cursor with `%ROWTYPE` to fetch and display complete employee records (emp_id, emp_name, designation, salary). Implement exception handling for the following errors:**

1. **NO_DATA_FOUND**: When no employees are found in the database.
2. **OTHERS**: For any other errors that occur.

**Steps:**

- Modify the `employees` table by adding `emp_id`, `emp_name`, `designation`, and `salary` fields.
- Insert sample data into the `employees` table.
- Declare a cursor using `%ROWTYPE` to fetch complete rows from the `employees` table.
- Implement exception handling to catch the relevant exceptions and display appropriate messages.

**Output:**  
The program should display employee records or the appropriate error message if no data is found.

**Program**
```
-- 1. Ensure all columns exist and add a fresh record
INSERT INTO employees (emp_id, emp_name, designation, salary, dept_no) 
VALUES (105, 'John Wick', 'Security Consultant', 120000, 40);

-- 2. Verify the data exists
SELECT * FROM employees;

SET SERVEROUTPUT ON;

DECLARE
    -- Step 1: Declare the cursor for the full table
    CURSOR emp_full_cursor IS 
        SELECT emp_id, emp_name, designation, salary FROM employees;

    -- Step 2: Declare a record variable using %ROWTYPE
    -- This variable 'v_emp_rec' will have fields: emp_id, emp_name, etc.
    v_emp_rec emp_full_cursor%ROWTYPE;
    
    v_rows_found BOOLEAN := FALSE;

BEGIN
    DBMS_OUTPUT.PUT_LINE('ID  | NAME                | DESIGNATION         | SALARY');
    DBMS_OUTPUT.PUT_LINE('--------------------------------------------------------');

    -- Open the cursor
    OPEN emp_full_cursor;

    LOOP
        -- Fetch the whole row into the %ROWTYPE record
        FETCH emp_full_cursor INTO v_emp_rec;
        EXIT WHEN emp_full_cursor%NOTFOUND;

        v_rows_found := TRUE;

        -- Access individual fields from the record using dot notation
        DBMS_OUTPUT.PUT_LINE(
            RPAD(v_emp_rec.emp_id, 4) || ' | ' || 
            RPAD(v_emp_rec.emp_name, 19) || ' | ' || 
            RPAD(v_emp_rec.designation, 19) || ' | ' || 
            v_emp_rec.salary
        );
    END LOOP;

    -- Step 3: Handle NO_DATA_FOUND if no rows were processed
    IF NOT v_rows_found THEN
        RAISE NO_DATA_FOUND;
    END IF;

    CLOSE emp_full_cursor;

EXCEPTION
    -- Step 4: Exception Handling
    WHEN NO_DATA_FOUND THEN
        DBMS_OUTPUT.PUT_LINE('Error: No data found in the employees table.');
    WHEN OTHERS THEN
        DBMS_OUTPUT.PUT_LINE('An unexpected error occurred: ' || SQLERRM);
        -- Clean up: close cursor if it was left open during an error
        IF emp_full_cursor%ISOPEN THEN
            CLOSE emp_full_cursor;
        END IF;
END;
/
```
**Output**
<img width="419" height="367" alt="image" src="https://github.com/user-attachments/assets/7e04a93e-32e7-492b-9bab-4817dc805556" />

---

### **Question 5: Cursor with FOR UPDATE Clause and Exception Handling**

**Write a PL/SQL program using a cursor with the `FOR UPDATE` clause to update the salary of employees in a specific department. Implement exception handling for the following cases:**

1. **NO_DATA_FOUND**: If no rows are affected by the update.
2. **OTHERS**: For any unexpected errors during execution.

**Steps:**

- Modify the `employees` table to include a `dept_no` and `salary` field.
- Insert sample data into the `employees` table with different department numbers.
- Use a cursor with the `FOR UPDATE` clause to lock the rows of employees in a specific department and update their salary.
- Implement exception handling to handle `NO_DATA_FOUND` or other errors that may occur.

**Output:**  
The program should update employee salaries and display a message, or it should display an error message if no data is found.

**Program**
```
-- 1. Ensure we have clean data for testing
-- (Assuming columns already exist from previous steps)
UPDATE employees SET salary = 50000, dept_no = 10 WHERE emp_id = 101;
UPDATE employees SET salary = 40000, dept_no = 10 WHERE emp_id = 103;
UPDATE employees SET salary = 75000, dept_no = 20 WHERE emp_id = 102;

-- 2. Verify current state
SELECT emp_id, emp_name, dept_no, salary FROM employees;
SET SERVEROUTPUT ON;

DECLARE
    -- Step 1: Declare cursor with FOR UPDATE clause
    CURSOR emp_update_cursor(p_dept NUMBER) IS
        SELECT emp_id, emp_name, salary 
        FROM employees 
        WHERE dept_no = p_dept
        FOR UPDATE OF salary; -- Locks the salary column for these rows

    v_dept_input NUMBER := 10; -- Target Department
    v_rows_count NUMBER := 0;

BEGIN
    -- Step 2: Loop through the locked rows
    FOR emp_rec IN emp_update_cursor(v_dept_input) LOOP
        
        -- Update the salary using WHERE CURRENT OF
        UPDATE employees 
        SET salary = salary * 1.10
        WHERE CURRENT OF emp_update_cursor;
        
        v_rows_count := v_rows_count + 1;
        DBMS_OUTPUT.PUT_LINE('Updated Salary for: ' || emp_rec.emp_name);
    END LOOP;

    -- Step 3: Handle NO_DATA_FOUND if no rows matched the dept
    IF v_rows_count = 0 THEN
        RAISE NO_DATA_FOUND;
    ELSE
        DBMS_OUTPUT.PUT_LINE('Total Records Updated: ' || v_rows_count);
        -- Commit the changes to release the locks
        COMMIT; 
    END IF;

EXCEPTION
    WHEN NO_DATA_FOUND THEN
        DBMS_OUTPUT.PUT_LINE('Error: No employees found in Department ' || v_dept_input);
        ROLLBACK; -- Release locks if no data found
    WHEN OTHERS THEN
        DBMS_OUTPUT.PUT_LINE('An unexpected error occurred: ' || SQLERRM);
        ROLLBACK; -- Important: Release locks if an error occurs
END;
/
```

**Output**

<img width="422" height="368" alt="image" src="https://github.com/user-attachments/assets/5ead481a-fb7a-41da-8e8f-aa6be5744a89" />

---

## RESULT
Thus, the program successfully executed and displayed employee details using a cursor. 

