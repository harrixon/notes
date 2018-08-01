#Knex Migration CLI
---
###Installing
`sudo npm install knex -g `
`mkdir myApp`
`cd myApp`
`npm init`
`npm install knex`
`npm install dotenv`
`npm install pg`
`knex init`
---
### ./migration/yyyymmddtttttt_new-change-on-database.js
`knex migrate:make new-change-on-database`
creates xxx_new-change-on-database.js:
(only exports.up = function(knex, Promise) {};
and exports.down = function(knex, Promise) {};)
```
// Create / Drop Table
exports.up = function (knex, Promise) {
    return knex.schema.createTable('users', (table) => {
        table.increments();
        table.string("name");
        table.string("email");
        table.timestamps(false, true);
    });
};

exports.down = function (knex, Promise) {
    return knex.schema.dropTable('users');
};

// Adding / Dropping Column
exports.up = function (knex, Promise) {
    return knex.schema.table('users', (table) => {
        table.integer('group_id').unsigned();
        table.foreign('group_id').references('groups.id');
    });
};

exports.down = function (knex, Promise) {
    return knex.schema.table('users', (table) => {
        table.dropForeign('group_id');
        table.dropColumn('group_id');
    })
};
```
---
###Run Migration
Migrate to latest version
`knex migrate:latest`
Rollback migrations
`knex migrate:rollback`
Current Version
`knex migrate:currentVersion`
---
