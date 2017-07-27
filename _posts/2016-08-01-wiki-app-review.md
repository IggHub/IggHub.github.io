---
layout:   post
title:    Wiki App Review
tags:     [rails]
comments: true
---

<img src="/assets/images/wikey_cloney/wikey-cloney-main.png" alt="wikey main page" style="width: 100%; display: block; margin: 0 auto;"/>

## Overview
I finished ~~blocipedia~~ (*renamed Wikey-Cloney*) app. It is a mock wiki app that allows creation of wikis. In addition, premium users have the ability to add, edit, and remove collaborators to the wiki using username. The project's github can be found [here](https://github.com/IggHub/blocipedia).

The app was done using Ruby on Rails. It uses several key gems, including:

1. [devise](https://github.com/plataformatec/devise) for authentication solution
2. [pundit](https://github.com/elabs/pundit) for authentication, administration, and roles management
3. [stripe](https://stripe.com/) for payment
4. [redcarpet](https://github.com/vmg/redcarpet) for in app markdowns

... and more.

## Challenges

The biggest challenge creating this app is my unfamiliarity with Rails. I had just started wtih Rails a few months ago and have not fully grasped Rails' [convention over configuration](http://rubyonrails.org/doctrine/) paradigm.


<img src="/assets/images/random/rails_doctrine.png" alt="Drawing" style="width: 250px; display: block; margin: 0 auto;"/>

I did not understand what [MVC](https://stackoverflow.com/questions/1931335/what-is-mvc-in-ruby-on-rails) means - I knew, theoretically, what M, V, and C stand for and what their "definition" is, but I still haven't grasped how they made work integrally in Rails. It's like driving a car for the first time after watching youtube tutorials (although I learned to drive before YouTube went insanely viral).

## Delivering the Code

The best way to learn is by doing. Since I did not have much knowledge to build from scratch, I had to deconstruct and reused some components from my first (guided) app, [bloccit](https://github.com/igghub/bloccit).

One of my first major challenge is to use devise for the very first time. There were a lot of convention that goes into the gem. Back then, I was not very familiar with authentication problem. I remember getting very confused commenting/ uncommenting the modules. I was able to pass the obstacle by reading the [getting started guide](https://github.com/plataformatec/devise#getting-started) and thanks to the people at [Riverside Ruby Meetup](https://www.meetup.com/Riverside-Ruby-User-Group/).

## Lessons Learned

Although I did not become a Rails expert after finishing this project, I learned a little more about MVC, about the gems I used, and the thought process of developing new app.

I learned that views share the route defined inside `routes.rb`, and variables in a view is determined from controllers, that controllers are associated to a route, and that a route mostly determines what action is being done ([CRUD](http://edgeguides.rubyonrails.org/getting_started.html#getting-up-and-running)) - while a route handles the rest of logic.

In addition, I learned how to get up and running with Devise. Learning Devise and Pundit helped me to understand a little more on authentication.

This app could definitely be refactored and done better. However, as a Rails beginner, I need to try out new things and improve the old things - by building new apps and going back to old apps.

*Don't forget to visit the [repo](https://github.com/IggHub/blocipedia) and give it a star if you like it!*
