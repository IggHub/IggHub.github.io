---
layout:   post
title:    Breaking down sophisticated code into manageable chunks?
tags:     [ruby, ruby design patterns, observer method]
comments: true
---

A super neat pattern that I recently learned is called Composite Pattern. It compartmentalizes codes and breaks them down into a hierarchy of objects based on their functionality.

That is a little mouthful. Think of a sequential action, any sequential action, like prepping cereal for dinner (I eat them for dinner). The task can be broken down like:

## Making Bowl of Cereal

![Cereal bowl](http://i.imgur.com/zOni4Z8.jpg)

1. Get bowl
2. Get cereal
3. Get milk
4. Put cereal in bowl*
5. Put milk in bowl

That is the task hierarchy to prepare cereal. Let's do this in Ruby.

## Making Bowl of Cereal in Ruby

```
class CompositeComponent
  def initialize
    @tasks = []
  end

  def perform_task
    @tasks.each {|task| task.perform_task}
  end

  def add_task(task)
    @tasks << task
  end

  def remove_task(task)
    @tasks.delete task
  end
end

class GetBowl
  def perform_task
    puts "Getting a shiny bowl"
  end
end

class GetCereal
  def perform_task
    puts "Reach the cabinet and get me some Rice Krispies"
  end
end

class GetMilk
  def perform_task
    puts "Get me some 1% organic milk from grassfed cows"
  end
end

class PourCereal
  def perform_task
    puts "Put me some cereal in my shiny bowl"
  end
end

class PourMilk
  def perform_task
      puts "Pour that milky goodness into my cereal-filled shiny bowl"
  end
end

class MakeCereal < CompositeComponent
  def initialize
    super
    add_task GetBowl.new
    add_task GetCereal.new
    add_task GetMilk.new
    add_task PourCereal.new
    add_task PourMilk.new
  end
end
```

To make it run, simply do:

```
$ MakeCereal.new.perform_task
Getting a shiny bowl
Reach the cabinet and get me some Rice Krispies
Get me some 1% organic milk from grassfed cows
Put me some cereal in my shiny bowl
Pour that milky goodness into my cereal-filled shiny bowl
```

Super easy, huh!

So what just happened?

![composite image diagram](http://i.imgur.com/peuaiYZ.jpg)

If you look at the diagram above, there are 5 "base" tasks under `MakeCereal` class. Those are the indivisible tasks to make cereal happen *(technically you could break it down even further indefinitely, i.e.: reach for cereal container, open cereal container, put it on table, &c. we need to stop at some point)*.

`MakeCereal` is treated as if it was another task, just like `GetBowl` or `PourCereal` even though all it does is to manage its underlings tasks.

`CompositeTask` is a "task management" class. It allows us to add, remove, and list classes and to perform methods (note the method name `perform_task` needs to match the functions inside each class (all 5 tasks have `perform_task` method) as well. We let `MakeCereal` to be subclass of `CompositeTask` because `MakeCereal` is the only class (in our example) that doesn't do original task but manages other tasks. Think of `CompositeTask` is a management tool.

## Making better, more elaborate breakfast

Our breakfast adventure doesn't end here. We can now add more classes to boost our breakfast experience.

![Composite image diagram with more tasks](http://i.imgur.com/9dLpQ38.jpg)

We can add `MakeSmoothie`, `MakeEggs`, and more! Each of those class will consist of even smaller sub-tasks, i.e. `GetEggs`, `FryEggs`, `GetFruit`, `CutFruit`, `WashFruit`, &c. Now go and make the best ultimate breakfast experience out there, my friend!

## Conclusion
What have we just learned?

1. Break down complicated tasks into smaller task
2. Assign a hierarchy of class to manage the smaller tasks, inherit `CompositeTask` class to help manage
3. The level of hierarchy can go indefinitely

Now try to add some extra classes. Also, add an additional hierarchy above `MakeCereal`, `HaveBreakfast` class! How would you do that?
<br />
<br />
<br />

- <small>Some [might argue](http://www.debate.org/debates/What-goes-first-The-Milk-or-the-Cereal/1/) that you should put milk first</small>
- <small>[Cereal image source](http://www.peachpit.com/articles/article.aspx?p=1964563&seqNum=2)</small>
