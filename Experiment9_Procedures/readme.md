# Experiment 9: PL/SQL – Procedures and Functions

## AIM
To understand and implement procedures and functions in PL/SQL for performing various operations such as calculations, decision-making, and looping.

---

## THEORY

PL/SQL (Procedural Language/SQL) extends SQL by adding procedural constructs like variables, conditions, loops, procedures, and functions. Procedures and functions are subprograms that help modularize the code and improve reusability.

### **Procedure**
A PL/SQL **procedure** is a subprogram that performs a specific action. It does not return a value directly but can return values using `OUT` parameters.

**Syntax:**
```sql
CREATE OR REPLACE PROCEDURE procedure_name (parameters)
IS
BEGIN
   -- statements
END;
```

To call the procedure

```sql
EXEC procedure_name(arguments);
```

### **Function**
A PL/SQL **function** is a subprogram that returns a single value using the RETURN keyword.

```sql
CREATE OR REPLACE FUNCTION function_name (parameters)
RETURN datatype
IS
BEGIN
   -- statements
   RETURN value;
END;
```

To call the function:

```sql
SELECT function_name(arguments) FROM DUAL;
```

Key Differences:

-A procedure does not return a value, whereas a function must return a value.
-Functions can be called from SQL queries, procedures cannot (in most cases).

## 1. Write a PL/SQL Procedure to Find the Square of a Number

### Steps:
- Create a procedure named `find_square`.
- Declare a parameter to accept a number.
- Inside the procedure, compute the square of the input number.
- Use `DBMS_OUTPUT.PUT_LINE` to display the result.
- Call the procedure with a number as input.

**Expected Output:**  
Square of 6 is 36

**Program**
```
-- Step 1: Create the procedure
CREATE OR REPLACE PROCEDURE find_square (num IN NUMBER) IS
    result NUMBER;
BEGIN
    -- Step 2: Calculate square
    result := num * num;

    -- Step 3: Display result
    DBMS_OUTPUT.PUT_LINE('Square of ' || num || ' is ' || result);
END;
/

-- Step 4: Call the procedure
BEGIN
    find_square(6);
END;
/
```
**Output**

<img width="418" height="352" alt="image" src="https://github.com/user-attachments/assets/f7c5e33d-5eb7-4d71-80e8-974f9b9cd8de" />

---

## 2. Write a PL/SQL Function to Return the Factorial of a Number

### Steps:
- Create a function named `get_factorial`.
- Declare a parameter to accept a number.
- Use a loop to calculate the factorial.
- Return the result using the `RETURN` statement.
- Call the function using a `SELECT` statement or in an anonymous block.

**Expected Output:**  
Factorial of 5 is 120

**Program**
```
SET SERVEROUTPUT ON;

DECLARE
    -- Step 1: Define function inside the block
    FUNCTION get_factorial (num IN NUMBER) RETURN NUMBER IS
        fact NUMBER := 1;
    BEGIN
        -- Step 2: Calculate factorial
        FOR i IN 1..num LOOP
            fact := fact * i;
        END LOOP;
        RETURN fact;
    END;

    result NUMBER;
BEGIN
    -- Step 3: Call the function
    result := get_factorial(5);

    -- Step 4: Display result
    DBMS_OUTPUT.PUT_LINE('Factorial of 5 is ' || result);
END;
/
```
**Output**

<img width="415" height="364" alt="image" src="https://github.com/user-attachments/assets/6a1e4fd7-838d-4725-b875-59f2bc558c0d" />



---

## 3. Write a PL/SQL Procedure to Check Whether a Number is Even or Odd

### Steps:
- Create a procedure named `check_even_odd`.
- Accept an input parameter.
- Use the `MOD` function to check if the number is divisible by 2.
- Display whether it is Even or Odd using `DBMS_OUTPUT.PUT_LINE`.

**Expected Output:**  
12 is Even

**Program**
```
SET SERVEROUTPUT ON;

-- Part 1: Create the Procedure
CREATE OR REPLACE PROCEDURE check_even_odd (p_num IN NUMBER) IS
BEGIN
    -- Use the MOD function to check for divisibility by 2
    IF MOD(p_num, 2) = 0 THEN
        DBMS_OUTPUT.PUT_LINE(p_num || ' is Even');
    ELSE
        DBMS_OUTPUT.PUT_LINE(p_num || ' is Odd');
    END IF;
END;
/

-- Part 2: Execute/Call the Procedure
BEGIN
    check_even_odd(12);
END;
/
```
**Output**

<img width="404" height="347" alt="image" src="https://github.com/user-attachments/assets/72d4dc33-8f9a-4738-a772-9051ebd4a014" />


---

## 4. Write a PL/SQL Function to Return the Reverse of a Number

### Steps:
- Create a function named `reverse_number`.
- Accept an input number as parameter.
- Use a loop to reverse the digits of the number.
- Return the reversed number.
- Call the function and display the output.

**Expected Output:**  
Reversed number of 1234 is 4321

**Program**
```
SET SERVEROUTPUT ON;

-- Part 1: Create the Function
CREATE OR REPLACE FUNCTION reverse_number (p_num IN NUMBER) 
RETURN NUMBER IS
    v_temp_n NUMBER;
    v_rev_num NUMBER := 0;
    v_remainder NUMBER;
BEGIN
    v_temp_n := p_num;

    -- Use a loop to extract each digit and reverse
    WHILE v_temp_n > 0 LOOP
        v_remainder := MOD(v_temp_n, 10);          -- Get the last digit
        v_rev_num := (v_rev_num * 10) + v_remainder; -- Shift and add
        v_temp_n := TRUNC(v_temp_n / 10);          -- Remove the last digit
    END LOOP;

    -- Return the reversed number to the caller
    RETURN v_rev_num;
END;
/

-- Part 2: Execute/Call the Function
BEGIN
    DBMS_OUTPUT.PUT_LINE('Reversed number of 1234 is ' || reverse_number(1234));
END;
/
```


**Output**

<img width="419" height="356" alt="image" src="https://github.com/user-attachments/assets/64ea9725-e0ed-4c9a-8004-a556797af094" />


---

## 5. Write a PL/SQL Procedure to Display the Multiplication Table of a Number

### Steps:
- Create a procedure named `print_table`.
- Accept an input number.
- Use a loop from 1 to 10 to multiply the input number.
- Display the multiplication results using `DBMS_OUTPUT.PUT_LINE`.

**Expected Output:**  
Multiplication table of 5:  
5 x 1 = 5  
5 x 2 = 10  
5 x 3 = 15  
...  
5 x 10 = 50

**Program**
```
SET SERVEROUTPUT ON;

-- Part 1: Create the Procedure
CREATE OR REPLACE PROCEDURE print_table (p_num IN NUMBER) IS
BEGIN
    DBMS_OUTPUT.PUT_LINE('Multiplication table of ' || p_num || ':');
    DBMS_OUTPUT.PUT_LINE('---------------------------');

    -- Step 2: Use a loop from 1 to 10
    FOR i IN 1..10 LOOP
        -- Step 3: Display the multiplication results
        DBMS_OUTPUT.PUT_LINE(p_num || ' x ' || i || ' = ' || (p_num * i));
    END LOOP;
END;
/

-- Part 2: Execute/Call the Procedure
BEGIN
    print_table(5);
END;
/
```

**Output**

<img width="413" height="355" alt="image" src="https://github.com/user-attachments/assets/f9fb5555-03c9-4800-85f8-fbb466cf0a55" />


## RESULT
Thus, the PL/SQL programs using procedures and functions were written, compiled, and executed successfully.
