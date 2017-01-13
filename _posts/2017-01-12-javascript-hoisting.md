---
layout:   post
title:    Javascript Hoisting
tags:     [javascript]
comments: true
---

What do you think the code below would print?

```
var a = "Hello JS!";
console.log(a);
```

`Hello JS!`, of course!

Let's switch it up a bit, what do you think would the code below print?

```
console.log(b);
var b = "Hello again JS!";
```

If you guessed `undefined`, you are correct again! Let's try this again.

```
c = "Well hello JS!";
var c;
console.log(c);
```

You may think that `var c` would overwrite `c="Well hello JS!"` assignment above and return `undefined`. If that's the case, then you're wrong :). It returns, surprisingly, `Well hello JS!`. This is called hoisting. JS apparently does not execute code line by line (well, technically it does, but not the way we might have thought initially).

Turns out that JS splits code **declaration** and code **assignment** separately.

### Declaration

When you are `declaring` a new variable, you are declaring a code. An example of code **declaration** is `var a;`. You are declaring to JS that there will be a new variable named `a`.

### Assignment

When you are `assigning` a variable, you are giving that variable a value, be it a string, a number, a function, an array, or an object. `a = "Hello JS!";` would be an example of **assignment**.

>**Note**: This is a part that might flip your hat. When you say `var a = "Hello JS!";` you are first declaring `var a`, and then assigning the value `a = "Hello JS!"`. JS sees it as two separate actions:

```
var a;
a = "Hello JS!";
```

JS *always* runs all declarations first (top-to-bottom, left-to-right) and then runs assignments and the rest later (top-to-bottom, left-to-right).

Revisiting the first example:

```
var a = "Hello JS!";
console.log(a);
```

Is seen as:

```
var a;
-----
a = "Hello JS!";
console.log(a);
```

Second example is actually seen as:

```
var b;
-----
console.log(b);
b = "Hello again JS!";
```

Third:

```
var c;
-----
c = "Well hello JS!";
console.log(c);
```

Let's do one more example. Try to do this on your own first, and then look at my answers.

```
console.log(d);
var e;
e = "Hola e!";
console.log(e);
var d = "Hi d!";

```

Separating declarations and assignments, we get:

```
var e;
var d;
-----
console.log(d); //d hasn't been defined yet
d = "Hi d!";
e = "Hola e!"
console.log(e); //prints 'Hola e!'
```

If you guess such, congratulations! You are are on your way to understand hoisting. Next, try to figure out yourself - how does hoisting work with functions?
