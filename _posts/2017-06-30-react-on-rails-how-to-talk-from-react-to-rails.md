---
layout:   post
title:    How to talk from React to Rails while using react_on_rails
tags:     [rails, react]
comments: true
---

<img src="/assets/images/react_and_rails.png" alt="react n rails" style="width: 100%; display: block; margin: 0 auto;"/>

## The problem

Just like all cool kids do, I to have a React + Rails app that I am working on. At some point, you will probably wonder,

> *"Hmm, I have this React client inside my Rails app, they are inside one gigantic folder... but how do I send data from React to Rails and vice versa?"*

If you do, congrats! I too thought the same way and got mildly confused.

## The solution

The solution is actually fairly simple. They are not really connected... technically they are, but for practical purposes, they might as well not. Here is how I connect them:

### First, turn Rails into API provider

The main reason I used Rails in the first place was because I needed some sort of backend API provider, and I was going to use Rails for that. Let's say I have a model `schedule` and I want to display index data in React. Inside `schedules_controller`, I have this index method:

```
  def index
    @schedules = Schedule.all
    render json: @schedules
  end
```

Don't forget the routes:

```
  scope :api do
    resources :schedules, only: [:index, :show, :create, :update, :destroy]
    ...
```

With this, when I go to `localhost:3000_or_whatever_address/api/schedules`, I will see all schedules in JSON format. It is available for grabs.

### Second, make React to GET JSON data

To talk to APIs, I use [`fetch`](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API). I created a file name `Client.js` for my fetching activities. Inside I have `getSchedules()` method:

```
function getSchedules(cb){
  return fetch(`api/schedules`, {
    headers: {
      'Content-Type': 'application/json',
      'Accept': 'application/json'
    }
  }).then((response) => response.json()).then(cb);
};
```

In case you are wondering, `cb` stands for callback. What this does is it will go to `localhost:3000_or_whatever_address/api/schedules` and `GET` everything displayed. It will then save the `response` and convert it to JSON. Later, I will have a callback that stores them as states. Inside my React App, I imported it and created a function to use the Client.getSchedules()

```
import Client from './utils/Client';

export default class Schedule extends React.Component {
  ...
  getSchedules(){
    Client.getSchedules((schedules) => {
      this.setState(schedules)
    })
  }
  ...
}
```

I am going to walk through the steps. First, we invoke `Client.getSchedules()` method we defined earlier. Then we give it a [callback](http://javascriptissexy.com/understand-javascript-callback-functions-and-use-them/) as the argument. The callback is simply a function that updates schedules' states. Btw `this.setState(schedules)` is the same as `this.setState(schedules: schedules)`. Also, I would have already defined inside `Schedule` class a `schedules` state.

## Summary

That's it! In summary - when using react_on_rails, to communicate between Rails and React, we used Rails as a backend API provider and make React to GET the API backends. We can do this by telling Rails inside controller to render as JSON and to GET fetch from React. 
