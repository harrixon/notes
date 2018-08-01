# Bcrypt

#####bcrypt.js

```js
// npm i bcrypt
// hash = $2a$(2 chars work cost)$(22 chars salt 隨機的鹽含符號)(31 chars hash 加密的hash含符號)
// password = 12345678 , cost = 11 => random gen salt = EvgzzV0AGkKwv3bEiYKx4O
// => gen hash => TDoUubTaZm/MgHzRq/.Th4H6M3lmMLW
// whole hash = $2a$11$EvgzzV0AGkKwv3bEiYKx4OTDoUubTaZm/MgHzRq/.Th4H6M3lmMLW
const bcrypt = require('bcrypt');

module.exports.hashPassword = (plainTextPassword) => {
    return new Promise( /* saltRounds, */ (resolve, reject) => {
        bcrypt.genSalt((err, salt) => {
            if (err) {
                reject(err);
            }

            bcrypt.hash(plainTextPassword, salt, (err, hash) => {
                if (err) {
                    reject(err);
                }
                resolve(hash);
            });
        });
    });
};

module.exports.checkPassword = (plainTextPassword, hashedPassword) => {
    return new Promise((resolve, reject) => {
        bcrypt.compare(plainTextPassword, hashedPassword, (err, match) => {
            if(err) {
                reject(err);
            }
            resolve(match);
        });
    });
};
```

##### update on router.js

```js
router.get('/signup', (req, res) => {
    res.sendFile(__dirname + '/signup.html');
});

router.post('/signup', passport.authenticate('local-signup', {
    successRedirect: '/',
    failureRedirect: '/error'
}));
```

##### update on passport.js

```js
// new strategy
passport.use('local-signup', new LocalStrategy(
    (email, password, done) => {
        let user = users.find((user)=>user.email == email);
        if (user) {
            return done(null, false, { message: 'Email already taken' });
        } else {
            bcrypt.hashPassword(password)
                .then(hash => {
                	let newID = genUniqueId();
                    const newUser = {
                        id: newID,
                        email:email,
                        password: hash
                    };
               // vvv only push to arr in cache, not written in file / DB , basically is useless
                    users.push(newUser);
                    done(null, newUser);
                })
                .catch(err => console.log(err));
        }
    }
));

// update login-stragety
passport.use('local-login', new LocalStrategy(
    (email, password, done) => {
        let user = users.find((user)=>user.email == email)
        if (user == null) {
            return done(null, false, { message: 'Incorrect credentials.' });
        }

        bcrypt.checkPassword(password, user.password)
            .then(result => {
                if(result) {
                    return done(null, user);
                } else {
                    return done(null, false, { message: 'Incorrect credentials'});
                }
            })
            .catch(err => console.log(err));
    }
));
```

##### Signup.html

```html
<form action="/signup" method="post">
    <div>
        <label>email:</label>
        <input type="text" name="username"/>
    </div>
    <div>
        <label>Password:</label>
        <input type="password" name="password"/>
    </div>
    <div>
        <input type="submit" value="Sign up"/>
    </div>
</form>
```

