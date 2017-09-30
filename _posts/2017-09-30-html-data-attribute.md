---
layout:   post
title:    HTML Data Attribute
tags:     [HTML]
comments: true
---

One of the best ways to store information on HTML elements is to use `data-*` attribute. I found this trick very useful when iterating over a large number of objects and I needed to do an action on a particular iterated element. 

For example, I am iterating a lot of User's friends and their availabilities; I need to know the hour and friend ID each div represent. 

```
<% @friends.each do |friend| %>
  <% availability_of(friend) do |el| %>
    <div data-hour=<%= el %> data-user-id=<%= friend.id %>></div>
...
```

With that, each div will have a unique `data-hour` and `data-user-id`.

To retrieve the information, I can do something like this:


```
$(document).ready(function(){
  $(.'div-class').click(function(){
    var hourData = $(this).data('hour');
    var userIdData = $(this).data('userId');
    console.log('hour: ' + hourData + ' userId: ' + userId);
  })
})
```

And that's it. Each individual div I clicked will now console.log a unique hour-userId combination. I can probably see myself using this to do AJAX request on these divs.
