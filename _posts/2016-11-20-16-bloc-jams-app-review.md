---
layout:   post
title:    Bloc Jams Review
tags:     [javascript]
comments: true
---

<img src="/assets/images/bloc_jams/bloc-jams-main-wide.png" alt="bloc-jams main page" style="width: 100%; display: block; margin: 0 auto;"/>

## Overview

Bloc-jams is my first attempt at Vanilla Javascript app. It is a fully-responsive web-app to replicate [spotify webplayer](https://play.spotify.com/).

Technologies used to create this app:

1. HTML, CSS, JS
2. [jquery](https://jquery.com/)
3. [BuzzJS](http://buzz.jaysalvat.com/)
4. [netlify](https://www.netlify.com/)

Some features:

1. Bloc-Jams is a responsive app

<img src="/assets/images/bloc_jams/bloc-jams-responsive.gif" alt="bloc-jams responsive" style="width: 80%; display: block; margin: 0 auto;"/>


2. Bloc-Jams is a fully capable web music player

<img src="/assets/images/bloc_jams/bloc-jams-music-player-demo.gif" alt="bloc-jams player" style="width: 50%; display: block; margin: 0 auto;"/>

## Some Challenges

Since this is my first pure JS app, I had to learn Javascript. I have had previous exposure to Javascript when working on Rails, but Ruby was the main language then. This time, Javascript is the very back bone of the app and I had to take some time to get acquainted with JS. Second challenge was to put multiple HTML pages together. Third challenge was to learn how to use Buzz library and jquery library.

## Solutions

Learning functional Javascript was not too hard. One of the best books on JS is found on github, and it is free: [You-Dont-Know-JS](https://github.com/getify/You-Dont-Know-JS) by Kyle Simpson.

Second, to make an app made up from pure HTML/CSS/JS, three HTML pages were created for Bloc-Jams: `index`, `collection`, and `album`. When deploying (using Netlify), I set the main/ publishing directory to be `index.html`. I then linked `collection` and `album` to `index`.


![folder hierarchy](/assets/images/bloc_jams/bloc-jams-folder-hierarchy.png)

The files are arranged as such: 3 main HTML pages are on root directory. All JS files are kept inside a folder named `scripts`. All CSS are stored inside `styles` folder. All images and sound files are located inside `assets`.

Bloc jams mainly utilizes [Buzz JS library](http://buzz.jaysalvat.com/) for music playing ability. Buzz APIs can be found inside its documentation [here](http://buzz.jaysalvat.com/documentation/buzz/).

Bloc jams also uses some jQuery. JQuery documentation can be found [here](https://api.jquery.com/). On the downside, since multiple HTML pages are used, jQuery must be imported on each page, making the code less DRY (`<script src="https://code.jquery.com/jquery-2.2.4.min.js"></script>`).

Another challenge is to change the icons when play button is clicked, when user hovers over the tracks, play button is displayed, when a track is clicked when no song is playing, the play button is replaced with pause button, &c. The front-end logic to create the buttons can be found inside `album.js` file inside this [repo](https://github.com/IggHub/bloc-jams/blob/master/scripts/album.js).

<img src="/assets/images/bloc_jams/bloc-jams-play-icon.png" alt="bloc-jams responsive" style="width: 75%; display: block; margin: 0 auto;"/>


## Conclusion

Bloc-jams turns out very good.
