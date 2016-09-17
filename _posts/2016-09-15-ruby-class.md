---
layout:   post
title:    Superman Ruby class (beginner)
tags:     [class,ruby]
comments: true
---

One of the most important feature of object-oriented language such as Ruby is creating a [Class object](https://ruby-doc.org/core-2.2.0/Class.html). Here are three things you need to know to use class!

## Creating a Class is super easy

I recently saw [Batman vs Superman](https://en.wikipedia.org/wiki/Batman_v_Superman:_Dawn_of_Justice). So let's create a `Superman` Class!

```
class Superman
end
```

... and that's it! You have just created a new class. Super!!

## Add methods inside class

That's cool. But what can you do with the newly created `Superman` class that has nothing in it?

A man of Krypton can do a lot of things; flying and super strength are two of his many super attributes. Let's add those two!

```
class Superman
  def flying
    puts "WOOSH!!!"
  end

  def super_strength
    puts "Ka-POW!!!"
  end
end
```

To initiate the class and access those methods, you need to create new instance of class.

```
clark = Superman.new
clark.flying #"WOOSH!!!"
clark.super_strength #"Ka-POW!!!"
```

Easy, right? Note that if you don't do `Superman.new` somewhere in the code, you'll get this error:

```
Superman.flying
undefined method `flying' for Superman:Class
```

## Initiate variables during class' initialization

What that means is, the moment `clark = Superman.new` is called, we can declare a variable! Usually instance variables.

```
class Superman
  attr_accessor :name, :origin

  def initialize
  	@name = "Clark Kent"
  	@origin = "Krypton"
  end

  def flying
    puts "WOOSH!!!"
  end

  def super_strength
    puts "Ka-POW!!!"
  end
end
```

The commands below are now available:

```
clark = Superman.new
=> #<Superman:0x00563cdc04f848 @name="Clark Kent", @origin="Krypton">
clark.name
=> "Clark Kent"
clark.origin
=> "Krypton"
```

Pretty super, doesn't it? `initialize`, as the name suggest, initializes when `clark = Superman.new` is called. `instance variable` `@name` and `@origin` are now available to `Superman` class. `attr_accessor` allows those variables to be read-able and write-able. Check out this [SO post](http://stackoverflow.com/questions/4370960/what-is-attr-accessor-in-ruby) for more information.

## Todo:

- Try to create a class that accepts an argument. Two arguments? What about *varying* number of arguments?

```
clark = Superman.new("Daily Planet")
```
- Create a new class on your own! Say, `Batman` class.

```
class Batman
  #dark-knight methods#
end
```
