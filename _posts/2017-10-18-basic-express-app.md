---
layout:   post
title:    Basic Express App
tags:     [Javascript, Express]
comments: true
---

[Express](http://expressjs.com/) is a minimal nodeJS framework to get basic app running in minutes. To start simple express app using node, create a new folder and go there `mkdir express-study && cd express-study`. Run `npm init`, followed by `npm install --save express` to save express as dependencies.

## Getting Started

While still inside the folder, create a JS file, I named mine `express1.js`:

```
var express = require(‘express’);
var http = require(‘http’);
var app = express();

app.get(“/“, function(req, res, next){
  res.send(“Index”);
});

app.get(“/foo”, function(req, res, next){
  res.send("Foo”);
});

app.get(“/bar”, function(req, res, next){
  res.send(“Bar”);
});

http.createServer(app).listen(8000);
```

To run it, run `node express1.js`. When you go to `localhost:8000`, you will see your shiny new app running on `localhost:8000`, `localhost:8000/foo`, and `localhost:8000/bar`! If that is not simple, I don't know what is.

## More on Routing

Sometimes you want to go to specific URL, like `/foo/1` or `/foo/2/bar/3`. To do that, we can use `:someId`.

```
app.get("/foo/:fooId", function(req, res, next){
  res.send("Hello Foo #" + req.params.fooId);  
})
```

`:fooId` is the ID parameter. It is stored inside `req.params.fooId` and can be accessed anytime.

## Summary

1. Express is a minimalist nodeJS framework to get up and running quickly.
2. It uses a **routing** system. In this case, we set up 3 (4 if counting the last example) routings: `"/"`, `"/foo"`, and `"/bar"`.
3. It can accept other  HTTP verbs such as GET, POST, PUT, DELETE, and more. The examples above demonstrate GET using `app.get()`.
