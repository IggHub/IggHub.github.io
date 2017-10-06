---
layout:   post
title:    Regex in Ruby
tags:     [Ruby, Regex]
comments: true
---

Most of the time, I am dealing with Strings in Ruby (or any other language). It is not an understatement to say that Regular Expression (Regex) is very important.

I remember feeling very overwhelmed when I first learned Regex for the first time few years ago. I will go over the very basics of Regex to get started.

## Why learn Regex

Regex is a powerful tool for string manipulation. It can, in essence, do the three followings:

- Test a string for pattern matching
- Extract a string with matching all/ some pattern
- Substitute the string and replacing parts that match the pattern

Match, extract, and replace. Pretty much almost everything we need when playing with string.

## How to use Regex

Using Regex in Ruby is as simple as `/abc/`. The code on the left will search `"abc"` pattern inside a given string.

To find the offset location of a pattern inside a string, we can use `=~`.

```
/taco/ =~ "I like tacos"
#=> 7
```

It also works if we reverse the order.

```
"I like tacos" =~ /taco/
#=> 7
```

Regex will *literally* match pretty much everything you put between `/` and `/`. The only characters they won't match literally are: `. | ( ) [ ] { } + \ ^ $ * ?`.

To substitute string with pattern, we can use `sub` method. Sub will change the **first** instance of matching pattern. If we need to substitute **all** matching patterns, we use `gsub` method.

```
mexican_food = "Tacos are awesome"
mexican_food.sub(/taco/i, "burrito")
#=> "burritos are awesome"
```

By the way, the `i` at the end of `/taco/` indicates that the pattern I am searching is *case insensitive*. The pattern, `/taco/`, is capitalized in the string `"Tacos..."`. If I don't put `i` there, Regex will fail to find the match because there are no `"taco"` in the string (but we have `"Taco"`). The pattern to use `sub` method is easy. It takes in two arguments: the first being the pattern, the second being the string we wanted to replace it with.

Note that `sub` method does not modify the original string. It returns a new string. To modify the original string directly using `sub` (or `gsub`), we add `!`. `mexican_food.sub!(/taco/i, "burrito")` will replace the original `mexican_food`, because I like burritos more than tacos.

## Anchors

Two useful Regex anchors are beginning (`^`) and end (`$`) anchors.

```
str = 'hey johnny boy'
match = (/^john/).match(str)
#=> nil

match = (/john/).match(str)
#=> #<MatchData "john">
```

The above example shows that using `^` on pattern tells Ruby to look `"john"` specifically in the beginning of the string. Since `str` started out with `'hey'`, it returns `nil`. Once `^` is removed, we have a match.

## Character Classes

`[]` can be used to match *any* of the characters inside. `[aeiou]` can be used to match **a** vowel. `[,:;!?]` is used to match punctuation. Special characters, `[.|(){}+^$*?]` are turned off inside brackets.

```
str = "it's my life"
match = /[aeiou]/.match(str)
#=> <MatchData "i">
match = /[aeiou][aeiou]/.match(str)
#=> nil
match = /[aeiou][^aeiou]/.match(str)
#=> <MatchData "it">
```

Two or more brackets can be used in a pattern. The second example returns nil because `str` does not have two consecutive vowels. However, it has a vowel followed by non-vowel, `"it"`. By the way, when used inside brackets, caret (`^`) becomes "non". `[^aeiou]` becomes "give me a match for any non-vowels".

## Repetitions

Regex pattern can be repeated.

```
a* = 0 or more a
a+ = 1 or more a
a? = 0 or 1
a{m, n} = at least m and at most n a
a{m,} = at least m a
a{,n} = at most n a
a{m} = exactly m a
```

Personally, when first learning Regex, I have trouble memorizing them all. In fact, I still forget some of them. If you can only memorize just one, I would go with `a{m,n}` because it encompasses everything on the list. For example:

```
a{0,} = a*
a{1,} = a+
a{0,1} = a?
```

Examples:

```
str = "nachoooooooo!"
match = (/ho?/).match(str)
#=> #<MatchData "ho">
match = (/ho*/).match(str)
#=> #<MatchData "hoooooooo">
```

## Grouping

Several characters and special characters can be grouped together in Regex with `()`.

```
str = "falala"
/(al)+/.match(str)
#=> #<MatchData "alal" 1:"al">
str = "falalalalala"
#=> #<MatchData "alalalalal" 1:"al">
```

## Substitution and pattern

With our combined knowledge of `sub` and various regex patterns, we can use pattern substitution.

```
str = "taco guys are the best"
str.gsub(/[aeiou]/, "*")
#=> "t*c* g*ys *r* th* b*st"
```

Substitution methods can also take a block.

```
str = "WhAt HapPenEd?"
str.downcase.gsub(/^\w/) {|first| first.upcase}
#=> ""What happened?"
```


For more Ruby Regex APIs, check out [rubular](http://rubular.com/) and [Ruby docs on Regex](http://ruby-doc.org/core-2.4.2/Regexp.html)
