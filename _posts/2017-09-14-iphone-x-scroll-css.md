---
layout:   post
title:    iPhone X Scroll CSS
tags:     [CSS, jQuery]
comments: true
---

When iPhone X was released a few days ago (9/12/2017), I was impressed. Not by the price tag, but by the scroll animation on [Apple's page](https://www.apple.com/iphone-x/). I thought it was worthy to study it and recreate it on my own.


<img src="https://s3-us-west-1.amazonaws.com/iggy-github-page/front-end/iphone_x.gif" style="display: block; margin: 0 auto;" />

I will go over the key points in making this happen.

## jQuery's .scroll() and .scrollTop()

The magic is in jQuery's [`.scroll()`](https://api.jquery.com/scroll/) function. It is fired up when a scrolling is performed. Just as important, [`.scrollTop()`](https://api.jquery.com/scrollTop/) is used to track scroll location.

```
$(function(){
  //fires whenever scrolling happens
  $(window).scroll(function(){

    var newLocation = $(document).scrollTop();
    ...
```    

First, I set my `container` to have a fixed width (that way I'll know how far to scroll down and when animation should end). In this case, my height is set to be `1200px`.

```
.container {
  position: relative;
  margin: 75px auto;
  min-height: 1200px;
  max-width: 1300px;
}
```

Second, based on the container height, I plan where animation would occur. If I have a container 1200px tall and 250px later I want the object to be scaled, say three times (`scale(3)`), it is a simple math. With `newLocation`, I know exactly where my position is at all time. If I want it to zoom 3 times by the time it hits location 250, and I start at location 0, and my initial scale was 1, I need to transition 250px scrolling down and turn that into 2 scale.

Ok, that does not really making any sense. Let's look at it from different way, At position 0, my scale is 1. At position 250, my scale will be 3. At position 125 then, my scale will be 2 (assuming linear interval).

The formula is simple. If I move 1 down, I am adding to the scale 1/125 of additional scale. If I move 125 down from position 0, I will have added 125/125 of additional scale - which is 1. If I move 250 down from position 2, I will be adding 250/125 to the original scale. Simple.


This is the code that I use:

```
$(document).scrollTop();
    console.log(newLocation);
    //calculates scale decimals. We want it to increase it incrementally
    var addScale = newLocation / 62.5;
    var newScale = 1 + addScale;
    var scaleString = "scale(" + newScale + ")";

    //scales the x-box (no pun intended). Stop scaling once it hits location 125
    if(newLocation < 125 ) {
      $('.box').css({
        "transform": "translateX(-50%)" + scaleString
      });
      ...
```

The example above covers scaling. In addition to scale, I also played with opacity and position. Feel free to play around with the code below. Try to get the opacity and translate transform transition effect down on your own.

Comment below if you have any question!

<p data-height="265" data-theme-id="dark" data-slug-hash="EwxvRw" data-default-tab="js,result" data-user="iggPen" data-embed-version="2" data-pen-title="iPhone X scroll animation" class="codepen">See the Pen <a href="https://codepen.io/iggPen/pen/EwxvRw/">iPhone X scroll animation</a> by Igor Irianto (<a href="https://codepen.io/iggPen">@iggPen</a>) on <a href="https://codepen.io">CodePen</a>.</p>
<script async src="https://production-assets.codepen.io/assets/embed/ei.js"></script>
