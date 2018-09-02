# Transaction

- Transactions

1. [Bank transaction](#bank-transaction)
2. [Isolation in Transaction](#isolation-in-transaction)
3. [ACID Principle](#ACID-principle)

---

### What is transaction ?

A Transaction is a unit of work performed 
which normally includes batch of command that 
you want to run reliably independent of other transaction.

---

### Database Transaction

Any change on the data of the database can be represented by as a transaction.

Normally transactions is used to make sure two things:

1. To make sure multiple changes on the data to either succeed completely or fail completely. Therefore no partial changes exists when a transaction is completed or failed.
2. To make sure that concurrent access on the data will not corrupt the data in an unexpected way. Hence either the changes on the data is completed successfully. Or the data is failed completely.

---

### Bank Transaction

Example: 

1. Withdraw $1000 from Alice Account
2. Deposit $1000 to Bob Account

The hypothetical SQL : 

```sql
UPDATE bank_accounts SET amount = amount - 1000 WHERE account_holder = 'Alice';
UPDATE bank_accounts SET amount = amount + 1000 WHERE account_holder = 'Bob';
```

So the process is :

```js
   BEGIN with origianl state
			 v
			SQL
v					v
success				error
v					v
COMMIT				ROLLBACK (means undo)
v					v
updated state		original state
```

---

### Isolation in transaction

- No other transaction section can be started on same data(s) being targeted by active transaction
- Changes made in transaction are not accessible before committed

#### Isolation Level

![Isolation levels](C:\harrixon\accelerate_notes\isolation_levels.png)

`Dirty Read` , `Non-repeatable read`, `Phantom read`,`serialization anomaly` 
are problems of concurrency access of the database. 

Each level from top to bottom is more stricter than the previous one and prevent more problems.

The isolation levels, ordered by the level of strictest in ascending order will be as follow:

- **Read Uncommitted** : It is an isolation level that is not available on PostgreSQL. It allows an transaction to read uncommitted changes from other transaction. Hence it allows the problem of `Dirty Read`.
- **Read Committed**: It is the default isolation level of PostgreSQL. It allows an transaction to read committed changes from other transaction. It does not prevent the problems of `Repeatable read` using the same query
- **Repeatable Read**: It is an isolation level that guarantees any repeated queries within a single transaction would always yield the same result even though other changes are committed. It does not prevent the problems of `phantom read`
- **Snapshot**: It is an isolation level available in other databases like `oracle`. It uses a `snapshot` of the data to avoid using locks. Hence it is an `optimistic`concurrency control
- **Serializable**: It is the strictest isolation level. It locks both read and write from other transactions to the table whenever any `read` and `write` operations are run. It guarantees that the transactions can be run safely in a serialized manner. Otherwise an error called `ERROR: could not serialize access due to concurrent update`will be shown.

To use another level of isolation in postgreSQL :

```sql
BEGIN;
SET TRANSACTION ISOLATION LEVEL Serializable;
-- Run some queries
COMMIT;
```

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



------

## ACID Principle

- `Atomcity` Atomicity requires that each transaction be “all or nothing”: if one part of the transaction fails, then the entire transaction fails, and the database state is left unchanged. An atomic system must guarantee atomicity in each and every situation, including power failures, errors and crashes. To the outside world, a committed transaction appears (by its effects on the database) to be indivisible (“atomic”), and an aborted transaction does not happen.
- `Consistency` The consistency property ensures that any transaction will bring the database from one valid state to another. Any data written to the database must be valid according to all defined rules, including constraints, cascades, triggers, and any combination thereof. This does not guarantee correctness of the transaction in all ways the application programmer might have wanted (that is the responsibility of application-level code), but merely that any programming errors cannot result in the violation of any defined rules.
- `Isolation` The isolation property ensures that the concurrent execution of transactions results in a system state that would be obtained if transactions were executed sequentially, i.e., one after the other. Providing isolation is the main goal of concurrency control. Depending on the concurrency control method (i.e., if it uses strict - as opposed to relaxed - serializability), the effects of an incomplete transaction might not even be visible to another transaction.
- `Durability` The durability property ensures that once a transaction has been committed, it will remain so, even in the event of power loss, crashes, or errors. In a relational database, for instance, once a group of SQL statements execute, the results need to be stored permanently (even if the database crashes immediately thereafter). To defend against power loss, transactions (or their effects) must be recorded in a non-volatile memory.

------

## 