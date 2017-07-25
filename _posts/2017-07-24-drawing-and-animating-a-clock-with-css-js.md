---
layout:   post
title:    Drawing And Animating Clock With CSS And JS
tags:     [HTML, CSS, Javascript, Animation]
comments: true
---

![Hey](/assets/images/clock-screenshot.png)
I saw an article at [CSSAnimation.rocks](https://cssanimation.rocks/clocks/) on how to animate a clock drawn in CSS. I decided to give it a try. Here is the [codepen](https://codepen.io/IggScribbz/pen/pwMGrN).

How it works:

1. Get the current datetime (`new Date();`)
2. Get the hour, minute, and second and save to their respective variables
3. Create an array of objects to point out the div class and value (`clockObj`)
4. Using selectors, iterate to select the CSS classes (.hours, .minutes, .seconds) and assign them with current datetime. It calculates the angle using basic geometry
5. When page is loaded, (`document.ready`), it assigns the hour-hand, minute-hand, and second-hand, and initialize them to their respective angles
6. I redid the code on my own just to make sure I understand it. There are some lines that I removed because I thought they were redundant. Once you think you got it down, you should totally try it on your own without looking at the code - find ways to refactor it and make it super awesome!
7. Lastly (and most importantly), the animation is done on the last 4 code blocks on CSS. It uses step timing-function to make it work like actual second/ minute hand
