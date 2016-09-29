---
layout:   post
title:    How Ruby Strategy Pattern Works
tags:     [ruby, ruby design patterns, strategy pattern]
comments: true
---

The second [Design Patterns](https://www.amazon.com/Design-Patterns-Ruby-Russ-Olsen/dp/0321490452) is Strategy Pattern. Compared to [last post](https://igghub.github.io/2016/09/19/template-method/)'s design pattern, Strategy Pattern does not rely on inheritance... directly.

## I love fastfood

Not gonna lie, I love fast food. I don't go them often (usually twice a week), but back when I was still young and my metabolism was super high, I would go to [in-n-out](http://www.in-n-out.com/) and [chick-fil-a](http://www.chick-fil-a.com/) almost everyday. In fact, chick-fil-a plays a big part of me meeting my wife. I will tell the story one of these days.


## Selecting different fast food places must be, well, fast.

Let's say I have a `FastFoodRestaurant` class. I want to be able to access many different fast food restaurants from `FastFoodRestaurant` class. Fast. If it takes too long my tummy will be sad :(


```
class FastFoodRestaurant
	attr_reader :meal
	attr_accessor :restaurant

	def initialize(restaurant)
		@restaurant = restaurant
		@meal = "Large fries and water cup please!"
	end

	def gimme_burger
		@restaurant.gimme_menu(self)
	end
end
```

Perfect! Now let's create two fast food classes, i.e. `ChickFilA` and `InNOut` class.

```
class ChickFilA
	def gimme_menu(context)
		puts "One chick-fil-a chicken sandwich, no pickles!"
		puts context.meal
	end
end

class InNOut
	def gimme_menu(context)
		puts "One double-double, with grilled onion!"
		puts context.meal
	end
end
```

## Gettin' the meal

To use this, run something like:

```
$ hungry_iggy = FastFoodRestaurant.new(ChickFilA.new)
=> #<FastFoodRestaurant:0x0055718bff6a20 @restaurant=#<ChickFilA:0x0055718bff6a48>, @meal="Large fries and water cup please!">

$ hungry_iggy.gimme_burger
One chick-fil-a chicken sandwich, no pickles!
Large fries and water cup please!
```
Super easy! Here are some observation:

1. Both `ChickFilA` and `InNOut` are not subclasses of `FastFoodRestaurant`. No one is inheriting anything.
2. Strategy pattern uses what's called strategies. In this example, `gimme_burger` are the strategies employed by each `ChickFilA` and `InNOut` class.
3. Strategy pattern uses context. It is kind of like superclass, except it is not. `FastFoodRestaurant` is a context

The way I think of it is: I am hungry. I want to eat foods in the *context* of Fast Food. Looking at my `FastFoodRestaurant` options, `ChickFilA` and `InNOut` are two of my *strategies* by giving me their respective burgers (`gimme_burger`)

Super easy. If I decided to add `JackInTheBox`, I can create a new class and even modify its methods without breaking anything in `FastFoodRestaurant` context.

## Todo:

Try creating a new class of your own favorite fast food restaurants - as an extra, modify the variables so instead of `meal`, you have `desserts`, `sides`, and `condiments`!
