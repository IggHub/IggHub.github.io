---
layout:   post
title:    Bootstrap Spacing Tricks
tags:     [bootstrap, html, css]
comments: true
---

There is a super neat trick that I recently learned to create extra spacing.


![before-spacing](http://i.imgur.com/gXIl0tr.png)

*This is what it looks like initially. My login form is too close to the header and I needed to quickly create some spacing.*

![after-spacing](http://i.imgur.com/gVwVUeE.png)

*This is what it looks like after applying the trick. With just one line of code, I can quickly create and control the amount of space I need.*

The key is to use bootstrap's [`form-group` class](https://v4-alpha.getbootstrap.com/components/forms/#form-groups). Form group stacks vertically. The text inside it is `&nbsp;`*, a non-breaking space.

`<div class="form-group">&nbsp;</div>`

Each of that adds one newline. By putting several of them together, I was able to get a nice vertical space like shown on second screenshot.

```
<div class="form-group">&nbsp;</div>
<div class="form-group">&nbsp;</div>
<div class="form-group">&nbsp;</div>
<div class="form-group">&nbsp;</div>
```

I then created a partial in rails, i.e.: `layouts/_spacing.html.erb` and put several of `form-group`s in the partial. When I need a significant vertical spacing, I summon the partial:

`<%= render :partial => 'layouts/spacing' %>`



&nbsp;


&nbsp;


*For explanation how `&nbsp;` works, refer to [this SO post](https://stackoverflow.com/questions/1357078/whats-the-difference-between-nbsp-and)*
