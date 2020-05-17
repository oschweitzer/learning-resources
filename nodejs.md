# Node.js

- [Node.js](#nodejs)
  - [Basics](#basics)
    - [Definition](#definition)
    - [Core modules](#core-modules)
    - [Request](#request)
    - [Response](#response)
    - [Routing](#routing)
    - [Parsing request bodies](#parsing-request-bodies)
    - [Blocking and non-blocking code](#blocking-and-non-blocking-code)
    - [Behind the scene](#behind-the-scene)
      - [Event loop phases](#event-loop-phases)
    - [Node.js modules](#nodejs-modules)
  - [Express.js](#expressjs)
    - [Middlewares](#middlewares)
  - [Session & cookies](#session--cookies)
    - [Cookies](#cookies)
    - [Session](#session)
  - [Authentication](#authentication)

## Basics

### Definition

Node.js is a JavaScript runtime. It allows to run JavaScript on the server (or anywhere else from the browser).

Node.js uses V8, which is the JavaScript engine built by Google. A JS engine takes JS code and compile it to machine code. Node.js takes V8 and adds some extra features, like working with your local filesystem (opening files, reading files...).

Of course, because Node.js doesn't run on a web browser, JS features linked to the browser (like DOM manipulation) don't exist in Node.js.

### Core modules

Node.js comes with a bunch of core modules allowing the developers to handle basic features of Node.js such as HTTP (`http` module), HTTPS (`https` module), path (`path` module), filesystem (`fs` module), operating system features (`os` module)...

```javascript
const http = require('./http'); // import the http code module

const server = http.createServer((req, res) => console.log(req));
server.listen(3000);
```

### Request

In Node.js requests are represented through an object. This object contains the HTTP headers, the URL, the HTTP method used by the client...

### Response

The server response is also represented through an object. When a request is coming, the response object will not contain anything interesting, because we have to fill data to the response and send it to the client.

In the response, you can set HTTP headers and of course write the response body. 

```javascript
const http = require('./http'); // import the http code module

const server = http.createServer((req, res) => {
    res.setHeaders('Content-Type', 'text/html');
    // We write data in the response...
    res.write('<html>');
    res.write('<head><title>Hello</title></head>');
    res.write('<body>My first Node.js app</body>');
    res.write('</html>');

    res.end(); // after this function call, you can't write data in the response anymore.
});
server.listen(3000);
```

### Routing

**Routing** is the way client requests are handled by the server. A route is a combination of an URL and a HTTP method.

```javascript
const http = require('http');
const fs = require('fs');

const server = http.createServer((req, res) => {
  const url = req.url;
  const method = req.method;
  if (url === '/') { // Handling the default route
    res.write('<html>');
    res.write('<head><title>Enter Message</title><head>');
    res.write('<body><form action="/message" method="POST"><input type="text" name="message"><button type="submit">Send</button></form></body>');
    res.write('</html>');
    return res.end(); // here we return to be sure that nothing will be written in the response anymore.
  }
  if (url === '/message' && method === 'POST') { // Handling another route
    fs.writeFileSync('message.txt', 'DUMMY');
    res.statusCode = 302;
    res.setHeader('Location', '/'); // redirection
    return res.end();
  }
  res.setHeader('Content-Type', 'text/html');
  res.write('<html>');
  res.write('<head><title>My First Page</title><head>');
  res.write('<body><h1>Hello from my Node.js Server!</h1></body>');
  res.write('</html>');
  res.end();
});

server.listen(3000);
```

### Parsing request bodies

Incoming data are put in a special data structure called **streams**. Streams are widely used in Node.js. Incoming request body is split into data chunks. This concept allows the server to start working with data early. We are not directly working with these chunks, but we are using **buffers** that are structure holding multiple chunks.

To listen to a stream, we will use an event listener on the request. This listener is available through the `on('data', ())` function. In order to know when we have received all the data, we will use the `on('end', ())` listener.

```javascript
const http = require('http');
const fs = require('fs');

const server = http.createServer((req, res) => {
  const url = req.url;
  const method = req.method;
  if (url === '/') {
    res.write('<html>');
    res.write('<head><title>Enter Message</title><head>');
    res.write('<body><form action="/message" method="POST"><input type="text" name="message"><button type="submit">Send</button></form></body>');
    res.write('</html>');
    return res.end();
  }
  if (url === '/message' && method === 'POST') {
    const body = [];
    req.on('data', (chunk) => {
      console.log(chunk);
      body.push(chunk);
    });
    req.on('end', () => {
      const parsedBody = Buffer.concat(body).toString(); // we know that the incoming data will be text with the following format: "message=yourText"
      const message = parsedBody.split('=')[1]; // we only need the text after the '='.
      fs.writeFileSync('message.txt', message);
    });
    res.statusCode = 302;
    res.setHeader('Location', '/');
    return res.end();
  }
  res.setHeader('Content-Type', 'text/html');
  res.write('<html>');
  res.write('<head><title>My First Page</title><head>');
  res.write('<body><h1>Hello from my Node.js Server!</h1></body>');
  res.write('</html>');
  res.end();
});

server.listen(3000);
```

:warning: Be careful of the execution order of events. In the example above, the callback function of a `on()` event will be executed when the associated event occurs, meaning that the response could be send before the `message.txt` will be created.

### Blocking and non-blocking code

In its core modules, Node.js provides two versions of many of its functions. For example, to write a file, Node.js provides the `writeFileSync` and the `writeFile` functions. When postfixed with the `sync` word, the function is synchronous, meaning it's a blocking function. It can useful for very little file, but it's recommended to never use the sync versions of these functions. On the other way, functions without the `sync` word are asynchronous, meaning they are non-blocking functions.

```javascript
const http = require('http');
const fs = require('fs');

const server = http.createServer((req, res) => {
  const url = req.url;
  const method = req.method;
  if (url === '/') {
    res.write('<html>');
    res.write('<head><title>Enter Message</title><head>');
    res.write(
      '<body><form action="/message" method="POST"><input type="text" name="message"><button type="submit">Send</button></form></body>'
    );
    res.write('</html>');
    return res.end();
  }
  if (url === '/message' && method === 'POST') {
    const body = [];
    req.on('data', chunk => {
      console.log(chunk);
      body.push(chunk);
    });
    return req.on('end', () => {
      const parsedBody = Buffer.concat(body).toString();
      const message = parsedBody.split('=')[1];
      fs.writeFile('message.txt', message, err => {
        // the following code will be executed after the file is written.
        res.statusCode = 302;
        res.setHeader('Location', '/');
        return res.end();
      });
    });
  }
  res.setHeader('Content-Type', 'text/html');
  res.write('<html>');
  res.write('<head><title>My First Page</title><head>');
  res.write('<body><h1>Hello from my Node.js Server!</h1></body>');
  res.write('</html>');
  res.end();
});

server.listen(3000);
```

### Behind the scene

Node.js is single threaded but has some mechanisms to handle multiple incoming requests and long running tasks. The first element is the **Event Loop**. The event loop is started by Node.js and will handle callback functions of called when events occur. The event loop will only handle fast callback functions.

For long-running tasks, there is a second mechanism called **Worker**. Workers are created in a worker pool. This worker pool is totally detached of your JavaScript code and can run in different threads. It is responsible for doing all the heavy lifting. There is one connection between the event loop and the worker pool, when a worker is done, it will trigger the callback of the operation it just did. For example, the worker will read a file and when it's done, it will trigger the callback of the read event.

#### Event loop phases

The event loop is a loop that will, at the beginning of each new iteration, check if there are any timer (`setTimeout` and `setInterval`) callbacks it should execute. Then, the event loop will check any other open callbacks (I/O operations for example). Then, the event loop will enter a poll phase. That means it will look for new IO events and execute the callbacks immediately if possible. If that's not possible, it will defer the execution and register this as a pending callback. Then, the event loop will check and execute `setImmediate()` callbacks. Then,, Node.js will execute all close event callbacks (`on('close', ...)`).

### Node.js modules

To import modules in Node.js, we already saw the `require(...)` keyword. 

To export code in Node.js, there are a few ways. 

One way is to use the `module.exports` global object or keyword. 

```javascript
// routes.js
const fs = require('fs');

const requestHandler = (req, res) => {
    /* Do some logic here */
};

module.exports = requestHandler;

// We can export multiple things
module.exports = {
     handler: requestHandler,
     someText: 'Some hard coded text'
};

// Without an object
module.exports.handler = requestHandler;
module.exports.someText = 'Some text';

// Same as above, the exports is simply an alias of module.exports
exports.handler = requestHandler;
exports.someText = 'Some hard coded text';
```

Then to import this, use `require()` again, but put a path and not just the module name (like for core modules).

```javascript
const http = require('http');
const routes = require('./routes');

const server = http.createServer(routes.handler);
```

## Express.js

[Express.js](https://expressjs.com/) is a JavaScript library that helps you handling server side code. Developers want to focus on business logic and Express.js helps you by handling the heavy lifting. This framework is widely used for Node.js applications but some other frameworks exist ([Fastify](https://www.fastify.io/), [SailsJs](https://sailsjs.com/) or [Koa](https://koajs.com/) for example).

### Middlewares

Express.js uses the concept of middleware a lot. Basically, a middleware is a piece of code, a plugin, that will take the incoming request, do something with it and then pass it to another middleware, etc. This model allows Express.js to be very pluggable by adding third party packages (middlewares) to add some  extra functionalities.

```javascript
const http = require('http');
const express = require('express');

const app = express();

// To add a middleware, simply call the 'use()' function.
app.use((req, res, next) => {
    console.log('In the middleware!');
    next(); // Allows the request to continue to the next middleware in line
});

app.use((req, res, next) => {
    console.log('In another middleware!');
    res.send('<h1>Hello from Express!</h1>'); // we are not calling next() because this is the last middleware in line
});

const server = http.createServer(app);

server.listen(3000);
```

## Session & cookies

### Cookies

A cookie is piece of information send by a server and stored in the client browser. This cookie can then be send by the client within a HTTP request to the same server. This allows the server to know if two requests come from the same client. It is mostly used for data tracking, sessions (shopping cart, game score, logins...) or custom configuration (user preferences, themes...).

To create a cookie, the server needs to set the HTTP header 'Set-Cookie' with a key-value pair. The client will store this value, and then the server should look at the 'Cookie' header to check if the cookie is present.

```javascript
exports.getLogin = (req, res, next) => {
  const isLoggedIn = req
    .get('Cookie')
    .split(';')[1]
    .trim()
    .split('=')[1];
  res.render('auth/login', {
    path: '/login',
    pageTitle: 'Login',
    isAuthenticated: isLoggedIn
  });
};

exports.postLogin = (req, res, next) => {
  res.setHeader('Set-Cookie', 'loggedIn=true');
  res.redirect('/');
};
```

### Session

A session is a construct allowing to share information between all requests of a particular client. A session is stored on the server side. For a session to work, the client needs to tell the server to which session he belongs. This can be achieved via a session cookie.

With express.js (and the package `express-session`), it is very easy to create a session.

```javascript
/* in the main server file */
app.use(
  session({ secret: 'my secret', resave: false, saveUninitialized: false })
);

/* in a middleware */
req.session.isLoggedIn = true;
```

## Authentication

