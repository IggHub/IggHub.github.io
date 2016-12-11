---
layout:   post
title:    Setting up RSpec in Rails in a minute
tags:     [ruby-on-rails, RSpec]
comments: true
---

I read [RSpec book](https://www.amazon.com/RSpec-Book-Behaviour-Development-Cucumber/dp/1934356379) to help me understand RSpec. I will share my learning progress here.
## Setting up RSpec into Rails project

If you haven't already, start making new app on Rails:

```
rails new myAwesomeTestApp --without production
```

You may ask, what is `--without production` doing there? Well, since we are learning RSpec and am not planning to deploy it, I want to save myself from unnecessary grief and not worry about production gems. This simply omits all the production gems since we are not planning to deploy it.

To install RSpec, all you need is add it into your gemfile:

```
group :development, :test do
  ...
  gem 'rspec-rails', '~> 3.4'
end
```

I usually specify the exact rspec version (in this case, `3.4`). `gem 'rspec-rails'` will do the job. By not specifying it, you are under the risk of having the gem updating itself to the latest one every time you run `bundle install/update`. This may cause the gem dependencies to break = more headache.

Specifying exact version locks the gem version in your app. To check which version is available, I usually go to [rubygems.org](https://rubygems.org/) and look for the desired gems. In this case, as I am typing this, RSpec `3.5` is the latest version.

Finally:

```
bundle install
rails generate rspec:install
```

The latter replaces `test` folder from your rails app with `spec` folder.

That's it! You are now ready to test!

## Recap:

Start RSpec in our environment with this easy 3 steps:

1. Add RSpec gem in development and test group.
```
group :development, :test do
  ...
  gem 'rspec-rails', '~> 3.4' #I would recommend using the latest version
end
```

2. `bundle install`

3. Install RSpec by running `rails generate rspec:install`. This will replace the current `test` folder (created by default when starting new rails app) with `spec` folder.


## Todo:

I learned a whole lot about RSpec and other testing tools including `faker`, `factory_girl`, and `capybara` [here](https://www.sitepoint.com/learn-the-first-best-practices-for-rails-and-rspec/). If you never had any TDD experience, I would strongly suggest to check out the site! Find out what each of those gem does.

Next: setting up other testing environment. Stay tuned!
