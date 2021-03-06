---
layout:   post
title:    attr_accessor in two minutes
tags:     [attr_accessor,ruby]
comments: true
---

I have been coding with ruby for almost 9 months without really knowing what `attr_accessor` does. Yesterday it finally clicked. I will share what made it click for me. Hopefully it will help you all too!

## The best way to understand and learn is to do it.

So run your [irb](http://ruby-doc.org/stdlib-2.0.0/libdoc/irb/rdoc/IRB.html) or go to [repl.it](https://repl.it/) and type the following lines!

```
class BankAccount

  def initialize( account_owner )
    @owner = account_owner
    @balance = 0
  end

  def deposit( amount )
    @balance = @balance + amount
  end

  def withdraw( amount )
    @balance = @balance - amount
  end
end
```

This creates a BankAccount class. It accepts one argument, `account_owner`.

## The following commands are available
*Observe what is being returned and most importantly, why it works:*

```
$ bankiee = BankAccount.new("Iggy")
$ bankiee
$ bankiee.deposit(10)
$ bankiee.withdraw(5)
```

What if I want to check `owner`'s name or account `balance`?

## Unfortunately, the following commands do not work
```
$ bankiee.owner
$ bankiee.balance
```
They both return `undefined method` error. *Naturally*.

Because there is neither `def owner` nor `def balance` defined. `owner` and `balance` are both instance variables (`@`); they are not methods. What if we want to call them like we'd call a method? Do not fret!! This is where `attr_accessor` come to save the day!! `attr_accessor` makes them behave like a method so you can do `bankiee.owner` and `bankiee.balance` without defining `owner` and `balance` method!

Add `attr_accessor :balance, :owner` on the class. Now try `bankiee.owner` and `bankiee.balance` again. Voila! Magic.


## Conclusion

To put it in a level that I understand (my CS background is almost nil, so I need to make it as simple as possible for me to understand):

1. Methods are call-able after an class initiated (`bankiee = BankAccount.new('Iggy')`)
2. Balance and owner are not methods, therefore they are not call-able outside class.
3. `attr_accessor` makes variables call-able like you would call a method.

### Question to ponder:

Since `attr_accessor` act to make `balance` and `owner` behave like a method, what is another way to make `bankiee.owner` and `bankiee.balance` work without using `attr_accessor`?

[Hint](http://stackoverflow.com/questions/4370960/what-is-attr-accessor-in-ruby)
