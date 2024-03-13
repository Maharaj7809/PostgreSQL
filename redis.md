select version();

const Pool = require("pg").Pool;

const pool = new Pool({
  user: "postgres",
  password: "Maharaj@123",
  host: "localhost",
  port: 5432,
  database: "perntodo"
});

 module.exports = pool;

const pool = require("./db");


 const newTodo = await pool.query( "INSERT INTO todo (description) VALUES($1) RETURNING *", [description] );
 const allTodos = await pool.query("SELECT * FROM todo");
 const todo = await pool.query("SELECT * FROM todo WHERE todo_id = $1", [ id]);
 const updateTodo = await pool.query("UPDATE todo SET description = $1 WHERE todo_id = $2", [description, id]);
 const deleteTodo = await pool.query("DELETE FROM todo WHERE todo_id = $1", [ id ]);




insert into movies(movie_id , movie_name , movie_genre, imdb_ratings)
values (101,'vertigo','Mystry', 'romance', 8.5),(102,'vertigo','Mystry', 'romance', 8.5), (103,'vertigo','Mystry', 'romance', 8.5), (104,'vertigo','Mystry', 'romance', 8.5);


create table employees(

 emp_id int not null primary key,
 emp_name varchar(20) ,
 email_id varchar(20) ,
 Gender varchar(10) ,
 Department varchar(10) ,
 Adress varchar(20) ,
 Salary real 

);

drop table employees;


select * from movies;

select * from employees;



--where clause


delete from movies where movie_id = 103;

select * from movies where imdb_ratings > 8.5;

select * from movies where imdb_ratings between 8.5 and 9.5;

select * from movies where movie_genre = 'romance';

select movie_genre , movie_name from movies where imdb_ratings < 8.5;

select * from movies where imdb_ratings in (8.7, 9.0);

select * from employees where Department is null;

select * from employees where country = 'India' and salary > 100000;

select * from employees where country = 'India' or department ='sales';






--distinct clause



select distinct Adress from employees;

select distinct department from employees;




--order by clause


select * from employees order by salary desc;

select * from employees order by salary desc limit 5;

select * from employees order by salary desc limit 5 offset 3;

select * from employees order by salary desc fetch first 3 row only;

select * from employees order by salary desc offset 3 rows fetch first 3 row only;




--like clause



select emp_name,email from employees where emp_name like 'A%';

select emp_name,email from employees where emp_name like 'S%';

select emp_name,email from employees where emp_name like '%d';

select emp_name,email from employees where emp_name like '%ish%';

select emp_name,email from employees where emp_name like '_u%';



--aggregate clause



select sum(salary) as total_salary from employees;

select avg(salary) as avg_salary from employees;

select min(salary) as min_salary from employees;

select count(distinct department) as total_dept from employees;





-- alter, update and delete clause


alter table employees rename column Adress to country;

update movies set imdb_ratings = 9.5 where movie_id = 101;

update employees set department = 'IT' where  department is null;

delete from movies where movie_id = 103;

truncate table movies; --much faster then delete  it doesn't need to scan the table for rows to delete.



--group by clause


select country, avg(salary) as avg_salary from employees group by country order by average_salary desc;

select gender, max(salary) as max_salary from employees group by gender;

select country , count(emp_id)  from employees group by country;




--having clause


select country , avg(salary ) as average_salary from employees group by country having average_salary > 100000;

select country, count(emp_id) as total_emp from employees group by country having count(emp_id)<30 order by count(emp_id) ;




--case expression


select department , country , salary,
case
when salary < 10000 and salary > 5000 then 'low'
when salary > 10000 and salary < 20000 then'medium'
when salary > 20000 then 'high'
end as salary_range
from employees order by salary desc ;


select emp_name, department , country , salary from employees where salary >(select avg(salary) from employees);




--sql functions


select abs(-10);

select mod(10,3);

select least(2,54,67,4,90,56.5);

select power(5,3);

select sqrt(100);

select sin(90);

select floor(6.45);




--string functions



select char_length('hello');

select concat('hello','world');

select reverse('hello');



--inner join(A ∩ B)

select testproduct_id, product_name, category_name  from test
inner join categories on test.category_id = categories.category_id;


--outer join 

--left join (A)

select testproduct_id, product_name, category_name  from test
left join categories on test.category_id = categories.category_id;


--right join (B)

select testproduct_id, product_name, category_name  from test
right join categories on test.category_id = categories.category_id;

--Full join ( A + B - (A ∩ B)) 

 select testproduct_id, product_name, category_name  from test
 full join categories on test.category_id = categories.category_id;

--Cross join ( A * B)

 select testproduct_id, product_name, category_name  from test
 cross join categories on test.category_id = categories.category_id;





--user defined function 


 create or replace function count_emails() 
   returns integer as $total_emails$

  declare total_emails integer;

  begin
    select count(email) into total_emails from employees;
    return total_emails;
  end;

  $total_emails$ language plpgsql;

  select count_emails();
