# Node.js

- [Node.js](#nodejs)
  - [Basics](#basics)
    - [Definition](#definition)
    - [Core modules](#core-modules)
    - [Request](#request)
    - [Response](#response)
    - [Routing](#routing)
    - [Node.js streams](#nodejs-streams)
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
    - [Definition](#definition-1)
    - [Implementation](#implementation)
    - [Password encryption](#password-encryption)
    - [Protecting routes](#protecting-routes)
    - [CSRF Attacks](#csrf-attacks)
    - [CSRF token](#csrf-token)
  - [REST API](#rest-api)
    - [Routes](#routes)
    - [REST principles](#rest-principles)
    - [CORS](#cors)
    - [Authentication](#authentication-1)
  - [Websockets](#websockets)
    - [Definition](#definition-2)
  - [Links & references](#links--references)
    - [Full course](#full-course)
    - [Streams](#streams)
    - [Event Loop](#event-loop)

## Basics

### Definition

Node.js is a JavaScript runtime. It allows to run JavaScript on the server (or anywhere else from the browser).

Node.js uses V8, which is the JavaScript engine built by Google. A JS engine takes JS code and compiles it to machine code. Node.js takes V8 and adds some extra features, like working with your local filesystem (opening files, reading files...).

Of course, because Node.js doesn't run on a web browser, JS features linked to the browser (like DOM manipulation) don't exist in Node.js.

### Core modules

Node.js comes with a bunch of core modules allowing the developers to handle basic features of Node.js such as HTTP (`http` module), HTTPS (`https` module), path (`path` module), filesystem (`fs` module), operating system features (`os` module)...

```javascript
const http = require('./http'); // import the http code module

const server = http.createServer((req, res) => console.log(req));
server.listen(3000);
```

### Request

In Node.js, requests are represented through an object. This object contains the HTTP headers, the URL, the HTTP method used by the client...

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
  if (url === '/') {
    // Handling the default route
    res.write('<html>');
    res.write('<head><title>Enter Message</title><head>');
    res.write(
      '<body><form action="/message" method="POST"><input type="text" name="message"><button type="submit">Send</button></form></body>'
    );
    res.write('</html>');
    return res.end(); // here we return to be sure that nothing will be written in the response anymore.
  }
  if (url === '/message' && method === 'POST') {
    // Handling another route
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

### Node.js streams

In Node.js, data from network or file are put in a special data structure called **streams** which are widely used in Node.js. There are mainly used for reading/writing files and network communications.

Streams are used to avoid the issue to have a file (or a big amount of data) in memory all at once. Streams read chunks of data, piece by piece, processing its content without keeping it all in memory. Streams allow to process data as soon has we have it, we don't have to wait for the whole data to be received to start processing it. These are the two main advantages of streams.

Node.js has 4 types of stream:

- `Writable`: streams to which we can write data (for example to write a file on the filesystem).
- `Readable`: streams from which we can read data (for example to read a file from the filesystem).
- `Duplex`: streams that are both Readable and Writable.
- `Transform`: streams that can modify the data as it is written or read.

You already encountered two of these stream types. `Request` is a `Readable` stream and `Response` is a `Writable` stream.

Internally, streams are working on buffers. A **buffer** is a temporary memory that a stream takes to hold some data until it is consumed. In Node.js, by default, a buffer memory works on `String` and `Buffer` (it is possible to work with object with the `objectMode` property). When pushing data into a stream, the data will be pushed into the stream buffer. It this buffer is full, the push will return false.

### Parsing request bodies

Incoming data are handled by Node.js with streams. To listen to a stream, we will use an event listener on the request (because Node.js streams extend the `EventEmitter` class). This listener is available through the `on('data', ())` function. In order to know when we have received all the data, we will use the `on('end', ())` listener.

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
    req.on('data', (chunk) => {
      console.log(chunk);
      body.push(chunk);
    });
    req.on('end', () => {
      // we know that the incoming data will be text with the following format: "message=yourText"
      const parsedBody = Buffer.concat(body).toString();
      // we only need the text after the '='.
      const message = parsedBody.split('=')[1];
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

:warning: Be careful of the execution order of events. In the example above, the callback function of an `on()` event will be executed when the associated event occurs, meaning that the response could be send before the `message.txt` will be created.

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
    req.on('data', (chunk) => {
      console.log(chunk);
      body.push(chunk);
    });
    return req.on('end', () => {
      const parsedBody = Buffer.concat(body).toString();
      const message = parsedBody.split('=')[1];
      fs.writeFile('message.txt', message, (err) => {
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

Node.js is single threaded but has some mechanisms to handle multiple incoming requests, blocking and long running tasks. One major element is the **Event Loop**. The event loop is started by Node.js and will handle callback functions called when events occur. The event loop will only handle fast callback functions. The event loop runs on a single thread and it runs until there is no more work to do (resulting in the program exiting).

In the real world it is difficult to support all the different types of I/O (file I/O, network I/O,DNS, etc.). Some I/O can be performed using native hardware implementation and then be completely asynchronous (network I/O use [epoll](https://en.wikipedia.org/wiki/Epoll), [kqueue](https://en.wikipedia.org/wiki/Kqueue), [IOCP](https://en.wikipedia.org/wiki/Input/output_completion_port) and a few others). But certain I/O types can't be performed using these implementations (file I/O for example), so they can't be completely asynchronous. To solve that, a thread pool has been introduced to handle these complex tasks.

This **thread pool**, also known as **worker pool**, is totally detached of your JavaScript code and, by default, will contain 4 threads. This pool is responsible for doing all the heavy lifting. There is one connection between the event loop and the thread pool, when a worker is done, it will trigger the callback of the operation it just did. For example, the worker will read a file and when it's done, it will trigger the callback of the read event.

> Keep in mind that Node.js doesn't perform all the I/O in the thread pool.

In order to handle this entire complex process, Node.js uses [libuv](https://libuv.org/), a library initially developed for Node.js with a focus on asynchronous I/O.

#### Event loop phases

The event loop will, at each iteration, run through a series of different phases (four phases actually). Each phase has a FIFO (First In First Out) queue of callbacks (an event queue) to execute. When a phase is finished, the event loop will check two intermediate queues (process.nextTick callbacks and Microtasks aka Promise callbacks) for any available items.

1. During the first phase, the event loop will check if there are any timer (from `setTimeout` and `setInterval` functions) callbacks it should execute.

2. Then, the event loop will check for I/O callbacks. Then, it will enter a poll phase. That means it will look for new I/O events and execute the callbacks immediately if possible. If that's not possible, it will defer the execution and register this as a pending callback.

3. Then, the event loop will check and execute `setImmediate()` callbacks.

4. Node.js will then execute all close event callbacks (`on('close', ...)`).

Finally, if there are no more callbacks to run, Node.js will exit the loop. Otherwise, the loop will run a new iteration.

![Node.js event loop diagram](https://miro.medium.com/max/2000/1*2yXbhvpf1kj5YT-m_fXgEQ.png)

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
  someText: 'Some hard coded text',
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

Express.js uses the concept of middleware a lot. Basically, a middleware is a piece of code, a plugin, that will take the incoming request, do something with it and then pass it to another middleware, etc. This model allows Express.js to be very pluggable by adding third party packages (middlewares) to add some extra functionalities.

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
  res.send('<h1>Hello from Express!</h1>');
  // we are not calling next() because this is the last middleware in line
});

const server = http.createServer(app);

server.listen(3000);
```

## Session & cookies

### Cookies

A cookie is piece of information send by a server and stored in the client browser. This cookie can then be send by the client within an HTTP request to the same server. This allows the server to know if two requests come from the same client. It is mostly used for data tracking, sessions (shopping cart, game score, logins...) or custom configuration (user preferences, themes...).

To create a cookie, the server needs to set the HTTP header 'Set-Cookie' with a key-value pair. The client will store this value, and then the server should look at the 'Cookie' header to check if the cookie is present.

```javascript
exports.getLogin = (req, res, next) => {
  const isLoggedIn = req.get('Cookie').split(';')[1].trim().split('=')[1];
  res.render('auth/login', {
    path: '/login',
    pageTitle: 'Login',
    isAuthenticated: isLoggedIn,
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

### Definition

In the web world, you don't want any client to see everything your frontend proposes. You would want some clients to have a personal space (for a shopping site for example), and special clients such as administrators (could be the site developers) to have access to special pages on our website, etc. Each client should be able to authenticated with an email and a password for example.

### Implementation

A client should first sign up and login. When logging in, a login request is send to the server. The server will create a session so, when the same client will make another request, the server will know that this client is already logged in.

The server will check if the credentials are valid and if so, responds to the request with a 200 HTTP code and a cookie that will be send again for each client requests so that the server will know that the client is authenticated.

### Password encryption

Most of the JavaScript application uses the [bcryptjs](https://github.com/kelektiv/node.bcrypt.js#readme) package to handle password encryption and comparison (to check credentials). Bcrypt is a password hashing function available in different other languages.

### Protecting routes

To protect routes, we can use a middleware that will check if the user is logged in using the session.

```javascript
// Example of authentication middleware
module.exports = (req, res, next) => {
  if (!req.session.isLoggedIn) {
    return res.redirect('/login');
  }
  next();
};
```

### CSRF Attacks

CSRF (Cross-Site Request Forgery) attacks consist in sending a client on a "fake" web site that looks like yours (with an email link for example). This fake web site will send data to your server, but the data will be changed. For example, if you have a banking website, instead of sending money to a person A (which was the client intended action), it will send money to a person B. This works because the client credentials and session are valid, we could say that the client session has been stolen in some way.

To protect against that, you want to be sure that the client will only access to rendered views that we created. So, one solution would be to use a token. This token will be generated by the server on every rendered pages, and the client will have to send the token along with each requests. This token would be almost impossible for an attacker to fake.

### CSRF token

Using Express.js, there is a npm package called [csurf](https://github.com/expressjs/csurf) that will help to handle these tokens. It provides a middleware to use after the session middleware (because csurf will use the session).

## REST API

REST (REpresentational State Transfer) is a way of building APIs. Node.js can be used to build servers that will serve or render their own user interface (using [EJS](https://ejs.co/) for example). Nowadays, it is most common to avoid reloading entire web pages, but instead we try to get data from servers in order to update part of web pages or even having only one page (something we called SPA for Single Page Application). Getting data instead of full HTML pages, allows to decouple frontend and backend. That is why we need REST API, but keep in mind that the server logic (validation, authentication, databases...) stays the same between servers using REST and those who don't.

### Routes

With REST API, routes are still a combination of a path and an HTTP method. Routes are also called **API endpoints**. These HTTP methods are and should be used for particular process.

- GET: get a resource from the server.
- POST: post a resource to the server (i.e. create or append a resource).
- PUT: put a resource onto the server (i.e. create or overwrite a resource).
- PATCH: update parts of an existing resource on the server.
- DELETE: delete a resource on the server.
- OPTIONS: determine whether follow-up request is allowed (sent automatically)

Of course you can do whatever you want with these methods since you will develop the logic that will handle the routes, **BUT** it's good practice to use the appropriate HTTP methods for your logic.

### REST principles

- Uniform interface: Clearly define the API endpoints with defined requests and response data structures.
- Stateless interactions: Server and the client don't share any connection history, every request is handled separately. There are no session with the client, so the authentication will be handled in another way.
- Cacheable: Servers may set caching headers to allow the client to cache responses.
- Client-server: Server and client are separated, client is not concerned with persistent data storage.
- Layered system: Server may forward requests to other APIs.
- Code on demand: Executable code may be transferred from server to client.

### CORS

[CORS (Cross Origin resource Sharing)](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS) is a common "issue" you might encounter. Basically it happens when your client and your server runs on different domain (hostname + port). For example, if I have a client and a server running on `http://localhost:3000`, which can be the case if I render my HTML through my Node.js server app, then, we are not going to have a CORS error. However, if I develop a REST API, by definition will I have a client and a server running on separate domains (one on `http://localhost:4000` and one on `http://localhost:3000` for example), and have a CORS error.

This error can only be solved in the server, by setting some headers.

- `Access-Control-Allow-Origin`: usually set to `*`.
- `Access-Control-Allow-Methods`: put all the HTTP methods you want to allow, `GET, POST, PUT, PATCH, DELETE` for example.
- `Access-Control-Allow-Headers`: put all the HTTP headers you want to allow, `Content-Type, Authorization` for example.

### Authentication

Because of its nature, REST APIs can't use the same authentication strategy we saw (using session). The server will not store anything about the client.

The input of the user will be still validated as before but the server will not create a session, it will send back a token.

This token will be stored by the client and send alongside each client requests. This token contains JSON data and a signature generated on the server with a certain private key (only stored on the server), we called this type of token a JSON Web Token (JWT). The token signature can only be verified by the server, so it can be edited or faked.

## Websockets

### Definition

Websocket is a protocol used to do real time communication in web. Typical workflow in the web, is the pull pattern. Basically a client sends a request to a server and, after some time, the server will send a response to the client. This workflow is fine and widely used. But what if we want the server to send some data to the client, some changes for example. Well, to do that we are going to use WebSocket.

WebSocket is a protocol that uses HTTP to establish a communication (HTTP handshake), and then will define the way it will communicate. WebSocket uses a push pattern, meaning pushing data from the server to the client. Even more, websocket is a persistent connection between client and server, so actually both parties can send data at any time.

The most known library to handle websocket in JavaScript is [socket.io](https://socket.io/)

## Links & references

### Full course

- https://www.udemy.com/course/nodejs-the-complete-guide/: NodeJS complete course on Udemy (not free)
- https://nodejs.org/en/docs/guides/

### Streams

- https://jscomplete.com/learn/node-beyond-basics/node-streams
- https://medium.com/developers-arena/streams-and-buffers-in-nodejs-30ff53edd50f

### Event Loop

- https://nodejs.org/en/docs/guides/dont-block-the-event-loop/
- https://blog.insiderattack.net/event-loop-and-the-big-picture-nodejs-event-loop-part-1-1cb67a182810
