---
layout:   post
title:    Node Module Exports
tags:     [nodejs]
comments: true
---

Whenever I am referencing other file inside a node app, I need to export the variables before they are accessible at different file.

To do this, let's create a super simple node app. Create a directory and name it `node-study`. Run `npm init`.

Inside, create `foo.js` and `bar.js` in root.

```
//bar.js
function bar(){
  console.log("Hello from bar function!");
}

console.log("Hello from bar module!");

//foo.js
var bar = require('./bar');

console.log(bar);
```

Running `node foo.js` gives

```
Inside bar module!
{}
```

The first line was executed when we loaded `bar.js`. Second line was from `console.log(bar);`. However, it returns an empty object because although `bar.js` has been loaded, `bar()` (function) has not been defined/ exported. To transfer the function, we need to export it.

Add this into `bar.js`:

```
function bar(){
  ...

module.exports = bar;
```

And inside `foo.js`, call the function and comment out the `console.log(bar)`:

```
var bar = require('./bar');

//console.log(bar);
bar();
```

We get:

```
Inside bar module!
Inside bar() function!
```

To recap, node app has a free variable named `module`. It has a property named `exports` to export variables. Once a function is exported, it will be available on the file where that module was required.
