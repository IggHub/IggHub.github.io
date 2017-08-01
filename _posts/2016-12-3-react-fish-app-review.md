---
layout:   post
title:    React Fish App Review
tags:     [react, firebase]
comments: true
---

<img src="/assets/images/react_fish/react-fish.png" alt="react fish main page" style="width: 100%; display: block; margin: 0 auto;"/>

## Overview

React-fish is an app I made following the Wes Bos' course, [reactforbeginners](https://reactforbeginners.com/). I wanted to learn React and heard many good things about the course.

The app is a simple menu in a seafood restaurant. User can load a random array of seafood menu and add it to order. It automatically updates the list and adds up the total price. The app took advantage of React's [Virtual DOM](https://stackoverflow.com/questions/21109361/why-is-reacts-concept-of-virtual-dom-said-to-be-more-performant-than-dirty-mode) feature to make it load super fast.

The app uses some extra dependencies including:

1. [re-base](https://github.com/tylermcginnis/re-base) to talk to [Firebase](https://firebase.google.com/)
2. [react-router](https://github.com/ReactTraining/react-router) for routing
3. [react-addons-css-transition-group](https://facebook.github.io/react/docs/animation.html) for more CSS transitions

## What I learned

This is one of my very first React App. I learned that React comprised of many [components](https://facebook.github.io/react/docs/react-component.html) - components can be anything, but usually an HTML component (like a row in a table) and it is [reusable](https://facebook.github.io/react/docs/components-and-props.html).

A component can either be a child of another component or a parent.

### Props and States

A component can have props and states. A prop is immutable. A state if mutable. When props and states are being passed down to child component, they all become props (immutable). Props and states can be created anywhere inside a component.

For example, suppose I have a `ParentComponent` where I created a `name` and `hobby` state (`name: 'Iggy'`, `hobby: code`). I also created a method `calc` (`calc(){console.log("Hello!")}`). To pass them into child component, I can simply do

```
<ChildComponent myName={this.state.name} myHobby={this.state.hobby} calc={this.calc} />
```

Inside `ChildComponent`, I can invoke the name by `this.props.name` (again, both props and states are now props after passed into child component). Pretty neat!

### Talking to firebase

React-Fish uses Firebase to store data. To communicate, `rebase` is used. To use it, first we need to connect to firebase using our login:

```
import Rebase from 're-base';

const base = Rebase.createClass({
      apiKey: "RANDOMAPIKEYGOESHERE",
      authDomain: "some-site.firebaseapp.com",
      databaseURL: "https://some-site.firebaseio.com",
});

export default base;
```

Once authentication is created, we `import base` and do authentication everytime a certain component is mounted. This can be done using React's [`componentWillMount`](https://facebook.github.io/react/docs/react-component.html#componentwillmount) lifecycle method.

During the authentication, we sync our state with data from Firebase. i.e.:

```
componentWillMount(){
  this.ref = base.syncState(`${this.props.params.storeId}/fishes`,
    {
      context: this,
      state: 'fishes'
    });

  const localStorageRef = localStorage.getItem(`order-${this.props.params.storeId}`);

  if (localStorageRef) {
    this.setState({
      order: JSON.parse(localStorageRef)
    })
  }
}
```

This API calls happens every time `App.js` component is about to be mounted (I believe this should have been done inside `componentDidMount`, but this serves our purpose. For a more thorough discussion when to make AJAX calls in React, check out this [SO post](https://stackoverflow.com/questions/27139366/why-do-the-react-docs-recommend-doing-ajax-in-componentdidmount-not-componentwi)).

There are way more to react than this, but I will have to end here. As I explore React, I will keep sharing more snippets. Till then!

Repo can be found [here](https://github.com/IggHub/react-fish) and app **demo** found [here](https://igghub.github.io/react-fish/store/panicky-worried-data)
