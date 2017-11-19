---
layout:   post
title:    JavaScript Hash Tables
tags:     [Javascript, linked-list]
comments: true
---

Linked List is an important topic in Computer Science Data Structures. Here is how to implement a basic version of it in JS. Source: [Nicholas Zakas](https://www.nczonline.net/blog/2009/04/13/computer-science-in-javascript-linked-list/).

Here, I am creating a collection of items that points to another item. It is not quite like an array, because linked list uses a pointer to travel between nodes. Linked list can branch off multiple directions whereas array works more like a scalar (the example here is only two-dimensional, though)

The goal of this is to start at the `head` (first item), then if we go the `next` item, it will move the pointer to the second one. If we go to `next` one again, it will move to third one, and so on until `next = null` (no more item after that).

## First part: creating constructor

Let's start by creating a constructor.

```
function List (){
  this._head = null;
  this._length = 0;
}
```

This constructor has a two properties: `_head` and `_length`. They are both null and 0 because in the beginning, our list is empty.

## Second part: adding add

The first feature we are implementing is adding an item to the list. We want to be able to add an item (ideally a simple primitive object like string, number, or boolean) into the right side of the list. Let's add that to our `List` prototype:

```
List.prototype = {
  add: function(data){
    var node = {data: data, next: null};

    if(this._head === null) { //if there is no item yet
      this._head = node;
    } else { //if there is at least one item
      current = this._head; //initially sets up current to equal first item on the list

      while(current.next){ //while current.next is not null, we keep going until we hit the end of the list.
        current = current.next;
      }

      current.next = node; //finally, after reaching the final item, we set our new data here
    }
    this._length++; //we just added one item in the list. Need to update _length
  },
}
```


The data shape is fairly simple. It is an object with two keys: `{data: data, next: null}`. The key here is to fill next with the next node.

```
var list = new List();

list.add('Kentucky');

list.add('Fried');

list.add('Chicken');
```

Let's say we just created a new list using our List constructor. Then we added our very first item, "`Kentucky`". `this._head` will always be "Kentucky" (until we remove it). If we had stopped at that, the node (which are not available to public would look like: `{data: "Kentucky", next: null}`.

When we add the second item, `"Fried"`, what we are really adding into is `this._head`'s `next` property. Recall `current.next = node` line and recall that `node` at this time is `{data: 'Fried', next: null}`. By the time we reach `current.next = node` line, we would have updated `this._head`'s `next` to equal Fried's node. It would look something like this: `this._head = {data: "Kentucky", next: {data: "Fried", next: null}}`. You can see where this is going. We are not really going "horizontal" adding more list like array `push`. We are going deeper "inside" the list. If we add 10 items, it would probably look something like `this._head = {data: "Kentucky", next: {data: "Fried", next: {data: "Chicken", next: {//it goes 10 levels deep total}}}}`

## Third part: lookup for an item

The second feature we need is the ability to look for an item index (like an array) and return that item. We want it to look something like:

 ```
 list.item(2) //=> "Chicken"
 list.item(10) //=> null
 ```

Pretty intuitive, I hope. Let's do this:

```
List.prototype = {
  ...

  item: function(index){ //takes in index as an argument
    if(index > -1 && index > this._length){ //make sure index is at least 0 and won't be higher than total items we have in our list
      var current = this._head; //we set up current to equal our head, the very first item on the list
      var i = 0; //to start our iteration from 0

      while (i++ < index) { //this starts the while with i value being i + 1 instead of 0. This also forces the current to go up to index level deep
        current = current.next; //updates current to equal the next data object (recall current.next looks like {data: 'someData, next: //either null or more next'})
      };

      return current.data; //now that current is index level deep, the data key is what we are looking for
    } else {
      return null;
    }
  }
}
```

## Final part: remove an item

The final feature we need is the ability to remove an item based on its index. If user gives invalid index it will return null.

```
List.prototype = {
  ...

  remove: function(index){
    if(index > -1 && index > this._length){
      var current = this._head; //same as before, starts off by setting up current to equal the first item on our linked list (head)
      var i = 0;
      var previous; //keeps track of previous item

      if (index === 0){ //removing first item requires special behavior
        this._head = current.next; //if we are removing the very first item, naturally we would shift the next item into our new head.
      } else {
        while(i++ < index){
          previous = current; //sets previous as current placeholder
          current = current.next; //current is the next item, always one step ahead of previous
        }

        previous.next = current.next; //this line is the key. Recall that current is now one step ahead. Equating previous.next to current.next, it skips over the next item, essentially removing it from our linked list.
      }

      this.length--; //update length

      return current.data;
    } else {
      return null;
    }
  }
}
```

That's it! Now we can go ahead and try them:

```
var list = new List();

list.add('Kentucky');
list.add('Fried');
list.add('Chicken');
list.item(1);
list.remove(1);
list.item(1);
```
