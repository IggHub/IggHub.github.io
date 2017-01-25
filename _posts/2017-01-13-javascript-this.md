---
layout:   post
title:    How this in Javascript works
tags:     [javascript, this]
comments: true
---

If you have been programming JS for a few months, you probably see `this.something` a number of times and wonder what it means. I will explain a little bit about implicit binding this function here.

Let's say we have an object:

```
var Donutman = {
  name: "Donnie da Donutman",
  favDonut: ["Glazed", "Chocolate"]
}
```

We can invoke name by `Donutman.name`, donut array by `Donutman.favDonut`, and specific donut by `Donutman.favDonut[1]`. Let's add a new function inside `Donutman` object that prints out its name.

```
var Donutman = {
  name: "Donnie da Donutman",
  favDonut: ["Glazed", "Chocolate"],
  sayDonut: function(){
    console.log(this.name);
  }
}
```

To invoke the function, we can use `Donutman.sayDonut()`.

If you want to know what `this` refers to, look back at the command `Donutman.sayDonut()`; inside `sayDonut()` function we console log `this.name`. `this` refers to whatever is to the left side of the dot at the function call: `Donutman.sayDonut()`. Left of `sayDonut()` is `Donutman`. `this` = `Donutman`. You can see the function as:

```
console.log(Donutman.name)
```

A donut is not complete without coffee. Let's add another object inside `Donutman` object with a function that prints coffee flavor:

```
var Donutman = {
  name: "Donnie da Donutman",
  favDonut: ["Glazed", "Chocolate"],
  sayDonut: function(){
    console.log(this.name);
  },
  coffee: {
    flavor: "Black",
    sayCoffee: function(){
      console.log("I love " + this.flavor + " coffee");
    }
  }
}
```

When we run `Donutman.coffee.flavor`, it prints `"Black"`. When we run `Donutman.coffee.sayCoffee()`, it will print out `"I love Black coffee"`.

The same rule applies: `this` refers to whatever is on the left side of the dot where the function containing `this` lies on (`Donutman.coffee.sayCoffee()`)- in this case, it is `Donutman.coffee`. Using simple substitution, we substitute `this` inside `sayCoffee()` with `Donutman.coffee` and get:

`console.log("I love " + Donutman.coffee.flavor + " coffee");`
