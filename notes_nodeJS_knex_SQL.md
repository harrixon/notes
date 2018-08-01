# Basic SQL

### Starting PostgreSQL 9.5 database server

`sudo service postgresql start`
`<your-pw>`

---

### Go into `postgres@HARRIXON-PC ~~ ### 

`sudo su postgres`

---

### Access to certain db (eg: fruits)
`psql fruits`
now in PSQL shell

---

### Shells

`$-` is Bash shell
`=#` is PSQL shell

---

### List users and attributes
=#`\du`

---

### List DB and drop DB

=# `\list`

$ `dropdb <DB_name>`

---

### Create table
```sql
CREATE TABLE citrus (
    id SERIAL primary key,
    name VARCHAR(255) not null,
    color VARCHAR(255),
    taste VARCHAR(255)
);
```

`SERIAL` means that the counter ‘id’ increases with every row. `primary key` defines that id is the unique key by which each row can be identified.

`VARCHAR(255)` indicates that the column will be filled with text of a not predefined length. 255 indicates the maximum length the text can have.

`not null` tells PostgreSQL this value cannot be empty

---

### Delete table

```sql
DROP TABLE <the_tablename_you_want_to_delete>;
```

---

### Leave PostgreSQL shell

```sql
\q
```

---

### Add / Update / Delete data to table

```sql
INSERT INTO citrus (name, color, taste) VALUES ('lemon', 'yellow', 'sour');
INSERT INTO citrus (name, color, taste) VALUES ('orange', 'orange', 'juicy');
INSERT INTO citrus (name, color, taste) VALUES ('grapefruit', 'orange', 'bitter');
INSERT INTO citrus (name, color, taste) VALUES ('lime', 'green', 'sour');
INSERT INTO citrus (name, color, taste) VALUES ('tangerine', 'yellow', 'sweet');
```

```sql
UPDATE citrus SET color = 'orange' WHERE color = 'yellow';
```

```sql
DELETE FROM citrus WHERE name = 'tangerine';
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

### some title







---

### EX A

```sql
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
```

---

### EX B

```sql
DROP TABLE stock;
```

---

### EX C

```sql
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
insert into employee (employee_id, first_name, last_name, salary, contract_length) VALUES (
'5', 'John', 'Doe', 15000, 5);
insert into employee (employee_id, first_name, last_name, salary, contract_length) VALUES (
'6', 'Jane', 'Doe', 9000, 1);
/* C4 */
update employee set contract_length = '2', salary = '8000' where first_name = 'Nancy' and last_name = 'Greenberg';
/* C5 */
select * from employee order by salary desc;
```

---

### EX D

```js
var pg = require('pg');

var config = {
    user: 'harrixon',
    database: 'fruits',
    password: 'asdfgh',
    host: 'localhost',
    port: 5432,
    max: 10, // max number of clients in the pool
    idleTimeoutMillis: 30000, // how long a client is allowed to remain idle before being closed
}

var client = new pg.Client(config);

// $sudo service postgresql start

client.connect();

let query = 'SELECT * FROM citrus WHERE color = $1 order by id asc;';

client.query(query, ["orange"], function(err, results) {
    if(err) {
        console.log(err);
    }
    console.log(results.rows);
});
```

---

# SQL joins

## Primary Key and Foreign Key

 join two tables together using foreign key

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

### Transactions

##### eg: bank transaction

`Begin`	->	`Actual SQL `	-  err, `rollback` 	->	 original state

​						-  ok,  `commit`  	-> 	 updated state

---

##### Isolation

no other transaction section can be started on same data(s) being targeted by active transaction

---

## ACID Principle

- `Atomcity` Atomicity requires that each transaction be “all or nothing”: if one part of the transaction fails, then the entire transaction fails, and the database state is left unchanged. An atomic system must guarantee atomicity in each and every situation, including power failures, errors and crashes. To the outside world, a committed transaction appears (by its effects on the database) to be indivisible (“atomic”), and an aborted transaction does not happen.
- `Consistency` The consistency property ensures that any transaction will bring the database from one valid state to another. Any data written to the database must be valid according to all defined rules, including constraints, cascades, triggers, and any combination thereof. This does not guarantee correctness of the transaction in all ways the application programmer might have wanted (that is the responsibility of application-level code), but merely that any programming errors cannot result in the violation of any defined rules.
- `Isolation` The isolation property ensures that the concurrent execution of transactions results in a system state that would be obtained if transactions were executed sequentially, i.e., one after the other. Providing isolation is the main goal of concurrency control. Depending on the concurrency control method (i.e., if it uses strict - as opposed to relaxed - serializability), the effects of an incomplete transaction might not even be visible to another transaction.
- `Durability` The durability property ensures that once a transaction has been committed, it will remain so, even in the event of power loss, crashes, or errors. In a relational database, for instance, once a group of SQL statements execute, the results need to be stored permanently (even if the database crashes immediately thereafter). To defend against power loss, transactions (or their effects) must be recorded in a non-volatile memory.

---

##### PgClient transaction (simple API)

```js
function begin(done){
    client.query('BEGIN',function(err){
        if(err){
            // Handle error here
        }
        done();
    });
}

function commit(done){
    client.query('COMMIT',function(err){
        if(err){
            //Handle error here
        }
        done();
    });
}

function rollback(done){
    client.query('ROLLBACK',function(err){
        if(err){
            //Handle error here
        }
        done();
    });
}

begin(function(){
    client.query("UPDATE bank_accounts SET amount = amount - 1000 WHERE account_holder = 'Alice'",function(err,results){
        if(err){
            rollback(function(){
                console.log("Transaction is rolled back!");
            });
        }else{
            commit(function(){
                console.log("Transaction is committed!");
            });
        }
    });
});
```


