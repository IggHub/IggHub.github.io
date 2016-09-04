---
layout: post
title: Hello, Travis-CI!
tags: [travis-ci]
---

I have been noticing this in a lot of github repos.

![travis yml](http://i.imgur.com/TYGRAAA.png)

Succumbed to my curiosity, I googled what `.travis.yml` is all about.


### What is Travis-CI?

> Travis CI is a hosted, distributed continuous integration service used to build and test software projects hosted at GitHub.
>
>-[Wikipedia](https://en.wikipedia.org/wiki/Travis_CI)

The CI in [Travis-CI](https://travis-ci.org/) stands for [Continuous Integration](https://en.wikipedia.org/wiki/Continuous_integration). It means having continuous push from multiple developers to a centralized location.

Travis-CI ensures that the project does not break down from having so many pushes throughout the day.


### How does Travis-CI work?

I will be referring to Travis-CI's [complete beginner's guide](https://docs.travis-ci.com/user/for-beginners).

Let's say I am working on a [project](https://github.com/plaindocs/travis-broken-example)
([repo](https://github.com/plaindocs/travis-broken-example) &copy; Travis-CI). I clumsily added a broken code into `Test.php`

![1 + 1 = 1](http://i.imgur.com/ba4jzXZ.png)

This code _will_ raise an error code. But no one is going to check whether the project that I push will cause an error code because everyone else is busy writing codes. After all, if the build is being tested by human **every time a commit is pushed**, then it will take forever.

##### This is where Travis comes to save the day!

Travis-CI will look for `.travis.yml` file in a github repository account and use the "instructions" found on that file to test the build (_note: Travis currently only works with Github_).

On Travis-CI's dashboard, you will see this error:

![travis-error](http://i.imgur.com/XWUegCi.png)

### What happens behind the scene

Travis-CI is essentially another computer whose job is to run the project every time a commit is pushed.

![Travis dash](http://i.imgur.com/kvZtAhi.png)

1. Travis cloned the repo's master branch
2. Travis runs the build in the specified environment
3. Travis runs all the tests and scripts in `.travis.yml`, including PHP versions.

If all build returns no error, the build exited with 0. If it returns error, it exited with 1 and described how it failed.

![Travis-passing](http://i.imgur.com/Zce87do.gif)


## Summary

Travis-CI does the hard work of testing and running the build every time a push is made to master branch. When Travis detects a push that causes error, Travis can notify the developers so they can stop and kill the error source before it spreads like a disease!
