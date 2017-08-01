---
layout:   post
title:    Pomodoro-app-review
tags:     [angular, firebase]
comments: true
---

<img src="/assets/images/pomodoro/pomodoro-main.png" alt="pomodoro page" style="width: 100%; display: block; margin: 0 auto;"/>

## Overview
The Pomodoro timer is based on a classic [tomato kitchen timer](https://en.wikipedia.org/wiki/Pomodoro_Technique) technique. It is usually 25 minutes long with 5 minutes break interval.

Here is how to use it:

1. Decide a **specific** task to be done beforehand.
2. Focus on that and only that task for the designated time (25 min).
3. When timer rings, another set of timer for 5 min will start. At that time, one *must* take  a break.
4. Start the 25 min timer again.
5. The loop repeats 4 times. After which, it changes to a 30 min break timer. Then it starts the loop all over.

This app is created with [AngularJS](https://angularjs.org/). This is the second Angular App I created, the first being the remake of [bloc-jams](https://github.com/IggHub/bloc-jams); repo found [here](https://github.com/IggHub/bloc-jams-angular).

## Challenges

The challenge on this pomodoro app is to create a logic for the timer to countdown 25 minutes, then 5 minutes, then 25 minutes, 5 minutes, 25, 5, 25, and then 30 minutes - before starting over again.

I will be *selectively* going over some of the code, for the main timer code, go [here](https://github.com/IggHub/pomodoro/blob/master/app/scripts/app.js)

First, declare all necessary variables (units are in seconds)

```
$scope.countDownPomodoro = 1500; // 1500s = 25 min
$scope.countDownBreak = 300;
$scope.countDownLongBreak = 1800;
$scope.workCounter = 0;
$scope.hideStart = false;
$scope.bellTrigger = false;
```

`hideStart` and `bellTrigger` are set to be false initially. When it is clicked, `hideStart` will be true and will hide the start button. We also have a `workCounter` to count how many times the loop has occurred.

A scope function named `startPomodoro` will be fired whenever the 25 min timer starts. Note that it increments `workCounter` by one every time this function is run.

```
$scope.startPomodoro = function(){
  if (angular.isDefined(stop)) return;
  stop = $interval(function(){
    if ($scope.countDownPomodoro === 0){
      $scope.stopPomodoro();
      $scope.onBreak = true; //once timer hits 0, onBreak = true
      $scope.workCounter += 1;
```

The break is done similarly; `startBreak` will be fired whenever the 25 min run is completed and as long as

```
$scope.startBreak = function(){
  if (angular.isDefined(stop)) return;
  stop = $interval(function(){
    if ($scope.countDownBreak === 0){
      $scope.stopBreak();
      $scope.onBreak = false;
      $scope.resetBreak();
      $scope.hideStart=false;
      $scope.bellTrigger = true;
      //$scope.playing = true;
      } else {
        return $scope.countDownBreak--;
    }},1000);
  };
```

The `longBreak` is fired when the counter reaches 0:

```
$scope.startLongBreak = function(){
  if (angular.isDefined(stop)) return;
  stop = $interval(function(){
    if ($scope.countDownLongBreak === 0){
      $scope.stopLongBreak();
      //$scope.onBreak = true;
      $scope.resetLongBreak();
      ...
```

To store the tasks, a database was needed. [Firebase](https://firebase.google.com/) was utilized as backend solution.

## What I learned

Pomodoro timer app was a fun project. It was a simple timer app and can be done using basic Javascript without additional dependencies. In addition to Angular framework and Firebase, [BuzzJS](http://buzz.jaysalvat.com/) was used again to create a simple alarm.

I started to apply different approach to UI. The app uses bold, muted soft red color instead of white majority.


<img src="/assets/images/pomodoro/pomodoro-mobile.png" alt="pomodoro mobile page" style="width: 50%; display: block; margin: 0 auto;"/>

I also tested the app with different users and received helpful feedback! I was able to find a few bugs (one bug allowed user to enter an empty item, causing the task to be impossible to delete). User testability is important. Next time, the item list and remove feature will have to be more visible.


Repo: [pomodoro github](https://github.com/IggHub/pomodoro)
Demo: [pomodoro app](https://iggy-pomodoro.herokuapp.com/)
