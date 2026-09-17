---
title: Dendrogram Layout
---

## Overview

The dendrogram layout is suitable for visualizing hierarchical clustering data. Its feature is that all child nodes are laid out on the same level, node size is not considered, and each node is treated as 1px.

## Configuration

```js
const graph = new Graph({
  layout: {
    type: 'dendrogram',
    direction: 'LR',
    nodeSep: 30,
    rankSep: 250,
    radial: false,
  },
});
```

## Options

| Property  | Description                                            | Type                                       | Default | Required |
| --------- | ------------------------------------------------------ | ------------------------------------------ | ------- | -------- |
| type      | Layout type                                            | `dendrogram`                               | -       | ✓        |
| direction | Layout direction, [options](#direction)                | `LR` \| `RL` \| `TB` \| `BT` \| `H` \| `V` | `LR`    |          |
| nodeSep   | Node spacing, distance between nodes on the same level | number                                     | 20      |          |
| rankSep   | Rank spacing, distance between different levels        | number                                     | 200     |          |
| radial    | Whether to enable radial layout, [see below](#radial)  | boolean                                    | false   |          |

### direction

Tree layout direction options:

- `TB`: Root at the top, layout downward

- `BT`: Root at the bottom, layout upward

- `LR`: Root at the left, layout to the right

- `RL`: Root at the right, layout to the left

- `H`: Root in the middle, horizontal symmetric layout

- `V`: Root in the middle, vertical symmetric layout

### radial

Whether to enable radial layout mode. When enabled, nodes are distributed radially around the root node.

If `radial` is set to `true`, it is recommended to set `direction` to `'LR'` or `'RL'` for best results.
