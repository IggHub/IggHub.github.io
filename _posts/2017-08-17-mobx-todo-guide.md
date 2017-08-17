---
layout:   post
title:    MobX Todo App
tags:     [react, mobx]
comments: true
---

I created a simple mobx + react todo app [here](https://github.com/IggHub/mobx-react-todo-demo). It was my first attempt to make todo app with mobx independently. I will share how the process went. Feel free to visit the [mobx + react todo github page](https://github.com/IggHub/mobx-react-todo-demo) and clone the repo to go along with the tutorial.

## Setting up the environment

For starter, I cloned the [mobx-react-boilerplate](https://github.com/mobxjs/mobx-react-boilerplate). It is a small react app with mobx preinstalled.

Our todo app is very small. I only needed to create one additional store file, I named it `TodoStore.js`. The file tree looks like this:

```
-src
|
|- App.jsx
|- index.jsx
|- TodoStore.js
|- utils.js
```

I will discuss `App.jsx` and `TodoStore.js` here.

## TodoStore

This is what is inside TodoStore. Don't feel overwhelmed, I will break it down per section.

```
import {observable, action, computed, useStrict} from 'mobx';
import * as Utils from './utils.js';

useStrict(true);

class TodoStore {
  @observable todos = [{task: 'learn mobx', completed: false, id: Utils.guid()}, {task: 'make mobx app', completed: false, id: Utils.guid()}, {task: 'be mobx jedi', completed: false, id: Utils.guid()}];
  @observable todoFilter = 'all';

  constructor(){
    this.todos.map((todo) => {
      console.log("task: " + todo.task);
      console.log("id: " + todo.id);
      console.log("completed: " + todo.completed);
    })
  }
  @computed get completedTodoLength(){
    return this.todos.filter((todo) => {
      return todo.completed == true
    }).length;
  };
  @computed get todoLength(){
    return this.todos.length;
  };
  @action addTodo(todoItem) {
    this.todos.push({task: todoItem, completed: false, id: Utils.guid()})
    //this.todos = [{task: todoItem, completed: false, id: Utils.guid()}, ...this.todos]
  };
  @action removeTodo(index){
    this.todos.splice(index, 1);
    console.log(this.todos);
  };
  @action handleToggle(index){
    this.todos[index].completed = !this.todos[index].completed;
    console.log(this.todos[index].completed);
  };
  @action setFilterToCompleted(){
    this.todoFilter = 'completed';
    console.log("todoFilter: " + this.todoFilter);
  };
  @action setFilterToAll(){
    this.todoFilter = 'all';
    console.log("todoFilter: " + this.todoFilter);
  };
  @action deleteAllCompleted(){
    this.todos = this.todos.filter((todo) => {
      return !todo.completed
    });
  };
}

export default TodoStore;
```

### Observables
```
@observable todos = [{task: 'learn mobx', completed: false, id: Utils.guid()}, {task: 'make mobx app', completed: false, id: Utils.guid()}, {task: 'be mobx jedi', completed: false, id: Utils.guid()}];
@observable todoFilter = 'all';
```
`@observable todos` creates an array of objects named todos. It consists of task, completed, and id. The ID is taken from `utils.js`. When called, it generates a random string to be used as ID.

`@observable todoFilter` creates a keyword. I will use it later to filter the display.


### Constructor

```
constructor(){
  this.todos.map((todo) => {
    console.log("task: " + todo.task);
    console.log("id: " + todo.id);
    console.log("completed: " + todo.completed);
  })
}
```

The code above is not crucial to the app. Constructor gets executed when TodoStore gets instantiated for the first time (when `new TodoStore()` is called). I made it to print out each todo object attributes. Sometimes we can use constructor to save variables for initialization.

### Computed

I see computed API as "modifying the value to display". It does not mutate anything.

```
@computed get completedTodoLength(){
  return this.todos.filter((todo) => {
    return todo.completed == true
  }).length;
};
```

For example, `getCompletedTodoLength` gets all the todos array, filters the todos based on `completed` value, and find the length of that filtered todos. It does not mutate the actual `todos`.

```
@computed get todoLength(){
  return this.todos.length;
};
```

Similarly, `todoLength` returns todo array length without mutating the actual array.

### Action

The last API is action. There are a lot of actions even in our small todo app. With mobx, most of the actions are stored in one location, making it easy to manage them.

```
@action addTodo(todoItem) {
  this.todos.push({task: todoItem, completed: false, id: Utils.guid()})
};
@action removeTodo(index){
  this.todos.splice(index, 1);
  console.log(this.todos);
};
@action handleToggle(index){
  this.todos[index].completed = !this.todos[index].completed;
  console.log(this.todos[index].completed);
};
@action setFilterToCompleted(){
  this.todoFilter = 'completed';
  console.log("todoFilter: " + this.todoFilter);
};
@action setFilterToAll(){
  this.todoFilter = 'all';
  console.log("todoFilter: " + this.todoFilter);
};
@action deleteAllCompleted(){
  this.todos = this.todos.filter((todo) => {
    return !todo.completed
  });
};
```

WIP;
