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

I am using the codebase from last time. Let's get started:

```
LinkedList.prototype.reverse = function(){
  let head = this._head;

  if (!head || !head.next){
    return head;
  }

  var current_head = head.next;
  var reversed_head = head;
  reversed_head.next = null;

}
```
