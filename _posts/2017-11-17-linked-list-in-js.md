---
layout:   post
title:    Linked List in JS
tags:     [Javascript, linked-list]
comments: true
---

Linked List is an important topic in Computer Science Data Structures. Here is how to implement a basic version of it in JS. Source: [Nicholas Zakas](https://www.nczonline.net/blog/2009/04/13/computer-science-in-javascript-linked-list/).

Here, I am creating a collection of items that points to another item. It is not quite like an array, because linked list uses a pointer to travel between nodes. Linked list can branch off multiple directions whereas array works more like a scalar (the example here is only two-dimensional, though)

The goal of this is to start at the `head` (first item), then if we go the `next` item, it will move the pointer to the second one. If we go to `next` one again, it will move to third one, and so on until `next = null` (no more item after that).

## Create constructor

Let's start by creating a constructor.

```
function LinkedList (){
  this._head = null;
  this._length = 0;
}
```

This constructor has a two properties: `_head` and `_length`. They are both null and 0 because in the beginning, our list is empty.

## Adding to the list

The first feature we are implementing is adding an item to the list. We want to be able to add an item (ideally a simple primitive object like string, number, or boolean) into the right side of the list. Let's add that to our `List` prototype:

```
LinkedList.prototype = {
  add: function(data){
    var node = {data: data, next: null};

    if(this._head === null) {
      this._head = node;
    } else {
      current = this._head;

      while(current.next){
        current = current.next;
      }

      current.next = node;
    }
    this._length++;
    return node;
  },

}
```


The data shape is fairly simple. It is an object with two keys: `{data: data, next: null}`. The key here is to fill next with the next node.

```
var list = new LinkedList();

list.add('Kentucky');

list.add('Fried');

list.add('Chicken');
```

Let's say we just created a new list using our List constructor. Then we added our very first item, "`Kentucky`". `this._head` will always be "Kentucky" (until we remove it). If we had stopped at that, the node (which are not available to public would look like: `{data: "Kentucky", next: null}`.

When we add the second item, `"Fried"`, what we are really adding into is `this._head`'s `next` property. Recall `current.next = node` line and recall that `node` at this time is `{data: 'Fried', next: null}`. By the time we reach `current.next = node` line, we would have updated `this._head`'s `next` to equal Fried's node. It would look something like this: `this._head = {data: "Kentucky", next: {data: "Fried", next: null}}`. You can see where this is going. We are not really going "horizontal" adding more list like array `push`. We are going deeper "inside" the list. If we add 10 items, it would probably look something like `this._head = {data: "Kentucky", next: {data: "Fried", next: {data: "Chicken", next: {//it goes 10 levels deep total}}}}`

## Lookup for an item

The second feature we need is the ability to look for an item index (like an array) and return that item. We want it to look something like:

 ```
 list.item(2) //=> "Chicken"
 list.item(10) //=> null
 ```

Pretty intuitive, I hope. Let's do this:

```
LinkedList.prototype = {
  ...

  item: function(index){
    if (index > -1 && index < this._length){
      var current = this._head;
      var i = 0;
      while(i++ < index){
        current = current.next;
      }
      return current;
    } else {
      return null;
    }
  },
}
```

## Remove an item by index

The final feature we need is the ability to remove an item based on its index. If user gives invalid index it will return null.

```
LinkedList.prototype = {
  ...

  remove: function(index){

    if (index > -1 && index < this._length){

      var current = this._head;
      var previous;
      var i = 0;

      if (index === 0){
        this._head = current.next;
      } else {
        while(i++ < index){
          previous = current;
          current = current.next;
        }
      previous.next = current.next;
    }
    this._length--;
    return current.data;            
    } else {
      return null;       
    }
  },
}
```

Awesome! So far we created an add, search, and remove function.

```
var list = new LinkedList();

list.add('Kentucky');
list.add('Fried');
list.add('Chicken');
list.item(1);
list.remove(1);
list.item(1);
```

On the next blog, I will show how to create a `reverse` function to reverse all the lists. Check out the [github repo](https://github.com/IggHub/javascript-linked-list-demo) for full code.
