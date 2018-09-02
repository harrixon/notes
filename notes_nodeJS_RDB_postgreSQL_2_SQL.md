# SQL 

## Index

- Basic SQL
  1. [General Command](#general-command)
  2. [Create and Drop Tables](#create-and-drop-tables)
  3. [Add / Update / Delete data to table](#add-/-update-/-delete-data-to-table)
  4. [Altering a table](#altering-a-table)
  5. [Querying data](#querying-data)
  6. [Refined search](#refined-search)
  7. [Examples](#daily-exercise-as-example)

- SQL Joins
  1. [Primary Key and Foreign Key](#primary-key-and-foreign-key)
  2. [Different type of Joins](#different-type-of-joins)

- [SQL Injection](#SQL-injection)



## Getting started

```bash
## Starting PostgreSQL 9.5 database server
$ sudo service postgresql start
[sudo] password for postgres: password

## To use superuser postgres
$ sudo su postgres

## To go into PSQL shell
$ psql
```

---

## Within the PSQL shell (postgres=#)

### General command

```sql
/* To create a username same as the one in bash */
CREATE USER <username> WITH PASSWORD 'somepw' SUPERUSER;

/* To list all user */
\du

/* To list all db */
\l

/* To list all tables in a db */
\dt

/* To leave postgresql shell */
\q

/* To connect to a db */
\c <dbname>
```

### Create and Drop Tables

```sql
/* 
	To create table:
        SERIAL: the counter ‘id’ increases with every row
        primary key: id is the unique key by which each row can be identified
        VARCHAR: the column will be filled with text of a not predefined length
        (255): maximum length the text can have
        not null:  value cannot be empty
*/
CREATE TABLE citrus (
	id SERIAL primary key,
	name VARCHAR(255) not null,
	color VARCHAR(255),
	taste VARCHAR(255)
);

/* To drop a table */
DROP TABLE <some_table>;
```

### Add / Update / Delete data to table

```sql
INSERT INTO citrus (name, color, taste) VALUES ('lemon', 'yellow', 'sour');
INSERT INTO citrus (name, color, taste) VALUES ('orange', 'orange', 'juicy');
INSERT INTO citrus (name, color, taste) VALUES ('grapefruit', 'orange', 'bitter');
INSERT INTO citrus (name, color, taste) VALUES ('lime', 'green', 'sour');
/* Order of column doesn't matter */
INSERT INTO citrus (color, name, taste) VALUES ('yellow', 'tangerine', 'sweet');
```

```sql
UPDATE citrus SET color = 'orange' WHERE color = 'yellow';
```

```sql
DELETE FROM citrus WHERE name = 'tangerine';
```

### Altering a table

```sql
/* To change existing table structure */
ALTER TABLE table_name action;

/* To ADD | DROP | RENAME and column */
ALTER TABLE table_name ADD COLUMN new_column_name TYPE;
ALTER TABLE table_name DROP COLUMN column_name;
ALTER TABLE table_name RENAME COLUMN column_name TO new_column_name;

/* To change a default value of the column */
ALTER TABLE table_name ALTER COLUMN column_name [SET DEFAULT value | DROP DEFAULT];

/* To change the NOT NULL constraint */
ALTER TABLE table_name ALTER COLUMN column_name [SET NOT NULL| DROP NOT NULL];

/* To add a CHECKconstraint */
ALTER TABLE table_name ADD CHECK expression;

/* To add a constraint */
ALTER TABLE table_name ADD CONSTRAINT constraint_name constraint_definition;

/* To rename a table */
ALTER TABLE table_name RENAME TO new_table_name;
```

---

### Querying data

```sql
SELECT * FROM table_name;

id |    name    | color  | taste
----+------------+--------+--------
  1 | lemon      | yellow | sour
  2 | orange     | orange | juicy
  3 | grapefruit | orange | bitter
  4 | lime       | green  | sour
```

---

### Refined search

use `WHERE` to limit the search (to some specific col)

```sql
SELECT * FROM citrus WHERE color = 'orange';

id |    name    | color  | taste
----+------------+--------+--------
 2 | orange     | orange | juicy
 3 | grapefruit | orange | bitter
```

also with `AND` , `OR` to further refined the search

```sql
SELECT * FROM citrus WHERE color = 'orange' AND id > 2 OR color = 'green';

id |    name    | color  | taste
----+------------+--------+--------
 3 | grapefruit | orange | bitter
 4 | lime       | green  | sour
```

---

### Daily exercise as example

```sql
/* EX A */
fruits=# CREATE TABLE stock (
	id SERIAL primary key,
	name VARCHAR(255) not null,
	description_text VARCHAR,
	quantity_on_stock INTEGER,
	price DECIMAL);
	
/* CREATE TABLE */

fruits=# SELECT * FROM stock;

/*
id | name | description_text | quantity_on_stock | price
----+------+------------------+-------------------+-------
(0 rows)
*/

fruits=#
INSERT INTO stock (name , description_text , quantity_on_stock , price) VALUES ('apple' , 'apple' , '10' , '4');
/* INSERT 0 1 */
fruits=#
INSERT INTO stock (name , description_text , quantity_on_stock , price) VALUES ('orange' ,
'orange' , '20' , '5');
/* INSERT 0 1 */
/*
 id |  name  | description_text | quantity_on_stock | price
----+--------+------------------+-------------------+-------
  1 | apple  | apple            |                10 |     4
  2 | orange | orange           |                20 |     5
(2 rows)
*/

/* EX B */
fruits=#
DROP TABLE stock;
```

```sql
/* EX C */
fruits=#
CREATE TABLE employee(
	EMPLOYEE_ID SERIAL primary key,
	FIRST_NAME text,
	LAST_NAME text,
	SALARY INTEGER,
	CONTRACT_LENGTH INTEGER);
/* CREATE TABLE */

INSERT INTO employee (employee_id, first_name, last_name, salary, contract_length) VALUES (
1, 'Steven', 'King', 10000, 3);

INSERT INTO employee (employee_id, first_name, last_name, salary, contract_length) VALUES (2, 'Neena', 'Kochhar', 8000, 2);

INSERT INTO employee (employee_id, first_name, last_name, salary, contract_length) VALUES (
3, 'David', 'Austin', 12000, 2);

INSERT INTO employee (employee_id, first_name, last_name, salary, contract_length) VALUES (4, 'Nancy', 'Greenberg', 3000, 1);

SELECT * FROM employee;

/*
 employee_id | first_name | last_name | salary | contract_length
-------------+------------+-----------+--------+-----------------
           1 | Steven     | King      |  10000 |               3
           2 | Neena      | Kochhar   |   8000 |               2
           3 | David      | Austin    |  12000 |               2
           4 | Nancy      | Greenberg |   3000 |               1
(4 rows)
*/

/* C1 */
SELECT first_name,last_name FROM employee WHERE SALARY > 5000 AND SALARY < 11000;
/* C2 */
select * from employee where contract_length < 2;
/* C3 */
insert into employee (employee_id, first_name, last_name, salary, contract_length) VALUES ('5', 'John', 'Doe', 15000, 5);
insert into employee (employee_id, first_name, last_name, salary, contract_length) VALUES ('6', 'Jane', 'Doe', 9000, 1);
/* C4 */
update employee set contract_length = '2', salary = '8000' where first_name = 'Nancy' and last_name = 'Greenberg';
/* C5 */
select * from employee order by salary desc;
```

---

# SQL joins

## Primary Key and Foreign Key

Join two tables together using foreign key

```sql
/* examples:
stock:
id | quantity | price | citrus_id
----+----------+-------+-----------
  1 |       33 |    25 |         1
  2 |       50 |    15 |         2
  3 |       10 |    35 |         3
  4 |        0 |    20 |         4
  
citrus:
  id |    name    | color  | taste
 ----+------------+--------+--------
   1 | lemon      | yellow | sour
   2 | orange     | orange | juicy
   3 | grapefruit | orange | bitter
   4 | lime       | green  | sour
*/

SELECT citrus.name AS citrus_name, stock.price AS stock_price
FROM citrus
JOIN stock
ON citrus.id = stock.citrus_id;

/*
citrus_name | stock_price
-------------+-------------
lemon       |          25
orange      |          15
grapefruit  |          35
lime        |          20
*/

```

---

### Different type of Joins

```
JOIN stock on leftTable.column = rightTable.column
```

https://blog.codinghorror.com/a-visual-explanation-of-sql-joins/

- `INNER JOIN` : Join the rows only if both `leftTable.column` and `rightTable.column` are not null
- `LEFT OUTER JOIN` : Join the rows only if `leftTable.column` is not null. It does not matter whether `rightTable.column` is null or not.
- `RIGHT OUTER JOIN`: Join the rows only if `rightTable.column` is not null. It does not matter whether `leftTable.column` is null or not.
- `FULL OUTER JOIN` : Join the rows if `rightTable.column` is not null or `leftTable.column` is not null.

---

### EX A

```sql
fruits=#
select citrus.name as citrus_name, stock.quantity as stock_quantity
from citrus
join stock
on citrus.id = stock.citrus_id
where citrus.color = 'orange'
and stock.quantity > 0;

/*
 citrus_name | stock_quantity
-------------+----------------
 orange      |             50
 grapefruit  |             10
(2 rows)
*/

/* or: */
fruits=#
select * from citrus right outer join stock 
on citrus.id = stock.citrus_id 
where color='orange' 
and quantity > 0;

/*
 id |    name    | color  | taste  | id | quantity | price | citrus_id
----+------------+--------+--------+----+----------+-------+-----------
  2 | orange     | orange | juicy  |  2 |       50 |    15 |         2
  3 | grapefruit | orange | bitter |  3 |       10 |    35 |         3
(2 rows)
*/
```

---

### EX B

```sql
fruits=#
select * from citrus full outer join stock
on citrus.id = stock.citrus_id;

/*
 id |    name    | color  | taste  | id | quantity | price | citrus_id
----+------------+--------+--------+----+----------+-------+-----------
  1 | lemon      | yellow | sour   |  1 |       33 |    25 |         1
  2 | orange     | orange | juicy  |  2 |       50 |    15 |         2
  3 | grapefruit | orange | bitter |  3 |       10 |    35 |         3
  4 | lime       | green  | sour   |  4 |        0 |    20 |         4
(4 rows)
*/
```

---

## SQL Injection

- You will want to enter a lot of user input into your table
- But the inputs shall not be trusted
- Eg: receiving a `DROP TABLE` input
- Sanitize all database inputs
- Transform all user input to a string, so any command input cannot run

#### Example on how to do injection

```js
function getFruit(fruit, color) {
    var client = new pg.Client('postgres://accelerate:password@localhost:5432/fruits');

    client.connect();
    var query = 'SELECT * FROM citrus WHERE name = $1 AND color = $2;';

    client.query(query, [fruit, color], function(err, results) {
        if(err) {
            console.log(err);
        }

        console.log(results.rows);
    });
}
```

