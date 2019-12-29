# sql

Some SQL practice (SQLite and Dbeaver used in my case)

Single Table Queries (select, distinct, where, or, and, between, in, null ) : 

Show the departments and their locations from dept table : 

select dname, loc from dept;

Show All job titles from employee table, but each job title should appear only once

select distinct job from emp;

Show only employees that have commision greater than salary 

select * from emp where comm > sal;

Show only people who are salesman and their salary is over 1600

select *  from emp where job = 'SALESMAN' and sal >= 1600;

Show people who are not salesman and their salary is less than 2500 

select ename from emp where job != 'SALESMAN' and sal < 2500;

Show All Employees that are not managers and have salary greater than 2500 and also work in department no 20

select *  from emp where job != 'MANAGER' and sal > 2500 and deptno = 20;

Show names of those employees that are not managers nor salesman and have salary greater than or equal to 2000 

select ename from emp where job != 'MANAGER' and job != 'SALESMAN' and sal >= 2000;

Show names and hiring dates of those employees that work in Dallas or Chicago 

select ename, hiredate from emp where deptno = 20  or deptno = 30;

select ename, hiredate from emp where deptno in (20, 30);

Show those employees whose salary is between 1000 and 2000 : 

select * from emp where sal between 1000 and 2000;

Write a query that returns those employees that don't make any comission and have a salary greater than 1100 but less than 5000. Exclude those employees that have a salary equal to 3000. 

select * from emp where (comm is null or comm = 0) and (sal > 1100 and sal < 5000 and sal != 3000);

Return those employees that are salesman and that make either 300 dollars in commision or greater than 1000 dollars in commision. 

select * from emp where job = 'SALESMAN' and (comm = 300 or comm > 1000);


Return employee record with highest salary : 

select * from emp where sal = (select max(sal) from emp);

Return 2nd highest salary from emp table : 

select * from emp where sal = (select max(sal) from emp where sal NOT IN (select max(sal) from emp));


Grouping functions (min, max, avg, count), other stuff (group by, having) : 

1. Return max salaty from emp table : 

select max(sal) from emp;

2. Return min salary from emp table :   

select min(sal) from emp;

3. Return sum of all salaries from emp table :

select sum(sal) from emp;

4. Return maximum salary for manager : 

select max(sal) from emp where job = 'MANAGER';

5. Return average salary from emp table : 

select avg(sal) as "SAL Average" from emp;

5. Return how many employees are in emp table :  

select count(*) numOfEmps from emp;

6. select maximum salary for each job title : 

select max(sal), job from emp group by job

7. Select minimum salary for each job title :

select min(sal), job from emp group by job
8. Select average salary for each job title :  

select avg(sal), job from emp group by job

9. Select how many employees are hired in each position : 

select count(*), job from emp group by job

9. Select how many employees are hired as managers and president : 

select job from emp group by job having job in ('MANAGER', 'PRESIDENT')

9. Show only job titles that have only 2 employees : 

select count(*), job from emp group by job having count(*) = 2

10. Show only job titles that have only 2 employees and their salary is > 2000

select count(*), job from emp where sal > 2000 group by job having count(*) = 2

11. show department numbers that have at least 4 employees : 

select deptno from emp group by deptno having count(*) > 3

Multi Table Queries and Joins :

1. Show those employees that work in Chicago.

select * from emp where deptno = (select deptno from dept where loc = 'CHICAGO');

2. Show only Those employees that work in Dallas. Show their name, their job title and their Salary.

select emp.ename, emp.job, emp.sal from emp, dept where emp.deptno = dept.deptno and dept.loc = 'DALLAS';

3. Show employee names with their department names 

select emp.ename, dept.dname from dept INNER JOIN emp on emp.deptno = dept.deptno;



 4. Show employee names with their department names. It should show also departments that don't have employees in them.

select emp.ename, dept.dname from dept left join emp on emp.deptno = dept.deptno;

 5. Show employees with their department names. It should show also employees that don't have corresponding departments

select emp.ename, dept.dname from emp left join dept on emp.deptno = dept.deptno;

 6. Show how many employees are working in each department :

select dept.dname, count(*) from emp inner join dept on emp.deptno = dept.deptno group by dept.dname;

7. Write a SQL query to find employees who have the highest salary in each of the departments : 

select
    emp.deptno as 'Department',
    emp.empno as 'Employee',
    emp.sal
from
    emp
        join
    dept on emp.deptno = dept.deptno
where
    (emp.deptno , emp.sal) in
    (   select
            deptno, max(sal)
        from
            emp
        group by deptno
	);
