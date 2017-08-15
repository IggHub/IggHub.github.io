---
layout:   post
title:    MobX useStrict
tags:     [react, mobx]
comments: true
---

When I first saw `useStrict(true)` used, I had no idea what it does. I recently watched a tutorial on [egghead](https://egghead.io/) and it really helped me to understand it.

I am going to shamelessly borrowed the example. This is taken from egghead's [mobx lesson](https://egghead.io/lessons/react-use-mobx-actions-to-change-and-guard-state). In which we have class to store temperature-related data.

```
const {observable, computed, action, transaction, useStrict} = mobx;
const {observer} = mobxReact;
const {Component} = React;

useStrict(true);
const t = new class Temperature {
  @observable unit = "C";
  ...
```

There are 4 important APIs in mobx: `@observable`, `@action`, `@computed`, and `@observer`. I won't be going through them yet. I will most likely go through them in the future, but here is the [doc](https://mobx.js.org/refguide/api.html) for you curious bunch :).

I will gloss over `@observable`, though. Note that we define `unit` to be `@observable`. Two lines before, we had just declared `useStrict(true)`.

If I go to console:

```
>> t
//=> [object Object] {
  temperatureCelsius: 25,
  unit: "C"
}
>> t.unit
//=> "C"
```

If I wanted to change the unit to Kelvin, I get an error:
```
>> t.unit = "K"
//=> "[mobx] Invariant failed: It is not allowed to create or change state outside an `action` when MobX is in strict mode. Wrap the current method in `action` if this state change is intended"
```

This what `useStrict(true)` does. It makes our mobx `@observable` immutable. We are not allowed to change it directly anymore. If we wanted to change it directly, we needed to *remove* `useStrict(true)`.

```
>> t.unit = "K"
//=> "K"
```
To summarize, `useStrict(true)` prevents user from changing the `@observable` variables directly. If user wants to change `unit` from "C" to "K" or "F", user will need to create a new `@action` to change the variable. For example,

```
  ...
  @action setUnit(newUnit) {
    this.unit = newUnit;
  }
  ...
```

And we can just do something like this:

```
>> t.setUnit("F")
//=> "F"
>> t.unit
//=> "F"
```

For more info, check out mobx's [useStrict API docs](https://github.com/mobxjs/mobx/blob/gh-pages/docs/refguide/api.md#usestrict).
