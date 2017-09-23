---
layout:   post
title:    Wrapping form in Rails to use any method from any controller
tags:     [Rails, forms]
comments: true
---

One important thing to know to do in Rails is to use any method from any controller, not only from the controller of the view I am in.

Here is a case: suppose I am on `http://localhost:3000/users/6` (`views/users/show.html.erb`). Naturally, I am connected to `users_controller` and able to access the variables inside `show` method. What if I wanted to create a new availability, which has its own controller? I will need to figure out a way to access `create` method inside `availability_controller.rb`.

It is actually quite easy. Forms are very flexible and we can assign it to call *any* method from *any* controller, granted we give it a proper request method.

In this case, I want to **create** a new availability data. I need a POST request. Additionally, I am also going to display an array of numbers from 1 - 24 as dropdown select (so user can select one of the twenty-four hours). For form styling, I am using bootstrap.

```
<form method="post" action="/availability">
  <%= hidden_field_tag :authenticity_token, form_authenticity_token %>
  <select name="available_hour" class="custom-select">
    <% (1..24).to_a.each do |el| %>
      <option value=<%= el %>><%= el%></option>
    <% end %>
  </select>
  <button type="submit" class="btn btn-primary">Add Availability</button>
</form>
```

Here is the form. And just for reference, this is what I have inside availability_controller:

```
class AvailabilityController < ApplicationController
  def create
    @availability = current_user.availability.build(available_on: Date.today, available_hour: params[:available_hour])
    if @availability.save
      flash[:notice] = "New availability added!"
      redirect_to current_user
    else
      flash[:error] = "Error adding availability"
      redirect_to current_user
    end
  end
  ...
```

The create method takes a parameter, `:available_hour`, the value from the dropdown action.


<img src="https://s3-us-west-1.amazonaws.com/iggy-github-page/blog-posts/Screen+Shot+2017-09-23+at+3.08.03+PM.png" style="display: block; margin: 0 auto;" />

Here is the thing I want to emphasize:

This line is important: `<form method="post" action="/availability">`. I am wrapping my dropdown text inside the `<form>` tag. I told form to use `post` method using `availability` controller (action). You may ask where did I explicitly tell form to use `create` method. The truth is, Rails knows. Create method by default is `post` method. Index/ Show is by default a `get` method. Update is `put`. I don't need to tell Rails to look for availability_controller's create method, because by defining `/availability` action and `post` method, Rails already knows to access create method.

<img src="https://s3-us-west-1.amazonaws.com/iggy-github-page/blog-posts/Screen+Shot+2017-09-23+at+3.08.11+PM.png" style="display: block; margin: 0 auto;" />

See? To prove that it actually used `create` method from inside availability_controller, it shows the same message I wrote inside that controller. Easy!

By the way, don't forget to add `availability` resources inside `routes.rb`: `resources :availability`. Otherwise Rails won't register the actions.
