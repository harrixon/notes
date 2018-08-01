```js
const express = require('express');
const bodyParser = require('body-parser');
const fileUpload = require('express-fileupload');
const path = require('path');
const app = express();

app.use(bodyParser.urlencoded({ extended: false }));

// website with many static files that you want to serve, like CSS files, HTML files or image 
app.use(express.static('public'));

// creating middleware
app.use('/', function(req, res, next){
    console.log('Request Url: '+ req.url);
    next();
});

// OR, import the express-fileupload and setup express to use it as a middleware
app.use(fileUpload());

app.get('/',(req,res)=>{
	res.sendFile(path.join(__dirname,'index.html'));
});

app.post('/login',(req,res)=>{
	console.log(req.body);
	console.log(req.files);
	res.send("Login Information Received.");
});

app.listen(8080);

```



