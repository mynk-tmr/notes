Express is an open source Nodejs Web Framework
```js
const app = require('express')()
app.listen(4000, callback);
```


```js
//methods for every HTTP verb
app.get('/', (req, res, next) => {..})

//req is HTTP request object and has info about request parameters, headers, body.

//res is HTTP response object sent to client
```

##### Request Properties
```js
req.baseUrl req.hostname req.ip req.body
req.cookies //need cookie-parser middleware
```