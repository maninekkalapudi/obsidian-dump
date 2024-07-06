### Login

  

```SQL
mysql -u {username} -p {password}
```

### List Databases

List all the databases available in the DBMS.

```SQL
show databases; -- displays a list of databases;

--Eg:
	mysql> show databases;
	+--------------------+
	| Database           |
	+--------------------+
	| information_schema |
	| classicmodels      |
	| employees          |
	| mysql              |
	| performance_schema |
	| sys                |
	+--------------------+
```

  

### Select a Database

Selects the provided database and all the further queries run will be on this table. This can be used to change database to other

```SQL
use {database_name};

-- Eg:
	mysql> use employees;
	Database changed
```

  

### Show tables in a Database

```SQL
show tables;

--Eg:
	mysql> show tables;
	+----------------------+
	| Tables_in_employees  |
	+----------------------+
	| current_dept_emp     |
	| departments          |
	| dept_emp             |
	| dept_emp_latest_date |
	| dept_manager         |
	| employees            |
	| salaries             |
	| titles               |
	+----------------------+
```

  

### DESCRIBE

Displays the structure of the table. Here structure means column name, datatype, and constraints for each column.

```SQL
describe {table_name}

--Eg:
	mysql> describe employees;
	+------------+---------------+------+-----+---------+-------+
	| Field      | Type          | Null | Key | Default | Extra |
	+------------+---------------+------+-----+---------+-------+
	| emp_no     | int(11)       | NO   | PRI | NULL    |       |
	| birth_date | date          | NO   |     | NULL    |       |
	| first_name | varchar(14)   | NO   |     | NULL    |       |
	| last_name  | varchar(16)   | NO   |     | NULL    |       |
	| gender     | enum('M','F') | NO   |     | NULL    |       |
	| hire_date  | date          | NO   |     | NULL    |       |
	+------------+---------------+------+-----+---------+-------+
```

### SELECT

Selects the data from the table or table-like structure and displays it to the user.

```SQL
select * from {database_name}; -- Displays all the columns in the table. '*'- Wildcard
select col1,col2,.. from {database_name}; -- Displays only the list of columns
																					-- mentioned in the statement
-- Ex:
	mysql> select * from employees;
	+--------+------------+-------------+--------------+--------+------------+
	| emp_no | birth_date | first_name  | last_name    | gender | hire_date  |
	+--------+------------+-------------+--------------+--------+------------+
	|  10001 | 1953-09-02 | Georgi      | Facello      | M      | 1986-06-26 |
	|  10002 | 1964-06-02 | Bezalel     | Simmel       | F      | 1985-11-21 |
	|  10003 | 1959-12-03 | Parto       | Bamford      | M      | 1986-08-28 |
	|  10004 | 1954-05-01 | Chirstian   | Koblick      | M      | 1986-12-01 |
	|  10005 | 1955-01-21 | Kyoichi     | Maliniak     | M      | 1989-09-12 |
	|  10006 | 1953-04-20 | Anneke      | Preusig      | F      | 1989-06-02 |
	|  10007 | 1957-05-23 | Tzvetan     | Zielinski    | F      | 1989-02-10 |
	|	 ..... | .......... | .........   | ..........   | ..     | .......... |
	|	 ..... | .......... | .........   | ..........   | ..     | .......... |
	|	 ..... | .......... | .........   | ..........   | ..     | .......... |
	|	 ..... | .......... | .........   | ..........   | ..     | .......... |
	|	 ..... | .......... | .........   | ..........   | ..     | .......... |
	|	 ..... | .......... | .........   | ..........   | ..     | .......... |
	| 499994 | 1952-02-26 | Navin       | Argence      | F      | 1990-04-24 |
	| 499995 | 1958-09-24 | Dekang      | Lichtner     | F      | 1993-01-12 |
	| 499996 | 1953-03-07 | Zito        | Baaz         | M      | 1990-09-27 |
	| 499997 | 1961-08-03 | Berhard     | Lenart       | M      | 1986-04-21 |
	| 499998 | 1956-09-05 | Patricia    | Breugel      | M      | 1993-10-13 |
	| 499999 | 1958-05-01 | Sachin      | Tsukuda      | M      | 1997-11-30 |
	+--------+------------+-------------+--------------+--------+------------+
	300024 rows in set (0.37 sec)
	

	mysql> select first_name, last_name from employees;
	+------------+-----------+
	| first_name | last_name |
	+------------+-----------+
	| Georgi     | Facello   |
	| Bezalel    | Simmel    |
	| Parto      | Bamford   |
	| Chirstian  | Koblick   |
	| Kyoichi    | Maliniak  |
	| Anneke     | Preusig   |
	| Tzvetan    | Zielinski |
	| Saniya     | Kalloufi  |
	| Sumant     | Peac      |
	| Duangkaew  | Piveteau  |
	|	.......... | ......... |
	|	.......... | ......... |
	|	.......... | ......... |
	+------------+-----------+
```

  

### SELECT with DISTINCT and LIMIT

- DISTINCT- Displays only the records of a column
- LIMIT- Displays only top n records from the SELECT statement
    
    Ex: `SELECT * FROM {databasename} LIMIT 10` displays first 10 records
    

  

```SQL
select distinct(first_name), hire_date from employee limit 10;

--Eg:
	mysql> select distinct(first_name), hire_date from employees limit 10;
	+------------+------------+
	| first_name | hire_date  |
	+------------+------------+
	| Georgi     | 1986-06-26 |
	| Bezalel    | 1985-11-21 |
	| Parto      | 1986-08-28 |
	| Chirstian  | 1986-12-01 |
	| Kyoichi    | 1989-09-12 |
	| Anneke     | 1989-06-02 |
	| Tzvetan    | 1989-02-10 |
	| Saniya     | 1994-09-15 |
	| Sumant     | 1985-02-18 |
	| Duangkaew  | 1989-08-24 |
	+------------+------------+
```

  

### WHERE Clause

Where clause is used to filter the records from a table. Only those records which satisfies the condition will be in the result table. We can have multiple conditions to extract the data from the table. WHERE clause with AND operator will be used to achieve that.

```SQL
select * from {database_name} where condition;

--Eg:
	mysql> select first_name, last_name, hire_date from employees 
				 where hire_date > '2000-01-01';
	+------------+------------+------------+
	| first_name | last_name  | hire_date  |
	+------------+------------+------------+
	| Ulf        | Flexer     | 2000-01-12 |
	| Seshu      | Rathonyi   | 2000-01-02 |
	| Randi      | Luit       | 2000-01-02 |
	| Ennio      | Alblas     | 2000-01-06 |
	| Volkmar    | Perko      | 2000-01-13 |
	| Xuejun     | Benzmuller | 2000-01-04 |
	| Shahab     | Demeyer    | 2000-01-08 |
	| Jaana      | Verspoor   | 2000-01-11 |
	| Jeong      | Boreale    | 2000-01-03 |
	| Yucai      | Gerlach    | 2000-01-23 |
	| Bikash     | Covnot     | 2000-01-28 |
	| Hideyuki   | Delgrande  | 2000-01-22 |
	+------------+------------+------------+

select * from {database_name} where condition1 and condition2;

--Eg:
	mysql>  select first_name, last_name, hire_date, gender from employees 
					where hire_date > '2000-01-01' and gender = 'F';
	+------------+------------+------------+--------+
	| first_name | last_name  | hire_date  | gender |
	+------------+------------+------------+--------+
	| Seshu      | Rathonyi   | 2000-01-02 | F      |
	| Randi      | Luit       | 2000-01-02 | F      |
	| Ennio      | Alblas     | 2000-01-06 | F      |
	| Volkmar    | Perko      | 2000-01-13 | F      |
	| Xuejun     | Benzmuller | 2000-01-04 | F      |
	| Jaana      | Verspoor   | 2000-01-11 | F      |
	| Hideyuki   | Delgrande  | 2000-01-22 | F      |
	+------------+------------+------------+--------+
```

  

### WHERE Clause with IN operator

In the above query, we've seen WHERE clause filtering the data of a column for a single value. IN statement allows us to filter a column for a list of values. We can specify the list of values manually or use a SELECT statement which yields a list of values from a column

```SQL
select * from {table_name} where {column_name} IN (value1, value2, ...);
--Eg:
	mysql> select * from employees where first_name 
				 IN ('Chirstian', 'John', 'Claudi') limit 10;
	+--------+------------+------------+-----------+--------+------------+
	| emp_no | birth_date | first_name | last_name | gender | hire_date  |
	+--------+------------+------------+-----------+--------+------------+
	|  10004 | 1954-05-01 | Chirstian  | Koblick   | M      | 1986-12-01 |
	|  10067 | 1953-01-07 | Claudi     | Stavenow  | M      | 1987-03-04 |
	|  10465 | 1963-03-13 | Claudi     | Shackel   | M      | 1987-09-01 |
	|  11261 | 1963-09-05 | Claudi     | Demri     | F      | 1987-12-06 |
	|  11719 | 1960-04-25 | Claudi     | Baez      | M      | 1985-05-21 |
	|  12000 | 1957-11-09 | Claudi     | Angelov   | M      | 1989-01-12 |
	|  13385 | 1954-11-15 | Claudi     | Buchter   | M      | 1986-06-10 |
	|  14201 | 1964-07-03 | Chirstian  | Carrere   | F      | 1991-07-26 |
	|  14226 | 1956-05-26 | Chirstian  | Restivo   | F      | 1989-02-03 |
	|  14282 | 1961-06-22 | Claudi     | Aumann    | M      | 1985-07-06 |
	+--------+------------+------------+-----------+--------+------------+

select * from {table1} where {column1} IN (select {column1} from {table2});
--Eg:
	mysql> select * from employees where emp_no in 
				(select emp_no from dept_emp where dept_no = 'd001') limit 10;
	+--------+------------+------------+-----------+--------+------------+
	| emp_no | birth_date | first_name | last_name | gender | hire_date  |
	+--------+------------+------------+-----------+--------+------------+
	|  10017 | 1958-07-06 | Cristinel  | Bouloucos | F      | 1993-08-03 |
	|  10055 | 1956-06-06 | Georgy     | Dredge    | M      | 1992-04-27 |
	|  10058 | 1954-10-01 | Berhard    | McFarlin  | M      | 1987-04-13 |
	|  10108 | 1952-04-07 | Lunjin     | Giveon    | M      | 1986-10-02 |
	|  10140 | 1957-03-11 | Yucel      | Auria     | F      | 1991-03-14 |
	|  10175 | 1960-01-11 | Aleksandar | Ananiadou | F      | 1988-01-11 |
	|  10208 | 1960-01-02 | Xiping     | Klerer    | M      | 1991-12-23 |
	|  10228 | 1953-04-21 | Karoline   | Cesareni  | F      | 1991-08-26 |
	|  10239 | 1955-03-31 | Nikolaos   | Llado     | F      | 1995-05-08 |
	|  10259 | 1964-11-24 | Susanna    | Vesel     | M      | 1986-06-25 |
	+--------+------------+------------+-----------+--------+------------+
	
```

  

### WHERE Clause with BETWEEN Operator

WHERE clause with BETWEEN statement will help us to retrieve the rows based on between a range of values in a column.

```SQL
select * from {table_name} where {column_name} between {value1} and {value2}; 
-- value1 should be less than value2

--Eg:
	select * from employees where birth_date between '1965-01-01' and '1975-01-01';
	+--------+------------+------------+-------------+--------+------------+
	| emp_no | birth_date | first_name | last_name   | gender | hire_date  |
	+--------+------------+------------+-------------+--------+------------+
	|  10095 | 1965-01-03 | Hilari     | Morton      | M      | 1986-07-15 |
	|  10122 | 1965-01-19 | Ohad       | Esposito    | M      | 1992-12-23 |
	|  10291 | 1965-01-23 | Dipayan    | Seghrouchni | M      | 1986-09-29 |
	|  10410 | 1965-01-19 | Takahito   | Gecsei      | F      | 1993-10-16 |
	|  10476 | 1965-01-01 | Kokou      | Iisaka      | M      | 1987-09-20 |
	|  10480 | 1965-01-25 | Make       | Baba        | F      | 1988-05-18 |
	|  10663 | 1965-01-09 | Teunis     | Noriega     | M      | 1993-01-23 |
	|  10762 | 1965-01-19 | Lech       | Himler      | M      | 1992-01-21 |
	|  10933 | 1965-01-24 | Juyoung    | Seghrouchni | F      | 1993-08-02 |
	|  11015 | 1965-01-24 | Jeanna     | Riesenhuber | M      | 1992-05-29 |
	+--------+------------+------------+-------------+--------+------------+
```

### MIN() and MAX() - Aggregations

MIN() and MAX() will give the minimum and maximum of numeric or date column.

```SQL
select min(column_name), max(column_name) from table_name;

--Eg:
	mysql> select min(salary), max(salary) from salaries;
	+-------------+-------------+
	| min(salary) | max(salary) |
	+-------------+-------------+
	|       38623 |      158220 |
	+-------------+-------------+
```

  

We can use alias (AS) to change name of the columns in the result as mentioned below:

```SQL
select min(column_name) as min_value, max(column_name) as max_value from table_name;

--Eg:
	mysql> select min(salary) as min_salary, max(salary) as max_salary from salaries;
	+------------+------------+
	| min_salary | max_salary |
	+------------+------------+
	|      38623 |     158220 |
	+------------+------------+
```

  

### COUNT(), AVG() and SUM()- Aggregations

- COUNT()- Gives the number of rows in the table
- AVG()- Average of all the values in the column
- SUM()- Returns the total of a numeric column

```SQL
select count(salary), avg(salary), sum(salary) from salaries where emp_no in
(select emp_no from dept_emp where dept_no = 'd001');

--Eg:
	mysql> select count(salary), avg(salary), sum(salary) from salaries where emp_no in
	(select emp_no from dept_emp where dept_no = 'd001');
	+---------------+-------------+-------------+
	| count(salary) | avg(salary) | sum(salary) |
	+---------------+-------------+-------------+
	|        190861 |  71913.2000 | 13725425266 |
	+---------------+-------------+-------------+
```

  

### GROUP BY Clause

Groups the rows with same values and arranges data into distinct groups. Aggregations like COUNT(), SUM(), AVG() etc., are used along with GROUP BY on numerical columns to calculate results for each unique group.

```SQL
select col1, col2,... from {table_name} group by {col};
select MAX(col1), SUM(col2),... from {table_name} group by {col};
--Eg:
	select count(emp_no) from dept_emp group by dept_no;
	+---------------+
	| count(emp_no) |
	+---------------+
	|         20211 |
	|         17346 |
	|         17786 |
	|         73485 |
	|         85707 |
	|         20117 |
	|         52245 |
	|         21126 |
	|         23580 |
	+---------------+
```

  

### GROUP BY with HAVING Clause

The WHERE clause will filter the data of a table and HAVING clause will filter the aggregate functions after the GROUP BY clause

```SQL
SELECT COUNT(CustomerID), Country FROM Customers GROUP BY Country
HAVING COUNT(CustomerID) > 5;
```

  

### JOINS

1. Inner Join
    
    ![[Learning and Creation/Attachments/Untitled 3.png|Untitled 3.png]]
    
    ```SQL
    SELECT column_name(s)
    FROM table1
    INNER JOIN table2
    ON table1.column_name = table2.column_name;
    
    SELECT Orders.OrderID, Customers.CustomerName, Orders.OrderDate
    FROM Orders
    INNER JOIN Customers ON Orders.CustomerID=Customers.CustomerID;
    ```
    
2. Left Join
    
    ![[Untitled 1 2.png|Untitled 1 2.png]]
    
    ```SQL
    SELECT column_name(s)
    FROM table1
    LEFT JOIN table2
    ON table1.column_name = table2.column_name;
    
    SELECT Customers.CustomerName, Orders.OrderID
    FROM Customers
    LEFT JOIN Orders ON Customers.CustomerID = Orders.CustomerID
    ORDER BY Customers.CustomerName;
    ```
    
      
    
3. Right Join
    
    ![[Learning and Creation/Attachments/Untitled 2 2.png|Untitled 2 2.png]]
    
    ```SQL
    SELECT column_name(s)
    FROM table1
    RIGHT JOIN table2
    ON table1.column_name = table2.column_name;
    
    SELECT Orders.OrderID, Employees.LastName, Employees.FirstName
    FROM Orders
    RIGHT JOIN Employees ON Orders.EmployeeID = Employees.EmployeeID
    ORDER BY Orders.OrderID;
    ```
    
4. Full Outer Join
    
    ![[Learning and Creation/Attachments/Untitled 3 2.png|Untitled 3 2.png]]
    

```SQL
SELECT column_name(s)
FROM table1
FULL OUTER JOIN table2
ON table1.column_name = table2.column_name
WHERE condition;

SELECT Customers.CustomerName, Orders.OrderID
FROM Customers
FULL OUTER JOIN Orders ON Customers.CustomerID=Orders.CustomerID
ORDER BY Customers.CustomerName;
```