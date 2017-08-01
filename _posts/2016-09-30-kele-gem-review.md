---
layout:   post
title:    Kele Gem Review
tags:     [ruby]
comments: true
---

<img src="/assets/images/kele/Kele-Okereke.jpg" alt="Kele Okereke" style="width: 100%; display: block; margin: 0 auto;"/>

*Just kidding, this is not about that [Kele](https://en.wikipedia.org/wiki/Kele_Okereke), but [Kele gem](https://github.com/IggHub/kele) I created*

## Overview

This app is an attempt to create a [RubyGem](https://rubygems.org/) as an API client to communicate to [Bloc](https://www.bloc.io)'s [JSON Web Token](https://jwt.io/) to get student information. A similar guide to create RubyGems can be found in RubyGem [website](http://guides.rubygems.org/make-your-own-gem/). More info on Bloc's API endpoints is listed [here](http://docs.blocapi.apiary.io/#).

<img src="/assets/images/kele/kele-gif.gif" alt="Kele Okereke" style="width: 30%; display: block; margin: 0 auto;"/>

This app can be tested on `irb`. To test it - first, a Bloc username and password is required. Second, download/ fork the project from the [repo](https://github.com/IggHub/kele). Third, run `irb -r kele -I ./lib`. The APIs for `Kele` is listed on [README.md](https://github.com/IggHub/kele/blob/master/README.md)

## Challenges

The first challenge is to create RubyGem. Thankfully, the [guide](http://guides.rubygems.org/make-your-own-gem/) on RubyGems site laid the process out simply. There are few patterns to follow. I will pick-and-choose the patterns, for more details, check out the [site](http://guides.rubygems.org/rubygems-basics/) for more info. The basic RubyGem conventions are:

### Have one Ruby file with the same name as the gem

```
% tree
.
├── shiny.gemspec
└── lib
    └── shiny.rb
```

To create a gem named `shiny`, a `shiny.rb` is created inside `lib` folder. Specs *about* the gem is created in the root path named `gem_name.gemspec`.

### Create a Class with the name of the gem inside the main ruby file

```
class Shiny
  def self.glitter
    puts "Bling Bling!!"
  end
end
```

This will create a `glitter` method.

Inside the `.gemspec` file, put all information:

```
Gem::Specification.new do |s|
  s.name        = 'shiny'
  s.version     = '0.0.1'
  s.date        = '2016-09-30'
  s.summary     = "Woooo.. so shiny!!"
  s.description = "The most brilliant gem in the world"
  s.authors     = ["Iggy So Shiny"]
  s.email       = 'iggy@bling.co'
  s.files       = ["lib/shiny.rb"]
  s.homepage    =
    'http://rubygems.org/gems/shiny'
  s.license       = 'MIT'
end
```

More Gem Specification parameters can be found [here](http://guides.rubygems.org/specification-reference/).

### Run `gem build`

```
 gem build shiny.gemspec
```

Then install it:

```
 gem install ./shiny-0.0.0.gem
```

### Once done, don't forget to require it

Inside `irb`, run `require 'shiny'`

Since we created inside `Shiny` class a method called `glitter`, we can run

```
Shiny.glitter #=> "Bling Bling!"
```

And that's it! Super simple. To recap: we need to create a specific structure, create a ruby file with the same name as the gem's intended name, create `.gemspec` file with all specs parameters, `build` it, and `install` it.

The final step is to `push` it to `RubyGems`

```
gem push shiny-0.0.0.gem
```

That's the gist of it.

To create a gem specific to our need - namely to function as an API Client to Bloc, we need to communicate to Bloc's external-facing API itself. We can do the communicating using [httparty](https://github.com/jnunemaker/httparty) and we will set it inside `initialize` method. The URI is `https://www.bloc.io/api/v1/sessions`.

```
def initialize(email, password)
  response = self.class.post("https://www.bloc.io/api/v1/sessions", body: {"email": email, "password": password})
  raise "invalid email/pass" if response.code != 200
  @auth_token = response["auth_token"]
end
```

Thus in `irb`, we can do

```
kele_client = Kele.new('myAwesomeEmail@bloc.com', 'mySuperSecretPassword')
```

Once we are approved, we can get the information we need. For example, to get information about me, I can create a method:
```
def get_me
  url = "https://www.bloc.io/api/v1/users/me"
  response = self.class.get(url, headers: { "authorization" => @auth_token })
  body = JSON.parse(response.body)
end
```

Running `Kele.new('myAwesomeEmail@bloc.com', 'mySuperSecretPassword').get_me` gives:

<img src="/assets/images/kele/kele_get_me.png" alt="kele get_me" style="width: 75%; display: block; margin: 0 auto;"/>

For url APIs, check out [Bloc's API Documentation](http://docs.blocapi.apiary.io/#reference/0/sessions/show-roadmap).

## Outcomes

At the heart RubyGem is functionality. Gems are created to shorten many otherwise lengthy processes. After creating Kele gem, I learned that the process is not nearly as intimidating as I thought. Creating new gem could be done as fast as five minutes.

In addition to learning the basics of gem creation, I also learned about making external API request. Using httparty, I connected Kele to Bloc API to fetch necessary information. With this API knowledge, I can, in the future, create an app to connect to bigger APIs (i.e. Yelp, Facebook, Twitter) to create complex applications!
