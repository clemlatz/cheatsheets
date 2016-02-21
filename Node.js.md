# Node.js


## Introduction

Node.js allows to build scalable network applications using Javascript on the
server-side. It's a wrapper around the V8 JavaScript Runtime, both are written in C.

It's not:
* A web framework
* For beginners
* Multi-threaded

### Why JavaScript ?

> Javascript has certain characteristics that makes it very different than other
> dynamic languages, namely that it has no concept of threads. Its model of
> concurrency is completely based around events.

Ryan Dahl, creator of Node.js

### Blocking vs. non-blocking

Blocking code example:

```javascript
var contents = fs.readFileSync('/etc/hosts'); // 1
console.log(contents); // 2
console.log('Doing something else.'); // 3
```

Non-blocking code example:

```javascript
fs.readFile('/etc/hosts', function(err, contents) { // 1
  console.log(contents); // Whenever file reading is finished
});
console.log('Doing something else'); // 2
```

### Basic HTTP server

```javascript
var http = require('http'); // Requiring http module

http.createServer(function(request, response) {
  response.writeHead(200);
  response.write("Yo, world");
  response.end();
}).listen(8080);
```

```
$ node hello.js # runs the server
```


## Events

Just like in the DOM, many objects in Node emit events.

We can easily create a custom event emitter object:

```javascript
var EventEmitter = require('events').EventEmitter;
var logger = new EventEmitter();
logger.on('error', function(message) {
  console.log('ERR: ' + message);
});

logger.emit('error', 'Spilled milk!');
logger.emit('error', 'Cracked eggs!');
```

Our basic HTTP server could be rewritten as:

```javascript
var http = require('http');
var server = http.createServer(); // returns an EventEmitter
server.on('request', function(request, response) {
  // ...
});
server.on('close', closeCallback);
server.listen(8080);
```

## Streams

Streams allows to process data piece by piece, or chunk by chunk. Most commons
streams are readable (like the `request` object from the `http` module) and
writable (like `response` or `process.stdout`) streams.

The following example will process the request and stream a response back to
the client before the request has even finished being send:

```javascript
http.createServer(function(request, response) {
  response.writeHead(200);
  request.on('readable', function() { // triggered when data is received
    var chunk = null;
    while (null !== (chunk = request.read())) {
      console.log(chunk.toString()); // data from buffer may be binary
      response.write(chunk); // no need to call toString();
    }
  })
  request.on('end', function() { // when all data has been received
    response.end();
  });
}).listen(8080);
```

This is called piping and can written in a single line using the `pipe` method:

```javascript
http.createServer(function(request, response) {
  response.writeHead(200);
  request.pipe(response);
}).listen(8080);
```

Copy a file without loading it entirely in memory:

```javascript
var fs = require('fs'); // filesystem module
var file = fs.createReadStream("readme.md");
var newFile = fs.createWriteStream("readme_copy.md");
file.pipe(newFile); // where the magic happens
```

Accept a file upload and stream back the progress:

```javascript
http.createServer(function(request, response) {
  var newFile = fs.createWriteStream("readme_copy.md");
  var fileBytes = request.headers['content-length']; // Get file size from request headers
  var uploadedBytes = 0;

  request.on('readable', function() { // triggered when data is received
    var chunk = null;
    while (null !== (chunk = request.read())) {
      uploadedBytes += chunk.length; // get current chunk size
      var progress = (uploadedBytes / fileBytes) * 100;
      response.write("progress: " + parseInt(progress, 10) + "%\n");
    }
  })
  request.pipe(newFile); // save uploaded file to disk as it is uploaded
}).listen(8080);
```


## Modules

When requiring a module with a `/`, Node will look in specific directories.

* `require('./module')`: looks for module.js in the current directory
* `require('../module')`: looks in the parent directory
* `require('/absolute/path/to/dir')` will look the a specific directory

When requiring with only the module name (`require('module')`), Node will look
into the `node_modules` directory, then look for a `node_modules` in current
current directory's parent, etc., until root directory.

Each folder in the `node_modules` directory in considered a package.

### Creating a module:

```javascript
// hello.js
var hello = function() {
  console.log("hello!");
}
module.exports = hello;

// goodbye.js
exports.goodbye = function() {
  console.log("bye!");
}
```

### Requiring modules:

```javascript
// app.js
var http = require("http");
var fs = require("fs");
var hello = require("./hello"); // our custom module
var gb = require("./goodbye");

hello(); // Using our module
gb.goodbye();
```

### Public/private methods in modules

```javascript
// module.js
var foo = function() { /* /// */ };
var bar = function() { /* /// */ };
var baz = function() { /* /// */ }; // private method

// public methods:
exports.foo = foo;
exports.bar = bar;

// app.js
var module = require('./module');

module.foo();
module.bar();
module.baz(); // Nope
```

### NPM

[NPM](http://npmjs.org) is the package manager for Node. It'a command-line 
utility that comes with Node, uses a module repository, handles dependency 
management.

* `npm install {package}`: install a package
* `npm install -g {package}`: globally install a package with executables
* `npm search request`: search for a package

The `package.json` files contains information about the an application or a
package like its name, version and dependencies (and their version numbers).

```javascript
{
  "name": "My Package",
  "version": 1,
  "dependencies": {
    "connect": "1.8.7"
  }
}
```

* `npm install` will look through `package.json` and install missing 
dependencies to the `node_modules` directory
* `npm install --save {package}` will install and add the dependency to the
`package.json` file

NPM uses semantic versionning: MAJOR.MINOR.PATCH
* `"package": "~1"`: will fetch most recent version >= 1.0.0 & < 2.0.0
* `"package": "~1.8"`: >= 1.8.0 & < 1.9.0
* `"package": "~1.8.7"`: >= 1.8.8 & < 1.9.0


## Express

Node is very low-level. The `express` package is a web framework.

* `npm install --save express`

### Basic web page example

```javascript
var express = require('express');

var app = express(); // creating an express app

// Creating an endpoint for root route
app.get('/', function(request, response) {
  response.sendFile(__dirname + "/index.html"); // read and sends a file from current dir.
});
app.listen(8080); // Listen on port 8080
```

### A More complex example

```javascript
var app = require('express')();
var request = require('request');
var url = require('url');

// Creating an dynamic route
app.get('/tweets/:username', function(req, response) {
  
  // Get username from url params
  var username = req.params.username; // Get param from url
  
  // Get tweets from Twitter API
  var options = {
    protocol: 'http:',
    host: 'api.twitter.com',
    pathname: '/1/statuses/user_timeline.json',
    query: { screen_name: user, count: 10 }
  }  
  var twitterUrl = url.format(options);
  
  // Make the request and pipe the result to the response.
  request(twitterUrl).pipe(response);
});
```

### Templating

* `npm install ejs`

```javascript
// app.js
// ...require...
app.get('/tweets/:username', function(req, response) {

  // ...build API URL...

  request(url, function(err, res, body) {
    var tweets = JSON.parse(body);
    response.locals = { tweets: tweets, name: username };
    response.render('tweets.js');
  });
});
```

```html
<h1>Tweets for @<%= name %></h1>
<% tweets.forEach(function(tweet)) { %>
  <li><%= tweet.text %></li>
<% }); %>
```


## Socket.io

Socket.io helps building real-time web applications.

* `npm install --save socket.io`

### Basic chat application

```javascript
var express = require('express');
var app = express();
var server = require('http').createServer(app);
var io = require('socket.io')(server);

io.on('connection', function(client) {
  console.log('Client connected!');
  
  // User joining
  client.on('join', function(name) {
    client.name = name;
    console.log(name + " joined the chat.");
  })
  
  // Receiving message from client
  client.on('messages', function(data) {
    var message = client.name + ": "+ data;
    client.broadcast.emit('messages', message); // Sending to all clients
    client.emit('messages', message); // Sending back to sender
  });
});

app.get('/', function(req, res) {
  res.sendFile(__dirname + '/index.html');
});

server.listen(8080);
```

```html
  <script src="/socket.io/socket.io.js"></script>
  <script>
    var server = io.connect('http://localhost:8080');
    
    // When connected to server
    server.on('connect', function() {
      var nickname = prompt("What's your name?");
      server.emit('join', nickname);
    });
    
    // Receiving message from server
    server.on('messages', function(data) {
      insertMessage(data); // insert into the DOM
    });
    
    // Sending message to server
    $('#chat_form').on('submit', function() {
      var message = $('#chat_input').val();
      
      server.emit('messages', message);
    })
    
  </script>
```


## Persisting data

## Memory

In our chat example, messages could be stored in memory.

```javascript
var messages = [];
var storeMessage = function(name, data) {
  messages.push({ name: name, data: data });
  if (messages.length > 10) {
    messages.shift(); // Keep no more than 10 messages
  }
};

// (...)

// Store messages as they arrive
client.on('messages', function(data) {
  var message = client.name + ": "+ data;
  client.broadcast.emit('messages', message); // Sending to all clients
  client.emit('messages', message); // Sending back to sender
  storeMessage(client.name, data);
});

// Replay saved messages when client joins
client.on('join', function() {
  messages.forEach(function(message) {
    client.emit("messages", message.name + ": " + message.data);
  });
});
```

But they would disappear as soon as the server restarts.

### Redis

* `npm install --save redis`

```javascript
var redis = require('redis');
var client = redis.createClient();

// Set simple key value pair
client.set("message1", "Hello, this is dog.");
client.set("message2", "Hello, this is spider.");

// Get value from key (async)
client.get("message1", function(err, reply) {
  console.log(reply); // => Hello, this is dog.
});

// Add a string to the messages list
var message = "Hello, this is dog.";
client.lpush("messages", message, function(err, reply) {
  console.log(reply); // Optional callback that returns the list length
  client.ltrim("messages", 0, 1); // Keep the first two strings only
});

// Get strings from the messages list
// 0 to -1 means all in list
client.lrange("messages", 0, -1, function(err, messages) {
  console.log(messages);
});
```

Our `storeMessage` function with redis:

```javascript
var redisClient = redis.createClient();
var storeMessage = function(name, data) {
  var message = JSON.stringify({ name: name, data: data });
  redisClient.lpush("messages", message, function(err, response) {
    redisClient.ltrim("messages", 0, 9); // Keep only last 10 messages
  });
};
```

And sending all messages on connection:

```javascript
client.on('join', function(name) {
  redisClient.lrange("messages", 0, 1, function(err, messages) {
    messages.reverse(); // so they're emitted in the corrected order
    messages.forEach(function(message) {
      message = JSON.parse(message);
      client.emit("messages", message.name + ": " message.data);
    });
  });
});
```

Users could be store as sets, a list of unique data.

```javascript
// Adding to set
client.sadd("names", "Dog");
client.sadd("names", "Spider");

// Remove from set
client.srem("names", "Spider");

// Get all members in set
client.smembers("names", function(err, names) {
  console.log(names);
});
```
