---
layout: post
title: ExpressJs
date: 2023-02-25
categories: [Backend, ExpressJs]
---

# Express.js 

Express.js is a popular web application framework for Node.js, which allows developers to build server-side web applications quickly and efficiently. It provides a range of features and functionality for handling HTTP requests, routing, middleware, templating, and much more.

## Key Concepts and Features

- `Routing`: Express provides a simple and flexible routing system for mapping HTTP requests to appropriate handlers.
- `Middleware`: Express middleware functions can be used to modify or intercept incoming HTTP requests or outgoing responses.
- `Template engines`: Express supports a variety of template engines such as EJS, Handlebars, Pug, etc.
- `Error handling`: Express provides error handling mechanisms to catch and handle errors that occur during request processing.
- `Static files`: Express can serve static files such as images, CSS, and JavaScript files, by using the built-in `express.static` middleware function.
- `Integration with other Node.js modules`: Express can be easily integrated with other Node.js modules such as `body-parser, cookie-parser, and session middleware.`

## Getting Started

To get started with Express.js, you first need to install it using Node Package Manager (npm) by running the command `npm install express`. After installation, you can create a new Express app using the `express` command and start building your application.

## Example Express.js Server

This example creates an instance of the Express application by calling `express()`, defines a route for handling HTTP `GET` requests to the root URL (`/`), and starts the server listening on port 3000.

The `app.get()` method is used to `define a route for handling GET requests` to the specified URL path (`'/'` in this case). The route handler function takes two parameters: the request object (`req`) and the response object (`res`). In this example, the route handler simply sends the string `'Hello World!'` as the response using the `res.send()` method.

Here's the code for the example server:

```javascript
const express = require('express');
const app = express();

app.get('/', (req, res) => {
  res.send('Hello World!');
});

app.listen(3000, () => {
  console.log('Server started on port 3000');
});
```

## Express.js Request Object


In an Express.js application, the Request object is passed to the route handler function as the first argument. Here are some of the most commonly used properties and methods of the Request object:

- `req.method`: the HTTP method used for the request `('GET', 'POST', 'PUT', 'DELETE', etc.)`
- `req.url`: the URL path for the request `('/users/123', '/login', etc.)`
- `req.params`: an object containing the URL parameters extracted from the URL path `({id: '123'} for the URL '/users/123')`
- `req.query`: an object containing the URL query parameters `({search: 'foo'} for the URL '/search?search=foo')`
- `req.headers`: an object containing the HTTP headers sent with the request `({host: 'example.com', accept: 'application/json'})`
- `req.body`: an object containing the HTTP request body (if the request has a body, `for a POST or PUT request)`
- `req.cookies`: an object containing the `cookies` sent with the request
- `req.ip`: the IP address of the client that sent the request
- `req.protocol`: the protocol used for the request `('http' or 'https')`

In addition to these properties, the Request object also has several methods for working with the request data, such as `req.get()` for `retrieving a specific HTTP header`, `req.is()` for `checking the request Content-Type header`, and `req.accepts()` for checking the `supported MIME types for the response.`

## Express.js Response Object

In an Express.js application, the Response object is used to send the response back to the client. Here are some of the most commonly used methods and properties of the Response object:

- `res.send()`: sends a response of various types, such as a string, HTML, JSON, or an array of bytes.
```js
 res.send('Hello, World!');
```
- `res.json()`: sends a JSON response.
```js
res.json({name: 'Abdo', age: 21});
```
- `res.sendFile()`: sends a file as an octet stream.
```js 
res.sendFile('/path/to/file');
```
- `res.render()`: renders a view template with a given set of data.
```js
res.render('index', {title: 'Home', message: 'Welcome to my website!'});
```
- `res.redirect()`: redirects the client to a different URL It sets the Location header to the new URL and sets the status code to `302` by default.
```js
res.redirect('/users');
```
- `res.status()`: sets the HTTP status code for the response.
```js
res.status(404).send('Page not found');
```
- `res.cookie()`: sets a cookie in the response It takes a name, value, and options object as arguments.
```js
res.cookie('username', '0xAbdoAli', {maxAge: 3600000});
```
- `res.clearCookie()`: clears a cookie from the response It takes the name of the cookie as an argument.
```js
res.clearCookie('username');
```
- `res.header()`: sets an individual header for the response It takes a name and value as arguments.
```js
res.header('X-Powered-By', 'Express');
```
- `res.set()`: sets multiple headers for the response It takes an object containing header names and values as an argument.
```js
res.set({ 'Content-Type': 'text/plain', 'X-Powered-By': 'Express' });
```
- `res.type()`: sets the MIME type for the response.It takes a MIME type string as an argument.
```js
res.type('application/json');
```
- `res.format()`: sends a response based on the "Accept" HTTP header.It takes an object containing response format handlers as arguments.
```js
res.format({
  'text/plain': function() {
    res.send('Hello, World!');
  },
  'application/json': function() {
    res.json({name: 'Abdo', age: 21});
  }
});
```
- `res.attachment()`: sends a file as an attachment.It sets the Content-Disposition header to attachment, which prompts the user to download the file.
```js
res.attachment('/path/to/file');
```
- `res.end()`: ends the response process.
- `res.append()`: method is used to append a specified value to the HTTP response header. This method allows you to add multiple headers with the same name.
example 
``` js
app.get('/', function(req, res) {
  res.append('Cache-Control', 'public, max-age=3600');
  res.append('Vary', 'Accept-Encoding');
});
````

The resulting HTTP response headers would look something like this:

``` http
HTTP/1.1 200 OK
Cache-Control: public, max-age=3600
Vary: Accept-Encoding
Content-Type: text/html; charset=utf-8
Content-Length: 12
Date: Sat, 25 Feb 2023 09:00:00 GMT
Connection: keep-alive
```
- `res.set()` method is used to set response headers for an HTTP response. 
```js 
res.set({
  'Content-Type': 'application/json',
  'Cache-Control': 'no-cache'
});
```
## Express.js GET Request

In Express.js, GET and POST both are two common HTTP requests used for building REST API's. GET requests are used to send only limited amount of data because data is sent into header while POST requests are used to send large amount of data because data is sent in the body Post method is secure because data is not visible in URL bar but it is not used as popularly as GET method

### This example demonstrates how to handle a GET request in Express.js.

```html
<!-- index.html -->
<html>
  <body>
    <form action="/process_get" method="GET">
      First Name: <input type="text" name="first_name"><br>
      Last Name: <input type="text" name="last_name">
      <input type="submit" value="Submit">
    </form>
  </body>
</html>
```
```js
// get_example1.js
const express = require('express');
const app = express();

app.use(express.static('public'));

app.get('/', (req, res) => {
  res.sendFile(__dirname + '/index.html');
});

app.get('/process_get', (req, res) => {
  const response = {
    first_name: req.query.first_name,
    last_name: req.query.last_name,
  };
  
  console.log(response);
  res.json(response);
});

const server = app.listen(8000, () => {
  const { address, port } = server.address();
  console.log(`Example app listening at http://${address}:${port}`);
});
```
The `HTML` file contains a form with two input fields and a submit button. When the form is submitted, a GET request is sent to the server at the `/process_get` endpoint.
In `get_example1.js`, we use the `express.static` middleware to serve static files and define a route for the root URL to send the `index.html` file to the client.
We also define a route for the `/process_get` endpoint to handle the GET request sent from the form submission. Using the `req.query` object, we retrieve the values of the `first_name` and `last_name` parameters from the URL query string. We then send a response object back to the client using the res.json method, which sets the appropriate headers and sends the response in JSON format.

## Express.js Cookies Management

In Express.js, cookies can be managed using the `cookie-parser` middleware. This middleware is responsible for parsing and setting cookies in the HTTP response headers.

To use the `cookie-parser` middleware, first, we need to install it using NPM
```js
npm install cookie-parser
```
Then, we can require the `cookie-parser` module and use it in our Express.js application:
```js
const express = require('express');
const cookieParser = require('cookie-parser');

const app = express();

app.use(cookieParser());
```
After we add the middleware, we can use the `res.cookie()` method to set a cookie in the HTTP response header:
```js
app.get('/setcookie', (req, res) => {
  res.cookie('user_name', '0xAbdoAli');
  res.send('Cookie has been set');
});
```
In the above example, we set a cookie with the name "cookie_name" and value "cookie_value".

We can also set additional options for the cookie, such as its expiration date, path, and domain:
```js
app.get('/setcookie', (req, res) => {
  res.cookie('user_name', '0xAbdoAli', { 
    maxAge: 900000, 
    httpOnly: true,
    secure: true 
  });
  res.send('Cookie has been set');
});
```
`In the above example, we set a cookie with a maximum age of 900,000 milliseconds (15 minutes), and with the httpOnly and secure flags set. The httpOnly flag tells the browser to only allow the cookie to be accessed via HTTP requests, preventing it from being accessed by scripts. The secure flag tells the browser to only send the cookie over HTTPS connections.`

To retrieve a cookie, we can use the `req.cookies` object:

```js
app.get('/getcookie', (req, res) => {
  const cookieValue = req.cookies.user_name;
  res.send(`The value of user_name is ${cookieValue}`);
});
```
To delete a cookie, you can set its value to an empty string and set its `max age to 0`, which will tell the browser to delete the cookie. Here's an example using the `res.clearCookie()` method:
```js
app.get('/delete-cookie', (req, res) => {
  res.clearCookie('user_name').send('Cookie deleted');
});
```
## Express.js Middleware

Middleware in Express.js refers to functions that are executed in the request-response cycle between the client and the server. Middleware functions have access to the `request` and `response` objects and can perform actions on them, modify them, or terminate the request-response cycle.

Middleware functions are executed in a sequential manner, and each middleware function has the ability to pass control to the next middleware function in the chain using the `next()` function. Middleware functions can also terminate the request-response cycle by sending a response to the client or by calling the `next()` function with an error parameter.

Here is an example of a middleware function that logs the incoming requests:
```js
const express = require('express');
const app = express();

// Define a middleware function
const logger = (req, res, next) => {
  console.log(`[${new Date().toISOString()}] ${req.method} ${req.url}`);
  next();
};

// Use the middleware function
app.use(logger);

// Define a route
app.get('/', (req, res) => {
  res.send('Hello World!');
});

// Start the server
app.listen(3000, () => {
  console.log('Server listening on port 3000');
});
```
In this example, we defined a middleware function `logger` that logs the incoming requests with the current timestamp, HTTP method, and URL. We then used the `app.use()` method to apply the logger middleware to all routes.


### Using Middleware

There are two ways of applying middleware:

#### Application/route level
Applies the middleware to an entire application or the entirety of a route on either the entry point application object, or to specific routes (view working with routes).

```ts
.use();
```
The `.use();` method is a method that can be applied to the application object or to route objects. It is used for applying middleware and can take in a route, and middleware as arguments

```ts
app.use(middleware);
```

### Endpoint level
Applies middleware to a specific endpoint.

```ts
students.get('/', middleware, (req, res) => { // do stuff });
```
### Applying Multiple Middleware

It's possible to apply multiple middlewares to an application/route our endpoint.

Using an Array

```ts
const middleware = [cors, logger];

app.use(middleware); // app level
students.get('/', middleware, (req, res) => { // do stuff }); // endpoint level
```

Listing Middleware

```ts
app.use(cors(), logger); // app level 
students.get('/', cors(), logger, (req, res) => { // do stuff }); // endpoint level
```

## router

### router.route()

the `router.route()` method is used to create a chainable route handler for a specific route path. It allows you to define multiple HTTP methods (such as GET, POST, PUT, DELETE) for a single route path in a concise manner.

Here's an example that demonstrates how to use `router.route()`:
```js
const express = require('express');
const router = express.Router();

// Define the route handlers using router.route()
router.route('/api/users')
  .get((req, res) => {
    // Handle GET request for /api/users
    res.send('Get users');
  })
  .post((req, res) => {
    // Handle POST request for /api/users
    res.send('Create user');
  })
  .put((req, res) => {
    // Handle PUT request for /api/users
    res.send('Update user');
  })
  .delete((req, res) => {
    // Handle DELETE request for /api/users
    res.send('Delete user');
  });

// Use the router in your application
app.use(router);
```
In the above example, the `router.route('/api/users')` creates a route handler for the `'/api/users'` path. The subsequent HTTP methods (get, post, put, delete) are chained to the route handler using dot notation.
