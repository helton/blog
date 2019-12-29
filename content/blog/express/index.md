---
title: Express
date: "2019-12-29T13:01:00.000Z"
description: "Bringing Node.js to the next level"
tags: ["express", "javascript", "library", "node"]
---

ExpressJS is a web application framework for NodeJS. It's minimal and flexible, great for building Web APIs.

Popular services are built on it, like *MySpace* and *Ghost*.

It's also the foundation for other tools and frameworks, like *Kraken* and *Sails*.

## Installing (via yarn)

`yarn add express`


## Installing (via npm)

`npm install express --save`

## Getting Started

Let's write our first Expression app.

I'm gonna use ES6 features here because it's cool (and because I want to). To try it by yourself make sure you use [Babel](https://babeljs.io/) or anything that can transpile your code to ES5.

First, the boilerplate: importing stuff:

```javascript
const express = require('express');
const app = express();
```

Well, that's enough to get an `app` instance. Now let's define our single route. With that our server will be able to answer `http://localhost:<port>`

```javascript
app.get('/', (req, res) => { 
    res.send('Hello, World');
});
```

The `get` method provide a callback function, where we can access the **request** (`req`) and the **response** (`res`) parameters. These parameters inherit from [http.IncomingMessage](https://nodejs.org/api/http.html#http_class_http_incomingmessage) and [http.ServerResponse](https://nodejs.org/api/http.html#http_class_http_serverresponse) Node classes respectively, so we're able to access their methods as well. For instance, we can do:

```javascript
app.get('/', (req, res) => {
    res.write("Hello, it's me again");
    res.end();
});
```

Is that so? Not quite, we still need to tell it to listen some port:

```javascript
app.listen(3000);
```

What if I want to get some information to know if the server is really running? Well, you can attach a callback function when calling `listen`:

```javascript
const server = app.listen(3000, () => console.log('Server running at http://localhost:' + server.address().port));
```

Now our app is ready. Let's put it to run:

`$ node app.js`

You should see the following output:

`Server running at http://localhost:3000`

I didn't get it. What did we do exactly?

## Middleware

Well, we defined a custom [middleware](http://expressjs.com/en/guide/writing-middleware.html). They're functions executed **sequentially** that access the **request**, **response** and the next **middleware**, being responsable to handle the response (or pass it to the other *middleware*). They are stacked into each other.

Inside each middleware we can do things like:

- Validation
- Authentication
- Data Parsing

and so on...

When a request comes in, it passes through each middleware before reaching the routes, like a plumbing pipe.

Calling `next()` the middlware passes the control to the next middlware available in the stack to handle the request. Don't ~~ever ever~~ try to call `next()` after your response is completed (it'll cause **errors**, believe me).

![Middleware Stack](./middleware-stack.png)

For a more detailed explanation, take a look at: [Understanding expressjs middleware with a visual example](http://javascript.tutorialhorizon.com/2014/09/19/understanding-expressjs-middleware-with-a-visual-example/)

I also borrowed the image below from the Express site itself. It helps to explain each middleware piece.

![Express Middleware](./express-middleware-stack.png)

So, you need a JSON as a result, right? Easy, just send a JavaScript object as response:

```javascript
app.get('/blocks-as-json', (req, res) => {
  let blocks = ['Fixed', 'Movable', 'Rotating'];
  res.send(blocks);
});
```

The `send` function converts Object and Arrays to JSON.

Did I hear HTML? All right, `send` function can handle it as well (magic!):

```javascript
app.get('/blocks-as-html', (req, res) => {
  let blocks = ['Fixed', 'Movable', 'Rotating'];
  let html = '<ul>';
  blocks.forEach(block => html += `<li>${block}</li>`);
  html += '</ul>';
  res.send(html);
});
```

Of course, **this is just an example**. If you really need to do **server-side rendering** go for [ESJ](http://www.embeddedjs.com/), [Pug](https://pugjs.org/api/getting-started.html) (formerly *Jade*), [Handlebars](http://handlebarsjs.com/) ou any shinny template engine you like.

## Redirecting

To redirecto to a relative path, use the `redirect` method available on `response` object:

```javascript
app.get('/i-will-not-be-here-for-a-while', (req, res) => {
  res.redirect('/');
});
```

By default it'll respond with a **HTTP 302** status code (Moved Temporarly). In order to change it (to **HTTP 301** [Moved Permanently], for instance), pass it as the first parameter:

```javascript
app.get('/i-am-definitely-not-here', (req, res) => {
  res.redirect(301, '/');
});
```

## Send files

Suppose that we have a **index.html** file in a public folder that we want to serve at `/home`. How can we do it with Express? Piece of cake:

```javascript
app.get('/home', (req, res) => {
  res.sendFile(__dirname + '/public/index.html');
});
```

Pay attention to the `__dirname`. It's necessary to correctly locate our **index.html** based on the current path. We could've use `./public/index.html` instead as well.

## Send files with express.static

We can also use the `express.static` middleware (the only **middleware** that's current shipped with Express) to do the job:

```javascript
app.use('/home', express.static(__dirname + '/public/index.html'));
```

What if I have a bunch of files in the public direcetory? You don't expect me to reference one-by-one, don't you?
Of course not. Just tell `express.static` to **serves everything** under the specified folder:

```javascript
app.use('/home', express.static('public'));
```
