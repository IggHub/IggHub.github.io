---
layout:   post
title:    Blocitoff App Review
tags:     [rails]
comments: true
---

<img src="/assets/images/blocitoff/blocitoff-superman.png" alt="wikey main page" style="width: 100%; display: block; margin: 0 auto;"/>

## Overview
Blocitoff is a classic todo app. It is the third Rails app I created. I added an additional feature to delete todos that are older than 5 minutes using rails' [custom rake tasks](http://guides.rubyonrails.org/command_line.html#custom-rake-tasks). Some of the technologies used:

1. [devise](https://github.com/plataformatec/devise) for authentication
2. [bootstrap](http://getbootstrap.com/) for layout

## Challenges

By now, I have had a few months experience with Rails. I am slowly trying to understand Rails' [MVC](https://stackoverflow.com/questions/1931335/what-is-mvc-in-ruby-on-rails)) concept. This app serves to fortify and further that knowledge.

The new concept / challenge that I encounter creating this app was to understand how scheduling works. Luckily, Rails allow creation of [custom rake tasks](http://railscasts.com/episodes/66-custom-rake-tasks). I watched [screencast](http://railscasts.com/episodes/66-custom-rake-tasks), read [articles](http://railsguides.net/how-to-generate-rake-task/) [here](http://guides.rubyonrails.org/command_line.html#custom-rake-tasks), [here](https://stackoverflow.com/questions/23049075/run-a-custom-rake-task-on-rails), and [more](http://technology.customink.com/blog/2015/02/08/customizing-rake-tasks-in-rails-41-and-higher/) to understand what rake task is and how to implement it. In the end, it was a pretty simple `task` (see what I did there? :D).


## Shipping Code
The original intention was to make it automatically delete todos 7 days older or more. I changed it to ~~two~~ five minutes so I can test it easier.

In short, the key is to create `lib/tasks/tood.rake` file, inside:

```
namespace :todo do
  desc "Delete items older than two minutes"
  task delete_items: :environment do
    Item.where("created_at <= ?", Time.now - 2.minutes).destroy_all
  end
end
```

This allow me to run rake `todo:delete_items`. It will look inside `Item`, find all items that were created between `Time.now` and `2.minutes` ago, then `destroy_all` items.

<img src="/assets/images/blocitoff/timer-impatient.gif" alt="wikey main page" style="width: 65%; display: block; margin: 0 auto;"/>
<p style="text-align: center;"><code>destroy_all</code> in the 60s</p>

## Learning experience
This project was straight-forward and simple. One of the new things I learned was Rails' rake task allow me to automate virtually anything; in this case, a simple search-and-destroy action.

This action could have been automated further using [`whenever`](https://github.com/javan/whenever) gem or [`sidekiq`](https://github.com/mperham/sidekiq) gem.

It is also worth mentioning that by doing this, I am also increasing my understanding for [Rails' opinionated frameworks](https://stackoverflow.com/questions/802050/what-is-opinionated-software).

*As always, the repo can be found [here](https://github.com/IggHub/blocitoff). If you like this, give it a star!*

![blocitoff-demo](/assets/images/blocitoff/blocitoff_demo.gif)
