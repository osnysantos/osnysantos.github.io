---
title:  "Simple auth using Express and Node"
date:   2016-09-25 09:28:00 -0300
categories: node express
---

Recently I've played a lot with **Node** and **Express** in web development, it's simple and has everything we need to be able to develop, has module for basically everything and you can see that the community is growing every day: [http://www.modulecounts.com][1].

But sometimes we need to protect the apps with a simple authentication (user/password) just to be able to demonstrate to clients, it's necessary in many cases.

Recently I had an experience using **[http-auth][1]** and surprised me by the ease and speed of configuration, it has integration with various frameworks such as Express, Koa, Hapi, etc, below I demonstrate how to set up with Express.

First, you need to install it:

{% highlight bash %}
npm install http-auth
{% endhighlight %}

Now I show you my script (app.js) with the Express/Node code and then the code using http-auth.

{% highlight javascript linenos %}
'use strict';

var express = require('express');
var app = express();

app.use(express.static('www'));

app.get('*', function (req, res) {
  res.render('index.html.ejs');
});

app.listen(4000, function() {
  console.log('Node app is running');
});
{% endhighlight %}

And now with http-auth:

{% highlight javascript linenos %}
'use strict';

var express = require('express');
var app = express();
var auth = require('http-auth');

var basic = auth.basic({
  realm: 'Secret'
}, function(username, password, callback) {
  callback(username == 'username' && password == 'password');
});

app.use(auth.connect(basic));
app.use(express.static('www'));

app.listen(4000, function() {
  console.log('Node app is running');
});
{% endhighlight %}

Pretty simply, there are 3 steps you need to do:

1. Save a reference of htto-auth in a variable
{% highlight javascript linenos %}
var auth = require('http-auth');
{% endhighlight %}

2. Setup the auth module following the doc:
{% highlight javascript linenos %}
var basic = auth.basic({
  realm: 'Secret'
}, function(username, password, callback) {
  callback(username == 'username' && password == 'password');
});
{% endhighlight %}

3. Setup Middleware:
{% highlight javascript linenos %}
app.use(auth.connect(basic));
{% endhighlight %}



[1]: http://www.modulecounts.com
[2]: https://github.com/http-auth/http-auth
