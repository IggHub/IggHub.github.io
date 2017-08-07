---
layout:   post
title:    Reusing table cell in React
tags:     [react, reusable]
comments: true
---

React's components are meant to be [reusable](https://facebook.github.io/react/docs/components-and-props.html). When I first started React, I was not doing it.

This post will show an example how to reuse a component several times. I found this super awesome codepen (almost randomly). It got my interest because how DRY it was. Codepen can be found [here](https://codepen.io/Shamiul_Hoque/pen/LNavdZ) (not my account nor my pen. Credit 100% goes to codepen creator).

The pen has one class, `EditableCell`, to create the cell as an input. EditableCell contains a single `<td>` element. It is called multiple times on `ProductRow` class. Here is the genius part: it passes a props, `cellData` to differentiate what each cell is used for. It is more accurate to say that cellData is an object.

For example:

```
<EditableCell onProductTableUpdate={this.props.onProductTableUpdate} cellData={
  type: "qty",
  value: this.props.product.qty,
  id: this.props.product.id
}/>
```

The `id` makes each cell unique. The `value` can be edited by user, and `type` is used to differentiate the column type (in this case, `Name`, `price`, `quantity`, and `category`).


This is the best part:

```
handleProductTable(evt) {
  var item = {
    id: evt.target.id,
    name: evt.target.name,
    value: evt.target.value
  };
var products = this.state.products.slice();
var newProducts = products.map(function(product) {

  for (var key in product) {
    if (key == item.name && product.id == item.id) {
      product[key] = item.value;

    }
  }
  return product;
});
  this.setState({products:newProducts});
};
```

`handleProductTable` takes in an event (`cellData`) and updates the state based on the `cellData` object. It then finds the correct object element based when the name (`item.name`) matches `product`'s `key` and their `id`'s and updates it `product[key] = item.value` - before it updates the state.

Brilliant! I would highly recommend to check out the pen and redo it yourself.
