---
name: g6
description: AntV G6 5.x 图可视化引擎开发指南，附带一套完整的本地离线文档（API 参考 + 使用手册，中英双语）。凡是涉及 G6、@antv/g6、图可视化、关系图、网络拓扑图、流程图、知识图谱、ER 图、依赖图、节点/边/Combo、图布局（force、dagre、radial、mindmap 等）、图交互（拖拽、框选、缩放、hover 高亮）、自定义节点/边/行为/插件/主题、图导出图片，或 G6 v4 升级 v5 的任务，都必须使用本 skill：先查本地文档再写代码，禁止凭记忆写 API。Use for any G6 / @antv/g6 / graph visualization task, even if the user doesn't say "G6" explicitly.
---

# AntV G6 5.x 开发

本 skill 内置了 AntV G6 **5.x** 的完整官方文档快照，位于本 skill 目录下的 `references/`（加载 skill 时系统会给出 base directory，下列路径均相对它）。

- `references/3d.md` — ★ 3D 开发总入口：`@antv/g6-extension-3d` 全部配置的整合版（安装、注册、相机、光源、3D 布局），涉及 3D 一律先读它
- `references/api/` — API 参考：Graph、数据、元素、事件、坐标、布局、插件、主题、渲染、导出等
- `references/manual/` — 使用手册：快速开始、数据、元素、交互、布局、插件、主题、动画、FAQ、升级指南等
- `references/backup/` — 3D 相机设置原始文档（内容已并入 `3d.md`）

文档均为中英双语，同一主题有两份：`<主题>.zh.md` 与 `<主题>.en.md`。**用户用中文提问时读 `.zh.md`，否则读 `.en.md`**，不要两份都读。唯一例外：`custom-transform` 只有中文版。快照已做过清理：站点嵌入占位（`<embed>`）、图片、徽章等与内容无关的噪音均已剔除。

## 核心规则：先查文档，再写代码

G6 5.x 与网上大量流传的 4.x 代码 API 几乎完全不同，而模型记忆中的 G6 大多是 v4 写法（如 `modes`、`fitView: true`、`linkCenter`、样式直接写在数据上）。因此：

1. **写任何 G6 代码之前，先 Read 本 skill 中与任务相关的文档**；涉及的每个主题（布局、交互、自定义节点……）各读对应文件。
2. **只使用文档中出现过的 API 和配置项**。文档里查不到的 API 一律不要编造；如果用户要求的能力找不到对应 API，明确说明并给出最接近的文档方案。
3. 用户贴出的 G6 代码若含 v4 写法，指出差异并按 5.x 改写，可参考 `references/manual/whats-new/upgrade.zh.md`（及 `upgrade-to-5-1.zh.md`）。

## 最小正确示例（5.x 骨架）

生成完整示例时以此为骨架，其余能力在此基础上叠加：

```js
import { Graph } from '@antv/g6'; // 或 <script src="https://unpkg.com/@antv/g6@5/dist/g6.min.js">

const graph = new Graph({
  container: 'container', // DOM 元素或其 id
  autoFit: 'view',
  data: {
    // 样式放 style，业务数据放 data，类型用 type 指定
    nodes: [{ id: 'node1', data: { label: '节点1' }, style: { size: 20 } }],
    edges: [{ source: 'node1', target: 'node2' }],
  },
  node: {
    style: { size: 10 },
    palette: { field: 'group', color: 'tableau' }, // 按数据字段着色
  },
  layout: { type: 'd3-force' }, // 布局是对象，type 指定布局名
  behaviors: ['drag-canvas', 'zoom-canvas', 'drag-element'], // 交互直接用 behaviors，没有 modes
});

graph.render();
```

## 文档地图

`references/` 完整目录树：路径相对 `references/`，语言后缀 `.zh.md` / `.en.md` 省略。按任务定位文件后直接 Read，先读 overview 理解概念，再读对应 API 页确认签名。

```text
3d.md                              ★ 3D 总入口（安装/注册/相机/光源/D3Force3D 全配置），3D 任务必读

api/                               # API 参考：类、方法、配置项签名
├── graph、option                  # 图实例方法（render、setData、addItemData、fitView…）/ Graph 配置项总览
├── data                           # 图数据 CRUD（addData、updateData、removeData…）
├── element                        # 元素操作 API
├── behavior、plugin               # 交互 / 插件 API
├── layout、theme、transform       # 布局 / 主题 / 数据处理 API
├── event                          # 事件监听（图形事件、画布事件、事件对象）
├── canvas、viewport               # 画布操作 / 视口操作
├── coordinate                     # 坐标转换（client→canvas→graph…）
├── render                         # 绘制与渲染
└── export-image                   # 导出图片

manual/                            # 使用手册
├── getting-started/               # installation、quick-start、step-by-step、integration/{react,vue,angular}
├── graph/                         # graph、option（配置项详解）、extension(s)
├── data                           # 数据格式：nodes/edges/combos，style/data/type 的分工
├── element/
│   ├── overview、state            # 元素总览 / 元素状态（selected、active、highlight、disabled…）
│   ├── node/                      # overview、custom-node、react-node、vue-node；
│   │                              #   内置：Circle Rect Diamond Ellipse Donut Hexagon Star Triangle Image Html
│   ├── edge/                      # overview、custom-edge；内置：Line Polyline Quadratic Cubic CubicHorizontal CubicVertical
│   ├── combo/                     # overview、custom-combo；内置：CircleCombo RectCombo
│   └── shape/                     # overview（图形与 KeyShape）、properties（原子 Shape 属性）、label-shape（标签）
├── behavior/                      # overview、custom-behavior；
│                                  #   内置：DragCanvas ZoomCanvas ScrollCanvas DragElement DragElementForce FocusElement
│                                  #         ClickSelect BrushSelect LassoSelect HoverActivate CollapseExpand CreateEdge
│                                  #         AutoAdaptLabel FixElementSize OptimizeViewportTransform
├── layout/                        # overview、custom-layout；
│                                  #   内置：Force D3Force D3Force3D ForceAtlas2 Fruchterman Circular Radial Concentric
│                                  #         Dagre AntvDagre ComboCombined CompactBox Dendrogram Mindmap Indented
│                                  #         Fishbone Snake Grid Mds Random
├── plugin/                        # overview、custom-plugin；
│                                  #   内置：Minimap Tooltip Contextmenu Legend GridLine Background Title Toolbar Fullscreen
│                                  #         Snapline History Hull Timebar Watermark Fisheye BubbleSets EdgeBundling EdgeFilterLens
├── theme/                         # overview、custom-theme、palette、custom-palette
├── animation/                     # animation、custom-animation
├── transform/                     # overview、custom-transform（仅中文）、MapNodeSize、ProcessParallelEdges、PlaceRadialLabels
├── further-reading/               # renderer、event、coordinate、download-image、iconfont、bundle、3d（已并入 3d.md）
├── whats-new/                     # upgrade（v4→v5 必读）、upgrade-to-5-1、feature（5.x 新特性）
├── faq、introduction、contribute  # 常见问题 / 简介 / 参与贡献
└── （原 extension/ 目录仅有的两个 3D 空占位文件已删除，3D 内容统一看 3d.md）

backup/                            # CameraSetting 相机设置原始文档，内容已并入 3d.md
```

## 典型 v4 习惯 vs 5.x 写法

| v4 习惯（不要再用） | 5.x 写法 |
|---|---|
| `modes: { default: [...] }` + `graph.setMode()` | `behaviors: [...]` + `graph.setBehaviors()` |
| `fitView: true` / `fitCenter: true` | `autoFit: 'view'` / `autoFit: 'center'` |
| 节点数据上直接写 `label`、`size` 等样式 | 样式放 `style`，业务数据放 `data`，类型用 `type` |
| `linkCenter` | 已移除；连线自动按 连接桩 → 轮廓 → 中心 顺序尝试 |
| `groupByTypes`、`autoPaint` | 已移除；绘制需手动调用 `render()` / `draw()` |
| 字符串布局 `layout: 'force'` | 对象布局 `layout: { type: 'force' }` |

遇到表中没有的差异，以 `manual/whats-new/upgrade.zh.md` 和对应主题文档为准，不要凭本表外推。
