# Experiment 5: Subqueries and Views
## NAME : A.Chandhna
## REGISTRATION NUMBER : 212224040051

## AIM
To study and implement subqueries and views.

## THEORY

### Subqueries
A subquery is a query inside another SQL query and is embedded in:
- WHERE clause
- HAVING clause
- FROM clause

**Types:**
- **Single-row subquery**:
  Sub queries can also return more than one value. Such results should be made use along with the operators in and any.
- **Multiple-row subquery**:
  Here more than one subquery is used. These multiple sub queries are combined by means of ‘and’ & ‘or’ keywords.
- **Correlated subquery**:
  A subquery is evaluated once for the entire parent statement whereas a correlated Sub query is evaluated once per row processed by the parent statement.

**Example:**
```sql
SELECT * FROM employees
WHERE salary > (SELECT AVG(salary) FROM employees);
```
### Views
A view is a virtual table based on the result of an SQL SELECT query.
**Create View:**
```sql
CREATE VIEW view_name AS
SELECT column1, column2 FROM table_name WHERE condition;
```
**Drop View:**
```sql
DROP VIEW view_name;
```

**Question 1**
Write a SQL query to List departments with names longer than the average length
Departments Table (attributes: department_id, department_name)
![image](https://github.com/user-attachments/assets/601b7491-6659-406c-b3ef-09a48970ea30)
~~~
SELECT department_id, department_name
FROM Departments
WHERE LENGTH(department_name) > (
    SELECT AVG(LENGTH(department_name)) FROM Departments
);
~~~


**Output:**

![image](https://github.com/user-attachments/assets/b4b34710-2470-484a-864c-20f49ada5ac0)


**Question 2**
Write a SQL query to Retrieve the medications with dosages equal to the lowest dosage
Table Name: Medications (attributes: medication_id, medication_name, dosage)
![image](https://github.com/user-attachments/assets/b3ce9e5e-6c7e-4df8-b7bb-ff451f1b5528)
~~~
SELECT medication_id, medication_name, dosage
FROM Medications
WHERE dosage = (
    SELECT MIN(dosage)
    FROM Medications
);
~~~

**Output:**
![image](https://github.com/user-attachments/assets/48b57122-79e7-4546-a7a3-a47045cc3faa)



**Question 3**
Write a SQL query to Identify customers whose city is different from the city of the customer with the highest ID
SAMPLE TABLE: customer
~~~
name             type
---------------  ---------------
id               INTEGER
name             TEXT
city             TEXT
email            TEXT
phone            INTEGER
~~~

~~~
SELECT *
FROM customer
WHERE city <> (
    SELECT city
    FROM customer
    WHERE id = (SELECT MAX(id) FROM customer)
);
~~~
**Output:**

![image](https://github.com/user-attachments/assets/7ac19955-caf9-4ec4-9728-cf306c683979)


**Question 4**
Write a SQL query to Retrieve the names of customers who have a phone number that is not shared with any other customer.
SAMPLE TABLE: customer
~~~
name             type
---------------  ---------------
id               INTEGER
name             TEXT
city             TEXT
email            TEXT
phone            INTEGER
~~~
~~~
SELECT name
FROM customer
WHERE phone IN (
    SELECT phone
    FROM customer
    GROUP BY phone
    HAVING COUNT(*) = 1
);
~~~

**Output:**

![image](https://github.com/user-attachments/assets/d3aaaee9-420a-4a99-9392-e2c54b498511)


**Question 5**
Write a SQL query that retrieve all the columns from the table "Grades", where the grade is equal to the maximum grade achieved in each subject.

Sample table: GRADES (attributes: student_id, student_name, subject, grade)
![image](https://github.com/user-attachments/assets/c0d045aa-4f81-4584-ab4a-cd9ad6ec0363)
~~~
SELECT *
FROM Grades g
WHERE grade = (
    SELECT MAX(grade)
    FROM Grades
    WHERE subject = g.subject
);
~~~

**Output:**

![image](https://github.com/user-attachments/assets/1622ea28-49bc-4fa5-9237-a65aab635a0e)

**Question 6**
Write a SQL query to retrieve all columns from the CUSTOMERS table for customers whose Address as Delhi

Sample table: CUSTOMERS
~~~

ID          NAME        AGE         ADDRESS     SALARY
----------  ----------  ----------  ----------  ----------

1          Ramesh     32              Ahmedabad     2000
2          Khilan        25              Delhi                 1500
3          Kaushik      23              Kota                  2000
4          Chaitali       25             Mumbai            6500
5          Hardik        27              Bhopal              8500
6          Komal         22              Hyderabad       4500

7           Muffy          24              Indore            10000
~~~
~~~
SELECT *
FROM CUSTOMERS
WHERE ADDRESS = 'Delhi';
~~~
**Output:**

![image](https://github.com/user-attachments/assets/400b166d-7512-49dc-9eb6-aca2ca7714b8)

**Question 7**
Write a SQL query to Retrieve the names and cities of customers who have the same city as customers with IDs 3 and 7

SAMPLE TABLE: customer
~~~

name             type
---------------  ---------------
id               INTEGER
name             TEXT
city             TEXT
email            TEXT
phone            INTEGER
~~~
~~~
SELECT name, city
FROM customer
WHERE city IN (
    SELECT city
    FROM customer
    WHERE id IN (3, 7)
);
~~~

**Output:**

![image](https://github.com/user-attachments/assets/b60183fb-f979-4821-931b-b2f8b297ad78)

**Question 8**
Write a SQL query that retrieves the names of students and their corresponding grades, where the grade is equal to the maximum grade achieved in each subject.
Sample table: GRADES
![image](https://github.com/user-attachments/assets/bf246299-c6d3-4770-854d-d9c0fdf37cbd)
~~~
select student_name   ,  grade
from GRADES g
where grade =
(
     select max(grade)
     from GRADES
     where subject = g.subject
);
~~~


**Output:**

![image](https://github.com/user-attachments/assets/6738e361-a609-4902-a782-c6e06040f2ac)


**Question 9**
Write a SQL query to retrieve all columns from the CUSTOMERS table for customers whose salary is LESS than $2500.

Sample table: CUSTOMERS
~~~

ID          NAME        AGE         ADDRESS     SALARY
----------  ----------  ----------  ----------  ----------

1          Ramesh     32              Ahmedabad     2000
2          Khilan        25              Delhi                 1500
3          Kaushik      23              Kota                  2000
4          Chaitali       25             Mumbai            6500
5          Hardik        27              Bhopal              8500
6          Komal         22              Hyderabad       4500

7           Muffy          24              Indore            10000
~~~
~~~
select *
from CUSTOMERS
where SALARY < 2500;
~~~


**Output:**

![image](https://github.com/user-attachments/assets/d7c3e604-6c72-4a2c-a51c-f34c15b61bc7)

**Question 10**
From the following tables write a SQL query to find the order values greater than the average order value of 10th October 2012. Return ord_no, purch_amt, ord_date, customer_id, salesman_id.

Note: date should be yyyy-mm-dd format
ORDERS TABLE
~~~

name            type
----------     ----------
ord_no          int
purch_amt    real
ord_date       text
customer_id  int
salesman_id  int
~~~
~~~
select * 
from ORDERS
where purch_amt >
(
     select purch_amt
     from ORDERS
     where ord_date = '2012-10-10'
);
~~~

**Output:**

![image](https://github.com/user-attachments/assets/4e78fefc-67a9-4d14-84b9-e972e14fd3eb)



## RESULT
Thus, the SQL queries to implement subqueries and views have been executed successfully.
