---
layout:   post
title:    Choosing a distinct element while iterating an array in React
tags:     [react, iteration]
comments: true
---

## The problem

One app that I was working on has an array of objects that I iterate to display multiple times on React.

<img src="/assets/images/sundae/sundae-scorer-table.png" alt="sundae app scorer table" style="width: 80%; display: block; margin: 0 auto;"/>

For example, the screenshot above shows four `score` tables. When clicked, it shows a chart of the element that was clicked. See below:

<img src="/assets/images/sundae/sundae-scorer-cumulative.png" alt="sundae app scorer table" style="width: 90%; display: block; margin: 0 auto;"/>

How can React know which chart I selected?

## Give something unique to each element

In this case, each table element has a unique `index`.

```
<ScoreCharts scoreData={Object.values(this.props.student)[0]} index={this.props.index}... >
```

With that knowledge, we can easily identify which table was clicked. We pass down the index props into our charts component, `ScoreCharts`. Whenever the user clicks on a certain table, it will have an associated index value. We tell React to display only the charts with matching index and `selectedId`. Whenever user selects a table, it sends to `ScoreCharts` its `selectedId`. Here is what `ScoreCharts` look like:

```
export default class ScoreCharts extends React.Component{
  render(){
    const revealOnlySelectedData = (this.props.index === this.props.selectedChartId) ? <ChartsContainer toggleDisplayChart={this.props.toggleDisplayChart} index={this.props.index} scoreData={this.props.scoreData} averageScores={this.props.averageScores} cumulativeScores={this.props.cumulativeScores} /> : <div></div>

    return (
      <div>

        {revealOnlySelectedData}
        ...
```

`revealOnlySelectedData` is the key for specificity. We *only* reveals whatever chart with the `index` matching `selectedChartId`.

Whenever you have an element that shares similar attributes with other elements and you wanted to choose a specific one, give each of those elements something different, i.e.: a unique index/ id. With that, each element becomes unique. When an event occurs, perform an id/ index comparison with `event.selectedId` and display only the matching ids.
