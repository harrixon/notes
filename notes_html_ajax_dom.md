# AJAX and DOM

### environment

/folder/index.html
/folder/trial.js
/folder/server/data.json

on terminal @ `.../folder/trial` run `http-server ./` to start up server



### documents

trial.js

```javascript
window.onload = function(){
	
    var http = new XMLHttpRequest();

    http.onreadystatechange = function() {
		//  request is complete and status of no error
        if (http.readyState == 4 && http.status == 200){

            console.log(JSON.parse(http.response));

        }

    };

    http.open("GET" , "server/data.json", true);

    http.send();

    console.log("test");

};

/* READY STATES

0 - request not initialized
1 - request has been set up
2 - request has been sent
3 - request is in process
4 - request is complete

*/
```

data.json

```java
{
    "name": "Donald Duck",
    "occupation": "none"
}
```

index.html

```html
<!DOCTYPE html>
<html lang="en">
    <head>
        <script src="trial.js"></script>
    </head>
    <body></body>
</html>
```

