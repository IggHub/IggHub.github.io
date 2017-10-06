---
layout:   post
title:    Ruby Nested Arguments
tags:     [Ruby]
comments: true
---

Ruby language can take nested arguments.

For example:

```
a, (b, c), d = 1, 2, 3, 4
#=> a=1, b=2, c=nil, d=3

a, (b, c), d = [1, 2, 3, 4]
#=> a=1, b=2, c=nil, d=3

a, (b, c), d = 1, [2, 3], 4
#=> a=1, b=2, c=3, d=4

a, (b, *c), d = 1, [2, 3, 4], 5
#=> a=1, b=2, c=[3,4], d=5


#To ignore values, we can use a blank splat operator
a, *, b = 1, 2, 3, 4, 5, 6
#=> a=1, b=6
```


