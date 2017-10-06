---
layout:   post
title:    Ruby Variable Scopes
tags:     [Ruby]
comments: true
---

If we do something like this, we will have an error because `x` is defined only inside the loop.

```
[1,2,3,4,5].each do |x|
  y = x + 1
end

[x, y]
```

However, if we predefined `x`, it will update `y` and return `x` without error.

```
x = "x value"
y = "y value"

[1,2,3,4,5].each do |x|
  y = x + 1
end

[x,y]
#=> ["x value", 15]
```

Here `x` has been pre-defined, so calling it will return the `x` that was defined before the loop started. The `x` from inside the loop did not make it out because it has a different scope. `y` was not displaying `"y value"` but 15 because `y` was not defined *by* the loop. 

It may seem a little confusing at first, but if you only learn one thing, know that if a variable is defined inside a loop (i.e.: `{|a| ...}`or `...do |a|`), that variable *lives inside the loop only*. It cannot make it out of the scope it was defined from.
