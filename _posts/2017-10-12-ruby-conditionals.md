---
layout:   post
title:    Ruby Conditionals
tags:     [Ruby]
comments: true
---

Ruby has basic conditionals involving `true`, `false`, and `nil` values. Some examples are:

```
nil && 100
#=> nil

false && 100
#=> false

"stuff" && 100
#=> "stuff"

nil || 100
#=> 100

false || 100
#=> 100

"more stuff" || 100
#=> "more stuff

```

Similar to most other languages, Ruby also has its own true/false and/ or operator. In addition, it also has `nil` comparison. 

A useful trick to assign value to variable *only if* that variable is not already set is to use `||=` operator.

```
my_var ||= 100
```

The code above will assign value of 100 to `my_var` if and only if `my_var` has never been assigned value before. It is similar to `nil || 100 #=> 100` example.
