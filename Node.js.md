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
