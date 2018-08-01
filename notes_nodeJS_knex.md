# Knex.js

###a SQL query builder library for JavaScript

```bash
npm install knex pg
```

```js
const knex = require('knex')({
    client: 'postgresql',
    connection: {
        database: "fruits",
        user: "accelerate",
        password: "password"
    }
});
```

---

###Code Comparison

```sql
// SQL
SELECT * FROM users INNER JOIN groups  on users.group_id = users.id 
                    INNER JOIN bookings on bookings.user_id = users.id 
                    WHERE users.status = 'active';
```

```js
// js with pg
var client = new pg.Client('postgres://accelerate:password@localhost:5432/bookings');

client.connect();
/* We have to write everything in one line because
the backtick preserves the newline characters in the string. */
var query = `SELECT * FROM users INNER JOIN groups on users.group_id = groups.id INNER JOIN bookings on bookings.user_id = users.id where users.status = 'active'`;

client.query(query, function(err, results) {
    if(err) {
        console.log(err);
    }

    console.log(results.rows);
});
```

```js
// js with pg + knex
const knex = require('knex')({
    client: 'postgresql',
    connection: {
        database: "bookings",
        user: "accelerate",
        password: "password"
    }
});

let query = knex.select("*").from("users")
	.innerJoin("groups","users.group_id","groups.id")
	.innerJoin("bookings","bookings.user_id","users.id")
	.where("user.status",'active');

console.log(query.toSQL()); // You can print the SQL in this way.

query.then((rows)=>{
    // You can access the fetched row here.
}).catch((error)=>{
    console.log(error);
    //Handle the error here.
});
```

---

### Test your knex query online

http://michaelavila.com/knex-querylab/

some knex code will not run without a appending .then()

---

### Common Methods

##### Select method

```js
// all 3 are the same
// Using knex() the constructor.
knex("table").column(["column1","column2","column3"])

// Using select() method
knex.select('column1','column2','column3').from('table')

// Using the column() method
knex.column('column1','column2','column3').select().from('table')
```

---

##### Where method

```js
knex.select("column1","column2","column3").from("table")
    .where("column1","ABC")
    .orWhere("column2",'>',"DEF")
    .andWhere("column3","like","%AC%");

// AND

knex.select("column1","column2","column3").from("table")
    .where("column1","ABC")
    .whereIn("table_2_id",function(){
        this.select("id").from("table2")
    });
```

gives these SQL :

```sql
SELECT "column1", "column2", "column3" 
FROM "table" 
WHERE "column1" = 'abc' 
OR "column2" > 'DEF' 
AND "column3" LIKE '%AC%'

/* AND */

SELECT "column1", "column2", "column3" 
FROM "table" 
WHERE "column1" = 'ABC' 
AND "table_2_id" IN (
    SELECT "id" FROM "table2"
)
```

---

##### Join / On method

```sql
knex.select("column1","column2","column3").from("table")
    .innerJoin("table2","table_2_id","table2.id")
    .where("column1" , "ABC")
    
/* AND */

knex.select("column1","column2","column3").from("table")
    .innerJoin("table2",function(){
        this.on('table.table_2_id','=','table2.id').onNotNull('table2.id')
    }).where("column1","ABC")
```

gives these SQL :

```sql
SELECT "column1", "column2", "column3" 
FROM "table" 
INNER JOIN "table2" ON "table_2_id" = "table2"."id" 
WHERE "column1" = 'ABC'

/* AND */

SELECT "column1", "column2", "column3" 
FROM "table" 
INNER JOIN "table2" ON "table"."table_2_id" = "table2"."id" 
AND "table2"."id" IS NOT NULL  
WHERE "column1" = 'ABC'
```

---

##### GroupBy / OrderBy / Distinct method

```js
// Using order by
knex.select("column1","column2","column3").from("table").orderBy("column1");

// Using group by
knex.count("column1").column("column2").from("table").groupBy("column2")

// Using distinct 
knex.distinct("column1").from("table");
```

gives these SQL : 

```sql
-- using order by 
SELECT "column1","column2","column3" FROM "table" ORDER BY "column1"

-- using group by
SELECT COUNT("column1"), "column2" FROM "table" GROUP BY "column2"

-- using distinct
SELECT DISTINCT "column1" FROM "table"
```

---

##### Insert

```js
// Using knex() the constructor.
knex("table").insert({column1:"value1",column2:"value2",column3:"value3"})

// Using insert() method to start the method call
knex.insert({column1:"value1",column2:"value2",column3:"value3"}).into("table")
```

both give same SQL : 

```sql
INSERT INTO "table" ("column1","column2","column3") VALUE ("value1","value2","value3");
```

```js
/* OR do both with 1 line using array: */
knex.insert([
    {column1:"value1",column2:"value2",column3:"value3"},
    {column1:"value4",column2:"value5",column3:"value6"}
]).into("table").returning("id"); // Returning id returns the id of the inserted row.
```

---

##### Batch Insert

- inserting tons of rows in 1 SQL will affect performance
- use `batchInsert`  to automatically wrap a batch of rows for each transaction

```js
// The rows could be a lot more.
var rows = [{column1:"value1",column2:"value2",column3:"value3"},
            {column1:"value4",column2:"value5",column3:"value6"},
            ...,
            {column1:"value9997",column2:"value9998",column3:"value9999"},];

var batchSize = 30; // <= 30 rows pre transaction
knex.batchInsert("table",rows,batchSize)
    .returning('id')
    .then((ids)=>{
        // Handle the returning ids here.
    })
    .catch((error)=>{
        // handle the error here.
    });
```

---

##### Update

```js
knex("table").update({column1:"value1",column2:"value2"}).where("column3","value3");
```

becomes : 

```sql
UPDATE "table" SET "column1" = 'value1', "column2" = 'value2' WHERE "column3" = 'value3';
```

---

##### Delete

```js
knex("table").where("column1","value1").delete();
```

becomes : 

```sql
DELETE FROM "table" WHERE "column1" = 'value1';
```

---

### Transaction

example from [knexjs official website](http://knexjs.org/#Transactions) :

```js
var Promise = require('bluebird');
// Using trx as a query builder:
knex.transaction(function(trx) {
    var books = [
        {title: 'Canterbury Tales'},
        {title: 'Moby Dick'},
        {title: 'Hamlet'}
    ];
    return trx
    .insert({name: 'Old Books'}, 'id')
    .into('catalogues')
    .then(function(ids) {
        return Promise.map(books, function(book) {
            book.catalogue_id = ids[0];
            // Some validation could take place here.
            return trx.insert(book).into('books');
        });
    });
})
.then(function(inserts) {
    console.log(inserts.length + ' new books saved.');
})
.catch(function(error) {
    // If we get here, that means that neither the 'Old Books' catalogues insert,
    // nor any of the books inserts will have taken place.
    console.error(error);
});
```





