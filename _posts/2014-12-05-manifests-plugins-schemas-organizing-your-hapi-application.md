---
layout: post
title: Manifests, Plugins and schemas - Organizing your hapi application
category: hapi.js
medium_url: https://medium.com/@dstevensio/manifests-plugins-and-schemas-organizing-your-hapi-application-68cf316730ef
---

If you're building things for the web, love JavaScript and haven't yet discovered [hapi.js](http://hapijs.com/), you're missing out. Billed as "a rich framework for building applications and services", what you'll find is a feature-rich, battle-tested ([Walmart on Black Friday](http://thechangelog.com/116/) is a pretty good yardstick in my book) and rapidly evolving foundation upon which to build your web application.

Go ahead and read their [Getting Started guide](http://hapijs.com/tutorials/getting-started) if this is the first you're hearing of the framework.

## Composing your server

The simple, quick, get started in a hurry way to create a server in hapi is as such:

```js
var Hapi = require(‘hapi');
var server = new Hapi.Server();
server.connection({ port: 80 });

server.start(function (err) {
  // Server running on port 80
});
```

(Existing hapi users: if this doesn't look right to you, note that in v8.0.0 the Server, Pack and Plugin interfaces were merged in to a single Server interface. Check out the [release notes](https://github.com/hapijs/hapi/issues/2186) for details.)

That's pretty cool, but what I like to do is utilize a method called "Composing" a server. Prior to v8.0.0, this was done via hapi's Pack interface, but now we use a separate module, [Glue](https://github.com/hapijs/glue), to handle server composition.

```js
var Glue = require('glue');
var manifest = {
  connections: [{
      port: 80
  }]
};

Glue.compose(manifest, function (err, server) {
  server.start(function (err) {
    // server running on port 80
  });
});
```

With this contrived, simplistic example, it doesn't seem worth it — but we'll obviously find any true real-world application will have a far greater number of configuration options, and this, I believe, is where the composer approach comes in to its own.

Let's consider a scenario whereby we have an application which consists of two component services: one that serves a UI, and one that serves an API. Another contrived example for the purpose of explanation, let's say we need to be able to set a company slogan that will be used in branding on both services, so we'll place it in a commonly accessible location. Here's how the manifest object would look:

```js
var manifest = {
  server: {
    app: {
      slogan: 'We push the web forward'
    }
  },
  connections: [
    {
      port: 80,
      labels: ['web-ui']
    },
    {
      port: 9000,
      labels: ['api']
    }
  ]
};
```

Everything in one place, nicely organized — just how I like it. Clearly readable, and configuration happens in a single place.

Now, I like things clean and uncluttered, so it's at this point that I've taken things one short step further, moving this manifest object in to a JSON file:

```js
{
  "server": {
    "app": {
      "slogan": "We push the web forward"
    }
  },
  "connections": [
    {
      "port": 80,
      "labels": ["web-ui"]
    },
    {
      "port": 9000,
      "labels": ["api"]
    }
  ]
}
```
<em>manifest.json</em>

Then, using node's `require` behavior that returns an object from a `require`-d JSON file, we pull it in to the Glue compose:

```js
var Glue = require('glue');

Glue.compose(require('./config/manifest.json'), function (err, server) {
  server.start(function (err) {
    // UI running on port 80,
    // API running on port 9000
  });
});
```

## Modularizing your application components logically

Traditionally, we've continued an existing trend of organizing the behind-the-scenes backend parts of our web applications in to Controllers. A directory layout may look like this, assuming a site with a homepage, an about page, and a search page:

```
/
|_ index.js
|_ lib
   |_ controllers
     |_ about.js
     |_ home.js
     |_ search.js
   |_ views
     |_ about.html
     |_ home.html
     |_ layout.html
     |_ search.html
```

The Controller file would likely look something like the following, assuming this is <em>home.js</em>:

```js
module.exports = function (request, reply) {

  var context = {
    pageTitle: 'Home Page'
  };

  reply.view('home', context);

};
```

And then in our main application file, index.js in this example, we'd add the route to the server as such:

```js
server.route({
  path: '/',
  method: 'GET',
  handler: require('./lib/controllers/home')
});
```

So, a pretty familiar pattern and for small applications, manageable and simple. However, as I was building more and more complex applications using hapi, I wanted to better modularize different parts of functionality. This isn't a downfall of hapi itself, I've long experimented with different ways of logically organizing web applications in Express and, in an increasingly distant past professional life, PHP.

The difference with hapi is that I found one that both made sense and has been consistently useful, scalable and beneficial to collaboration.

## Application components as plugins

Instead of the traditional Controller approach, I've taken to grouping my web application in to logical sections and handling each section as a plugin.

Taking our example above, let's rework things in the sections-as-plugins approach, introducing the logical grouping:

```
/
|_ index.js
|_ lib
  |_ modules
    |_ company
       |_ about.js
       |_ home.js
       |_ index.js
       |_ package.json
    |_ search
       |_ index.js
       |_ package.json
       |_ search.js
  |_ templates
    |_ about.html
    |_ home.html
    |_ layout.html
    |_ search.html
```

The homepage and the about page now fall under a grouping of "company" — pages pertaining to information about the company or service. This isn't a suggestion that you should do the same grouping, just an example to illustrate the plugin to page ratio doesn't need to be 1:1.

## The structure of the plugin's directory

Each plugin has two essential files: index.js and package.json. This isn't a hapi requirement — when I say essential, I just mean to achieve the clean organization that I've found so useful.

The main plugin file, index.js, will set up the routes that the plugin will handle. We'll also register some attributes of the plugin, which will be pulled in from the accompanying `package.json`.

Let's take a look at the company module from the example directory structure above, starting with the index.js file:

```js
exports.register = function (server, options, next) {

  server.route({
    path: '/',
    method: 'GET',
    handler: require('./home')
  });

  server.route({
    path: '/about',
    method: 'GET',
    handler: require('./about')
  });

  next();

};

exports.register.attributes = {
  pkg: require('./package.json')
};
```

Then, the package.json:

```js
{
  "name": "exampleCompanySection",
  "version": "1.0.0"
}
```

So we've setup the functionality that will allow us to register the plugin, given it a unique name and set the current version, and declared two routes that the plugin will handle — ‘/' and ‘/about'.

The files home.js and about.js will contain the logic for handling those respective routes. Let's look at how they would be structured:

```js
module.exports = function (request, reply) {

 var context = {
 pageTitle: 'Home Page'
 };

 reply.view('home', context);

};
```
<em>home.js</em>

```js
module.exports = function (request, reply) {

 var context = {
 pageTitle: 'About Us'
 };

 reply.view('about', context);

};
```
<em>about.js</em>

## Route definitions in each plugin?

I've heard some feedback that defining all your routes across multiple plugins, rather than a central definition of all routes in your application, is troubling to some. For me, I like that all the routes for a certain grouping of sections or pages or services are declared in one place. You don't end up with route conflicts, because hapi forbids those and you'll get an error on starting your server if you declare a route that already exists in the application.

## On the location of your plugins

hapi is flexible and configurable as to the location of its plugins. If you are building a self-contained application and want to have all your logic in one repository, taking the approach as in the example above let's you develop in a familiar fashion. The modules directory name is completely arbitrary, you can call it whatever you want of course.

A second option is to have each plugin published to a private NPM repository that you control. No one else has access to your source code, but you can manage the plugins as you would any other dependency, and decouple the foundation of your application from the individal pieces of application logic per section.

Finally, there's nothing to stop you from publishing them to the public NPM repository. The only application I can see this having is a site that seeks mirrors — lots of different entities hosting the same site, to distribute the load — but you may think of other benefits to this approach.

## Registering plugins with the server

Once we've set everything up this way, we next need to register the plugin(s) with the server, so that they are activated. We do that in our manifest file, like so:

```js
{
  "server": {
    "app": {
      "slogan": "We push the web forward"
    }
  },
  "connections": [
    {
      "port": 80,
      "labels": ["web-ui"]
    },
    {
      "port": 9000,
      "labels": ["api"]
    }
  ],
  "plugins": {
    "./company": null,
    "./search": null
  }
}
```
<em>manifest.json</em>

Note that if you're using an NPM approach (public or private repository) you would put the plugin name (that you defined in the plugins package.json) instead of the relative path prefixed with a "./" that is in the example above.

The final step to make this work is a small change to our main application file, in this case the root index.js to add a relativeTo path for Glue to be able to find the plugin(s). Again, this step is not necessary if you're using the NPM approach:

```js
var Glue = require('glue');
var manifest = require('./config/manifest.json');
var options = {
  relativeTo: __dirname + '/lib/modules'
};
Glue.compose(manifest, options, function (err, server) {
  server.start(function (err) {
    // UI running on port 80,
    // API running on port 9000
  });
});
```

## Further benefits of this approach

Consider maintaining a hapi application in a large team — imagine product teams at a large company. Perhaps there's a team dedicated to search pages, one to product detail pages, another to cart/checkout. By modularizing in this way, each plugin could be per team, and in its own repository that the team owns and works on. Then, there's a common foundation all teams can continue to update from a central repository, pulling in the rest of the site, while they hack away on their area of ownership.

Also, imagine an application with a search component or something similarly complex that has a number of subcomponents. You can modularize these within the plugin directory or a sub-directory thereof, maintaining a clean organization to your project.

## But wait, there's more...

If you like this approach to organizing your hapi application, you might be interested in further options that become available to you in doing so.

I gave a short talk "Using hapi plugins to version your API" at the JSFest Oakland hapi day on December 11th, 2014. Thanks to my employer Zappos for covering the cost of attending to speak.

* Video: [YouTube](https://www.youtube.com/watch?v=Mth0XRQ93rM)
* Slides: [SlideShare](http://www.slideshare.net/shakefon/hapidays-2014)
* Github Repo: [github.com/dstevensio/hapiday14](https://github.com/dstevensio/hapiday14)
* Plugin I created for the API versioning: [github.com/dstevensio/consistency](https://github.com/dstevensio/consistency)
