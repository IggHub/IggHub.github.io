---
layout:   post
title:    RSpec intro - 1 - model testing
tags:     [ruby-on-rails, RSpec]
comments: true
---

Let's put out RSpec knowledge into test (pun intended) right away by making a simple application we can test on. Let's generate a new app to collect shopping list.

## Generating shopping-list app

`rails new shopping-list`

We will use rails' scaffold function to generate a `List` of `items`.

`rails generate scaffold List item:string`

This creates a scaffold of `List` that takes `items` input. Don't forget to migrate the scaffold.

`rake db:migrate`

Great! With those 3 command line codes we already have a simple functioning web app. This is why Rails is super awesome. Next step, let's apply what we learned last time and install RSpec into our app.

## Installing RSpec

On `Gemfile`, if you're running Rails `5.0.0.1` like I did, you'll see a list of gems with descriptions (in `#`). You can check what version of Rails you are running by typing `rails -v`. It doesn't really matter which rails version you are running, but I would suggest to run the latest Rails (preferably >= 5) to ensure that most gems that I list here works with yours.

You'll see development and test group block (Rails >= 5)
```
group :development, :test do
  # Call 'byebug' anywhere in the code to stop execution and get a debugger console
  gem 'byebug', platform: :mri
end
```

Add RSpec (latest RSpec as I am typing this was `3.5`), `bundle install`.

```
group :development, :test do
  # Call 'byebug' anywhere in the code to stop execution and get a debugger console
  gem 'byebug', platform: :mri
  gem 'rspec-rails', '~> 3.5'
end
```
Install RSpec `rails generate rspec:install` to install `spec` folder. Delete the old `test` folder (generated automatically when we created the app for the first time). And we are done!

## Creating our first model test

There are generally 3 major things that can be tested: models, controllers, and UI. The easiest and most important one is models, so we will start there. Since our app is incredibly simple (create new list for items), our model testing will be simple. Testing complexity adds up as app complexity increases.

RSpec's folder (`spec` folder) mimics our `app`'s folder hierarchy. Our `list` model is located in `app/models/list.rb`, so to test `list.rb`, we need to create, in `spec` folder: `spec/models/list_spec.rb`. Note how it follows ` ./models/model_name_spec.rb` convention.

First thing, always include this line `require 'rails_helper`. This will include `rails_helper.rb` file located at the root `spec` folder. You can take a look inside to sort of see what that file does. I will discuss it later, but you can think of `rails_helper` as the place where you store all rspec's configs.

Since we are testing `List`, we need to let RSpec know.

```
require 'rails_helper'

describe List do
end
```
This creates a block / wrapper for `List` tests. All our `List` tests will go inside the block. Let's get testing!

## Our first RSpec model test

In `list.rb` model, add the following:

```
class List < ApplicationRecord
  validates :item, presence: true, uniqueness: true
end
```

The following will create validation to items in list. `presence` validates its existence and `uniqueness` makes sure that there are no duplicate item values in `lists` database.

```
require 'rails_helper'

describe List do
  context "valid list" do
    it 'is valid with a list' do
      list = List.new(item: "kale")
      expect(list).to be_valid
    end
  end
end
```

First we create a new item using `List.new()`, and we set our `item` to be a `kale` item. We then `expect` newly-created `list` to be valid. It will return valid because `kale` well, exists in database. `it "doesSomething" do` block describes what ought to happen/ expectation/ description of the test. `context "someContext" do` block merely adds another layer of test description. We can technically run the test with just:
```
describe List do
  it 'is valid with a list' do
    list = List.new(item: "kale")
    expect(list).to be_valid
  end
end
```
Run `rspec` on command window.

Next, we want to test various scenarios where list is invalid. Add below the first context wrapping:
```
  context "invalid list" do
    it 'is invalid without a list' do
      list = List.new(item: nil)
      expect(list).to_not be_valid
    end

    it "shows an error if invalid" do
      list = List.new(item: nil)
      list.valid?
      expect(list.errors[:item]).to include("can't be blank")
    end

    it 'is invalid when there is a duplicate item' do
      List.create(item: "spinach")
      list = List.new(item: "spinach")
      list.valid?
      expect(list.errors[:item]).to include("has already been taken")
    end
  end
```
Let's test them all. Type `rspec` on command line to test everything.

```
rspec
```

First we set `item` to be `nil` (alternatively, we could achieve same effect by `list = List.new`). Because `item` is `nil` and we already set up item presence validation, our `item` will fail validation, hence `expect(list).to_not be_valid`.

Second, we are testing the error message that Rails is complaining about. We created new list, `list = List.new`, then we checked whether `list` is valid or not (`list.valid?`). Since we know it is not valid, Rails will complain that `"Item can't be blank"`. You can see the actual message if you go to localhost server, enter new `item`, but leave it blank.

Third, recall we also set up `uniqueness` validation to prevent duplicate `item` values. First we `create` a new list, `List.create(item: "spinach")`. Unlike `new`, `create` persists the data creation (within testing scope). Then we generate new list of the same item, `"spinach"`. We then check its validity, `list.valid?`. Since we already created `"spinach"`, we can expect Rails to complain that `"spinach"` has already been taken. We can test this complain message by writing RSpec expectation `expect(list.errors[:item]).to include("has already been taken")`. Note the message `"has already been taken"` must be typed exactly.


And that's it! Our testing is still very simple. We will dig deeper into testing next time. Right now, we just want to warm up and get cozy with how RSpec does testing. Play around with the syntax, add/ remove contexts, and add one or two new model tests on your own! For a list of validations, check out [rubyonrails.org guide on  validations](http://guides.rubyonrails.org/active_record_validations.html)
