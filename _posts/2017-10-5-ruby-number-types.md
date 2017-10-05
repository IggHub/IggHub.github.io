---
layout:   post
title:    Ruby Number Types
tags:     [Ruby, Number]
comments: true
---

Ruby numbers can support integer, floats, rational, and complex numbers. There are two types of Ruby number types: `Bignum` and `Fixnum`.

Numbers within (-2<sup>30</sup> .. 2<sup>30</sup> -1) or (-2<sup>62</sup> .. 2<sup>62</sup> -1) belong to `Fixnum` class and are held in binary form. Anything outside that range fall under `Bignum` class.

```
100.class
#=> Fixnum

1000000000000000000000000000000000000000.class
#=> Bignum
```

When two (or more) numbers interact, the resulting class will be the more generic one. If two or more numbers from the same class interact, it will return the same class. For example, adding an integer with another integer will return another integer. Adding an integer with a float will return a float because float is more generic than integer.

```
5 + 5
#=> 10
5 + 5.0
#=> 10.0
5 + Complex(1,2)
#=> (6 + 2i)
5 + Rational(1,2)
#=> (11/2)
5.0 + Rational(1,2)
#=> 5.5
```
