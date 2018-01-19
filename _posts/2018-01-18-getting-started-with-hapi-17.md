---
layout: post
title: Getting Started with Hapi 17
category: hapi.js
---

Hapi 17 was recently released, and brings an exciting change — in an effort to drive adoption of async/await, the
entire codebase replaced callbacks with the newer language feature.

This makes for some changes to how you configure your application. The intent of this post is to get you up and
running as quickly as possible.

## Installation

```
npm i --save hapi
```

or,

```
yarn add hapi
```

## Setting up your server

```js
const Hapi = require("hapi");

// define some constants to make life easier
const DEFAULT_HOST = "localhost";
const DEFAULT_PORT = 3000;
const RADIX = 10;
```

You can change the default host and port to your liking, this is just an example of some common defaults.

Major change — no more connections, you define 1 server which equals 1 connection. Only relevant if you used to have (for instance)
an API server and a UI server defined in one place previously (using server.connection API - that's gone now).

Also, Hapi.server is no longer a class-like structure so no more new keyword - just use the Hapi.server method directly:

```js
// define your server
const server = Hapi.server({
  host: process.env.HOST || DEFAULT_HOST, 
  port: parseInt(process.env.PORT, RADIX) || DEFAULT_PORT,
  app: {}
});
```

We use the `HOST` and `PORT` values passed in via an environment variable (common method used in cloud server hosting) and fall
back to our previously defined default host/port if no such environment variable is set.

Note that Hapi expects port to be an integer, so use `parseInt` to make sure the value passed from the environment is correct type.

## Enclosing in an async function

The methods to register plugins and start your server are asynchronous, and you use the await keyword to wait for their return value.
For it to be valid syntactically to use the await keyword, you must be within an async function — so we wrap everything in a main
function, which you can name whatever you like:

```js
const myServer = async () => {
  
};

myServer(); // don't forget to call it
```

We want to make sure to call the function, or nothing will happen — so we add the call up front so we don't forget.

## Error-handling

If Hapi can't start, or can't register a plugin, it throws an error. We should therefore be ready to handle such an error, 
so we wrap in a `try..catch`:

```js
const myServer = async () => {
  try {
  
  } catch (err) {
  
    console.log("Hapi error starting server", err);
  
  }
};

myServer(); // don't forget to call it
```

In your catch block, at the very least you should log something to the console so you have an indication why it failed - 
otherwise it will just silently exit.

## Registering plugins

Inside the `try` block above, we register any plugins we may need.

This could be Hapi official plugins for things like Logging, Static File serving, caching, exception handling, etc.

But also, it’s a good idea to modularize your web application in to the plugin approach too (so, a plugin you define for
everything to do with one piece of functionality you provide, another plugin for a separate piece of functionality, etc).

For now, let’s assume we’re going to be building an API that has 1 route, that returns "Hello World", so we're going to
make that a plugin later in this post.

We will register it like so:

```js
try { // this is the try block from above
  await server.register(api);
} catch (err) { // ...etc
```

In `server.register`, we can pass a single object (1 plugin) or an array of objects (multiple plugins).

And we'll require it at the top of the file:

```js
const Hapi = require("hapi");
const api = require("./api");
```

## Starting your server

```js
try { // this is the try block from above

  await server.register(api);
  
  await server.start();
  
  console.log(`Hapi server running at ${server.info.uri}`);

} catch (err) { // ...etc
```

This starts the server after registering your plugin, then logs out to the console the URI it started up on.

## Sharing properties within your app

The app property on the object you pass to Hapi.server is optional, but is a great place to store variables that
you will use throughout your application.

Whatever is set on that object will be available on every instance of server being passed (think: Plugins, Extension
Points) and every time you have a request object (think: Route Handlers, etc).

For example, if you have `app: { foo: "bar" }` then in your route handler, given:

```js
(request, reply) => {
  console.log(request.server.settings.app.foo); // will log "bar"
}
```

and in a plugin definition:

```js
async (server, options) => {
  console.log(server.settings.app.foo); // logs "bar"
}
```

## Defining a plugin

So to contain your functionality of an API route that replies "Hello, World" you create the file, with a path relative to your
file above of `./api/index.js` (so if your server is at `YOUR_DIRECTORY/server.js` then this one will be `YOUR_DIRECTORY/api/index.js`:

```js
module.exports = {
  name: "ApiPlugin",
  register: async (server, options) => {
  
    server.route([
      {
        method: "GET",
        path: "/api/hello",
        handler: async (request, h) => {
          return "Hello, World";
          
        }
      }
    ]);
  
  }
}
```

Note: both `name` and `register` are required. name should be a unique name for your plugin, and register contains your 
registration method for the plugin.

In `server.route`, we can again pass a single object (1 route) or an array of objects (multiple routes).

Method is the HTTP method that the route will respond to, while path defines the path that will be matched — 
see https://hapijs.com/api#path-parameters for path parameters.

In the handler method signature, `request` is the request object and `h` is a response toolkit — you won't use this too often.

Whatever you return from the handler is what is sent to the client — `Content-Type` is inferred from what you send.

If you return an object from a handler, for instance, it will be sent back as `application/json` without you doing anything special.

If you return a string, it will be `text/plain`.

To get more control over the response, you can use `h.response` method, so let's say you want to make it HTML - first, you create
a response:

```js
const response = h.response('<html><head><title>hi</title></head><body>Hello</body></html>');
```

Then you can set the Content-Type:

```js
response.type("text/html");
```

and then you must return it:

```js
return response;
```

You can do redirects with 

```js
h.redirect(PATH_TO_REDIRECT)
```

You can set a cookie with

```js
h.state(COOKIE_NAME, COOKIE_VALUE, <OPTIONS>)
```

## Running your server

You can now start things up by running:

`node server.js`

To go further with your application, check out the official [Hapi API docs](https://hapijs.com/api).
