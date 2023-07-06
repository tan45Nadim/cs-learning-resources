## CREATE & ALTER
```SQL=
CREATE TABLE table_name (
    var_name INT,
    var_name2 VARCHAR(20),
    var_name3 VARCHAR(20) NOT NULL,
    var_name4 VARCHAR(20) UNIQUE,
    var_name5 VARCHAR(20) DEFAULT 'undecided', 
    var_name6 INT AUTO_INCREMENT,
    PRIMARY_KEY(var_name)
);
-- NOT NULL : Can't be empty while inserting
-- UNIQUE : All the contrains must be unique
-- Default : If not given, default value will be inserted

----ALTER
-- The ALTER TABLE statement is used to add, delete, or modify columns in an existing table.

-- add column
ALTER TABLE table_name
ADD column_name datatype;

-- drop column
ALTER TABLE table_name
DROP column_name;

-- remane column
ALTER TABLE table_name
RENAME COLUMN old_name to new_name;

-- The ALTER TABLE statement is also used to add and drop various constraints on an existing table

-- ALTER TABLE table_name
-- ADD FOREIGN KEY(var_name)
-- REFERENCES table_name(var_name)
-- ON DELETE SET NULL;

-- ALTER TABLE table_name
-- ADD FOREIGN KEY(super_id)
-- REFERENCES table_name(emp_id)
-- ON DELETE SET NULL;

ON DELETE SET NULL
-- if parent key is deleted, foreign key is set 'null'.
ON DELETE CASCADE
-- if parent row is deleted, child key row will be delete.
---------------
```

#### example

```sql=
CREATE TABLE employee (
    emp_id INT PRIMARY KEY,
    first_name VARCHAR(40),
    last_name VARCHAR(40),
    birth_day DATE,
    sex VARCHAR(1),
    salary INT,
    super_id INT,
    branch_id INT
);

CREATE TABLE branch (
    branch_id INT PRIMARY KEY,
    branch_name VARCHAR(40),
    mgr_id INT,
    mgr_start_date DATE,
    FOREIGN KEY(mgr_id) REFERENCES employee(emp_id) ON DELETE SET NULL
);

ALTER TABLE employee
ADD FOREIGN KEY(branch_id)
REFERENCES branch(branch_id)
ON DELETE SET NULL;

ALTER TABLE employee
ADD FOREIGN KEY(super_id)
REFERENCES employee(emp_id)
ON DELETE SET NULL;

CREATE TABLE client (
    client_id INT PRIMARY KEY,
    client_name VARCHAR(40),
    branch_id INT,
    FOREIGN KEY(branch_id) REFERENCES branch(branch_id) ON DELETE SET NULL
);

CREATE TABLE works_with (
    emp_id INT,
    client_id INT,
    total_sales INT,
    PRIMARY KEY(emp_id, client_id),
    FOREIGN KEY(emp_id) REFERENCES employee(emp_id) ON DELETE CASCADE,
    FOREIGN KEY(client_id) REFERENCES client(client_id) ON DELETE CASCADE
);

CREATE TABLE branch_supplier (
    branch_id INT,
    supplier_name VARCHAR(40),
    supply_type VARCHAR(40),
    PRIMARY KEY(branch_id, supplier_name),
    FOREIGN KEY(branch_id) REFERENCES branch(branch_id) ON DELETE CASCADE
);
---------------------------
```

```sql=
-- The DESCRIBE statement is used more to obtain information about a table structure
-- The EXPLAIN statement is used to obtain a query execution plan.
-- The DESCRIBE statement is a shortcut for SHOW COLUMN statement

explain employee;
show columns from employee;

describe employee;
describe branch;
describe client;
describe works_with;
describe branch_supplier;

SET FOREIGN_KEY_CHECKS = OFF; -- disable 'foreign' Keys
SET FOREIGN_KEY_CHECKS = ON; -- enable 'foreign' Keys

-- dispaly a specific table
SELECT * FROM employee;
SELECT * FROM branch;
SELECT * FROM client;
SELECT * FROM works_with;
SELECT * FROM branch_supplier;

-- remove a specific table
DROP TABLE employee;
DROP TABLE branch;
DROP TABLE client;
DROP TABLE works_with;
DROP TABLE branch_supplier;

```
## INSERT & UPDATE
```sql= 
-- insert
INSERT INTO table_name VALUES( , , , );
INSERT INTO table_name(var, var) VALUES( , , );

-- update
UPDATE table_name
SET var_name = 'Something' 
WHERE var_name = 'Something2';

UPDATE table_name
SET var_name = 'Something' 
WHERE var_name = 'Something2' OR var_name = 'Something2';

UPDATE table_name
SET var_name2 = 'Some', var_name3 = 'Some'
WHERE var_name = 'Something2';
```

#### example
```sql=
-- Corporate
INSERT INTO employee VALUES(100, 'Akash', 'Ullah', '2001-11-19', 'M', 20000, NULL, NULL);
INSERT INTO branch VALUES(1, 'Corporate', 100, '1990-02-22');

UPDATE employee
SET branch_id = 1
WHERE emp_id = 100;

-- Scranton
INSERT INTO employee VALUES(101, 'Karim', 'Hasan', '1999-01-30', 'F', 202110, 100, 1);
INSERT INTO employee VALUES(102, 'Mike', 'Dane', '2005-11-1', 'F', 60000, 100, NULL);
INSERT INTO branch VALUES(2, 'Scranton', 102, '1992-9-30');

UPDATE employee 
SET branch_id = 2
WHERE emp_id = 102;

INSERT INTO employee VALUES(103, 'Rahim', 'Khan', '1991-05-30', 'M', 2110, 102, 2);
INSERT INTO employee VALUES(104, 'Angel', 'Martin', '1996-11-1', 'F', 230, 102, 2);
INSERT INTO employee VALUES(105, 'Abir', 'Ahsan', '2000-03-30', 'M', 20, 102, 2);
-- Stamford
INSERT INTO employee VALUES(106, 'Joy', 'DATTA', '2019-12-1', 'F', 330, 100, NULL);
INSERT INTO branch VALUES(3, 'Stamford', 106, '1999-9-30');

UPDATE employee 
SET branch_id = 3
WHERE emp_id = 106;

INSERT INTO employee VALUES(107, 'Md. Jaman', 'Uddin', '1419-1-30', 'M', 233330, 106, 3);
INSERT INTO employee VALUES(108, 'Siam', 'Ahmed', '9999-12-1', 'M', 388888, 106, 3);

-- CLIENT
INSERT INTO client VALUES(400, "MC School", 2);
INSERT INTO client VALUES(401, "DCC", 2);
INSERT INTO client VALUES(402, "BCIC College", 3);
INSERT INTO client VALUES(403, "Bangla College", 3);
INSERT INTO client VALUES(404, "NSU", 2);
INSERT INTO client VALUES(405, "BracU", 1);

-- WORKS_WITH
INSERT INTO works_with VALUES(105, 400, 4321);
INSERT INTO works_with VALUES(102, 401, 12);
INSERT INTO works_with VALUES(108, 402, 2244);
INSERT INTO works_with VALUES(107, 403, 44332);
INSERT INTO works_with VALUES(108, 404, 1151);
INSERT INTO works_with VALUES(105, 404, 2000);
INSERT INTO works_with VALUES(107, 405, 26000);
INSERT INTO works_with VALUES(102, 404, 110000);
INSERT INTO works_with VALUES(105, 405, 33200);

-- BRANCH SUPPLIER
INSERT INTO branch_supplier VALUES(2, 'Asad Haq', 'PAPER');
INSERT INTO branch_supplier VALUES(2, 'Ahm Kabir', 'DFD');
INSERT INTO branch_supplier VALUES(3, 'AzD', 'PAPER');
INSERT INTO branch_supplier VALUES(2, 'Polash', 'PCf');
INSERT INTO branch_supplier VALUES(3, 'Hasan', 'WU');
INSERT INTO branch_supplier VALUES(3, 'ADFD', 'PAPER');
INSERT INTO branch_supplier VALUES(3, 'AxP', 'customs forms');
-----------
```

## FUNCTION
```SQL=

```

#### example


```sql=
--Function

SELECT COUNT(emp_id)
FROM employee;

SELECT AVG(salary)
FROM employee;

SELECT SUM(salary)
FROM employee;

SELECT COUNT(sex)
FROM employee
WHERE sex = 'M';

SELECT COUNT(sex), sex
FROM employee 
GROUP BY sex;

SELECT client_id, SUM(total_sales)
FROM works_with
GROUP BY client_id;
```

## WILDCARDS

```SQL=

```

#### example
```sql=
-- WIldcards

SELECT *
FROM client
WHERE client.client_name LIKE '%c%';

SELECT *
FROM branch_supplier
WHERE branch_supplier.supplier_name LIKE '%sh';

SELECT * 
FROM employee
WHERE employee.last_name LIKE '__r%';

SELECT employee.emp_id, employee.first_name, employee.last_name
FROM employee
WHERE employee.last_name LIKE 'kh%';

SELECT * 
FROM client 
WHERE client.client_name LIKE '% %';
-------------
```

## UNION

```SQL=

```

#### example
```sql=
-- UNION

SELECT first_name
FROM employee
union 
SELECT branch_name
FROM branch;

SELECT employee.first_name as "Employee & Branch NAMES"
FROM employee
union
SELECT branch.branch_name
FROM branch;

SELECT client.client_name as C_N, client.branch_id as B_ID
FROM client
union
SELECT branch_supplier.supplier_name, branch_supplier.branch_id
FROM branch_supplier; 
```


## JOIN
```SQL=

```

#### example
```sql=
-- JOIN

SELECT employee.emp_id, employee.first_name, branch.branch_name
FROM employee
JOIN branch
ON employee.emp_id = branch.mgr_id;

SELECT branch.branch_name, client.client_id, client.client_name
FROM client
JOIN branch
ON client.branch_id = branch.branch_id;

SELECT works_with.emp_id, works_with.client_id, works_with.total_sales
FROM works_with
JOIN client
ON works_with.client_id = client.client_id;
```


## NESTED QUERIES
```SQL=

```

#### example
```sql=
-- Nested Queries

-- Find names of all employees who have sold over 10000
SELECT employee.first_name, employee.last_name
FROM employee
WHERE employee.emp_id in (
    SELECT works_with.emp_id
    FROM works_with
    WHERE works_with.total_sales > 10000
);

-- Find all clients who are handles by the branch mgr_id = 106
SELECT client.client_id, client.client_name 
FROM client
WHERE client.branch_id = (
    SELECT branch.branch_id
    FROM branch
    WHERE branch.mgr_id = 106
);


 -- Find all clients who are handles by the branch that Mike Dane manages
 -- Assume you DONT'T know Mike's ID
SELECT client.client_id, client.client_name 
FROM client
WHERE client.branch_id = (
    SELECT branch.branch_id
    FROM branch
    WHERE branch.mgr_id = (
        SELECT employee.emp_id
        FROM employee
        WHERE employee.first_name = 'MIKE' AND employee.last_name = 'dane'
        -- LIMIT 1
    ) 
);

-- Find the names of employees who work with clients handled by the scranton branch
SELECT employee.first_name, employee.last_name
FROM employee
WHERE employee.emp_id in (
    SELECT works_with.emp_id
    FROM works_with
) AND employee.branch_id = 2;


-- Find the names of all clients who have spent more than 100,00 dollars
SELECT client.client_name
FROM client
WHERE client.client_id in (
    SELECT works_with.client_id
    FROM works_with 
    WHERE works_with.total_sales > 10000
);
---------------
```

## DELETE
```SQL=
DELETE FROM table_name
WHERE var_name = something;
-- Delete row having the 'something' in 'var_name' column
```

#### example
```sql=
DELETE FROM employee
WHERE emp_id = 102;

DELETE FROM branch
WHERE branch_id = 2;
```

## EXTRA
#### example

```sql=
SELECT * 
FROM employee 
ORDER BY salary ASC;

SELECT * 
FROM employee 
ORDER BY salary DESC;

SELECT * 
FROM employee
ORDER BY sex, first_name, last_name;

SELECT *
FROM employee 
LIMIT 5;

SELECT first_name, employee.last_name
FROM employee;

SELECT employee.first_name, employee.last_name 
FROM employee;

SELECT first_name as forename, last_name as surname
FROM employee;

SELECT distinct sex
FROM employee;

SELECT * 
FROM employee 
WHERE sex = 'F';

SELECT * FROM
employee WHERE brAnch_id = 2;

SELECT employee.emp_id, employee.first_name, employee.last_name, employee.salary
FROM employee
WHERE salary > 3000;

SELECT *
FROM employee
WHERE branch_id = 2 AND sex = 'm';

SELECT *
FROM employee 
WHERE sex = 'M' AND birth_day >= 2000-01-01 AND salary > 3000;

SELECT *
FROM employee
WHERE salary between 100 AND 3000;

SELECT *
FROM employee
WHERE first_name in ('Abir', 'Rahim', 'Karim', 'Safiq');
-----------
```

## TRUNCATE
#### example


```sql=
-- TRUNCATE
-- it removes all rows from a table,
-- but the table structure and its columns, constraints, indexes, and so on remain

TRUNCATE TABLE branch_supplier;
```

Source [Giraffe Academy](https://www.youtube.com/playlist?list=PLLAZ4kZ9dFpMGXTKXsBM_ZNpJwowfsP49)
