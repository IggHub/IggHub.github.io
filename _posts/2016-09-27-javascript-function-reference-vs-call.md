---
layout:   post
title:    Javascript Function Reference vs Call
tags:     [javascript, function]
comments: true
---

What's the difference between the following two `var`s?

```
var x = addOne;
var y = addOne(1);
```

Today I learned two ways of using Javascript function: **referencing** and **calling**.

The first one, `var x = addOne;` is called function referencing, while the second one, `var y = addOne(1);` is function calling. Instead of giving them the actual definition, let's see what each of them does!

```
function plusOne(n){
	return (n + 1);
};
```

## Function referencing

Let's create `var x`:

```
$ var x = addOne;
=> undefined
$ x
=> [Function: addOne]
```

What just happened? when we created `var x` without the parentheses `()`, when I type `x`, it says that `x` is a function!

## Function calling

Let's do `var y`:

```
$ var y = addOne(11);
=> undefined
$ y
=> 12
```

Oh, what!! `y` returns the value of `11 + 1`, `12`!

## Referencing vs Calling

By referencing a function (the one *without* parentheses `()`), we are essentially referencing that `var` to equal a function. `x` is now `addOne` function itself. Since `x` is now the `addOne` function, it works like `addOne` function. `x` can now accept an argument. Try this:

```
$ x(10)
=> 11
```

Alternatively, calling a function (the one with parentheses `()`) returns what the function's result. The result of `addOne(10)` is `11`.

```
$ y(10);
TypeError: y is not a function
  at eval:1:1
```

As expected, `y(arg)` does not work.

## Quiz

```
$ var a = addOne;
$ var b = addOne(10);
$ var c = a(b);
$ var d = addOne(c);
$ d;
$ e = a(a(a(1)));
```

What will `d` and `e` return?
