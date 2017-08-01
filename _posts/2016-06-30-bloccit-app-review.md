---
layout:   post
title:    Bloccit App Review
tags:     [rails]
comments: true
---

<img src="/assets/images/bloccit/bloccit_teaser.png" alt="bloccit main page" style="width: 100%; display: block; margin: 0 auto;"/>

## Overview
[Bloccit](https://github.com/IggHub/bloccit) is the first ever completed [Ruby on Rails](http://rubyonrails.org/) app I made with the help of [bloc](http://bloc.io/). It is a [reddit](https://www.reddit.com/) replica with many topics (user-generated), where users can vote up/ down, and add comments. Some of technologies used to create Bloccit:

1. Ruby on Rails (framework)
2. [Heroku](https://www.heroku.com/) (deploy)



Rails app uses many gems/ add-ons. Some of the important gems used:

1. [bootstrap-sass](https://github.com/twbs/bootstrap-sass) for design framework
2. [bcrypt](https://github.com/codahale/bcrypt-ruby) for password hashing

Additionally, some of the gems for testings include:

3. [rspec-rails](https://github.com/rspec/rspec-rails) Rails' popular testing framework
4. [shoulda](https://github.com/thoughtbot/shoulda) to shorten testing codes and make it human-readable
5. [factory_girl_rails](https://github.com/thoughtbot/factory_girl_rails) - factory generator for testing

## Challenges

Being the first ever Rails app, there were a lot of mysteries surrounding it. The first problem was to get it set-up on my machine. The second was to understand the purpose of each folder/ file inside a new rails app, and the third was to find places where I can seek for help if I get stuck.

## Ship Code!

For the first challenge, I visited [installrails](http://installrails.com/) - it guided me step-by-step how to install Rails into Mac. Additionally, SO has resources on installing Rails as well, i.e. [here](https://stackoverflow.com/questions/6840719/installing-ruby-on-rails-mac-os-lion). Being a popular framework comes with many perks, one of them being a great community support. Any imaginable question under the sun has probably been asked and possibly answered. This is one reasons why it is better for beginner in development like me to use (older)framework with great support.

After I ran my first `rails new bloccit`, all I saw was a lot of random folders with files filled with strange writings. This was challenging and could be very disheartening for beginner. Luckily, Bloc offered a step-by-step guide and explanation. After learning Rails' MVC and Rails' opinion, I took a dive into the `app` folder, where most development occurred. When faced with something completely new, it helps to fly high before diving deep.

Finding a community of great support helps tremendously. [SO](https://stackoverflow.com/) has been a consistent lifesaver. Additionally, I joined a local ruby [meetup](https://www.meetup.com/) where similar developers are meeting twice a month. I was able to find help from Ruby masters.

## What I Learned

Ruby on Rails is an [opinionated framework](http://guides.rubyonrails.org/getting_started.html#what-is-rails-questionmark). There is the correct "Rails Way" of developing to increase productivity.

Finding a framework that have great community support is also very helpful whenever I get stuck on a problem (happens about once or twice a day), this could sometimes outweigh learning the newest, shiniest framework that nobody has heard before.

Rails is an amazing framework. Ruby itself is a clear language, human-readable (more than most its contemporaries), and have matured enough to be stable and popular. Many small startups choose Ruby on Rails for [many reasons](https://www.quora.com/Why-do-so-many-startups-use-Ruby-on-Rails). Although Rails is opinionated any may be counterintuitive for beginners, once I grasp how to work around Rails, I found myself advocating RoR more and more.

I realized I did not really talk about technical parts of Bloccit but talked more about Rails. The repo can be found [here](https://github.com/IggHub/bloccit). Feel free to give it a star if you find it helpful!

<img src="/assets/images/bloccit/bloccit-rails.jpg" alt="rails image" style="width: 100%; display: block; margin: 0 auto;"/>
