---
layout:   post
title:    Reversing items in Linked List
tags:     [Javascript, linked-list]
comments: true
---

On the [last blog](https://igghub.github.io/2017/11/17/linked-list-in-js/) we explored how to create a basic linked list and added a few basic functionalities: `add`, `item`, and `remove`. Now I will show you how to create a function that reverses all the linked list items. This question, sometimes come up in coding interview.


```
HEAD -> 5 -> 12 -> 25 -> NULL

//link.reverse()

HEAD -> 25 -> 12 -> 5 -> NULL
```

I am using the codebase from last time. You can find the completed code [on this github repo](https://github.com/IggHub/javascript-linked-list-demo). Let's get started:

```
LinkedList.prototype.reverse = function(){
  var head = this._head;

  if (!head || !head.next){
    return head;
  }

  var current_head = head.next;
  var reversed_head = head;
  reversed_head.next = null;

  while(current_head){
    var temp = current_head;
    current_head = current_head.next;
    temp.next = reversed_head;
    reversed_head = temp;
  };

  this._head = reversed_head;
  return reversed_head;
}
```

This method has a runtime complexity of O(n) because it goes through the array once. It has a memory complexity of O(1).

Essentially, we are going through the entire list once from HEAD to NULL. We start by detaching the first item (head) and putting it to the end with `reversed_head = null;`. We make the item next to our former head the temporary head (`current_head`). Then we slowly build our list backwards (starting from NULL to HEAD).
