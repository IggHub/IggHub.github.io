---
layout:   post
title:    Informing Everyone Using Observer Method
tags:     [ruby, observer method]
comments: true
---

I like sweets. Especially donuts.

Let's say I go to a donut place called `YummyTastyDonut` (imaginary place) and I ordered a `"Fried Bacon Donut"` because I heard it is good for your cardiovascular health. Let's say after I ordered it, I decided to change it. How can I inform other class about my order change?

Now, in this situation, there are two classes involved: `Customer` (me) and `YummyTastyDonut` class. What is the best way to inform all interested parties that I changed my donut order?

## Observer method: examples

Introducing Observer method. I can create something like this:
```
class Customer
    attr_reader :name, :order
    attr_accessor :order_price

    def initialize(name, order, order_price)
        @name = name
        @order = order
        @order_price = order_price
        @observers = []
    end

	def change_order(new_order, new_price)
		@order = new_order
		@order_price = new_price
		notify_observers
	end

    def notify_observers
        @observers.each do |observer|
            observer.update(self)
        end
    end

    def add_observer(observer)
        @observers << observer
    end

    def delete_observer(observer)
        @observers.delete(observer)
    end
end

class YummyTastyDonut
    def update(changed_order)
        puts "Kitchen: Yo, change the order to #{changed_order.order}!"
        puts "Front: Order for #{changed_order.name}!"
        puts "Front: The price will now be #{changed_order.order_price}!"
    end
end
```

Let's test it:

```
$ igg = Customer.new("iggy", "Yummy Donut", 10)
=> #<Customer:0x0055d027122558 @name="iggy", @order="Yummy Donut", @order_price=10, @observers=[]>

$ donut = YummyTastyDonut.new
=> #<YummyTastyDonut:0x0055d027121ba8>

$ igg.add_observer(donut)
=> [#<YummyTastyDonut:0x0055d027121ba8>]

$ igg.change_order("Bacon Yummy Donut", 12)
Kitchen: Yo, change the order to Bacon Yummy Donut!
Front: Order for iggy!
Front: The price will now be 12!
```

Let's break it down really quick. I started out by initializing my `Customer` class that accepts 3 arguments (`:name, :order, :order_price`). I also initialized the `YummyTastyDonut` class.

Right now my `Customer` and `YummyTastyDonut` classes are still separate and are unaware of each other's existence. In `Customer` classes, note `@observer = []`. This is the heart of observer method. I can add everyone I wanted to inform regarding the changes that I made. Whenever I made a new **change**, it triggers *notification* so other classes are informed about it!

Running `igg.add_observer(donut)` appends donut into `@observer` array. Let's chack our `@observer` array:

```
$ igg.observers
undefined method `observers' for #<Customer:0x00555fb1bbd300>
```

Woops! We don't have `igg.observers` as a method. Let's go back to the code really quick and add `@observer` into our `attr_reader` so we can view it.
```
    attr_reader :name, :order, :observers
```

Remember, this makes `igg.observers` to be a valid method. Run the commands again and see `observers` now have the new `YummyTastyDonut` instance:
```
$ igg.observers
=> [#<YummyTastyDonut:0x00559ced1fdd50>]
```

## Adding more interested Classes

Since `@observers` is an array, that means I can add as many `Class` instance as I want!

Let's test it. Let's say there is a `DonutPolice` whose job is to monitor people who ordered yummy donuts.
```
class DonutPolice
	def update(changed_order)
		puts "Popo: Attention! #{changed_order.name} is now ordering #{changed_order.order} instead!"
	end
end

$ popo = DonutPolice.new
=> #<DonutPolice:0x00555fb1bb4020>
$  igg.add_observer(popo)
=> [#<YummyTastyDonut:0x00555fb1bbde40>, #<DonutPolice:0x00555fb1bb4020>]  
```

Now we have two interested classes that will get informed every time I made a change!

```
$ igg.change_order("Supreme Donut Bacon", 15)
Kitchen: Yo, change the order to Supreme Donut Bacon!
Front: Order for iggy!
Front: The price will now be 15
Popo: Attention! iggy is now ordering Supreme Donut Bacon instead!
```

## Wrapping up

There are 3 things to remember about observer method:

1. Store the interested classes in `@observer` array
2. Add/ remove interested classes
3. Notify everyone inside `@observer` array that a change has been made.

Next time, I will discuss how to refactor Observer method using modules. Stay tuned!
