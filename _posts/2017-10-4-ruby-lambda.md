---
layout:   post
title:    Ruby Lambda
tags:     [Ruby, lambda]
comments: true
---

Ruby has a lambda function that can be used to take block of action as an argument. Take this for example:

```
def give_me_action(&action)
  action
end

say_something = give_me_action {|el| puts "Did you just say #{el}?"}
#=>  #<Proc:0x007fda231a4fc8@(irb):20>

say_something.call "Hello world"
#=> "Did you just say Hello World?"

say_something.call 1000
#=> "Did you just say 1000?"

```

What happened above is, I defined a method, `give_me_action` and told Ruby to *expect* a block as an argument (the ampersand *&*). When I assigned it to `say_something`, instead of passing down a parameter like usual (`give_me_action(some_params)`), I gave it a block (`{|el| puts "Did you just say #{el}?"}`) to print a string.

The next time I call `say_something`, I give it a variable (the block accepts one param) and it will execute whatever block I gave.

This is similar to what Lambda function in Ruby does.

```
say_what = lambda {|el| puts "Did you just say #{el}?"}
#=> #<Proc:0x007fda23187f90@(irb):23 (lambda)>

say_what.call "hello lambda"
#=> Did you just say hello lambda?
```

Simple. Lambda allows you to assign a variable and give it a block instead of regular parameter. You can then `call` that variable and it will execute that block!
