PDF of [University Schema](https://github.com/tan45Nadim/cs-learning-resources/blob/main/MS_SQL_Server/universitySchema.pdf)

```sql=
create database universityDB
GO
use universityDB
GO

create table department (
    dept_name char(34) primary key,
    building nvarchar(18),
    budget int check (budget > 10000)
)
GO

create table course (
    course_id nvarchar(15) unique,
    title nchar(35),
    dept_name char(34),
    credits decimal(2, 1),
    primary key(course_id),
    constraint course_fk foreign key(dept_name) references department(dept_name)
)
GO

create table instructor (
    id int primary key,
    name nchar(32) not null,
    dept_name char(34),
    salary int,
	constraint ck_instructor check ((salary >= 5000) and (1000000 >= salary)),
    constraint instructor_fk foreign key(dept_name) references department(dept_name)
)
GO

create table student (
    id int primary key,
    name nvarchar(32) not null,
    dept_name char(34),
    tot_cred decimal(5, 2) null,
    foreign key(dept_name) references department(dept_name)
)
GO

create table advisor (
    s_id int,
    i_id int,
    primary key(s_id),
    constraint advisor_fk1 foreign key(s_id) references student(id),
    constraint advisor_fk2 foreign key(i_id) references instructor(id)
)
GO

create table prereq (
    course_id nvarchar(15),
    prereq_id nvarchar(15),
    primary key(course_id, prereq_id),
    constraint prereq_fk1 foreign key(course_id) references course(course_id),
    foreign key(prereq_id) references course(course_id)
)
GO

create table time_slot (
    time_slot_id char(6),
    day varchar(5),
    start_time time,
    end_time time not null,
    constraint time_slot_pk primary key(time_slot_id, day, start_time)
)
GO

create table classroom (
    building char(10),
    room_number char(10),
    capacity int not null,
	constraint ck_capacity check (capacity >= 15 and capacity <= 600), 
    constraint classroom_pk primary key(building, room_number)
)
GO

create table section (
    course_id nvarchar(15),
    sec_id tinyint,
    semester varchar(17),
    year smallint,
    building char(10),
    room_number char(10),
    time_slot_id char(6),
    constraint section_pk primary key(course_id, sec_id, semester, year),
    foreign key(course_id) references course(course_id),
    constraint section_fk foreign key(building, room_number) references classroom(building, room_number)
)
GO

create table takes (
    id int,
    course_id nvarchar(15),
    sec_id tinyint,
    semester varchar(17),
    year smallint not null,
    grade varchar(6),
    primary key(id, course_id, sec_id, semester, year),
    constraint takes_fk1 foreign key(id) references student(id),
    constraint takes_fk2 foreign key(course_id, sec_id, semester, year)
    references section(course_id, sec_id, semester, year)
)
GO

create table teaches (
    id int,
    course_id nvarchar(15),
    sec_id tinyint,
    semester varchar(17),
    year smallint not null,
    constraint teaches_pk primary key(id, course_id, sec_id, semester, year),
    constraint teaches_fk1 foreign key(id) references instructor(id),
    foreign key(course_id, sec_id, semester, year)
    references section(course_id, sec_id, semester, year)
)
GO

----------------------------------
--------- DATA INSERTION ---------
----------------------------------

insert into department values
	('Biology', 'Watson', '90000'),
	('Comp. Sci.', 'Taylor', '100000'),
	('Elec. Eng.', 'Taylor', '85000'),
	('Finance', 'Painter', '120000'),
	('History', 'Painter', '50000'),
	('Music', 'Packard', '80000'),
	('Physics', 'Watson', '70000')
GO

insert into [dbo].[course] values
	('BIO-101', 'Intro. to Biology', 'Biology', '4'),
	('BIO-301', 'Genetics', 'Biology', '4'),
	('BIO-399', 'Computational Biology', 'Biology', '3'),
	('CS-101', 'Intro. to Computer Science', 'Comp. Sci.', '4'),
	('CS-190', 'Game Design', 'Comp. Sci.', '4'),
	('CS-315', 'Robotics', 'Comp. Sci.', '3'),
	('CS-319', 'Image Processing', 'Comp. Sci.', '3'),
	('CS-347', 'Database System Concepts', 'Comp. Sci.', '3'),
	('EE-181', 'Intro. to Digital Systems', 'Elec. Eng.', '3'),
	('FIN-201', 'Investment Banking', 'Finance', '3'),
	('HIS-351', 'World History', 'History', '3'),
	('MU-199', 'Music Video Production', 'Music', '3'),
	('PHY-101', 'Physical Principles', 'Physics', '4')
GO

insert into [dbo].[instructor] values
	('22222', 'Einstein', 'Physics', '95000'),
	('12121', 'Wu', 'Finance', '90000'),
	('32343', 'El Said', 'History', '60000'),
	('45565', 'Katz', 'Comp. Sci.', '75000'),
	('98345', 'Kim', 'Elec. Eng.', '80000'),
	('76766', 'Crick', 'Biology', '72000'),
	('10101', 'Srinivasan', 'Comp. Sci.', '65000'),
	('58583', 'Califieri', 'History', '62000'),
	('83821', 'Brandt', 'Comp. Sci.', '92000'),
	('15151', 'Mozart', 'Music', '40000'),
	('33456', 'Gold', 'Physics', '87000'),
	('76543', 'Singh', 'Finance', '80000')

GO

insert into [dbo].[student] values
	('00128', 'Zhang', 'Comp. Sci.', '102'),
	('12345', 'Shankar', 'Comp. Sci.', '32'),
	('19991', 'Brandt', 'History', '80'),
	('23121', 'Chavez', 'Finance', '110'),
	('44553', 'Peltier', 'Physics', '56'),
	('45678', 'Levy', 'Physics', '46'),
	('54321', 'Williams', 'Comp. Sci.', '54'),
	('55739', 'Sanchez', 'Music', '38'),
	('70557', 'Snow', 'Physics', '0'),
	('76543', 'Brown', 'Comp. Sci.', '58'),
	('76653', 'Aoi', 'Elec. Eng.', '60'),
	('98765', 'Bourikas', 'Elec. Eng.', '98'),
	('98988', 'Tanaka', 'Biology', '120')
GO

insert into advisor values
	('00128', '45565'),
	('12345', '10101'),
	('23121', '76543'),
	('44553', '22222'),
	('45678', '22222'),
	('76543', '45565'),
	('76653', '98345'),
	('98765', '98345'),
	('98988', '76766')
GO

insert into [dbo].[prereq] values
	('BIO-301', 'BIO-101'),
	('BIO-399', 'BIO-101'),
	('CS-190', 'CS-101'),
	('CS-315', 'CS-101'),
	('CS-319', 'CS-101'),
	('CS-347', 'CS-101'),
	('EE-181', 'PHY-101')
Go

insert into [dbo].[time_slot] values
	('A', 'M', '8:00', '8:50'),
	('A', 'W', '8:00', '8:50'),
	('A', 'F', '8:00', '8:50'),
	('B', 'M', '9:00', '9:50'),
	('B', 'W', '9:00', '9:50'),
	('B', 'F', '9:00', '9:50'),
	('C', 'M', '11:00', '11:50'),
	('C', 'W', '11:00', '11:50'),
	('C', 'F', '11:00', '11:50'),
	('D', 'M', '13:00', '13:50'),
	('D', 'W', '13:00', '13:50'),
	('D', 'F', '13:00', '13:50'),
	('E', 'T', '10:30', '11:45'),
	('E', 'R', '10:30', '11:45'),
	('F', 'T', '14:30', '15:45'),
	('F', 'R', '14:30', '15:45'),
	('G', 'M', '16:00', '16:50'),
	('G', 'W', '16:00', '16:50'),
	('G', 'F', '16:00', '16:50'),
	('H', 'W', '10:00', '12:30')
GO

insert into classroom values
	('Packard', '101', '500'),
	('Painter', 514, '15'),
	('Taylor', '3128', '70'),
	('Watson', 100, 30),
	('Watson', '120', 50)
GO

insert into section values
	('BIO-101', '1', 'Summer', '2017', 'Painter', '514', 'B'),
	('BIO-301', '1', 'Summer', '2018', 'Painter', '514', 'A'),
	('CS-101', '1', 'Fall', '2017', 'Packard', '101', 'H'),
	('CS-101', '1', 'Spring', '2018', 'Packard', '101', 'F'),
	('CS-190', '1', 'Spring', '2017', 'Taylor', '3128', 'E'),
	('CS-190', '2', 'Spring', '2017', 'Taylor', '3128', 'A'),
	('CS-315', '1', 'Spring', '2018', 'Watson', '120', 'D'),
	('CS-319', '1', 'Spring', '2018', 'Watson', '100', 'B'),
	('CS-319', '2', 'Spring', '2018', 'Taylor', '3128', 'C'),
	('CS-347', '1', 'Fall', '2017', 'Taylor', '3128', 'A'),
	('EE-181', '1', 'Spring', '2017', 'Taylor', '3128', 'C'),
	('FIN-201', '1', 'Spring', '2018', 'Packard', '101', 'B'),
	('HIS-351', '1', 'Spring', '2018', 'Painter', '514', 'C'),
	('MU-199', '1', 'Spring', '2018', 'Packard', '101', 'D'),
	('PHY-101', '1', 'Fall', '2017', 'Watson', '100', 'A')
Go

insert into [dbo].[takes] values
	('00128', 'CS-101', '1', 'Fall', '2017', 'A'),
	('00128', 'CS-347', '1', 'Fall', '2017', 'A-'),
	('12345', 'CS-101', '1', 'Fall', '2017', 'C'),
	('12345', 'CS-190', '2', 'Spring', '2017', 'A'),
	('12345', 'CS-315', '1', 'Spring', '2018', 'A'),
	('12345', 'CS-347', '1', 'Fall', '2017', 'A'),
	('19991', 'HIS-351', '1', 'Spring', '2018', 'B'),
	('23121', 'FIN-201', '1', 'Spring', '2018', 'C+'),
	('44553', 'PHY-101', '1', 'Fall', '2017', 'B-'),
	('45678', 'CS-101', '1', 'Fall', '2017', 'F'),
	('45678', 'CS-101', '1', 'Spring', '2018', 'B+'),
	('45678', 'CS-319', '1', 'Spring', '2018', 'B'),
	('54321', 'CS-101', '1', 'Fall', '2017', 'A-'),
	('54321', 'CS-190', '2', 'Spring', '2017', 'B+'),
	('55739', 'MU-199', '1', 'Spring', '2018', 'A-'),
	('76543', 'CS-101', '1', 'Fall', '2017', 'A'),
	('76543', 'CS-319', '2', 'Spring', '2018', 'A'),
	('76653', 'EE-181', '1', 'Spring', '2017', 'C'),
	('98765', 'CS-101', '1', 'Fall', '2017', 'C-'),
	('98765', 'CS-315', '1', 'Spring', '2018', 'B'),
	('98988', 'BIO-101', '1', 'Summer', '2017', 'A'),
	('98988', 'BIO-301', '1', 'Summer', '2018', null)
GO

insert into [dbo].[teaches] values
	('10101', 'CS-101', '1', 'Fall', '2017'),
	('10101', 'CS-315', '1', 'Spring', '2018'),
	('10101', 'CS-347', '1', 'Fall', '2017'),
	('12121', 'FIN-201', '1', 'Spring', '2018'),
	('15151', 'MU-199', '1', 'Spring', '2018'),
	('22222', 'PHY-101', '1', 'Fall', '2017'),
	('32343', 'HIS-351', '1', 'Spring', '2018'),
	('45565', 'CS-101', '1', 'Spring', '2018'),
	('45565', 'CS-319', '1', 'Spring', '2018'),
	('76766', 'BIO-101', '1', 'Summer', '2017'),
	('76766', 'BIO-301', '1', 'Summer', '2018'),
	('83821', 'CS-190', '1', 'Spring', '2017'),
	('83821', 'CS-190', '2', 'Spring', '2017'),
	('83821', 'CS-319', '2', 'Spring', '2018'),
	('98345', 'EE-181', '1', 'Spring', '2017')
GO

----------------------------------
--------- Result Table -----------
----------------------------------

select * from department
GO
select * from course
GO
select * from instructor
GO
select * from student
GO
select * from advisor
GO
select * from prereq
Go
select * from time_slot
Go
select * from classroom
GO
select * from section
GO
select * from takes
GO
select * from teaches
GO
```
