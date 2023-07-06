[companySchema_01](https://github.com/tan45Nadim/cs-learning-resources/blob/main/MySQL/CSE311L/companySchema_01.sql) &
[companySchema_02](https://github.com/tan45Nadim/cs-learning-resources/blob/main/MySQL/CSE311L/companySchema_02.sql)

## Lab 2

```sql=
-- Write a query that displays the last name , weekly salary,
-- department number of the employees. Name the salary column as "Weekly Salary".

select lname, ((salary * 12) /52) as weeklySalary, dno
from employee;
```

```sql=
-- Retrieve the birth date and address of the employee whose name
-- is 'John Smith'.

select bdate, address
from employee
where fname = 'john' and lname = 'smith'
```

```sql=
-- Retrieve the name and address of all employees who
-- work for the 'Research' department.

select fname, lname, address
from employee, department
where department.dname = 'Research' and dno = dnumber;
```

```sql=
-- For every project located in 'Stafford', list the project number,
-- the controlling department number, and the department manager's
-- last name, address, and birth date.

select project.pnumber, project.dnum, employee.lname, employee.address, employee.bdate
from project, employee, department
where project.plocation = 'stafford' and project.dnum = department.dnumberand department.mgrssn = employee.ssn;
```

```sql=
-- For each employee, retrieve the employee's name, and the
-- name of his or her immediate supervisor.

select e.fname, e.lname, s.fname, s.lname
from employee as e, employee as s
where e.superssn = s.ssn
```

```sql=
-- Retrieve the SSN values for all employees.

select ssn
from employee;
```

```sql=
-- SELECT SSN, DNAME FROM EMPLOYEE, DEPARTMENT

select ssn, dname
from employee, department
where employee.ssn = department.mgrssn
```

```sql=
-- Retrieve all the attribute values of EMPLOYEES who work in department 5.

select *
from employee
where dno = 5;
```

```sql=
-- Retrieve all the attributes of an employee and attributes
-- of DEPARTMENT he works in for every employee of ‘Research’ department.

select *
from employee, department
where employee.dno = department.dnumber and department.dname = 'Research';
```
```sql=
-- Find the first name and Last name of the employees who
-- are supervised by “Franklin Wong’?

select e.fname, e.lname
from employee as e, employee as s
where s.ssn = e.superssn and s.fname = 'Franklin' and s.lname = 'Wong'
```
```sql=
-- Find the last and first name of the female employees
-- who have a dependent with the same first name as themselves?

select employee.lname, employee.fname
FROM employee, dependent
WHERE employee.ssn = dependent.essn and employee.sex = 'F' and employee.fname = dependent.dependent_name
```

```sql=
-- For each department find out the department manager’s
-- last name, his start date and the name his dependents (if any)?

select lname, department.mgrstartdate, dependent.dependent_name
from department, employee, dependent
where department.mgrssn = employee.ssn and dependent.essn = department.mgrssn
```

```sql=
-- For each employee find out the employee’s last and first name,
-- the department name in which he works and the project name
-- he works in and the number of hours he work in those projects.

select employee.fname, employee.lname, department.dname, project.pname, works_on.hours
from employee, department, project, works_on
where employee.dno = department.dnumber and department.dnumber = project.dnum
and works_on.pno = project.pnumber and employee.ssn = works_on.essn

```

## Lab 3
```sql=
-- Display the employee last name, job ID, and start date of employees
-- hired between February 20, 2004, and May 1, 2006. Order the query in ascending
-- order by start date.

select Last_Name, Job_Id, Hire_Date
from emps
where Hire_Date between ('2004-02-20') and ('2006-05-01')
order by Hire_Date
-----
select Last_Name, Job_Id, Hire_Date
from emps
where Hire_Date >= ('2004-02-20') and Hire_Date <= ('2006-05-01')
order by Hire_Date
```

```sql=
-- Display the last name and department number of all employees in departments 20
-- and 50 in alphabetical order by name.

select Last_Name, Department_Id
from emps
where Department_Id between 20 and 50
order by Last_Name
-----
select Last_Name, Department_Id
from emps
where Department_Id >= 20 and Department_Id <= 50
order by Last_Name
```


```sql=
-- Retrieve the name and address of all employees who work for the
-- 'Research' department.

select fname, lname, address
from employee
where dno in (
    select dnumber
    from department
    where dname = 'Research'
)
```              

```sql=
-- Retrieve the name of each employee who has a dependent with the
-- same first name as the employee.

select e.fname, e.lname
from employee as e
where e.ssn in (
    select d.essn
    from dependent as d
    where d.essn = e.ssn and e.fname = d.dependent_name
)
-----
select e.fname, e.lname
from employee as e, dependent as d 
where d.essn = e.ssn and e.fname = d.dependent_name
-----
select e.fname, e.lname
from employee as e
where exists (
    select d.essn
    from dependent as d
    where d.essn = e.ssn and e.fname = d.dependent_name
)
```              
    
```sql=
-- Retrieve the names of employees who have no dependents.

select e.fname, e.lname
from employee as e
where not exists (
    select d.essn
    from dependent as d
    where d.essn = e.ssn
)
```              

```sql=
-- Retrieve the social security numbers of all employees who
-- work on project number 1, 2, or 3.

select distinct works_on.essn
from works_on
where works_on.pno in (1, 2, 3))
```

```sql=
-- Retrieve the names of all employees who do not have supervisors.

select fname, lname
from employee
where employee.superssn is null
```

```sql=
-- Retrieve all employees whose address is in Houston, Texas. Here,
-- the value of the ADDRESS attribute must contain the substring
-- 'Houston TX‘ in it.

select *
from employee
where employee.address like '%Houston TX%'
```

```sql=
-- Retrieve all employees who were born during the 1965s.

select *
from employee
where bdate like '1965%'
```

```sql=
-- Display the last name and hire date of every employee who was hired in 2007.

select Last_Name, Hire_Date
from emps
where Hire_Date like '2007%'
```

```sql=
-- Display the last name, salary, and commission for all
-- employees who earn commissions. Sort data in descending order of
-- salary and commissions.Title.

select Last_Name, Salary, Commission_pct
from emps
where Commission_pct is not null
order by Salary desc
```

```sql=
-- Display the last name of all employees who have an a and an e
-- in their last name.

select Last_Name
from emps
where Last_Name like '%a%' and Last_Name like '%e%'
```

## Lab 4

```sql=
-- Write a query to display the last name, department number, and
-- department name for all employees.

select Last_Name, depts.Department_id, Department_Name
from emps, depts
where emps.Department_Id = depts.Department_id
```

```sql=
-- Write a query to display the employee last name, department name,
-- location ID, and city of all employees who earn a commission.

select emps.Last_Name, depts.Department_Name, locs.Location_id, locs.City
from emps, depts, locs
where Commission_pct is not null and emps.Department_Id = depts.Department_id and 
depts.Location_id = locs.Location_id
```

```sql=
-- Write a query to display the last name, job, department number, and department name for all employees who work in Toronto.

select Last_Name, Job_Id, depts.Department_Id, depts.Department_Name
from emps
join depts on emps.Department_Id = depts.Department_id 
join locs on depts.Location_id = locs.Location_id and locs.City = 'Toronto'
--
select e.Last_Name, Job_Id, d.Department_Id, d.Department_Name
from emps e 
join depts d on e.Department_Id = d.Department_id 
join locs l on d.Location_id = l.Location_id and l.City = 'Toronto'
```

```sql=
-- Display the employee last name and employee number along with their manager’s last name and manager number. Label the columns Employee, Emp#, Manager, and Mgr#, respectively.

select e.Last_Name as 'Employee', e.Employee_Id as 'EMP#', m.Last_Name as 'Manager',
m.Employee_Id as 'Mgr#'
from emps e join emps m on e.Manager_id = m.Employee_Id
order by m.Employee_Id, e.Employee_Id
```

```sql=
-- Display the highest, lowest, sum, and average salary of all employees. Label the columns Maximum, Minimum, Sum, and Average, respectively. Round your results to the nearest whole number.

select round(MAX(salary)) 'Maximum', round(MIN(salary)) 'Minimum', round(SUM(salary)) 'Sum', round(AVG(salary)) 'Average'
from emps
```

```sql=
-- display the minimum, maximum, sum, and average salary for each job type

select Job_Id, MIN(salary), MAX(salary), SUM(salary), AVG(salary)
from emps
group by Job_Id
```

```sql=
-- Wite a query to display the number of people with the same job.

select emps.Job_Id, count(*)
from emps
group by emps.Job_Id
```

```sql=
-- Display the manager number and the salary of the lowest paid employee for that
-- manager. Exclude anyone whose manager is not known. Exclude any groups where the
-- minimum salary is $6,000 or less. Sort the output in descending order of salary.

select manager_id, MIN(salary)
from emps
where Manager_id is not null
group by Manager_id
having MIN(Salary) > 6000
order by MIN(Salary) DESC
```

```sql=
-- Write a query to display each department’s name, location, number of employees, and the average salary for all employees in that department. Label the columns Name, Location, Number of People, and Salary, respectively. Round the average salary to two decimal places.

select d.Department_Name 'Name', d.Location_id 'Location', COUNT(e.Employee_Id) 'Number of People', round(AVG(Salary), 2) 'Salary'
from emps e, depts d
where e.Department_Id = d.Department_id
group by d.Department_Name
--
select d.Department_Name 'Name', d.Location_id 'Location', COUNT(e.Employee_Id) 'Number of People', round(AVG(Salary), 2) 'Salary'
from emps e join depts d on
e.Department_Id = d.Department_id
group by d.Department_Name
----
----
select d.Department_Name 'Name', l.city 'Location', COUNT(e.Employee_Id) 'Number of People', round(AVG(Salary), 2) 'Salary'
from emps e, depts d, locs l
where e.Department_Id = d.Department_id and d.Location_id = l.Location_id
group by d.Department_Name
--
select d.Department_Name 'Name', l.city 'Location', COUNT(e.Employee_Id) 'Number of People', round(AVG(Salary), 2) 'Salary'
from emps e join depts d on
e.Department_Id = d.Department_id
join locs l on
d.Location_id = l.Location_id
group by d.Department_Name

```

## Lab 5
```sql=
-- Write a query to display the last name and hire date of any employee in the same department as Zlotkey. Exclude Zlotkey. 

select Last_Name, Hire_Date
from emps
where Last_Name <> 'Zlotkey' and 
Department_Id = (
	select Department_Id
    from emps
    where Last_Name = 'Zlotkey'
)
--
select e.Last_Name, e.Hire_Date
from emps e, emps z
where e.Last_Name <> z.Last_Name and  e.Department_Id = z.Department_Id
and z.Last_Name = 'Zlotkey'
```

```sql=
-- Create a query to display the employee ID, last names and salary of all employees who earn more than the average salary. Sort the results in ascending order of salary.

select Employee_Id, Last_Name, Salary
from emps
where Salary > (
	select AVG(Salary)
    from emps
)
order by Salary ASC
```

```sql=
-- Display the last name and salary of every employee who reports to King

select Last_Name, Salary
from emps
where Manager_id = (
	select Employee_Id
    from emps
    where Last_Name = 'king'
)
```

```sql=
-- Write a query to display the employee id, last names, and salaries of all employees who earn more than the average salary and who work in a department with any employee with a u in their name.

select Employee_Id, Last_Name, Salary
from emps
where Salary > (
	select AVG(Salary)
    from emps
) and Department_Id in (
	select Department_Id
    from emps
    where Last_Name LIKE '%u%'
)
```

## EXTRA
```sql=
-- add a column
ALTER TABLE sales_reps
ADD job_ID varchar(11)

-- modify a column
ALTER TABLE sales_reps
MODIFY job_ID varchar(22)

-- drop a column
ALTER TABLE sales_reps
drop COLUMN job_ID

-- change the name of a table
ALTER table sales_reps
RENAME R_sales_reps

--
TRUNCATE TABLE r_sales_reps
--

-- view a table
CREATE VIEW dekha AS
SELECT emps.Employee_Id, emps.First_Name, emps.Last_Name
from emps
where salary > 20000

-- create duplicate table
create table copy_employee 
SELECT * from employee

-- join tables 
CREATE table join_table as
select fname, lname, salary
from employee
UNION
select emps.First_Name, emps.Last_Name, emps.Salary
from emps

-- change a column name
ALTER TABLE table_join
CHANGE lname 2nd_Name varchar(11)
```
