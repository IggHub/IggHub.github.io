---
layout:   post
title:    A little useful JS reduce trick
tags:     [javascript, reduce]
comments: true
---

Today I want to share a little about `.reduce()` in JS. If you visited [JS reduce documentation](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/Reduce), you might shake your head and go, "what?"

It is a little confusing. But here is some basic premise: JS reduce takes in 2 arguments: first, a callback function and second, an initial value. The callback function is usually known as a reducer and can take up to 4 arguments. On this post, I will use two arguments. Maybe one day we will explore the usage of the other 2 arguments. The initial value is the starting value when reduce starts its 'iteration'.

`.reduce()` is usually used to sum up numbers. Let's start with that example. Suppose we have an array of numbers we want to sum up:

```
var numbers = [9, 8, 3];
var startingPoint = 0;
var reducer = function(acc, val){
  return acc + val;
}
```

We have an array `numbers`, an initial value `startingPoint`, and reducer callback `reducer`. Here is how to use it:

```
numbers.reduce(reducer, startingPoint);
```

In the above example, reducer takes in 2 arguments: `acc, val`. You can think of `acc` as accumulated value and `val` as current value that will be added to acc. Since we have an array of 3 elements, it will undergo 3 iterations.

```
/*
Iteration 1: acc = 0, val = 9. acc + val = 9.
Iteration 2: acc = 9, val = 8. acc + val = 17.
Iteration 3: acc = 17, val = 3. acc + val = 20.
Since there is no more element, iteration ends. Return acc + val value which is 20
*/
```

Reduce also works with string:

```
var strArray = ["hello", "JS", "reducer"];
var reducer = function(a,b) {return a + " " + b};
var starting = "";

strArray.reduce(reducer, starting);
// " hello JS reducer"
```

Now, a super cool trick you can do with reduce is to use an object to count how many times an element occurs in an array. Like counting vote. To do this, we will use an `object` as our initial/ starting value.

```
var donuts = ["chocolate", "chocolate", "glazed", "chocolate", "glazed", "glazed"];
var starting = {};
var reducer = function(tally, item){
  if (!tally[item]){
    tally[item] = 1;
  } else {
    tally[item] = tally[item] + 1;
  }
  return tally;
}
donuts.reduce(reducer, starting);
//{chocolate: 3, glazed: 3}
```

reducer is a little complicated. Let's break it apart. It takes 2 arguments: a tally and an item. Tally will accumulate all elements while item displays an array element from the array during the iteration. If that item does not exist, i.e. first time having `chocolate` element or first time registering `glazed` element, then it will create a new object and store a count of 1. `else` (if that item is found in the object) add 1 to the count with that item name.

You can check the count using `result["chocolate"]`, for example, and it will display the number of times `chocolate` occurs in the array (in this example, 3). But if you type in `result['strawberry']`, it will show `undefined`. If we want to add a new item, we would usually do something like, `result["strawberry"] = 1;`, and if we want to add the strawberry count, we would do `result["strawberry"] += 1;`. The same concept is done on the example above, except reduce took away all the repetition.
