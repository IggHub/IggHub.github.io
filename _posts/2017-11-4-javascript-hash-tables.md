---
layout:   post
title:    JavaScript Hash Tables
tags:     [Javascript, hash-tables]
comments: true
---

A hash table is a permutation of an associative array (associative array is also commonly known as object).


## JS Object

In JS, we can create an object easily as:

```
var awesomeObject = {'one': 1, 'two': 2, 'three': 3};

//alternatively

var awesomeObject = {};
awesomeObject['one'] = 1;
awesomeObject['two'] = 2;
awesomeObject['three'] = 3;
```

We can iterate through it:

```
for(var i in awesomeObject) {
  if(awesomeObject.hasOwnProperty(i)){
    console.log("object key: " + i  + ", value: " + awesomeObject[i])  
  }
}
```

Object in JS however, does not have `length` property.

```
awesomeObject.length //=> undefined
```


## JS Arrays

Besides API differences (for example, arrays have `length` method), arrays are really objects with numeric keys.

```
var awesomeArr = [];
awesomeArr[0] = 0;
awesomeArr['uno'] = 1;
awesomeArr['dos'] = 2;

awesomeArr //=> [0]
awesomeArr.length //=> 1
awesomeArr[0] //=> 0
awesomeArr.uno //=> 1
```

Interestingly, awesomeArr above only has length of one (instead of 3). However, `awesomeArr.uno` returns `1`. This is because `awesomeArr` is an array yet an underlying object. JS thinks `awesomeArr` is an arr (thus does not respond to non-numeric key calls), yet JS saves other non-numeric keys inside `awesomeArr`. This is pretty cool. Or perplexing.

Overall, `awesomeArr` is reliably an array, and it behaves like one.

```
awesomeArr[3] = 3;
awesomeArr //=> [0, undefined, undefined, 3]
```

## Creating Hash Table in JS

Disclaimer: JS has its own native Hash methods that I am about describe below - this section serves mostly as an explanation how it works.

```

function HashTable(obj)
{
    this.length = 0;
    this.items = {};
    for (var p in obj) {
        if (obj.hasOwnProperty(p)) {
            this.items[p] = obj[p];
            this.length++;
        }
    }

    this.setItem = function(key, value)
    {
        var previous = undefined;
        if (this.hasItem(key)) {
            previous = this.items[key];
        }
        else {
            this.length++;
        }
        this.items[key] = value;
        return previous;
    }

    this.getItem = function(key) {
        return this.hasItem(key) ? this.items[key] : undefined;
    }

    this.hasItem = function(key)
    {
        return this.items.hasOwnProperty(key);
    }

    this.removeItem = function(key)
    {
        if (this.hasItem(key)) {
            previous = this.items[key];
            this.length--;
            delete this.items[key];
            return previous;
        }
        else {
            return undefined;
        }
    }

    this.keys = function()
    {
        var keys = [];
        for (var k in this.items) {
            if (this.hasItem(k)) {
                keys.push(k);
            }
        }
        return keys;
    }

    this.values = function()
    {
        var values = [];
        for (var k in this.items) {
            if (this.hasItem(k)) {
                values.push(this.items[k]);
            }
        }
        return values;
    }

    this.each = function(fn) {
        for (var k in this.items) {
            if (this.hasItem(k)) {
                fn(k, this.items[k]);
            }
        }
    }

    this.clear = function()
    {
        this.items = {}
        this.length = 0;
    }
}

```

To run it, simply:

```
var hashTable = new HashTable({zero: 0, one: 1, two: 2, three: "Yeah!"});

//we can run the methods described above, i.e:

hashTable.items;
hashTable.values();
hashTable.getItem('one');
...
```

What happened above? I will do a mini code-mentary as I go through each section:

```
var hashTable = new HashTable({zero: 0, one: 1, two: 2, three: "Yeah!"});
```

Here I created a new variable called `hashTable` using `HashTable` constructor. I gave it an object argument. `hashTable` "receives" all HashTable methods and properties, in addition to having the array argument `{zero: 0, ...}` associated with it.

```
this.length = 0;
this.items = {};
for (var p in obj) {
  if (obj.hasOwnProperty(p)) {
    this.items[p] = obj[p];
    this.length++;
  }
}
...
```

`hashTable` has access to length (default 0) and items (default empty object). In addition, the next line automatically updates length and items with the new object that `hashTable` receives. `obj` is the object argument. `obj.hasOwnProperty(p)` ensures each object contain the given key.

```
this.setItem = function(key, value)
{
    var previous = undefined;
    if (this.hasItem(key)) {
        previous = this.items[key];
    }
    else {
        this.length++;
    }
    this.items[key] = value;
    return previous;
}
...
```

`setItem` takes in key and value. When run, it will insert into `hashTable.items` object a new key-value pair. First it declares a temporary argument called `previous` (I could have named it `temp` to make it understandable). Then `if (this.hasItem(key))` checks that key exists. If it exists, it won't update length. If key does not exist, it will update the length and then creates new key-value pair. Note it will create key-value pair regardless. Then it outputs the newly added value.

```
this.getItem = function(key) {
  return this.hasItem(key) ? this.items[key] : undefined;
}
```

`getItem` accepts a key. It checks whether the argument key exists in object or not. If yes, it returns the value, if not, undefined.

```
this.hasItem = function(key)
{
  return this.items.hasOwnProperty(key);
}
```

`hasItem` checks whether the given key exists in our object. It returns true/ false.

```
this.removeItem = function(key)
{
  if (this.hasItem(key)) {
    previous = this.items[key];
    this.length--;
    delete this.items[key];
    return previous;
  }
  else {
    return undefined;
  }
}
```

`removeItem` accepts a key argument and will remove the key-value pair if they exist. First it checks whether key-value pair exists by `this.hasItem(key)`. If it exists, it saves the value as `previous` (or temporary variable), updates the length by -1, deletes the key-value pair (`delete this.items[key]`), and outputs the value of the key we just deleted.

Retrospectively, if the key-value pair is not found, it returns undefined.

```
this.keys = function()
{
  var keys = [];
  for (var k in this.items) {
    if (this.hasItem(k)) {
      keys.push(k);
    }
  }
  return keys;
}
```
`keys` returns an array of all the keys found in `this.items` object. First it creates an array placeholder, `keys = [];`. Then it iterates through each key inside object. `this.hasItem(k)` checks whether each key-value pair exists. If they exist, we add the key into `keys` array using `keys.push(k)`. It outputs the `keys` array. If no keys are found, it returns an empty array.

```
this.values = function()
{
  var values = [];
  for (var k in this.items) {
    if (this.hasItem(k)) {
      values.push(this.items[k]);
    }
  }
  return values;
}
```

Is similar to `keys`, except it returns an collection of values using `values.push(this.items[k]);` instead of keys.

```
this.clear = function()
{
    this.items = {}
    this.length = 0;
}
```

`clear` resets the entire object (`items`) to `{}` and `length` to `0`.

```
this.each = function(fn) {
  for (var k in this.items) {
    if (this.hasItem(k)) {
      fn(k, this.items[k]);
    }
  }
}
```

Lastly, `each` accepts a `function` as an argument. This is similar to JS' native `.each` method where user implements a function to each array element. `each` runs user-inputted function to each key-value pair. First `(var k in this.items)` iterates the object based on keys. Then `if (this.hasItem(k))` checks whether such key exists - if it does, `fn(k, this.items[k])` runs the function. The function itself (that user provided) accepts two arguments: key (`k`) and value (`this.items[k]`).

Phew - that's it! Hash Table is pretty powerful when used properly. Its application makes it great to store configuration data, to return multiple values from function (like `.each`), and more!

Source: [javascript_hashes](http://www.mojavelinux.com/articles/javascript_hashes.html)
