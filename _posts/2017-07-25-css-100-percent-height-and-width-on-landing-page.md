---
layout:   post
title:    CSS 100 Percent Height And Width On Landing Page
tags:     [html, css, javascript]
comments: true
---

![Google Docs Landing Page](/assets/images/google-docs-landing-page.png)
Not all landing pages are equal. One particular type of landing pages that caught my attention is like the one [google docs have](https://www.google.com/docs/about/). After the page is loaded, you are presented with a beautifully scaled main image with 100% width and 100% height. It fits your screen just right. Even if you resize the screen, the landing page sizes itself up nicely. Try it!

I created a simple fiddle to mimic the behavior. I wanted to share how I did that on this blog.

The fiddle can be found [here](https://jsfiddle.net/iggyfiddle/bszam38z/3/) I will briefly talk about the important parts.

## How to set up the CSS so main div is always 100% width and height

![first-fiddle](/assets/images/landing-fiddle-1.png)

My header is using `fixed` position so it remains visible when I scroll down. To get that effect, I set `width: 100%; height: 10%; position: fixed;`.

I call the main `div` `primary-content`. It needs to be on the root element/ outermost element/ sibling element with the header and other main `div`s. This is because I am using `position: absolute;`. The coordinate of `primary-content` will be relative to its parent element, which in this case, the `body`. To get the desired effect, I set `top: 10%` because the `nav` has `height: 10%;`. It makes this element to start off 10% from the top. I set `left: 0; right: 0; bottom: 0;` to stick it to the left, right, and bottom corners. After all corners are setup, I set `width: 100%; height: 90%;`. Width so it will stretch and fill the screen horizontally. Height is 90% because again, 10% is already occupied by `nav`. Lastly, I set up `background: green;` just so I can see the color of the screen. I will delete it later.

![second-fiddle](/assets/images/landing-fiddle-2.png)

To create the second, third, and more elements, I repeat the step above. The important part to create the second div is to set `position: absolute; height: 100%; width: 100%; top: 100%;`. Height and Width should be self-explanatory. Position is absolute because we want to make it relative to its parent element, the `body`. Lastly, `top: 100%;` because we want to start our div right at 100%, where the `primary-content` div ended. Feel free to play around with my fiddle. This will create a second div that fits nicely on the screen: 100% width and 100% height. It is also responsive and will change sizes as you resize screen.

That's it! Pretty simple. To learn more about CSS positions, I would highly suggest to check out [`CSSLayout`](http://learnlayout.com/) website.

If there is any question, feel free to ask below/ send me an email!
