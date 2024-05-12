---
title: "Express"
date: 2024-05-12T21:01:06+10:00
draft: true
---

# Express

Express is an unopinionated web framework for `Node.js`. It allows you to build and serve websites. Most of my personal and professional experience with Express has been with it acting as a back-end server.

There are a few main parts of Express. These are `Routing` and `Middleware`.

The hello world example of an Express app is:

```Javascript
const express = require('express')
const app = express()
const port = 3000

app.get('/', (req, res) => {
  res.send('Hello World!')
})

app.listen(port, () => {
  console.log(`Example app listening on port ${port}`)
})
```

## Routing

### Basic Routing

Routing is what happens when a client requests an endpoint.

```Typescript
app.METHOD(PATH, HANDLER)
```

Within the handler there is usually a `Request object` and a `Response object`.

```Typescript
app.get('/', (req, res) => {
  res.send('Hello World!')
})
```

#### Request object

[docs](https://expressjs.com/en/4x/api.html#req)

The request object contains all the query strings, params, HTTP Headers and so one. I'll just list a few common request object properties and methods.

##### req.baseUrl

this returns the base url that the router is mounted on.

```typescript
var greet = express.Router();

greet.get("/jp", function (req, res) {
  console.log(req.baseUrl); // /greet
  res.send("Konichiwa!");
});

app.use("/greet", greet); // load the router on '/greet'
```

##### req.body

this returns the key value pairs of a route's body.

```typescript
var express = require("express");

var app = express();

app.use(express.json()); // for parsing application/json
app.use(express.urlencoded({ extended: true })); // for parsing application/x-www-form-urlencoded

app.post("/profile", function (req, res, next) {
  console.log(req.body);
  res.json(req.body);
});
```

##### req.fresh

returns whether or not the cache is stale. Sending the headers `Cache-Control: no-cache` will reload everything but will set this to be false.

##### req.hostname

returns the hostname of the client. If the `trust proxy` setting evaluates to false, then it'll get the value from `X-Forwarded-Host`.

##### req.method

returns the HTTP method of the request.

##### req.params

returns the params of a route.

```javascript
Request URL: http://localhost:3000/users/34/books/8989
req.params: { "userId": "34", "bookId": "8989" }

// the route
app.get('/users/:userId/books/:bookId', (req, res) => {
  res.send(req.params.userId) // does this work?
})

```

#### Response object

[docs](https://expressjs.com/en/4x/api.html#res)

#### App object

[docs](https://expressjs.com/en/4x/api.html#app)

The App object is just the Express app. It has all the routing and middleware methods as well as template engine and rendering.

```typescript
var express = require("express");
var app = express();

app.get("/", function (req, res) {
  res.send("hello world");
});

app.listen(3000);
```

#### Router object

[docs](https://stackoverflow.com/a/33261362)

## Middleware

Middleware functions are just functions that have access to a `request object` and the `response object` and the next middleware function in an application’s request-response cycle.

When a middleware function is called, it can do some stuff, modify request and response objects, and then either end the request-response cycle or call the **next()** middleware function in the stack.

### Types of Middleware

#### Application-level middleware

These middleware are applied to the `app object` through app.use():

```javascript
const myLogger = function (req, res, next) {
  console.log("LOGGED");
  next();
};

app.use(myLogger);
```

Or app.METHOD() where METHOD can be GET, PUT, POST or any other `HTTP method`.

```jsx
app.get(
  "/user/:id",
  (req, res, next) => {
    // if the user ID is 0, skip to the next route
    if (req.params.id === "0") next("route");
    // otherwise pass the control to the next middleware function in this stack
    else next();
  },
  (req, res, next) => {
    // send a regular response
    res.send("regular");
  }
);
```

A common pattern is to declare middleware in an array for reusability.

```jsx
function logOriginalUrl(req, res, next) {
  console.log("Request URL:", req.originalUrl);
  next();
}

function logMethod(req, res, next) {
  console.log("Request Type:", req.method);
  next();
}

const logStuff = [logOriginalUrl, logMethod];
app.get("/user/:id", logStuff, (req, res, next) => {
  res.send("User Info");
});
```

#### Router-level middleware

#### Error-handling middleware

#### Error Handling

[docs](https://expressjs.com/en/guide/error-handling.html)

#### Express Maintained Middleware
