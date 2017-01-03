---
layout:   post
title:    RSpec intro - 2 - factorygirl baby steps
tags:     [ruby-on-rails, RSpec]
comments: true
---

Earlier we were creating new instances by doing this:

```
context "valid list" do
  it 'is valid with a list' do
    list = List.new(item: "kale")
    expect(list).to be_valid
  end
end
```

It's all good until we start creating `new` item every time we are testing.

```
context "invalid list" do
  it 'is invalid without a list' do
    list = List.new #this one is good too!
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
    list.valid? #checks validity
    expect(list.errors[:item]).to include("has already been taken")
  end
end
```

Note how on every single `it...do` block, I have a `List.new...`. This can get tedious when I have tens, if not hundreds of testing blocks. Wouldn't it be nice to create a new list once, and call it over and over again? Rails have something like that, out-of-the-box, called `fixtures`. I won't be talking about fixtures, but an alternative called `factorygirl`. For more detail, visit [factorygirl github](https://github.com/thoughtbot/factory_girl_rails). Here I will be showing how to get started with factories and I will demonstrate a few testing blocks using factories so you can start creating factories on your own!

## Setting up factorygirl

In gemfile, add (`byebug` was default when I installed Rails 5; I installed rspec-rails on last post.)

```
group :development, :test do
  gem 'byebug', platform: :mri
  gem 'rspec-rails', '~> 3.5'
  gem 'factory_girl_rails'
end
```

If you go to their main github page, you'll see this section right after gemfile part:

>Generators for factories will automatically substitute fixture (and maybe any other fixture_replacement you set). If you want to disable this feature, add the following to your application.rb file:

```
config.generators do |g|
  g.factory_girl false
end
```

It talks about generators something. What does it do? Well, whenever you invoke generators (`rails generate` / `rails g` something), you are using generators. You can change it so whenever you generate something, it will/ will not generate `factories` automatically. Configuring it to `false` will make it so that everytime you run `rails generate SOMETHING`, it won't generate `factories` for you. You can find it on `config/applications.rb`

Once you have `factory_girl_rails` on your gemfile, `bundle install`, and configured your generators (or ignore the config; your factories will still run) you are good to go.

On your `spec` folder, create a new folder named `factories`, and inside create a ruby file named your model schema. In our case, I have `spec/factories/lists.rb`

## Creating our first test

Inside `lists.rb`, add

```
FactoryGirl.define do
  factory :list do
    item "tomato"
  end
end
```

Let's test it. Back to our model spec, add the factory testing block:
```
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
  ...
```

This should pass. A few things in mind:

- The list in `factory :list do` corresponds to our model. Try replacing that with `:awesome_list` both places, and rails will complain that it doesn't know `:awesome_list` constant.
- `build` is similar to `new`. To persist data, use `FactoryGirl.create(:list)` instead.

That's it. We will learn more about factorygirl in the future. For now, try to replace all testing block with factorygirl, test it, and see if they all pass. We will learn more about factorygirl in the future, and integrate it into our model testing.
