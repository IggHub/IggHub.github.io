---
layout:   post
title:    Better way to fetch multiple JSON
tags:     [promise, react, fetch]
comments: true
---

I wanted to fetch some data using React from my Rails-API backend so I can manipulate them later. There are two data sources that I was trying to get it from: `students` and `scores`.

This is what I did before:

1. I created `fetch` method to get students, then during `componentDidMount` lifecycle, it fetches students and saves it to `this.state.students` as an array of objects using callbacks.

2. I created `fetch` method to get `scores`, then during `componentDidMount` lifecycle, it fetches scores and saves it to `this.state.scores` as an array of objects using callbacks.

3. I created a method to merge those two together, call it `rearrangeStudentsScores` that takes values from both `this.state.students` and `this.state.scores`.

Code:
```
//fetch students data
function getStudents(cb){
  return fetch(`api/students`, {
    headers: {
      'Content-Type': 'application/json',
      'Accept': 'application/json'
    }
  }).then((response) => response.json())
  .then(cb)
};

//fetch scores data
function getScores(cb){
  return fetch(`api/scores`, {
    headers: {
      'Content-Type': 'application/json',
      'Accept': 'application/json'
    }
  }).then((response) => response.json())
  .then(cb)
};

//execute both students and scores fetching
function getStudentsAndScores(cbStudent, cbScores, cbStudentsScores){
  getStudents(cbStudent).then(getScores(cbScores)).then(cbStudentsScores);
}
```

In my React:

```
//get students and scores, and save them to states. Then execute the array manipulation
getStudentsAndScores(){
  Client.getStudentsAndScores(
    (students) => {this.setState({students})},
    (scores) => {this.setState({scores})},
    this.rearrangeStudentsWithScores
  )
};

//array manipulation
rearrangeStudentsWithScores(){
  console.log('hello rearrange!')
  console.log('students:')
  console.log(this.state.students);
  console.log('scores:');
  console.log(this.state.scores);    
  //array manipulation goes here//
  ...
```

The problem is sometimes (about 50% of the time), `this.state.scores` would still be `[]` when `rearrangeStudentsWithScores` is executed. I am guessing that it was not fast enough to `setState` before it executes `rearrangeStudentsWithScores`. I need to *make sure* that both `this.state.students` and `this.state.scores` are both fulfilled before executing `rearrangeStudentsWithScores`... but how can I do that?

Enter [`Promise.all()`](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Global_Objects/Promise/all) to the rescue! With `Promise.all()`, I can make sure that everything inside is fulfilled before doing anything. I will leave the explaining what it does to documentation. I will however, show you how I did it:


First, I modified my functions to it does not use callbacks (cb) anymore. Then I use `Promise.all()` inside `getStudentsAndScores`.

```
function getStudents(){
  return fetch(`api/students`, {
    headers: {
      'Content-Type': 'application/json',
      'Accept': 'application/json'
    }
  }).then((response) => response.json())
};

function getScores(){
  return fetch(`api/scores`, {
    headers: {
      'Content-Type': 'application/json',
      'Accept': 'application/json'
    }
  }).then((response) => response.json())
};

function getStudentsAndScores(){
  return Promise.all([getStudents(), getScores()])
}
```

On my React end, it is actually less coding:

```
componentDidMount(){
  this.getStudentsAndScores();
};

getStudentsAndScores(){
  Client.getStudentsAndScores().then(([students, scores]) => {
    this.setState({
      students,
      scores
    }, () => this.rearrangeStudentsWithScores())
  })
}
rearrangeStudentsWithScores(){
  console.log('students:')
  console.log(this.state.students);
  console.log('scores:');
  console.log(this.state.scores);
  //array manipulation here//
  ...
```

Note:
1. `Promise.all()` takes in an array (of promises)
2. Because it takes an array, its `then` chain must be followed with an array (i.e. `Client.getStudentsAndScores.then(([array1, array2, array3, ... arrayN]) => {//do something//})`)

Now `this.state.scores` and `this.state.students` will be both fulfilled before `rearrangeStudentsWithScores` is run.

That's it. If you want to drop by and say hello, have questions, or see an error, please feel free to drop a comment or email me!
