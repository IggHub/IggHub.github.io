---
layout:   post
title:    Setting Up Tmuxinator
tags:     [Ruby, tmux, tmuxinator]
comments: true
---

I started using [vim](http://www.vim.org/) as my text editor lately. To compliment vim, I am using tmux to display a second (or third) terminal on the screen. To manage my tmux configuration, I use [tmuxinator](https://github.com/tmuxinator/tmuxinator).

## Installing tmuxinator

Installing tmuxinator is super simple if you have [`rvm`](https://rvm.io/rvm/basics). Make sure Ruby version meets the tmuxinator's minimum Ruby version requirement (2.2.7 was the minimum requirement when I installed it this week, 10/9/2017). I decided to install `2.3.5` because why not?

```
rvm install 2.3.5
rvm use 2.3.5
```

To install tmuxinator, just install the gem:

```
gem install tmuxinator
```

And that's it!

## Configure the layout

For me, the hardest part is to figure out how to place different panes and windows. There are 5 basic layouts: `even-horizontal`, `even-vertical`, `main-horizontal`, `main-vertical`, and `tiled`. You can find the information on those 5 [here](http://manpages.ubuntu.com/manpages/precise/en/man1/tmux.1.html#contenttoc6).

I like to have two main windows: the first one called `editor` and the second one called `misc`. The first one will be mainly for vim, taking up the horizontal top, with small terminal space on the bottom for my `git` commands. The second window I like to split it into three vertical panes of equal width to run the server, perform testing, and debug (or `rails c`).

For that, I created a configuration named `rails`.

```
#create new tmuxniator configuration
tmuxinator new rails

#edit the configuration
tmuxinator edit rails
```

On `windows:` line, I have the following setup:

```
windows:
  - editor:
    layout: main-horizontal
    panes:
      - vim
      - #git/ misc
  - server:
    layout: even-vertical
    panes:
      - #rails s
      - #rspec
      - #rails c
```

Right after `windows:`, I described the two windows I wanted to have: `editor` window and `server` window. Inside each window, I define which layout I wanted to have. I then decsribe how many `panes` and what command (if any) to run on each pane.


For Tmux basic, check out this [blog from thoughtbot](https://robots.thoughtbot.com/a-tmux-crash-course). Tmuxinator github repo can be found [here](https://github.com/tmuxinator/tmuxinator)
