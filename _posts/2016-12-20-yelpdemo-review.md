---
layout:   post
title:    Yelp (Clone) App Review
tags:     [rails]
comments: true
---

<img src="/assets/images/yelpdemo/yelp-main.png" alt="wikey main page" style="width: 100%; display: block; margin: 0 auto;"/>

## Overview
I recently found a website to learn Rails called [baserails](https://www.baserails.com/). It teaches Rails by building, not just reading. The projects were intended to be more beginner-ish (some intermediate). One of the project that I decided to tackle was creating a [yelp](https://www.yelp.com/) replica.

Some of the main gems used are:

1. [fog-aws](https://github.com/fog/fog-aws) to support Amazon Web Services
2. [mini-magick](https://github.com/minimagick/minimagick) for image-editing
3. [devise](https://github.com/plataformatec/devise) for authentication
4. [searchkick](https://github.com/ankane/searchkick) to enable search ability in Rails App.

The app allows users to edit and create reviews of existing restaurant. It also allows creation of new restaurant by admin accounts. Additionally, using searchkick gem, it allows user to quickly search for a specific restaurant.

![yelp-restaurant](/assets/images/yelpdemo/yelp-review.png)

## Challenges And Shipping Codes

There were two main challenges creating this app: limiting restaurant creation to admin user and enabling search.

### Giving some account admin access

First, we give `User` model an attribute named `admin` having boolean values. (If account is an admin, then `User.first.admin #=> true`, otherwise `false`).

Second, since we want only admin to be able to create new restaurant, we create a filter inside `restaurants_controller`:

```
before_action :check_user, except: [:index, :show, :search]

...
private


def check_user
  unless (current_user.admin?)
    redirect_to root_url, alert: "Sorry, only admin can do that"
  end
end
```

Only `index`, `show`, and `search` actions can be done by `any` user. Other restaurant-related action will require admin attribute to be true.


### Implement search

To add searching ability, `searchkick` [gem](https://github.com/ankane/searchkick) is used. To use it, [elasticsearch](https://www.elastic.co/guide/en/elasticsearch/reference/current/setup.html) needs to be installed. Then add the line `searchkick` into the `model` that will be searched. More info on setup can be found on the github [getting started](https://github.com/ankane/searchkick#getting-started) page.

Don't forget to add the gem: `gem 'searchkick'`

Inside the controller, a `search` method takes in a search params to search:

```
def search
  if params[:search].present?
    @restaurants = Restaurant.search(params[:search])
  else
    @restaurants = Restaurant.all
  end
end
```

![yelp-search](/assets/images/yelpdemo/yelp-search.png)

## Lessons


There are large variety of gems that can do practically almost everything. Take `searchkick` for example, by using it, it saved me hours of coding so I don't have to create my own search method. This is another reason why Rails is magical. Rails have a plethora list of gems that can simplify the creation of Yelp-app by many fold. I can see why many small (or large) entrepreneurs prefer to use Rails framework.

The demo app can found [here](https://iggy-yelpdemo.herokuapp.com/) and repo [here](https://github.com/IggHub/yelpdemo).
