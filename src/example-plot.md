---
title: Example plot
toc: false
---

# Observable Plot — simple example

This page demonstrates a small, responsive chart using Observable Plot and the `resize` helper used elsewhere in this project.


## Interactive Scatter Plot with Dynamic Highlighting

```js
// generate random scatter data
const scatterData = Array.from({length: 150}, (d, i) => ({
  x: Math.random() * 100,
  y: Math.random() * 100,
  id: i
}));
```

```js
const threshold = view(Inputs.range([0, 100], {value: 50, step: 1, label: "Filter threshold"}))
```

<div style="margin-bottom: 1rem;">${threshold}</div>

```js
function interactivePlot(data, thresh, {width} = {}) {
  return Plot.plot({
    title: `Interactive scatter — points ${thresh.toFixed(0)}+`,
    width,
    height: 400,
    marginLeft: 50,
    marginBottom: 50,
    x: {label: "X axis", domain: [0, 100]},
    y: {label: "Y axis", domain: [0, 100], grid: true},
    color: {
      type: "linear",
      domain: [0, 100],
      range: ["#4487f1", "#ff4757"],
      legend: true
    },
    marks: [
      Plot.dot(data, {
        x: "x",
        y: "y",
        fill: "y",
        r: (d) => d.y > thresh ? 4 : 2,
        opacity: (d) => d.y > thresh ? 1 : 0.3,
        tip: true
      }),
      Plot.ruleX([thresh], {stroke: "orange", strokeWidth: 2, strokeDasharray: "5,5"})
    ]
  });
}
```

<div class="card">
  ${resize((width) => interactivePlot(scatterData, threshold, {width}))}
</div>
