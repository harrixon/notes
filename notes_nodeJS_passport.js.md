# Passport.js 

##### users.js

```js
// storing in .js for demo purpose
module.exports = [
    {
        id : 0,
        email :"tom@tom.com",
        password: 'tom'
    },{
        id: 1,
        email : "peter@peter.com",
        password: 'peter'
    }
];
```

#####database : passport.js 

```js
// npm install --save passport passport-local
const passport = require('passport');
const LocalStrategy = require('passport-local').Strategy;
//    ^^^ Login check logic u write
// visit http://www.passportjs.org/ for strategy written by others

// read in the list of users n info
const users = require('./users');

// same 'app' as   vvvv    with express
module.exports = (app) => {
    // vvv Tell express we are using passport as authentication Middleware
    app.use(passport.initialize());
    app.use(passport.session());
	
    // vvv Write ur own strategy, done is a cb()
    passport.use('local-login', new LocalStrategy(
        (email, password, done) => {
            let user = users.find((user)=> user.email == email);
            if (user == null) {
                return done(null, false, { message: 'Incorrect credentials.' });
            }
            if (user.password === password) {
                return done(null, user);
            }
            return done(null, false, { message: 'Incorrect credentials.' });
        }
    ));

    passport.serializeUser((user, done) => {
        // unique user id in our db(user.js) is hashed n stored in session
        // once user is authen'ed
        done(null, user.id);
    });
   // ^^^vvv These 2 uses express-session middleware to en/de-crypt user data to verify info
    passport.deserializeUser((id, done) => {
        let user = users.find((user)=> user.id == id);
        if (user == null) {
            done(new Error('Wrong user id.'));
        }
        done(null, user);
    });
};
```

##### Router.js

```js
const passport = require('passport');

module.exports = (express) => {
    const router = express.Router();

    function isLoggedIn(req, res, next) {
        if (req.isAuthenticated()) {
         // ^^^^^^^^^^^^^^^^ Is influenced by passport.js
         // turns TRUE once passport.js successfully authenticated the user.
            
            return next();
            //     ^^^ Authen user, go on to do wtever is set in that router 
        }
		// not logged in , redir to login page
        res.redirect('/login');
    }


    router.get('/secret',  isLoggedIn, (req, res) => {
        res.send('Here you go, a secret');
    });

    router.get('/login', (req, res) => {
        res.sendFile(__dirname + '/login.html');
    });

    router.post('/login', passport.authenticate('local-login', {
        successRedirect: '/',
        failureRedirect: '/error'
    }));

    router.get('/error', (req, res) => {
        res.send('You are not logged in!');
    });

    router.get('/', (req, res) => {
        res.sendFile(__dirname + '/index.html');
    });

    return router;
};
```

##### app.js

```js
const express = require('express');
const app = express();
const session = require('express-session');
const setupPassport = require('./passport');
const bodyParser = require('body-parser');
const router = require('./router')(express);
const port = process.env.PORT || 3030;

// vvv Keep track of users who are logged in so we dont need to authen them everytime
app.use(session({
    // vvv Key to en/de-crypt session data
    secret: 'supersecret'
}));

app.use(bodyParser());

setupPassport(app);

app.use('/', router);

app.listen(port);
console.log('listening on port ', port);
```

##### HTML

```html
<!-- login.html -->
<form action="/login" method="post">
    <div>
        <label>email:</label>
        <input type="text" name="username"/>
    </div>
    <div>
        <label>Password:</label>
        <input type="password" name="password"/>
    </div>
    <div>
        <input type="submit" value="Log In"/>
    </div>
</form>

<!-- index.html -->
<!DOCTYPE html>
<html>
    <head>
        <meta charset="utf-8">
        <title>success</title>
    </head>
    <body>
        <h1>Passport App</h1>
        <a href="/secret"><button type="button" name="button" >secret</button></a>
    </body>
</html>
```

### Serialize and De-serialize User

> 1. Where does user.id go after passport.serializeUser has been called?

The user id (you provide as the second argument of the `done` function) is saved in the session and is later used to retrieve the whole object via the `deserializeUser` function.

`serializeUser` determines, which data of the user object should be stored in the session. The result of the serializeUser method is attached to the session as `req.session.passport.user = {}`. Here for instance, it would be (as we provide the user id as the key) `req.session.passport.user = {id:'xyz'}`



> 1. We are calling passport.deserializeUser right after it where does it fit in the workflow?

The first argument of `deserializeUser` corresponds to the key of the user object that was given to the `done` function (see 1.). So your whole object is retrieved with help of that key. That key here is the user id (key can be any key of the user object i.e. name,email etc). In `deserializeUser` that key is matched with the in memory array / database or any data resource.

The fetched object is attached to the request object as `req.user`



Visual Flow :

```js
passport.serializeUser(function(user, done) {
    done(null, user.id);
                 |
});              | 
                 |
                 |____________________> saved to session req.session.passport.user = {id:'..'}
                                   |
                                  \|/           
passport.deserializeUser(function(id, done) {
                   ________________|
                   |
                  \|/ 
    User.findById(id, function(err, user) {
        done(err, user);
                   |______________>user object attaches to the request as req.user

 	});
});
```

