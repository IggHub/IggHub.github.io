---
layout:   post
title:    JavaScript Object Notation
tags:     [Javascript, JSON]
comments: true
---

## On JSON

JSON, short for JavaScript Object Notation, is a mechanism used to serialize data structure into strings. It is also known as *"fat-free XML"* because it uses less characters for similar usage. It looks awful similar to JS' object notation, with chief differences being:

1. JSON keys need to be enclosed in double-quotates
2. JS Object keys can be enclosed in double, single, or no quotes.

Syntatically, a JSON would look like:

```
{“key1”: value1, “key2”: value2, … “keyN”: valueN}
```

## JSON Functions

Two main functions when working with JSON are: `JSON.stringify()` and `JSON.parse()`. The former is used to convert JS object to JSON and the latter to convert JSON to JS object.

### stringify()

To convert regular JS object to JSON, we can use `JSON.stringify()` method.

```
JSON.stringify(value[, replacer[, space]])

// example:

var obj = {dunkin: "donut", krispy: "kreme"};
var json = JSON.stringify(obj);

console.log(json);
//=> {"dunkin":"donut","krispy":"kreme"}
```

`stringify()` method takes one argument and two more optional arguments. The first argument, as seen on example above, accepts a JS object. The second optional argument, the `replacer`, is used to handle key-value pairs. For example:

```
var obj = {dunkin: "donut", krispy: "kreme", random: "donut"};
var arr = ["dunkin", "krispy"];
var json = JSON.stringify(obj, arr);

console.log(json);
//=> {"dunkin":"donut","krispy":"kreme"}
```


The third argument, `space`, is used to improve readability.

```
var obj = {foo: 0, bar: [null, false, true]}
var json = JSON.stringify(obj, null, 2);

console.log(json);
//=>
  {
    "foo": 0,
    "bar": [
      null,
      true,
      false
      ]
  }
```


### parse()

`JSON.parse()` is used to convert JSON format to JS object notation.

```
JSON.parse(text[, reviver])
```

For example:

```
var json = JSON.stringify({dunkin: "donuts"}); //this returns a JSON
var obj = JSON.parse(json); //parse the JSON and we should expect it to return an object

console.log(obj);
//=> {dunkin: "donuts"}
```

`JSON.parse()` takes one argument and a second optional one. The second argument, `reviver`, can be used to modify the key-value pair. It accepts a function with two arguments: `(key, value)`. For example, supposed we want to add ten to every values:

```
function addTen(key, value) {
  if (key === ""){
    return value;
  } else {
    return (value + 10);
  }
}

var json = JSON.stringify({one: 1, two: 2, three: 3});
var obj = JSON.parse(json, addTen);

console.log(obj);
//=> {one: 11, two: 12, three: 13}
```

## JSON Supported Data Types

JSON supports various data types:

1. Strings
2. Booleans
3. Arrays
4. Objects
5. Null

### Strings

JSON requires strings to be enclosed in double-quotes:

```
var json = "{\"dunkin\":\"donut\"}";  //the \" is an escape symbol for double-quotes
```

### Booleans

Just like JS booleans, JSON accepts true/ false values:

```
var json = "{\"dunkin\":true, \"krispy\":false}";
```

### Arrays

When defining array as values, it needs to be in square brackets (`[]`) and can accept various data types:

```
var json = "{\"dunkin\":["donut", true, [\"krispy\", \"kreme\"], {}]}";
```

### Objects

Again, the keys need to be enclosed in double-quotes:

```
var json = "{\"dunkin\":{\"donut\":{\"krispy\":true}}}";
```

### null

JSON supports `null` as value:

```
var json = "{\"foo\":null}";
```
