# Experiment 7: PL/SQL – Variables, Control Structures and Loops

## AIM
To write and execute simple PL/SQL programs using variables, loops, and conditional statements.


## THEORY

PL/SQL, which stands for Procedural Language extensions to the Structured Query Language (SQL). It is a combination of SQL along with the procedural features of programming languages.

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

# PL/SQL Programs – Steps and Expected Output

## 1. Write a PL/SQL program to find the Greatest of Two Numbers

### Steps:
- Declare two numeric variables and initialize them.
- Use an `IF` statement to compare the values.
- Display the greater number using `DBMS_OUTPUT.PUT_LINE`.

**Expected Output:**  
Greater number is: 80

**Program**
```
DECLARE
    -- Step 1: Declare and initialize two numeric variables
    num1 NUMBER := 50;
    num2 NUMBER := 80;
BEGIN
    -- Step 2: Use an IF statement to compare values
    IF num1 > num2 THEN
        DBMS_OUTPUT.PUT_LINE('Greater number is: ' || num1);
    ELSE
        -- Step 3: Display the greater number
        DBMS_OUTPUT.PUT_LINE('Greater number is: ' || num2);
    END IF;
END;
/
```
**Output**



<img width="401" height="372" alt="image" src="https://github.com/user-attachments/assets/4395ccaa-32ee-4c9a-a66c-dcb8f55f01c2" />


---

## 2. Write a PL/SQL program to Calculate Sum of First N Natural Numbers

### Steps:
- Declare a variable `n` and assign a value (e.g., 10).
- Initialize a `sum` variable to 0.
- Use a `WHILE` loop to iterate from 1 to `n`, adding each number to the sum.
- Display the result using `DBMS_OUTPUT.PUT_LINE`.

**Expected Output:**  
Sum of first 10 natural numbers is: 55

**Program**
```
DECLARE
    -- Step 1: Declare variable n and sum
    n NUMBER := 10;
    v_sum NUMBER := 0;
    i NUMBER := 1;
BEGIN
    -- Step 2: Use a WHILE loop to iterate from 1 to n
    WHILE i <= n LOOP
        v_sum := v_sum + i;
        i := i + 1;
    END LOOP;

    -- Step 3: Display the result
    DBMS_OUTPUT.PUT_LINE('Sum of first ' || n || ' natural numbers is: ' || v_sum);
END;
/
```
**Output**

<img width="422" height="379" alt="image" src="https://github.com/user-attachments/assets/66997424-32f3-4ed6-b54e-59bb016a9aeb" />


---

## 3. Write a PL/SQL program to generate Fibonacci series

### Steps:
- Declare the variable `n` to indicate how many terms to generate.
- Initialize the first two Fibonacci numbers (0 and 1).
- Use a loop to generate the next terms using the formula `c = a + b`.
- Print each term in the series.

**Expected Output:**  
n = 7  
Fibonacci sequence: 0, 1, 1, 2, 3, 5, 8

**Program**
```
DECLARE
    -- Step 1: Declare variables
    n NUMBER := 7;       -- Number of terms to generate
    a NUMBER := 0;       -- First Fibonacci number
    b NUMBER := 1;       -- Second Fibonacci number
    c NUMBER;            -- Variable to store the next term
    i NUMBER;
BEGIN
    DBMS_OUTPUT.PUT_LINE('n = ' || n);
    DBMS_OUTPUT.PUT('Fibonacci sequence: ');

    -- Loop to generate and print terms
    FOR i IN 1..n LOOP
        -- Print the current first number (a)
        IF i < n THEN
            DBMS_OUTPUT.PUT(a || ', ');
        ELSE
            DBMS_OUTPUT.PUT_LINE(a);
        END IF;

        -- Step 3: Use the formula c = a + b to generate next terms
        c := a + b;
        a := b;
        b := c;
    END LOOP;
END;
/
```
**Output**

<img width="415" height="378" alt="image" src="https://github.com/user-attachments/assets/38316b44-229f-40f2-b19b-30192582c21e" />


---

## 4. Write a PL/SQL Program to display the number in Reverse Order

### Steps:
- Declare a variable `n` and assign a value (e.g., 1535).
- Use a loop to extract each digit using modulo and reverse the number.
- Display the reversed number.

**Expected Output:**  
n = 1535  
Reversed number is 5351

**Program**
```
DECLARE
    -- Step 1: Declare and initialize the number
    n NUMBER := 1535;
    temp_n NUMBER;
    rev_num NUMBER := 0;
    remainder NUMBER;
BEGIN
    temp_n := n; -- Store original value for processing

    -- Step 2: Loop to extract digits and reverse
    WHILE temp_n > 0 LOOP
        remainder := MOD(temp_n, 10);        -- Get the last digit
        rev_num := (rev_num * 10) + remainder; -- Build the reversed number
        temp_n := TRUNC(temp_n / 10);        -- Remove the last digit
    END LOOP;

    -- Step 3: Display the result
    DBMS_OUTPUT.PUT_LINE('n = ' || n);
    DBMS_OUTPUT.PUT_LINE('Reversed number is ' || rev_num);
END;
/
```
**Output**

<img width="420" height="374" alt="image" src="https://github.com/user-attachments/assets/63fc0c90-5540-4381-b913-9ba187456976" />


---

## 5. Write a PL/SQL program to find the largest of three numbers

### Steps:
- Declare three numeric variables `a`, `b`, and `c`.
- Use nested `IF-ELSIF-ELSE` conditions to find the largest among the three.
- Display the largest number.

**Expected Output:**  
a = 10, b = 9, c = 15  
Largest of three number is 15

**Program**
```
DECLARE
    -- Step 1: Declare and initialize three numeric variables
    a NUMBER := 10;
    b NUMBER := 9;
    c NUMBER := 15;
BEGIN
    DBMS_OUTPUT.PUT_LINE('a = ' || a || ', b = ' || b || ', c = ' || c);

    -- Step 2: Use nested IF-ELSIF-ELSE conditions
    IF (a >= b) AND (a >= c) THEN
        DBMS_OUTPUT.PUT_LINE('Largest of three number is ' || a);
    ELSIF (b >= a) AND (b >= c) THEN
        DBMS_OUTPUT.PUT_LINE('Largest of three number is ' || b);
    ELSE
        -- Step 3: Display the largest number (c)
        DBMS_OUTPUT.PUT_LINE('Largest of three number is ' || c);
    END IF;
END;
/
```
**Output**

<img width="423" height="378" alt="image" src="https://github.com/user-attachments/assets/de1c6163-24ae-4a56-b9fb-c15ed93348f4" />


## RESULT
Thus, the PL/SQL programs using variables, conditionals, and loops were executed successfully.
