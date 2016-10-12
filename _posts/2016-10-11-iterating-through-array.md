---
layout:   post
title:    Iterating through array
tags:     [ruby, ruby design patterns, iterator method]
comments: true
---

When I hear ruby iterator, I thought of `each` method. For example, if I want to display all elements in an array, I would do:
```
array = ["first", "second", "third"]
array.each {|el| puts el}
```

Our next method is *iterator method*.

## Iterating Array manually
I can do something similar to `each` by creating a new class. Let's call it `ArrayIterator`.

```
class ArrayIterator
  def initialize(array)
    @array = array
    @index = 0
  end

  def has_next?
    @index < @array.length
  end

  def item
    @array[@index]
  end

  def next_item
    value = @array[@index]
    @index += 1
    value
  end
end
```

Let's initialize it and give it a try!
```
$ array = ['first', 'second', 'third']
$  some_array = ArrayIterator.new(array)
=> #<ArrayIterator:0x00565044a43090 @array=["first", "second", "third"], @index=0>
```

## Understanding iterator
The first part,   
```
def initialize(array)
    @array = array
    @index = 0
  end
```
Takes one argument input, usually an array, and stores that in `@array`. It also initializes `index` instance variable to have value of 0. In short, the moment I call `ArrayIterator.new`, `@array = ['first', 'second', third']` and `@index` is, well, `0`

The second part:
```
  def has_next?
    @index < @array.length
  end
```
method `has_next?` checks `@index` value and compares it with `@array.length`. Our array has length of `3`.  If we call this method right after we initialized it (right after calling`ArrayIterator.new` earlier), we will see that `@index` still has value of 0, and `@array.length` is 3. `0 < 3`. `has_next?` returns true. If we reach the end of our array (if `@index` is 3); `3 < 3` is `false`. There is nothing next. `has_next?` returns `false`.

Third part:

```
 def item
    @array[@index]
  end
```
This method  returns the element of current array index. If we had just initialized it, `@index` is still `0`; `@array[0]` will then return `"first"`.

The last method:
```
  def next_item
    value = @array[@index]
    @index += 1
    value
  end
```
returns the next item. If we had just initialized it, `@index` is `0`. `value` is `@array[0]`, which is `"first"`. `@index` is now `1`. Lastly, it returns `value`, which earlier was equated to be `"first"`. When we call the method again for the second time, `value = @array[1]`since `@index` is now `1`. It will return `"second"`.

This method works great if you want to create your own iterator! Note: it also works on string. Try this:

```
some_string = ArrayIterator.new("hola world!")
```

Do all the methods in `ArrayIterator` class and watch them work on string as they worked on array!

With `ArrayIterator` set up, we can do something very similar to `each` method:
```
 while some_array.has_next?
  puts "array element: #{some_array.next_item}"
end
array element: first
array element: second
array element: third
```

## Adding blocks
To mimic `each` method even more, we can write something like:
```
def each_element(array)
	i = 0
	while i < array.length
	yield(array[i])
		i+=1
	end
end
```

This loops over each array element. Don't forget to include a block for the `yield`. To run this, run the method and add a block after:
```
$ each_element(["uno", "dos", "tres"]) {|n| puts n}
uno
dos
tres
```
To understnad more about blocks, you can check this super concise and understandable tutorial [here](https://www.tutorialspoint.com/ruby/ruby_blocks.htm).

There you have it! Internal vs external iterator. While internal iterator is highly convenient because it is readily available, external iterator is slightly more beneficial if you want to do custom iteration (or you can just [monkey-patch](http://culttt.com/2015/06/17/what-is-monkey-patching-in-ruby/) those built-in iterators).

Till then, folks!
