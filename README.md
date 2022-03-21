# Part 1:  nestjs



### Generating a blank Express Application

The first thing we need to do is generate an express application using Node.js. To do this we'll execute the following commands from this directory:

```bash
# installing nestjs 
$ npm i -g @nestjs/cli
# create a new nestjs project
$ nest new project-name

```

Express will output the files that it creates:

```
create : .
   create : ./package.json
   create : ./app.js
   create : ./public
   create : ./public/javascripts
   create : ./public/images
   create : ./public/stylesheets
   create : ./public/stylesheets/style.css
   create : ./routes
   create : ./routes/index.js
   create : ./routes/users.js
   create : ./views
   create : ./views/index.ejs
   create : ./views/error.ejs
   create : ./bin
   create : ./bin/www

   install dependencies:
     $ cd . && npm install

   run the app:
     $ DEBUG=Start ./bin/www
```

Now install the dependencies for the application. Npm will read the dependencies out of package.json  and place them into a directory named **node_modules**. Execute the command:

```bash
# install dependencies described in package.json
npm install
```

In the **routes** folder delete the **user.js** file, and create two new javascript files, **authenticate.js** and **api.js**

```bash
# navigate to routes folder
cd ./routes
# remove users.js
rm users.js
# remove index.js
rm index.js
# create authentication.js and api.js files
touch authentication.js
touch api.js
```

Let's take a look at our generated **app.js** which is the main application file:

```js
var express = require('express');
var path = require('path');
var favicon = require('serve-favicon');
var logger = require('morgan');
var cookieParser = require('cookie-parser');
var bodyParser = require('body-parser');

var routes = require('./routes/index');
var users = require('./routes/users');

var app = express();

// view engine setup
app.set('views', path.join(__dirname, 'views'));
app.set('view engine', 'ejs');

// uncomment after placing your favicon in /public
//app.use(favicon(__dirname + '/public/favicon.ico'));
app.use(logger('dev'));
app.use(bodyParser.json());
app.use(bodyParser.urlencoded({ extended: false }));
app.use(cookieParser());
app.use(express.static(path.join(__dirname, 'public')));

app.use('/', routes);
app.use('/users', users);

// catch 404 and forward to error handler
app.use(function(req, res, next) {
    var err = new Error('Not Found');
    err.status = 404;
    next(err);
});

// error handlers

// development error handler
// will print stacktrace
if (app.get('env') === 'development') {
    app.use(function(err, req, res, next) {
        res.status(err.status || 500);
        res.render('error', {
            message: err.message,
            error: err
        });
    });
}

// production error handler
// no stacktraces leaked to user
app.use(function(err, req, res, next) {
    res.status(err.status || 500);
    res.render('error', {
        message: err.message,
        error: {}
    });
});


module.exports = app;
```

This is the boiler plate code required to run our express application. The [require]() statements allow us to import javascript code. We'll use these to import our API routing code and register them with Express. Let's remove the current require statements to **index.js** and **users.js**:

```js
var routes = require('./routes/index');
var users = require('./routes/users');
```

And replace them with:

```js
var api = require('./routes/api');
//We will uncomment this after implementing authenticate
//var authenticate = require('./routes/authenticate');
```

To register the route handlers with Express, we'll have to remove the current registrations with **index** and **users**:

```js
app.use('/', routes);
app.use('/users', users);
```

and replace it with

```js
//app.use('/auth', authenticate);
app.use('/api', api);
```

Now we're setup to load **authenticate.js** and **api.js** as application routers. Now we need to implement it.

## Implementing the RESTful API

RESTful APIs follow a convention which present *resources* to the client. In our case a **Post** is a resource and because of this we will implement a **/posts** API which will 
First we'll implement placeholder route handlers for the **/posts** api within **api.js**.

Every router begins with a require to express, and using the express Router class. At the end of the router implementation we **export** this module as the Router to be consumed by the code we added in **app.js**

```
var express = require('express');
var router = express.Router();

//  Some implementation....


module.exports = router;
```

Now between these two things we need to add the **/posts** api handlers. The way this works in express is by registering the handler to a specific HTTP method, such as GET, PUT, POST or DELETE. Let's add the handlers for the **/posts** route:


```js
var express = require('express');
var router = express.Router();

//api for all posts
router.route('/posts')
	
	//create a new post
	.post(function(req, res){

		//TODO create a new post in the database
		req.send({message:"TODO create a new post in the database"});
	})

	.get(function(req, res){

		//TODO get all the posts in the database
		req.send({message:"TODO get all the posts in the database"});
	})
	


module.exports = router;
```

Now that we got our **/posts** api completed we need to create some apis for individual posts. We can do this by building off of our **/posts** path and adding an ID specific route  for an individual post. We use the ':' notation in our route name which tells Express that that particular part of the route will be treated as a parameter:

```js
//api for all posts
router.route('/posts')
	
	//create a new post
	.post(function(req, res){

		//TODO create a new post in the database
		res.send({message:"TODO create a new post in the database"});
	})

	.get(function(req, res){

		//TODO get all the posts in the database
		res.send({message:"TODO get all the posts in the database"});
	})

```

Finally our full router implementation looks like this:

```js
var express = require('express');
var router = express.Router();

//api for all posts
router.route('/posts')
	
	//create a new post
	.post(function(req, res){

		//TODO create a new post in the database
		res.send({message:"TODO create a new post in the database"});
	})

	.get(function(req, res){

		//TODO get all the posts in the database
		res.send({message:"TODO get all the posts in the database"});
	})

//api for a specfic post
router.route('/posts/:id')
	
	//create
	.put(function(req,res){
		return res.send({message:'TODO modify an existing post by using param ' + req.param.id});
	})

	.get(function(req,res){
		return res.send({message:'TODO get an existing post by using param ' + req.param.id});
	})

	.delete(function(req,res){
		return res.send({message:'TODO delete an existing post by using param ' + req.param.id})
	});

module.exports = router;
```

To start the application, got to the **./Start** folder and execute:

```bash
# starts the application via npm
npm start
```

## Testing Your APIs

To test your API's we'll use [Advanced Rest Client](https://chrome.google.com/webstore/detail/advanced-rest-client/hgmloofddffdnphfgcellkdfbfbjeloo) a Chrome browser application that allows us to test our API's without having to write code. You can also use [Postman](https://chrome.google.com/webstore/detail/postman-rest-client/fdmmgilgnpjigdojojpjoooidkmcomcm) another great application to do the same.

Open Chrome and either install or open Advanced Rest Client. We will make a request to each API we implemented ensure we get the correct place-holder message:

![](ScreenShots/ss1.png)

![](ScreenShots/ss2.png)

Note that because in app.js we assigned express to use the **api** router at **/api** all routes in this file will have the prefix ''**/api**'

Ensure that you get the correct 'TODO' message for the remaining **/api/posts**.

To test the **/api/posts/:id** routes you can simply use any string for the :id part of the path. When we implement the API with MongoDB, we'll use the generated ID's from MongoDB.

