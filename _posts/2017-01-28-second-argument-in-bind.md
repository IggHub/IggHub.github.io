---
layout:   post
title:    What the second argument in bind does
tags:     [javascript, this, bind]
comments: true
---

If you look at JS Bind's [documentation](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Function/bind) you'll notice that it accepts more than one argument. What do those second, third, fourth, &c. arguments do?

Here is what the documentation says:

>arg1, arg2, ... Arguments to prepend to arguments provided to the bound function when invoking the target function.

This article will help to understand what that means using simple examples.

Let's say we have a simple function that adds first and second arguments:

```
var add = function(a,b){
  return a + b;
};
```

We can run this by giving it two Number arguments, for example `add(5,2); //returns 7`. If we want to 'bind' one of the variables to `add()` function so the next time we run it we only have to give `add` one variable, we can do so with bind. Consider the following:

```
var addFive = add.bind(null, 5);
```

Here we bound 5 to be an argument for add. Try typing `addFive;` and you will get the `add()` function itself! Running `addFive(2)` gives you 7 and `addFive(10)` naturally gives you 15. What we just did was we bound 5 into `add` and call the new bounded function `addFive`. `addFive` automatically inserts 5 into `add()` automatically. Pretty cool, huh?

To summarize, `bind()`'s first argument sets the `this` keyword to the provided value, while the argument(s) after that sets the arguments to the provided value.

Let's recap what we learned so far with bind with the following function:

```
var addThis = function(a,b){
  console.log(this.c);
  return a + b;
}

var obj = {c: 'Awesome!'}
```

Try running the function `addThis()`:
1. Without binding it
2. Binding it with an object and one additional argument (`.bind(obj, 10)`)
3. Binding it with an object and many additional arguments (`.bind(obj, 10, 11, 12)`)
4. Binding it with no object and an argument (`.bind(null, 10)`). See what differences do they make.

Can you guess what the each output is? If you're stuck binding the function, to bind it, do something like `var addTen = addThis.bind(...)`
