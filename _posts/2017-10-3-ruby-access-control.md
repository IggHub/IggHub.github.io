---
layout:   post
title:    Ruby Access Control
tags:     [Ruby]
comments: true
---

Access control is important when creating new `Class`. When creating a new class, I need to make sure I am in control of which methods are exposed to public and methods that aren't. If I have a class to manage user's bank account, I wouldn't want anyone to see other user's checking account info. I want to limit the accessibility so user cannot view sensitive information.

In Ruby, there are three different accessibility: `public`, `private`, and `protected`. I will go over `public` and `private` here.

```
class MyWallet
  def initialize(color, balance)
    @balance = balance
    @color = color
  end

  def color
    @color
  end

  private
    def balance
      @balance
    end
end
```

Here I have `MyWallet` class example. My wallet has a color and a balance. Color is a public method, because I don't mind if the whole world knows what color is my wallet. However, I would feel uncomfortable to share how much money I have.

```
wallet = MyWallet.new("black", 50)
wallet.color
#=> "black"
wallet.balance
#=> NoMethodError
```

By putting a method in private, all private methods become unavailable. To use the private methods, I can only call it from *within* the function.

```
class MyWallet
  def initialize(color, balance)
    @balance = balance
    @color = color
  end

  def color
    @color
  end

  private
    def balance
      @balance
    end

  public
    def are_you_rich
      return "Yeah!" if (balance > 1000)
    end
end
```

The method `are_you_rich` has access to private `balance` method because they are within the same class.

```
wallet = MyWallet.new(1001)
wallet.are_you_rich
#=> "Yeah!"
```
