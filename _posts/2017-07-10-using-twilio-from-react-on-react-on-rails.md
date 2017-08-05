---
layout:   post
title:    Using Twilio from React on react_on_rails
tags:     [rails, react, twilio]
comments: true
---

<img src="/assets/images/twilio_rails.png" alt="Twilio illustration" style="width: 100%; display: block; margin: 0 auto;"/>

## How to send multiple messages from React to Rails app

There are many guides how to use [Twilio-ruby](https://github.com/twilio/twilio-ruby/blob/master/README.md) gem. Twilio itself has a nice [tutorial](https://www.twilio.com/blog/2016/04/receive-and-reply-to-sms-in-rails.html) how to send (and receive) text message to one person at a time. What if you have a React app that sits on Rails that needs to use Twilio? What if you need to send several text messages in one request?

Here is what I did to send messages to *several people* in one request, from React to my Rails application.

## Setting up Twilio on Rails

I am going to skip Twilio setup on Rails. There are many resources, including this awesome [youtube tutorial](https://www.youtube.com/watch?v=vaobK_V0ue8), [twilio-ruby's README](https://github.com/twilio/twilio-ruby), and [twilio site](https://www.twilio.com/) has plenty of resources.

Assuming Twilio is working and Rails ready, let's get started:

## Create postMessage method in React

In this demo, I wanted to send a message to many workers on a specific dateTime. I created a method named `postMessage()` that accepts the actual message, an array of worker objects, and a dateTime object. Here is the method that I used:

```
function postMessage(message, workers, messageDateTime){
  let phoneArray = workers.map((worker) => {
    return PhoneHelpers.condensePhone(worker.phone)
  }).filter((el) => {return el !== ""});

  return fetch('text_it', {
    method: 'POST',
    headers: {
      'Content-Type': 'application/json',
    },
    body: JSON.stringify({
      message: message,
      phones: phoneArray,
      message_datetime: messageDateTime.getTime()
    })
  });
};
```

Let's break it apart:

```
let phoneArray = workers.map((worker) => {
  return PhoneHelpers.condensePhone(worker.phone)
}).filter((el) => {return el !== ""});
```
In my app, `workers` is an array of worker objects. It contains their name, phone number, and more. Here I wanted to extract only their phone numbers and map them in an array `phoneArray`. `PhoneHelpers.condensePhone()` is method to strip away non-numbers. The `filter` filters out all empty objects.

```
return fetch('text_it', {
  method: 'POST',
  headers: {
    'Content-Type': 'application/json',
  },
  body: JSON.stringify({
    message: message,
    phones: phoneArray,
    message_datetime: messageDateTime.getTime()
  })
});
```
This is the meat of the method. `fetch` runs `POST` method to `/text_it` address (I will cover this in a little bit). It will send an object to Rails containing `message`, `phones` (`phoneArray`), and `message_datetime` and will be converted to JSON string with [`JSON.stringify()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/JSON/stringify).

I created another method inside my React app called (coincidentally) `postMessage()`. Inside, I called the previous `postMessage()` method and gave it relevant arguments.

```
postMessage(){
  Client.postMessage(this.state.message, this.state.newWorkers, this.state.date);
};
```

### Create text_it in Rails

Recall our React `Client.postMessage()` sends a `POST` request to `.../text_it`.

Inside Rails routes, I have the a method inside `schedules_controller` called `text_dat_message` (for clarity's sake).

`post '/text_it' => 'schedules#text_dat_message'`

This is what the method looks like:

```
def text_dat_message
  phones = params[:phones] #1
  message_datetime = Time.at(params[:message_datetime] / 1000) #2
  phones.each_with_index do |num, index| #3
    HardWorker.perform_at(message_datetime + (5 * index).seconds, params[:message], num)
  end
end
```

First (`1`), we set `phones` to equal `params[:phones]`. Recall that phones inside `params[:phone]` is an array of phone numbers:

```
body: JSON.stringify({
  message: message,
  phones: phoneArray,
  ...
```

Second(`2`), we convert the `message_datetime` into dateTime that Rails understand.

Third(`3`), since `phones` is an array of phone numbers, we iterate through each one of them and assign it to a worker (`HardWorker`). I am using Sidekiq, since it uses worker, I named mine `HardWorker`. The code of `HardWorker` is:

```
class HardWorker
  include Sidekiq::Worker

  def perform(*args)
    TwilioSender.new.send_it(args[0], args[1])
  end
end
```

The worker then is nothing but a `TwilioSender` method. It takes three arguments: when, what, whom. Because I can't send multiple numbers simultaneously with Twilio, I have to send them at different time. In this case, every 5 seconds.

`message_datetime + (5 * index).seconds` sends the text every 5 second interval.

## Conclusion

That's it! In short, to send several text messages from React to Rails using Twilio, first I needed to perform a request (`POST`) to Rails' API with the information (mainly phone numbers and message). Once Rails received it, instead of sending the text message to all of the numbers simultaneously, it iterates through each number and send them at 5 seconds interval.

Happy coding!
