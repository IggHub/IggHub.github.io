---
layout:   post
title:    NodeJS Uncaught Exception
tags:     [nodejs]
comments: true
---

NodeJS runs processes often asynchronously. Sometimes it may get tricky to catch errors. For example, let's say I wanted to read a file and I accidentally forgot to give it a file name.

```
var fs = require("fs");
fs.readFile("", "utf8", function(error, data) {
  if (error) {
    throw error;
  }
  console.log(data);
});
```

Technically this won't throw an error because node will keep going before file reading was completed (thus bypassing `if (error)` line). To handle this, add this at the end:

```
process.on("uncaughtException", function(error) {
  console.log("Exception!")
});
```

For more info on `uncaughtException` process, check out the [doc at nodejs site](https://nodejs.org/api/process.html#process_event_uncaughtexception).
