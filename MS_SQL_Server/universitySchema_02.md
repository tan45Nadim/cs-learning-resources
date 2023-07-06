# University Schema Quries 

## Basic
```sql=
-- Find the department names of all instructors
select all dept_name
from instructor

select dept_name
from instructor
```

```sql=
-- Find the department names of all instructors, and remove duplicates

select distinct dept_name
from instructor
```

```sql=
-- Find the monthly salary of all instructors

select id, name, (salary / 12) as monthly_salary
from instructor
```

```sql=
-- To find all instructors in Comp. Sci. dept

select name
from instructor
where dept_name = 'Comp. Sci.'
```

```sql=
-- To find all instructors in Comp. Sci. dept with salary > 70000

select name
from instructor
where dept_name = 'Comp. Sci.' and salary > 70000
```

```sql=
-- Find the Cartesian product instructor X teaches

select * 
from instructor, teaches
```

```sql=
-- Find the names of all instructors who have taught
-- some course and the course_id

select instructor.name, teaches.course_id
from instructor, teaches
where instructor.id = teaches.id
```

```sql=
-- Find the names of all instructors in the Comp. Sci.
-- department who have taught some course and the
-- course_id

select instructor.name, teaches.course_id
from instructor, teaches
where instructor.id = teaches.id and instructor.dept_name = 'Comp. Sci.'
```

```sql=
-- Find the names of all instructors who have a higher salary than
-- some instructor in 'Comp. Sci'.

select distinct i.name
from instructor as i, instructor as cs
where i.salary > cs.salary and  cs.dept_name = 'Comp. Sci.'
```

```sql=
-- Find the names of all instructors with salary between $90,000 and
-- $100,000 (that is, ≥ $90,000 and ≤ $100,000)

select instructor.name
from instructor
where instructor.salary between 90000 and 100000
-- -- --
select instructor.name
from instructor
where instructor.salary >= 90000 and instructor.salary <= 100000
```

```sql=
--  List in alphabetic order the names of all instructors 

select name
from instructor
order by name asc
```

```sql=
--  List in alphabetic descending order the names of all instructors 

select name
from instructor
order by name desc
```

```sql=
-- Name all instructors whose name is neither “Mozart” nor Einstein”

select distinct instructor.name
from instructor
where instructor.name not in ('Mozart', 'Einstein')
```

```sql=
-- Name all instructors whose name is  “Mozart” and "Einstein”

select distinct instructor.name
from instructor
where instructor.name in ('Mozart', 'Einstein')
```

```sql=
-- Find the total number of (distinct) students who have taken course sections taught
-- by the instructor with ID 10101

select count(distinct s.id)
from takes as s, teaches as i
where i.course_id = s.course_id and i.semester = s.semester
and i.year = s.year and i.sec_id = s.sec_id
and i.id = 10101

```

```sql=
-- display two largest budget of departments.
select *
from department
where budget in (
(select max(budget)
from department)
union
(select max(budget)
from department
where budget not in (
	select max(budget)
	from department
)) )
order by budget des
-- -- -- --
select top 2 * 
from department
order by budget desc
```

```sql=
-- display two lowest budget of departments.
select top 2 * 
from department
order by budget asc
```

```sql=
-- display 2nd largest budget of departments.

select max(budget)
from department
where budget not in (
	select max(budget)
	from department
)
```

```sql=
-- find 4 th lowest budget

select * 
from department
where budget = (
	select max(budget)
	from department
	where budget in (
		select top 4 budget
		from department
		order by budget asc
))
```

```sql=
-- instructor number per department

select dept_name, (select count(*)
                   from instructor
                   where department.dept_name = instructor.dept_name) as num_instructor
from department
```

## Wildcard
```sql=
-- Find the names of all instructors whose name
-- includes the substring “an”

select instructor.name
from instructor
where name like '%an%'
```

```sql=
-- find out students name start with c, s and b.
select * 
from student
where name like '[csb]%'
```

```sql=
-- find out students name's 2nd letter contain h or a.
select * 
from student
where name like '_[ha]%'
```

```sql=
-- find out students name's 2nd and 3rd letter contain h and a.
select * 
from student
where name like '_ha%'
```

```sql=
-- 2nd last letter contains h or e or a
select * 
from student
where name like '%[hea]_'
```

```sql=
-- find out students name's last leter contains  h or i or a 
select * 
from student
where name like '%[hia]'
```

### Extra
```sql=
select instructor.name
from instructor
where name like '%_'

select len(dept_name) as length, dept_name, building
from department

select * from department
where dept_name like '_______'

select * from department
where dept_name like '_____'

select * from department
where dept_name like '_____%'

select * from department
where dept_name like '%_____'

--
select * from department
where dept_name like '[^bp]%'

select * from department
where dept_name like '[bp]%'

select * from department
where dept_name like '[^co]%'

select * from department
where dept_name like '[co]%'

select * from department
where dept_name like '%[^co]%'

select * from department
where dept_name like '%[g]%'

select * from department
where dept_name like '%g%'

select * from department
where dept_name like '[a-e]%'

select * from department
where dept_name like '%[a-e]'

select * from department
where dept_name like '[bc]%'
```

## Set Operation

```sql=
-- Find courses that ran in Fall 2017 or in Spring 2018

select section.course_id
from section
where section.semester = 'Fall' and section.year = '2017'
union
select section.course_id
from section
where section.semester = 'Spring' and section.year = '2018'
```

```sql=
-- Find courses that ran in Fall 2017 and in Spring 2018

select section.course_id
from section
where section.semester = 'Fall' and section.year = '2017'
intersect
select section.course_id
from section
where section.semester = 'Spring' and section.year = '2018'
```

```sql=
-- Find courses that ran in Fall 2017 but not in Spring 2018

select section.course_id
from section
where section.semester = 'Fall' and section.year = '2017'
except
select section.course_id
from section
where section.semester = 'Spring' and section.year = '2018'
```

```sql=
--Find the course name that has been offered both in fall 2017 and spring 2018

select distinct title, section.course_id
from section, course
where section.course_id = course.course_id  and section.course_id in (
	select section.course_id
	from section
	where semester = 'fall' and year = '2017'
	intersect
	select section.course_id
	from section
	where semester = 'spring' and year = '2018'
)
```


## Functions 

```sql=
-- Find the average salary of instructors in the Computer Science department

select avg(instructor.salary)
from instructor
where instructor.dept_name = 'Comp. Sci.'
```

```sql=
-- Find the total number of instructors who teach a course in the Spring 2018 semester

select count(distinct teaches.id)
from teaches
where teaches.semester = 'Spring' and teaches.year = 2018
```

```sql=
-- Find the number of tuples in the course relation

select count(*)
from course
```

```sql=
-- Find the average salary of instructors in each department

select instructor.dept_name, avg(instructor.salary) as AVG_SALARY
from instructor
group by instructor.dept_name
```

```sql=
-- Find the average salary of instructors in each department

select instructor.dept_name, avg(instructor.salary) as AVG_SALARY
from instructor
group by instructor.dept_name
```

```sql=
-- Find the names and average salaries of all departments whose 
-- average salary is greater than 42000

select dept_name, avg(salary) as AVG_Salary
from instructor
group by dept_name
having avg(salary) > 42000
```

## Nested Queries

```sql=
-- Find courses offered in Fall 2017 and in Spring 2018.

select section.course_id
from section
where section.semester = 'Fall' and section.year = 2017
and section.course_id in (
    select section.course_id
    from section
    where section.semester = 'Spring' and section.year = 2018
)
```

```sql=
-- Find courses offered in Fall 2017 but not in Spring 2018

select section.course_id
from section
where section.semester = 'Fall' and section.year = 2017
and section.course_id not in (
    select section.course_id
    from section
    where section.semester = 'Spring' and section.year = 2018
)
```

```sql=
-- SHOW all tuples in the instructor relation for those instructors associated 
-- with a department located in the Watson building

select * 
from instructor
where dept_name in (
    select dept_name
    from department
    where building = 'watson'
)
```

```sql=
-- show all instructors whose salary is less than the average salary of 
-- instructors

select * 
from instructor
where salary < (
    select avg(salary)
    from instructor
)
```

```sql=
--Find the course name that has been offered both in fall 2017 and spring 2018

select section.course_id, title
from section, course
where semester = 'fall' and year = '2017' and section.course_id = course.course_id  and
section.course_id in (
    select course_id
    from section
    where semester = 'spring' and year = '2018'
)
```

```sql=
-- Find the instructors with salary greater than 
-- the minimum salary of Comp.Sci. department.

select * 
from instructor
where salary > (
	select min(salary)
	from instructor
	where dept_name = 'Comp. Sci.'
)
```

## Some/Any

```sql=
-- Find names of instructors with salary greater than that of some (at least one)
-- instructor in the Comp. Sci. department.

select distinct i.name
from instructor as i, instructor as s
where i.salary > s.salary and s.dept_name = 'Comp. Sci.'
-- -- -- 
select name
from instructor
where salary > some (
    select salary
    from instructor
    where dept_name = 'Comp. Sci.'
)
-- -- -- 
select name
from instructor
where salary > any (
    select salary
    from instructor
    where dept_name = 'Comp. Sci.'
)
```

```sql=
-- Find the instructors with salary greater than 
-- the ANY instructor in the finance department.

select *
from instructor
where salary > some(
	select salary
	from instructor
	where dept_name = 'finance'
)
-- -- --
select *
from instructor
where salary > any(
	select salary
	from instructor
	where dept_name = 'finance'
)
```

```sql=
-- Find the instructors with salary greater than 
-- the maximum salary of any department.

select * 
from instructor
where salary >  any(
	select max(salary)
	from instructor
	group by dept_name
)
```



## All
```sql=
-- Find the names of all instructors whose salary is greater than the salary of all
-- instructors in the Comp. Sci. department.

select name
from instructor
where salary > all(
    select salary
    from instructor
    where dept_name = 'Comp. Sci.'
)
```

```sql=
-- Find the instructors with salary greater than 
-- the highest paid instructor in the finance department.


select *
from instructor
where salary > (
	select max(salary)
	from instructor
	where dept_name = 'finance'
)
-- -- --
select *
from instructor
where salary > all(
	select salary
	from instructor
	where dept_name = 'finance'
)

```


## Exists

```sql=
-- Find all courses taught in both the Fall
-- 2017 semester and in the Spring 2018 semester

select f.course_id
from section as f
where f.semester = 'Fall' and f.year = '2017'
and exists (
    select s.course_id
    from section as s
    where s.semester = 'Spring' and s.year = '2018'
    and f.course_id = s.course_id   
)
```

```sql=
-- Find courses offered in Fall 2017 but not in Spring 2018

select f.course_id
from section as f
where f.semester = 'Fall' and f.year = '2017'
and not exists (
    select s.course_id
    from section as s
    where s.semester = 'Spring' and s.year = '2018'
    and f.course_id = s.course_id
)
```
