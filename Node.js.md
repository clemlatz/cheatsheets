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

NPM uses semantic versionning: MAJOR.MINOR.PATCH
* `"package": "~1"`: will fetch most recent version >= 1.0.0 & < 2.0.0
* `"package": "~1.8"`: >= 1.8.0 & < 1.9.0
* `"package": "~1.8.7"`: >= 1.8.8 & < 1.9.0
