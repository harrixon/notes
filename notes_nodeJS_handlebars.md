#handlebars

####sth.handlebars

```handlebars
<script type='text/javascript' src="/js/users.js"></script>

<div>

    <div id='user-list'>

    </div>

</div>

<template id="template-info-container">

    <div class="info-container">

        <label class="lbl-info">First Name: </label><p></p>

        <label class="lbl-info">Last Name: </label><p></p>

        <label class="lbl-info">Email: </label><p></p>

    </div>

</template>
```

####sth.js

```js
$(()=>{
    $.get('/api/users').then((data)=>{
        console.log(data);

        let usersInfoContainer = $('#template-info-container').clone();
        let userInfo = usersInfoContainer.contents().find('p');

        data.forEach(e => {
            userInfo.eq(0).html(e.first_name);
            userInfo.eq(1).html(e.last_name);
            userInfo.eq(2).html(e.email);
            $('#user-list').append(usersInfoContainer.html());
        });
    });
});
```