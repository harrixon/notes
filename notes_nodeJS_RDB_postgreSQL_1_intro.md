# Rational Database RDB

## Introduction

- A relational database (RDB) is a collective set of multiple data sets organized by tables, records and columns
- RDBs establish a well-defined relationship between database tables
- Tables communicate and share information, which facilitates data searchability, organization and reporting
- RDBs use Structured Query Language (SQL) which allows us to program on these databases and query them for data
- PostgreSQL is one of the SQL available

---

## Setup

@Ubuntu / Windows

1. Download pgAdmin for graphic interface
2. Actually install postgresql by `$ sudo apt-get install postgresql postgresql-contrib`

---

## Starting PostgreSQL 9.5 database server

```bash
## Start the server with:
$ sudo service postgresql start

## Access default user 'postgres' with all permission
$ sudo su postgres
[sudo] password for postgres: password

## To create a new database: 
$ createdb <DBname>
## To create a new database with utf-8 encoding instead of SQL_ASCII:
$ createdb newdb --encoding='utf-8' --locale=en_US.utf8 --template=template0

## To drop a database:
$ dropdb <DBname>

## To login into psql shell
$ psql
psql (9.5.12)
Type "help" for help.

postgres=#

## Full version of login to certain db
$ psql -U <user_name> -W -h <hostname> <DB_name>
## Eg: 
$ psql -U johndoe -W -h 127.0.0.1 somedb
...
somedb=#
```

For the last command:

- user_name is the username of **PostgreSQL Server**
- `-U` provides the username to the command psql, default username = bash’s username if -U is omitted
- `-W` requires login with password. *have to log in with password if logging in remotely
- `-h` provides the hostname of the database server, default is localhost, omit this flag for peer authentication
- <DB_name> database name, default = username in bash

---

## Doing SQL in NodeJS

```bash
## install package pg */
npm install --save pg
```

```js
/* hook pg up in NodeJS server */
var pg = require("pg");

var config = {
    user: "<username>",
    database: "<target_DB>",
    password: "password",
    host: "localhost",
    port: "5432",
    max: 10, // max # of clients in pool
    idleTimeoutMillis: 30000,
}

var client = new pg.Client(config);

client.connect();
```

### Example

```js
const pg = require('pg');

const config = {
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

