---
layout:   post
title:    NodeJS Async Series And Waterfall
tags:     [nodejs]
comments: true
---

[AsyncJS](https://github.com/caolan/async) is a popular node libray to manage asynchronous behavior of multiple nodeJS functions. I will share two of AsyncJS' API: `series` and `waterfall`.

## async.series()

Series does what it sounds like, handles a series of functions in an array.

```
async.series([func1, func2, func3, ...], function(error, results){...})
```

Series usually takes two arguments: an array of functions and final callback. The function inside array of functions takes form of

```
function(callback){
  //do something
  callback(err, returnSomething)
}
```
For example,

```
var async = require("async");
async.series([
  function(callback) {
    setTimeout(function() {
      console.log("First in series!");
      callback(null, 'one')
}, 200); },
  function(callback) {
    setTimeout(function() {
      console.log("Second in series!");
      callback(null, "two");
    }, 100);
}
], function(error, results) {
  if (error) {
    console.log(error.toString());
  } else {
    console.log(results);
  }
});
```

Will return:

```
First in series!
Second in series!
[ 'one', 'second' ]
```

`async.series` has two elements; each of them is a `setTimeout` function; function two is shorter than first one, but function one still gets executed first because `async.series` won't execute the next element until the first one is finished. Once all of the elements are executed, it will run the final callback function (second argument in `async.series`). This callback function takes two argument: an error and a result. The error returns `null` if there is no error. The results, as you see from the return function, is a result array. Note the `callback(null, 'one')` - the function takes two arguments: error and return. The error is set to null and the return is set to `one`. The first function, once executed, will return `one` string.

## async.waterfall()

`async.waterfall` is very similar to `async.series`. The chief difference is that waterfall allows each array element to pass variable(s) to the next element, and so on. The final callback results accepts the result from the last function *only*.

```
var async = require("async");
async.waterfall([
  function(callback){
    callback(null, 1); //pass value of 1 to the next callback element
    },
  function(a, callback,){
    callback(null, a + 2, 6); //a is 1. Pass to next callback a + 3 and 6
  },
  function(b, c, callback){
    callback(null, Math.sqrt(b + c));
  }
  ], function(error, d){
  console.log(d);
  })
```

The first element passes to second element value of 1. Second element passes to third element 1 + 3 *and* 6. Third element accepts the 2 variables passed down from second element: 3 and 6, and performs a square root (9 square root is 3). This third element passes down to *final callbac function* value of 3.
