---
layout:   post
title:    Adding other users as friends in Rails
tags:     [Rails, associations, model]
comments: true
---

When you are creating Rails project that has `User` model and you want to add other users as friends like facebook, the task can get daunting. How can I, as a user, associate with other users? I can't really do `has_many :users` inside `User` model (user has many users and belongs to user). I will explain how it works. Feel free to download/ clone the repo: https://github.com/IggHub/adding-friends-demo.

Before I start, most of the credit goes to [RailsCasts](http://railscasts.com/episodes/163-self-referential-association?autoplay=true). I am simply reiterating the cast and made minor changes (the cast was published over 8 years ago).

## Getting Started

To get started, clone the [repo](https://github.com/IggHub/adding-friends-demo) or start your new rails project. If you choose to start on your own, after `rails new ...`, I added Devise and created `User` and `Friendship` model. I scaffolded the user and created 5 random user objects.

## Friendship Model

Friendship is the cornerstone of user to user association. Friendship will be used to keep track of user relationships. For example, if I log in as the first user (`id: 1`) and want to befriend John, user #4, I need some sort of recorder. We will call it friendship.

At its core, friendship only needs two attributes: `friend_id` and `user_id`. user_id will be the current_user's id (1) and friend_id will be everyone we are adding as friend(s).

Inside Friendship model, we need to describe two things: Friendship belongs to user and Friendship belongs to *another* user. Remember, Friendship connects two users. Obviously, doing `belongs_to :user` twice is redundant. We will call *everyone else I am adding* as `friends`. Friends are still users, but it's a good way to let Rails know we are dealing with the same models in a different way. It will make sense in a second:

```
#friendship.rb

belongs_to :user
belongs_to :friend, class_name: "User"
```

That's it. We just told Rails that Friendship belongs to two users: one as use and another as friend (but friend is really user).

## User Model

To complete the belongs_to association, we need to add has_many on users.

```
#user.rb

has_many :friends, through: :friendships
has_many :friendships
```

User has many friendships is intuitive for me. However, the first line, `has_many :friends, through: :friendships` is not. How I understand is, user will not be able to access both friendships and friends.

```
u = User.first
u.friendships     #displays all friendships associated with the first user
u.friends         #displays all friends first user has
```
I can also access all my friends, because everyone whom I befriended are recorded on Friendship. Calling `u.friends` is like telling Rails to look for all Friendship objects, to give out all friends found associated with my user id.

Again, if I (user #1), befriend John (user #4), I will be creating a friendship object that looks like this:

```
 => #<Friendship id: 1, user_id: 1, friend_id: 4, created_at: "2017-09-16 19:39:20", updated_at: "2017-09-16 19:39:20">
 ```

 Conversely, I can take a deeper look at Friendship object as well:

 ```
f = Friendship.first
f.user      #displays information on user1
f.friend    #displays information on user4
 ```

## Controller

I decided to go over controller last. Once the model and associations concept are understood, it is pretty easy to come up with a controller functions. The code sample has codes for both adding and removing friends. I will only go over how to add friends. The exact same code can also be found on RailsCasts (below is the copy/paste):

```
#friendships_controller
def create
  @friendship = current_user.friendships.build(:friend_id => params[:friend_id])
  if @friendship.save
    flash[:notice] = "Added friend."
    redirect_to root_url
  else
    flash[:error] = "Unable to add friend."
    redirect_to root_url
  end
end
```

First, we (current_user) create a new Friendship object. It will take a parameter, the friendee's id. Then we save it. Simple.

Here is the View code:

```
<%= link_to "Add Friend", friendships_path(:friend_id => user), :method => :post %>
```

Whenever "Add Friend" is clicked, it goes to create (`method: :post`) while passing down an argument (`friend_id`). It takes current_user's id for Friendship's user_id and friend_id for Friendship's friend_id.

That's all! We have just added other user as our friends. The blog is by no means comprehensive. There are a few things I didn't cover: how to remove friend, how to view who added me as friend, and how to display friend. All of them are covered on the RailsCasts (thanks, Ryan Bates!) and/ or the GH repo. I would highly suggest doing this again on your own to reinforce your understanding.

Questions? Comments? Drop it below.
