First time
[/login-form]
session > nothing (because no COOKIE with the key connect.sid) > res [set-cookie] > + req.session
v
bodyParser > nothing (not POST… nothing in the POST)
v
passport.initialize > nothing
v
passport.session > nothing (cant find anything in the req.session)
v
app.get(‘/login-form’)
[/login]
session > yes returning user > memory session store > + req.session ({})
v
bodyParser > yes it is a POST and we found sth in the body > + req.body ({username: tom, password: tom})
v
passport.initialize > nothing
v
passport.session > nothing (cant find anything in the req.session)
v
app.get(‘/login’) > passport.authenticate() > LocalStrategy > (our code, found the user) > SerializeUser > req.session.user == (the user returned by SerializeUser) > redirect the user the successRedirect
[/]
session > yes returning user > memory session store > + req.session ({user: serializedUser})
v
bodyParser > nothing (not POST… nothing in the POST)
v
passport.initialize > nothing
v
passport.session > YES! WE FOUND THE USER in req.session! > DeserializeUser > + req.user === (the user returned by DeserializeUser)
v
app.get(‘/‘)