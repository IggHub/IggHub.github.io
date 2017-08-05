---
layout:   post
title:    Sundae-App Review (WIP)
tags:     [rails, react]
comments: true
---

<img src="/assets/images/sundae/sundae-main.png" alt="sundae app page" style="width: 100%; display: block; margin: 0 auto;"/>

## Overview

Sundae-app is the latest project I am working on. It is a Sunday School management app aimed for church volunteers/ leaders. Being one of the leaders, I realized keeping scores from our [Awana](https://www.awana.org/) program weekly is no easy task. In addition, alternating between different people to teach every week is also another daunting task. Sundae-app was created to meet those needs.

Sundae-app is a [Rails](http://rubyonrails.org/) backend app as an API provider coupled with [React](https://facebook.github.io/react/) frontend using [react on rails](https://github.com/shakacode/react_on_rails) gem. It currently has two 'drawers' (features): reminder and scorer.

### Reminder

<img src="/assets/images/sundae/sundae-reminder-main.png" alt="sundae app reminder page" style="width: 80%; display: block; margin: 0 auto;"/>

Reminder allows users to create sticky notes of schedules. Once a date is determined, the listee will receive a text message a day before at 7:00 PM. The text-messaging ability utilizes [twilio](https://www.twilio.com/)'s [gem](https://github.com/twilio/twilio-ruby).

<img src="/assets/images/sundae/sundae-reminder-new.png" alt="sundae app reminder new" style="width: 50%; display: block; margin: 0 auto;"/>

User has the ability to add between one to three contacts followed by a message less than 140 characters long (~~following Twitter's 140 character limits~~).

### Scorer

<img src="/assets/images/sundae/sundae-scorer-table.png" alt="sundae app scorer table" style="width: 80%; display: block; margin: 0 auto;"/>

Scorer allows user to input weekly points in tabular format. Every week, user can `add` new week and enter the score of each students depending on their performance. Scorer comes with two charts: average and cumulative charts.

<img src="/assets/images/sundae/sundae-scorer-cumulative.png" alt="sundae app scorer table" style="width: 90%; display: block; margin: 0 auto;"/>

Cumulative chart measures student's individual performance and visualizes the goal point (red line).

<img src="/assets/images/sundae/sundae-scorer-average.png" alt="sundae app scorer table" style="width: 90%; display: block; margin: 0 auto;"/>

Average chart displays two area charts: one of the individual student's and another the class average, to measure how that individual performs weekly.

The charts are done using [recharts](http://recharts.org/#/en-US/) in React.

All in all, Sundae-app is not yet complete. It is currently still being developed as more features are added and existing features improved. For demo, visit [Sundae-app site](http://todayissundae.herokuapp.com/)

## Challenges

Some of the challenges creating Sundae-app so far:

### Choosing the system to integrate React + Rails

There were several options to use React inside Rails: [react-rails gem](https://github.com/reactjs/react-rails), [react_on_rails](https://github.com/shakacode/react_on_rails), and build separate Rails and React app. For more info, check out [this informative post](https://learnetto.com/blog/3-ways-to-use-react-with-ruby-on-rails-5) about how to implement each methods. I chose the second, react on rails, because it has a thorough tutorial how to get started all the way to heroku deployment. Tutorial can be found [here](https://github.com/shakacode/react_on_rails/blob/master/docs/tutorial.md).

### Communicating between React and Rails

Although react_on_rails places React inside Rails app, they are still, technically, separate entity. In order to communicate to Rails while using React, an API request. They were discussed on my previous posts {% if jekyll.environment == 'production' %}<a href="{{site.gh_pages_url}}/2017/06/30/react-on-rails-how-to-talk-from-react-to-rails/">HERE</a>{% else %}<a href="{{site.url}}/2017/06/30/react-on-rails-how-to-talk-from-react-to-rails/">HERE</a>{% endif %} and {% if jekyll.environment == 'production' %}<a href="{{site.gh_pages_url}}/2017/07/08/better-way-to-fetch-multiple-json/">THERE</a>{% else %}<a href="{{site.url}}/2017/07/08/better-way-to-fetch-multiple-json/">THERE</a>{% endif %}

### Using Twilio service from React frontend

The Reminder feature uses Twilio. Twilio sits in the backend on Rails. The Reminder page itself is created on React frontend. How can I connect those two together? I addressed the problem on this post {% if jekyll.environment == 'production' %}<a href="{{site.gh_pages_url}}/2017/07/10/using-twilio-from-react-on-react-on-rails/">HERE</a>{% else %}<a href="{{site.url}}/2017/07/10/using-twilio-from-react-on-react-on-rails/">HERE</a>{% endif %}.

### Using Sidekiq with Twilio and deploying it on heroku

The Reminder feature reminds people at certain time relative to the date the user chose. In order for Twilio to send the text message at the right time, a worker is needed. [Sidekiq](https://github.com/mperham/sidekiq) was the perfect sidekick for the job (and I am not apologizing for the pun >=D). Sidekiq itself is fairly easy to setup on development environment. Setting up sidekiq on heroku however, can be intimidating. This post HERE addresses how to run redis-to-go on heroku to run sidekiq worker.

### Structuring and iterating multiple student models in React

The Scorer feature iterates through an array of `students` array of objects. It tabulates the `points` attribute per student, per week. Keeping the code DRY was quite challenging. In the end, I managed to create a table cell component (`<td>`) and reused that on each column and each row. An example on how it was done was shown HERE

### Choosing a specific student when iterating in React

Under each student's table was `Display Chart` button. Recall the previous point where students are product of iteration. How then, did I make React to know which student was clicked? The issue was addressed on this post HERE


... (create most posts that addresses each one of these issues. like how to use fetch. How to use recharts. How to use Twilio.)

## What's next
