---
layout:   post
title:    RSpec intro - 3 - factorygirl model testing continued
tags:     [rails, rspec]
comments: true
---

Let's continue on with factories by refactoring our model specs to make it more concise.

```
require 'rails_helper'

describe List do
  it "has a valid factory" do
    expect(FactoryGirl.build(:list)).to be_valid
  end

  context "valid list" do
    it 'is valid with a list' do
      list = List.new(item: "kale")
      expect(list).to be_valid
    end
  end

  context "invalid list" do
    it 'is invalid without a list' do
      list = List.new
      expect(list).to_not be_valid
    end
    it "shows an error if invalid" do
      list = List.new(item: nil)
      list.valid?
      expect(list.errors[:item]).to include("can't be blank")
    end
    it 'is invalid when there is a duplicate item' do
      List.create(item: "spinach")
      list = List.new(item: "spinach"
      list.valid?
      expect(list.errors[:item]).to include("has already been taken")
    end
  end
end

```

## Refactor to include FactoryGirl

The best way for me to learn is to break things up. Let's break this code first.
```
context "invalid list" do
  it 'is invalid without a list' do
    list = FactoryGirl.build(:list, item: nil)
    list.valid?
    expect(list).to be_valid
  end
  ...
```

Naturally, to make it pass:
```
context "invalid list" do
  it 'is invalid without a list' do
    list = FactoryGirl.build(:list, item: nil)
    list.valid?
    expect(list).to_not be_valid
  end
  ...
```

What we just did is we refactored `list = List.new` (or `List.new(item:nil)`) with `FactoryGirl`. Try to refactor the second one (`shows and error when invalid`) and the third one (`is invalid when there is a duplicate item`) on your own. I will list it below, try it first, and look at mine if you get stuck.

Ok, let's refactor the rest of `invalid list` section:

```
...
context "invalid list" do
    it 'is invalid without a list' do
      list = FactoryGirl.build(:list, item: nil)
      list.valid?
      expect(list).to_not be_valid
    end

    it "shows an error if invalid" do
      list = FactoryGirl.build(:list, item: nil)
      list.valid?
      expect(list.errors[:item]).to include("can't be blank")
    end

    it 'is invalid when there is a duplicate item' do
      FactoryGirl.create(:list, item: 'spinach')
      list = FactoryGirl.build(:list, item: 'spinach')
      list.valid?
      expect(list.errors[:item]).to include("has already been taken")
    end
  end
end
```

Great. All our tests are using `FactoryGirls` now. Again, `FactoryGirl.create(some_list)` creates a *persistent* list instance, while `FactoryGirl.build(other_list)` builds a `non-persistent` list object.

## Shorten syntax

Meanwhile, looking at the code, I bet you're probably thinking what I'm thinking: wouldn't it be nice if we can shorten the syntax a little bit? Typing `FactoryGirl.build(:list)` is a bit cumbersome. If I can type `build(:list)` instead of typing the whole `FactoryGirl`, it would be sweet. But you can! Go to `spec/rails_helper.rb` and add `config.include FactoryGirl::Syntax::Methods`. This enables us to shorten the syntax method by eliminating the need to type `FactoryGirl.` Refactor our code again and remove `FactoryGirl.build(something)` into `build(something)` (do the same with `create` method).

Run `rspec` and it should pass.

## Generate fake data

If you're like me, then you are probably thinking, wouldn't it be nice if we can generate a random grocery name instead of hard-coding it? If you think that way, then you're right! We could automate the process with a gem called [faker](https://github.com/stympy/faker). To install, add to `:development` and `:test` gemfile section `gem 'faker'` and `bundle install`. You can read the gem github page for more information, but essentially it generates random name for our testing. Since we are grocery shopping, we will stick with [ingredients](https://github.com/stympy/faker/blob/master/doc/food.md) generator. Try to run this on `rails console` after installing the gem to see it in action: `Faker::Food.ingredient`.

In `specs/factories/lists.rb`, add

```
  factory :list do
    item Faker::Food.ingredient
  end
```

We learned two new things:

1. `FactoryGirl.build` (or `create`) builds/creates new object instance, just like you would using `List.new` or `List.create`; additionally, `create` creates persistent object while `build` creates transient object.
2. We can shorten the syntax by modifying `rails_helper` to eliminate the need of typing class instance `FactoryGirl` everytime we are generating new factory.
3. Lastly, we used `Faker` gem to generate random shopping list.

Next time, we will move on from `model` testing to `controller` testing. Stay tuned!
