# REDIS

- NoSQL (= key value data store)
- often used for caching (eg Twitter)

---

### Installing REDIS on ubuntu

```cmd
tar xzf redis-4.0.0.tar.gz
cd redis-4.0.0
make
sudo make install
// on encounter compilation errors, install following:
sudo apt install tcl
sudo apt install make  
sudo apt install build-essential
// make again
// try this if still not ok:
cd deps; make hiredis lua jemalloc linenoise
```

---

###Running REDIS

```cmd
$ redis-server --daemonize yes
$ redis-cli
127.0.0.1:6379> // now ready
127.0.0.1:6379> redis-cli shutdown  // shutdown, or ctrl+c
```

---

### Using REDIS

```cmd
// SET [key] [value]
SET location 'HONG KONG'
> OK

// GET [key]
GET location
> "HONG KONG"

// SETEX [key] [seconds] [value]  => set expire time in sec
SETEX weather 20 rainy
> OK

// DEL [key]
DEL location
(integer) 1

// LPUSH RPOP and more
```

https://redis.io/commands

---

### REDIS n Node

``` cmd
npm install --save redis
```

```js
var redis = require('redis');
var client = redis.createClient({
    host: 'localhost',
    port: 6379
});

client.on('error', function(err){
    console.log(err);
});
// =====
client.setex('location', 60, 'Hong Kong', function(err, data) {
    if(err) {
        return console.log(err);
    }

    client.get('location', function(err, data){
        if(err) {
            return console.log(err);
        }
        console.log('The value is ', data);
    });
});

// If no value in list, return NULL
// JSON.stringify b4 storing obj, JSON.parse after retrieving
```



