---
title: 脑图树 Mindmap
---

## 概述

脑图树布局适用于树状结构的层次化布局，支持左右两侧展开，深度相同的节点将会被放置在同一层。需要注意：布局**会**考虑节点的大小。参考更多脑图布局[样例](/examples#layout-mindmap)或[源码](https://github.com/antvis/hierarchy/blob/master/src/mindmap.js)。

## 配置方式

```js
const graph = new Graph({
  layout: {
    type: 'mindmap',
    direction: 'H',
    preLayout: false,
    getHeight: () => 32,
    getWidth: () => 32,
    getVGap: () => 16,
    getHGap: () => 72,
  },
});
```

## 配置项

| 属性      | 描述                                                                                                  | 类型                                | 默认值 | 必选 |
| --------- | ----------------------------------------------------------------------------------------------------- | ----------------------------------- | ------ | ---- |
| type      | 布局类型                                                                                              | `mindmap`                           | -      | ✓    |
| direction | 布局方向，[可选值](#direction)                                                                        | `H` \| `LR` \| `RL` \| `TB` \| `BT` | `LR`   |      |
| getHeight | 计算每个节点的高度                                                                                    | (d?: Node) => number                |        | ✓    |
| getWidth  | 计算每个节点的宽度                                                                                    | (d?: Node) => number                |        | ✓    |
| getVGap   | 每个节点的垂直间隙，注意实际两个节点间的垂直间隙是2倍的vgap                                           | (d?: Node) => number                |        |      |
| getHGap   | 每个节点的水平间隙，注意实际两个节点间的水平间隙是2倍的hgap                                           | (d?: Node) => number                |        |      |
| getSide   | 设置节点排布在根节点的左侧/右侧，如未设置，则算法自动分配左侧/右侧。注意：该参数仅在`H`布局方向上生效 | (d?: Node) => string                |        |      |

### direction

> `H` \| `LR` \| `RL` \| `TB` \| `BT` **Default:** `'LR'`

树布局的方向

- `'H'`：horizontal（水平）—— 根节点的子节点分成两部分横向放置在根节点左右两侧。可传入`getSide`方法指定每个节点的左右分布逻辑，不传则默认将前半部分放置在右侧，后半部分放置在左侧。

- `'LR' | 'TB'`：将子节点排布在根节点的右侧；

- `'RL'`：将子节点排布在根节点的左侧；

- `BT`：将子节点排布在根节点右侧，然后将整个图沿X轴旋转180°；

### getWidth

> _(d?: Node) => number_

每个节点的宽度

示例：

```javascript
(d) => {
  // d 是一个节点
  if (d.id === 'testId') return 50;
  return 100;
};
```

### getHeight

> _(d?: Node) => number_

每个节点的高度

示例：

```javascript
(d) => {
  // d 是一个节点
  if (d.id === 'testId') return 50;
  return 100;
};
```

### getHGap

> _(d?: Node) => number_

每个节点的水平间隙

示例：

```javascript
(d) => {
  // d 是一个节点
  if (d.id === 'testId') return 50;
  return 100;
};
```

### getVGap

> _(d?: Node) => number_

每个节点的垂直间隙

示例：

```javascript
(d) => {
  // d 是一个节点
  if (d.id === 'testId') return 50;
  return 100;
};
```

### getSide

> _(d?: Node) => string_

设置节点排布在根节点的左侧/右侧。注意：该参数仅在`direction`为`H`时生效。如未设置，会默认将子节点前半部分放置在右侧，后半部分放置在左侧，参考[getSide自动计算逻辑](https://github.com/antvis/hierarchy/blob/d786901874f59d96c47e2a5dfe17b373eefd72e3/src/layout/separate-root.js#L11)。

示例：

```javascript
(d) => {
  // d 是一个节点
  if (d.id === 'test-child-id') return 'right';
  return 'left';
};
```

### 布局适用场景

- 数据血缘图：`direction='H'`很适合渲染血缘图中查看指定节点的上下游血缘的场景，上游分布在中心节点的左侧，下游分布在右侧；
- 思维导图：构建自定义的思维导图组件。
