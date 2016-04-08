# Express

A web application framework for Node

```sh
$ npm install express --save
```

## Hello world example

```js
const express = require('express');
const app = express(); // create an express app instance

app.get('/', function(request, response) {
  response.send('Hello world');
});

app.listen(3000, function() {
  console.log('Listening on port 3000');
});
```

Start the app :

```sh
$ node app.js
```

## Response object

Inherits from `http` module's objects methods like `write()` or `end()`.

Send a response as `text/html`:

```js
response.send('Hello World');
```

Send a response as `application/json`:

```js
response.send(['One', 'Two', 'Three']);
```

Send a response as `application/json` (more explicit):

```js
response.json(['One', 'Two', 'Three']);
```

Send a temporary (301) redirect response:

```js
response.redirect('/new_url');
```

Send a permanent (302) redirect response:

```js
response.redirect(301, '/new_url');
```


## Middlewares

When a request arrives, it will pass through the middleware stack before a response is sent.


### Static

The only middleware shipped with Express. Used to serve static files.

Serve static file from the `/public` folder:

```js
app.use(express.static('public'));
```

Will serve by default an `index.html` file.


### Create a middleware

Add a middleware to the express stack:

```js
// middleware.js
module.exports = function(request, response, next) {
  next(); // will call the next middleware
};

// app.js
const middleware = require('./middleware');
app.use(middleware);
```
