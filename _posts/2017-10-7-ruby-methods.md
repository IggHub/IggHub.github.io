---
layout:   post
title:    Ruby Methods
tags:     [Ruby, Methods]
comments: true
---

Ruby methods is defined with `def` in front of its name. In addition, it needs to start with lowercase or underscore. Dangerous built-in methods usually have a trailing exclamation mark (`!`) at the end. They are dangerous because they tend to modify the receiver object itself (as opposed to returning a copy of the object).

## Method arguments

Methods can take x amount of arguments or no argument.

```
def my_method(arg1, arg2)
  #do something
end
```

`my_method` in the above example, expects exactly two arguments.

Sometimes arguments are optional. In that case, we can assign an argument a default value:

```
def my_method(arg1="default1", arg2="default2")
  p arg1, arg2
end

my_method
#=> ["default1", "default2"]

my_method("Hey")
#=> ["Hey", "default2"]
```

## Splat

Sometimes we don't know how many arguments are coming in. Maybe 1, maybe 2, maybe 10. In that case, we use splat (`*`) operator. Splat operator turns argument into array.

```
def splat(arg1, *stuff)
  "arg1 is: #{arg1} and stuff are: #{stuff.inspect}"
end

splat("one", "two", "three", "four")
#=> "arg1 is: one and stuff are: [\"two\", \"three\", \"four\"]"
```

## Double Splat

Similar to splat operator, Ruby has a *double splat* operator. Instead of turning variable arguments into array like splat, double splat turns arguments into key-value hash.

```
def double_splat(**stuff)
  p stuff
end

double_splat
#=> {}

double_splat('one', 'two')
#=> ArgumentError: wrong number of arguments (2 for 0)

double_splat(uno: 'one', dos: 'two')
#=> {:uno=>"one", :dos=>"two"}
```

## Blocks inside methods

Recall Ruby methods can be associated with blocks `{|var| do_something(var)}`.

```
def triple_me(arg1)
  yield(p1*3)
end

triple_me(5) {|el| "Current value: #{el}"}
#=> "Current value: 15"
```

The recursive calling of block and method may get confusing. The way I see it is, the block argument (`el`) becomes and argument for `yield()` inside `triple_me`. `triple_me` then, in my perception, expecting *two* arguments: a normal argument and a block argument. Thus we give it a normal argument (`5`) and a block argument `{|el| ...}`.


## Return value

Every method has a return value. It may not be explicit, but Ruby assumes the last executed statement as the return value (not always).
