---
layout:   post
title:    React-Native ListView Demo
tags:     [react-native]
comments: true
---

<img src="https://camo.githubusercontent.com/0f312f8f027645e809cc639173458e4f7a46a41e/68747470733a2f2f63646e2e61757468302e636f6d2f626c6f672f616c7465726e6174697665732d746f2d6e61746976652d6d6f62696c652d646576656c6f706d656e742f72656163742d6e61746976652d6c6f676f2e706e67" alt="app screenshot on ios simulator" style="width: 85%; display: block; margin: 0 auto;"/>

I didn't understand `ListView` component in React Native - until I read [this awesome blog post how to use ListViewby Spencer Carli](https://medium.com/differential/react-native-basics-how-to-use-the-listview-component-a0ec44cf1fe8).

Here are some of the main takeaway from the tutorial:

1. ListView is useful when dealing with (potentially) infinite data to scroll down.
2. ListView is just another layer of abstraction above [ScrollView](https://facebook.github.io/react-native/docs/using-a-scrollview.html) - it is essentially ScrollView over a downloaded data set
3. ListView requires at least two parameters: `dataSource` and `renderRow`

## dataSource and renderRow

They are confusing. When I first used listView, I could not figure out how to work it out. It turns out that I did not understand how it works. If you are new to ListView, there are two things you can do to get it working right-away:


First, when initializing `dataSource` state for listview, remember to use `this.state: {dataSource: new ListView.DataSource({rowHasChanged: (row1, row2) => row1 !== row2, ...})...`

```
constructor(props) {
  super(props);
  this.state = {
    dataSource: new ListView.DataSource({
      rowHasChanged: (row1, row2) => row1 !== row2,
    }),
    loaded: false,
    //more states here//
  };
}
```

Second, to setState and update listView data (it accepts a simple array of array of objects), use:

```
this.state = {
  dataSource: this.state.dataSource.cloneWithRows(['row 1', 'row 2', 'row 3']),
};
```

## ListView

ListView should work now.

```
<ListView
  dataSource={this.state.dataSource}
  renderRow={(data) => <View><Text>{data}</Text></View>}
/>
```

renderRow accepts a function to display how each element of array will be displayed. Sort of like [mapping in React](https://facebook.github.io/react/docs/lists-and-keys.html) works.

For more short tutorials how to use ListView, I would recommend checking out the [medium tutorial on ListView](https://medium.com/differential/react-native-basics-how-to-use-the-listview-component-a0ec44cf1fe8) and [Facebook tutorial on ListView](http://facebook.github.io/react-native/releases/0.23/docs/tutorial.html#content). They were published about a year ago, but everything still works. I used `create-react-native-app` with `expo` and both tutorials still work like a charm.

I also created a repo for the first tutorial for reference. Feel free to check out my [github repo on ListView Demo](https://github.com/IggHub/react-native-listview)
