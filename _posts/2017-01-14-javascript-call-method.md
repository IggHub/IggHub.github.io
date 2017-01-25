---
layout:   post
title:    How call in JS works
tags:     [javascript, call]
comments: true
---

If you have been programming javascript for a few months, chances are you would have seen the method `.call()` a few times. When you go to its documentation on [mozilla website](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Function/call), it says:

>The call() method calls a function with a given this value and arguments provided individually.

 Huh?? If you're like me, that completely went over my head. Right now I want to share a few examples to clarify what `.call()` method does.

If you haven't check out my previous post on JS `this` method. This post will borrow some ideas from that post. It is a super quick read, so if you haven't, go ahead!

Say I split up `sayDonut()` method from `Donutman` object. `sayDonut` still has `console.log(this.name)`. What exactly does `this` point to?

```
var Donutman = {
  name: "Donnie da Donutman",
  favDonut: ["Glazed", "Chocolate"]
}

var sayDonut = function(){
  console.log(this.name);
}
```

Obviously, when you run `sayDonut()`, it will not print out `Donnie da Donutman` because JS has no way knowing that you want to associate `sayDonut()`' `this.name` with `Donutman.name`. This is where `.call()` method is for. Since `sayDonut()` does not have `name` attribute and we want to connect it to `Donutman.name`, we will `call` `Donutman`'s name from `sayDonut()`:

```
sayDonut.call(Donutman);
```

Voila! Now it prints the right output. In short, `call` makes `console.log(this.name)` to equal `console.log(Donutman.name)`. Really neat!

If you look at the [documentation](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Function/call), it mentions that it takes multiple arguments:

```
fun.call(thisArg[, arg1[, arg2[, ...]]])
```

What this means, is that we can use the 2nd, 3rd, 4th, &c. arguments to pass down different arguments. Let's modify our `sayDonut()` to accept arguments:

```
var sayDonut = function(donut1, donut2){
  console.log(this.name + " likes to eat " + donut1 + " and " + donut2);
}
```

Now run `sayDonut.call(Donutman, "Plain", "Boston Creme")`. You can also add a new `donuts` array:

```
var donuts = ["Plain", "Boston Creme", "Strawberry", "Chocolate Sprinkles"]

sayDonut.call(Donutman, donuts[0], donuts[1]);
```

Now you know a little more about `.call()` method!
