# Test cases on Routing

- Unit test : Jasmine / Mocha

- With help of package : SuperTest

  > https://www.npmjs.com/package/supertest

- May also need help from other packages like : "jasmine-supertest"

  > https://www.npmjs.com/package/jasmine-supertest

---

### How to start:

```bash
$ jasmine init

$ npm install supertest --save-dev
```

### In apiRouter.js

```js
class ApiRouter {
    constructor(jwtAuth, express, services){
        this.jwtAuth = jwtAuth;
        this.express = express;
        this.services = services;
    }

    getRouter(){
        const router = this.express.Router();
        
        router.get(
            "/someFunction",
            this.jwtAuth.authenticate(),
            this.someFunction.bind(this)
        );
        
        return router;
    }
}
module.exports = ApiRouter;
```

### In services.js

```js
class Services {
    constructor(knex) {
        this.knex = knex;
    }

    async someFunction() {
        return new Promise((resolve,reject)=>{
            var result = await someFunc();
            resolve(result);
        })
    }
}
module.exports = Services;
```

### In router.spec.js

```js
const app = require("./../app");
const request = require('supertest')(app);
/*
	(app) can be replaced by other APIs that you want to test with
	eg: ("www.sampleweb.io/api/someFunction/")
*/

describe("API router will", () => {
    beforeAll((done)=>{
        // get login token
        request
            .post("/api/auth/login")
            .send({username: "username", password: "password"})
            .expect("Content-type", /json/)
            .expect(200)
            .end((err, res) => {
                if (err) {
                    console.log(err);
                } else {
                    this.token = res.body.token;
                    done();
                }
            });
    });
    
    describe("GET /api/someFunction", () => {
        it("should respond with JSON", done => {
            request
            	.get("/api/someFunction")
            	.set('Authorization', 'bearer ' + this.token)
            	.set("Accept", "application/json")
            	.expect("Content-type", /json/)
            	.expect(201)
                .end((err, res) => {
                	err ? console.log(err) : done();
            	});
        });
    });
});
```

