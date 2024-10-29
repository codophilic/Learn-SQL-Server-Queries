# Queries

## Basics

### Creating Table and Inserting Values

- SQL Queries

```
CREATE TABLE EMPLOYEE (
  empId int primary key,
  name varchar(15) not null,
  dept varchar(10),
  Salary numeric(18,4) not null
);


INSERT INTO EMPLOYEE(empId,name,dept,Salary) VALUES (1, 'Clark', 'Sales',1000),(2, 'Dave', 'Accounting',2000)
, (3, 'Ava', 'Sales',3000),(4, 'ABC', 'Sales',4000),(5, 'BCD', 'Sales',5000),(6, 'XYZ', null,4000.988888);


SELECT * FROM EMPLOYEE; Go
```

- Output:

```
Output:

empId       name            dept       Salary              
----------- --------------- ---------- --------------------
          1 Clark           Sales                 1000.0000
          2 Dave            Accounting            2000.0000
          3 Ava             Sales                 3000.0000
          4 ABC             Sales                 4000.0000
          5 BCD             Sales                 5000.0000
          6 XYZ             NULL                  4000.9889
```

### Adding Foreign Key

- SQL Queries

```

create table gender(
  id int primary key,
  name varchar(7) not null
);

alter table EMPLOYEE add constraint gender_as_FK foreign key (gender) references gender(id); -- Foreign Key

insert into gender(id,name) VALUES (1,"Male"),(2,"Female"),(3,"Others");

INSERT INTO EMPLOYEE(empId,name,dept,Salary,gender) VALUES (1, 'Clark', 'Sales',1000,1),(2, 'Dave', 'Accounting',2000,1)
, (3, 'Ava', 'Sales',3000,2),(4, 'ABC', 'Sales',4000,3),(5, 'BCD', 'Sales',5000,3),(6, 'XYZ', null,4000.988888,3);

select * from EMPLOYEE;

insert into EMPLOYEE values (7,"ABC",null,100,100); -- Cannot insert 100 because it does not exists in gender table because it has a foreign key relationship
```

- Output

```
Output:

empId       name            dept       Salary               gender     
----------- --------------- ---------- -------------------- -----------
          1 Clark           Sales                 1000.0000           1
          2 Dave            Accounting            2000.0000           1
          3 Ava             Sales                 3000.0000           2
          4 ABC             Sales                 4000.0000           3
          5 BCD             Sales                 5000.0000           3
          6 XYZ             NULL                  4000.9889           3
Msg 547, Level 16, State 1, Server 5a0ee01082d2, Line 33
The INSERT statement conflicted with the FOREIGN KEY constraint "gender_as_FK". The conflict occurred in database "db_42whkr33r_42wj5jgzn", table "dbo.gender", column 'id'.
The statement has been terminated.
```

### Adding default constraint or removing it

- SQL Queries

```
alter table gender add constraint default_value_for_gender default "Not Assigned" for name;
insert into gender(id) values (4);
select * from gender;
```

- Output

```
id          name           
----------- ---------------
          1 Male           
          2 Female         
          3 Others         
          4 Not Assigned  
```

- To remove

```
alter table gender drop constraint default_value_for_gender
```

### Adding or removing a column

- SQL Queries for Adding

```
Alter table to add a new column
alter table employee add phone varchar(20) not null default "Not Provided";
select * from employee;
```

- Output

```
empId       name            dept       Salary               gender      phone               
----------- --------------- ---------- -------------------- ----------- --------------------
          1 Clark           Sales                 1000.0000           1 Not Provided        
          2 Dave            Accounting            2000.0000           1 Not Provided        
          3 Ava             Sales                 3000.0000           2 Not Provided        
          4 ABC             Sales                 4000.0000           3 Not Provided        
          5 BCD             Sales                 5000.0000           3 Not Provided        
          6 XYZ             NULL                  4000.9889           3 Not Provided        
```

- SQL Queries for removing

>[!NOTE]
> - Before removing any column, if the column is associated with any constraint we need to delete that constraint first then delete the column or else we wil receive an error `ALTER TABLE DROP COLUMN phone failed because one or more objects access this column.`

```
alter table employee drop column phone ;
select * from employee;
```

- Output error

```
Msg 5074, Level 16, State 1, Server ba53ca26b538, Line 41
The object 'DF__EMPLOYEE__phone__3B75D760' is dependent on column 'phone'.
Msg 4922, Level 16, State 9, Server ba53ca26b538, Line 41
ALTER TABLE DROP COLUMN phone failed because one or more objects access this column.
```

- Updated query

```
alter table employee drop constraint DF__EMPLOYEE__phone__3B75D760;
alter table employee drop column phone ;
select * from employee
```

- Output

```
empId       name            dept       Salary               gender     
----------- --------------- ---------- -------------------- -----------
          1 Clark           Sales                 1000.0000           1
          2 Dave            Accounting            2000.0000           1
          3 Ava             Sales                 3000.0000           2
          4 ABC             Sales                 4000.0000           3
          5 BCD             Sales                 5000.0000           3
          6 XYZ             NULL                  4000.9889           3
```

- Whole Query

```


-- create
CREATE TABLE EMPLOYEE (
  empId int primary key,
  name varchar(15) not null,
  dept varchar(10),
  Salary numeric(18,4) not null,
  gender int
);


create table gender(
  id int primary key,
  name varchar(15)
);

alter table EMPLOYEE add constraint gender_as_FK foreign key (gender) references gender(id);

insert into gender(id,name) VALUES (1,"Male"),(2,"Female"),(3,"Others");




-- insert
INSERT INTO EMPLOYEE(empId,name,dept,Salary,gender) VALUES (1, 'Clark', 'Sales',1000,1),(2, 'Dave', 'Accounting',2000,1)
, (3, 'Ava', 'Sales',3000,2),(4, 'ABC', 'Sales',4000,3),(5, 'BCD', 'Sales',5000,3),(6, 'XYZ', null,4000.988888,3);


select * from EMPLOYEE;

alter table gender add constraint default_value_for_gender default "Not Assigned" for name;

insert into gender(id) values (4);
select * from gender;

alter table employee add phone varchar(20) not null default "Not Provided";
select * from employee;

alter table employee drop constraint DF__EMPLOYEE__phone__3B75D760;
alter table employee drop column phone ;
select * from employee
```

  
