
# What is a Controller?
Sails controllers work very similar to controllers in other MVC frameworks. Think of controllers
being the middle man between your model and your views.

# Where are Controllers Defined?
Controllers are defined in the **/api/controllers/** folder. By default when you create a
controller using the command line, you can add additional arguments that represent the controller
actions.

```
sails generate controller comment create destory tag like
```
generates:
```javascript

// Comment controller with generated actions.
var CommentController = {
	create: function(req, res) {

	},

	destroy: function(req, res) {

	},

	tag: function(req, res) {

	}

	like: function(req, res) {

	}
}

exports = CommentController;
```

Alternively if you add a federated flag ```-f``` to the end of your generate controller command the
controller will be created as a folder with each action being its own file. This is useful if
your actions contain a bunch of logic. No more super long controller files! The best part about this
is that, routing to these actions works the exact same way!

```
sails generate controller comment create destory tag -f
```
will create the directory, **api/controllers/comment/** with three files 
**/api/controllers/comment/create.js**,
**/api/controllers/comment/destory.js**, and
**/api/controllers/comment/tag.js**.
 
# Express Crash-Course
When responding to a request, Sails uses many of the same res methods as Express.

## res.view([view, options[, fn]])
This method is an enhanced version of  Express's **res.render()**. The method **res.view()**
automatically renders the appropriate view based on the controller and action. The original function
is still accessible via **res.render()**.


## res.send(body|status[, headers|status[, status]])
__from the <a href="http://expressjs.com/2x/guide.html#res.send()">express guide</a>.__
The **res.send()** method is a high level response utility allowing you to pass objects to respond
with json, strings for html, Buffer instances, or numbers representing the status code. The
following are all valid uses:

```javascript
	res.send(); // 204
	res.send(new Buffer('wahoo'));
	res.send({ some: 'json' });
	res.send('<p>some html</p>');
	res.send('Sorry, cant find that', 404);
	res.send('text', { 'Content-Type': 'text/plain' }, 201);
	res.send(404);
```

By default the Content-Type response header is set, however if explicitly assigned through
**res.send()** or previously with **res.header()** or **res.contentType()** it will not be set
again.

Note that this method ends the response, so you will want to use node’s **res.write()** for multiple
writes or streaming.

## res.json(obj[, headers|status[, status]])
__from the <a href="http://expressjs.com/2x/guide.html#res.json()">express guide</a>.__
Send a JSON response with optional headers and status. This method is ideal for JSON-only APIs,
however **res.send(obj)** will send JSON as well, though not ideal for cases when you want to send
for example a string as JSON, since the default for **res.send(string)** is text/html.

```javascript
	res.json(null);
	res.json({ user: 'tj' });
	res.json('oh noes!', 500);
	res.json('I dont have that', 404);
```

## res.redirect(url[, status])
__from the <a href="http://expressjs.com/2x/guide.html#res.redirect()">express guide</a>.__
Redirect to the given url with a default response status of 302.

```javascript
	res.redirect('/', 301);
	res.redirect('/account');
	res.redirect('http://google.com');
	res.redirect('home');
	res.redirect('back');
```

## Parameters
Request parameters can be defined in 3 ways.

__from the <a href="http://expressjs.com/2x/guide.html#req.param()">express guide</a>.__

Return the value of param **name** when present or **default**.

  - Checks route params (req.params), ex: /user/:id
  - Checks query string params (req.query), ex: ?id=12
  - Checks urlencoded body params (req.body), ex: id=12

<!-- To utilize urlencoded request bodies, req.body should be an object. This can be done by using
the _express.bodyParser middleware. -->