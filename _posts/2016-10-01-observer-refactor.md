---
layout:   post
title:    Refactoring Observer Method
tags:     [ruby, ruby design patterns, observer method]
comments: true
---

Last time we left off with this code using observer method in `Customer` class.

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

I am going to create a slight modification to my current code. The original method from [Ruby Design Patterns](https://www.amazon.com/Design-Patterns-Ruby-Russ-Olsen/dp/0321490452) refers observer code as having only one argument. Instead of having a method `change_order(new_order, new_price)` that takes 2 arguments, we will remove `change_order` method and add this instead:


```
  def order=(new_order)
    @order = new_order
    notify_observers
  end
```

Also since we are removing `order_price`, we will remove it as well from the class:

```
class YummyTastyDonut
    def update(changed_order)
        puts "Kitchen: Yo, change the order to #{changed_order.order}!"
        puts "Front: Order for #{changed_order.name}!"
    end
end
```

Try to run the following code above like before; instead of informing them about changed order and price, change just the order.

To use observer method with inheritance, we can do something like this:

```
class Subject
  def initialize
    @observers = []
  end

  def add_observer(observer)
    @observers << observer
  end

  def delete_observer(observer)
    @observers.delete(observer)
  end

  def notify_observers
    @observers.each do |observer|
      observer.update(self)
    end
  end
end
```

We then make `Customer` a subclass of `Subject`:

```
class Customer < Subject
  att_reader :name
  attr_accessor :order

  def initialize(name, order)
    super()
    @name = name
    @order = order
  end

  def order=(new_order)
    @order = new_order
    notify_observers
  end
end
```

This cleans the code up a little bit. However, there is a subtle problem: `Customer` is a subclass of `Subject`. What if I want to make `Customer` a subclass of something else like `BusinessProspects`, `Visitors`, or `DonutLovers`? Once a `Customer` becomes subclass of `Subject`, it cannot be a subclass of anything else.

Do not fret! To solve this problem, we can use... dun-dun-dun... module! Module is package of methods. Observer is nothing but packages of observer methods.

```
module Subject

  attr_reader :name
  attr_accessor :order

  def initialize
    @observers = []
  end


  def add_observer(observer)
    @observers << observer
  end

  def delete_observer(observer)
    @observers.delete(observer)
  end

  def notify_observers
    @observers.each do |observer|
      observer.update(self)
    end
  end  
end
```

And make sure to `include Subject` under `Customer` class.

```
class Customer
  include Subject

  def initialize(name, order)
    super()
    @name = name
    @order = order
  end

  def order=(new_order)
    @order = new_order
    notify_observers
  end
end
```

Lastly, there is a built-in ruby method called `Observable`!


```
require 'observer'

class `Customer`
  `include Observable`

  attr_reader :name
  attr_accessor :salary
  ...

```

Run the code:

```
$ donut = YummyTastyDonut.new
=> #<YummyTastyDonut:0x0055b59d24d940>
$ iggy = Customer.new("Iggy", "Tasty Donut")
=> #<Customer:0x0055b59d24cec8 @observers=[], @name="Iggy", @order="Tasty Donut">

$ iggy.add_observer(donut)
=> [#<YummyTastyDonut:0x0055b59d24d940>]

$ iggy.order = "Double Bacon Yummy Donut"
Kitchen: Yo, change the order to Double Bacon Yummy Donut!
Front: Order for Iggy!
=> "Double Bacon Yummy Donut"
```

## Challenging bonus - Ruby observer pattern

There is a Ruby `Observable` pattern built-in in Ruby (Source: [ruby-doc](http://ruby-doc.org/stdlib-1.9.3/libdoc/observer/rdoc/Observable.html)). The code will look something like this:

```
require 'observer'

class Customer

  include Observable

  attr_reader :name
  attr_accessor :order

  def initialize( name, order)
    @name = name
    @order = order
  end

  def order=(new_order)
    @order = new_order
    changed notify_observers(self)
  end
end
```

Bonus question: How can we run it the `Observable` pattern like we run the observer method? What are some advantages and disadvantages of using `Observable`?
