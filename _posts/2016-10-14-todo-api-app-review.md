---
layout:   post
title:    Todo-Api App Review
tags:     [rails]
comments: true
---

<img src="/assets/images/todo-api/todo-main.png" alt="todo-api main page" style="width: 100%; display: block; margin: 0 auto;"/>

## Overview
Todo-api is one unique Rails App because it involves very little layout/ views. It allows user interaction via command line using Rails API. (If you are not sure what an API is, there is a good article about API on [freecodecamp medium page](https://medium.freecodecamp.org/what-is-an-api-in-english-please-b880a3214a82)).

Unlike conventional todo-app, user creates, edit, and deletes the todo list using command line.

![todo-command-line](/assets/images/todo-api/todo-command-line.png)

To have Rails utilize JSON Api, [active_model_serializer](https://github.com/rails-api/active_model_serializers) gem was used. Additionally, [devise](https://github.com/plataformatec/devise) was used for login authentication.

User, once logged in, can access the todo via [`curl`] (https://curl.haxx.se/). i.e.:
`curl -u admin:password http://localhost:3000/api/users`


For complete list, checkout the original repo's [readme](https://github.com/IggHub/todo-api/blob/master/README.md).

## Challenges
Making API accessible to authorized users is a big challenge. `active_model_serializer` gem was a big help, because it allowed the data to be served in JSON format.

For example, if a user needs to create a new item, a serializer named `ItemSerializer` is created. We then tell the serializers which attributes are serialized:

```
class ItemSerializer < ActiveModel::Serializer
  attributes :id, :list_id, :name, :completed
```

To prevent  *any* user to make change to the list, an extra layer of authentication is needed. In this case, I created a new method called `authenticated?`, where it prompts user to enter username and password.

```
def authenticated?
  authenticate_or_request_with_http_basic('Administration') do |username, password|
    username == 'admin' && password == 'password'
  end
end
```

For simplicity's sake, the username is set to be `"admin"` and password `"password"`.

The method is called on `before_action` method for each relevant controllers, for example, `ItemsController`:

```
class Api::ItemsController < ApiController
  before_action :authenticated?
```

## Takehome

Developing todo-api taught me about APIs and how flexible Rails can be. I can make changes to data object (`Item` and `List`) through Controllers without necessarily touching Views. With this, I learned that Rails can be used as a data center, detaching it from views.

I can use Rails in the future as an API provider and use different frameworks (Angular/ React/ Ember) to consume Rails API.

If would you excuse me, I got some more API learning todo.
