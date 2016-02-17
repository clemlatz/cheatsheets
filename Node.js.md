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