> ← [组件索引](../SKILL.md#组件索引) · [README](../README.md)

# OLoading 加载 · 设计 Skill

> 组件集合节点：`1042:21478` · 组件名：OLoading 加载 · 变体总数：6（size × Dark）

---

## Part A：设计使用卡

### 组件概览

**OLoading 加载**：旋转环形加载指示器，用于告知用户系统正在处理中，需要等待。以旋转动画呈现当前加载状态，避免用户因无反馈而产生焦虑或重复操作。支持三种尺寸，可嵌入页面内容区或独立全屏展示。

---

### 适用场景

- ✅ **页面级加载**：整页数据请求中（如首次进入页面），大尺寸居中展示
- ✅ **局部内容加载**：卡片、列表、表格区域的数据刷新，中尺寸覆盖内容区
- ✅ **行内操作反馈**：按钮触发后等待接口返回，小尺寸与文字并排展示
- ✅ **滚动加载更多**：列表底部触发分页加载，小尺寸水平布局
- ❌ **不适合**：确定时长的进度（用进度条组件）；纯视觉装饰（语义不符）

---

### 变体说明

**size（尺寸）**
- `large` — 大尺寸，96px 环形 + 文字，垂直排列。**默认尺寸**，适合页面级或卡片级加载遮罩
- `medium` — 中尺寸，64px 环形 + 文字，垂直排列。适合局部内容区加载
- `small` — 小尺寸，16px 弧形图标 + 文字，水平排列。适合行内操作反馈、紧凑列表底部

**Dark（主题）**
- `off` — 浅色模式（默认），环形轨道为浅灰色，活跃弧为品牌蓝，文字为辅助色
- `on` — 深色模式，活跃弧色值切换为深色模式品牌蓝，文字切换为反色

---

### 布局结构

> 🧩 **布局结构**：large 和 medium 采用垂直（VERTICAL）排列，环形图标居上，"加载中"文字居下；small 采用水平（HORIZONTAL）排列，弧形图标居左，文字居右。

```
OLoading large（VERTICAL，内容居中）
├── [环形动画图标]（96×96px）
│     轨道环 VECTOR（88×88px，内嵌 4px）fill: color-control4（浅灰）
│     活跃弧 VECTOR（48×48px，右上象限）fill: color-primary1（品牌蓝）
└── [文字 Frame]（padding-top: 16px）
    └── [PARAGRAPH "加载中"]
          font: font_size-text1（16px）/ line_height-text1（24px）
          fill: color-info2

OLoading medium（VERTICAL，内容居中）
├── [环形动画图标]（64×64px）
│     轨道环 VECTOR（58.67×58.67px，内嵌 2.67px）fill: color-control4
│     活跃弧 VECTOR（32×32px，右上象限）fill: color-primary1
└── [文字 Frame]（padding-top: 16px）
    └── [PARAGRAPH "加载中"]
          font: font_size-tip1（14px）/ line_height-tip1（22px）
          fill: color-info2

OLoading small（HORIZONTAL，垂直居中，gap: 4px）
├── [环形动画图标]（16×16px）
│     轨道环 VECTOR（14.67×14.67px，内嵌 0.67px）fill: color-control4
│     活跃弧 VECTOR（8×8px，右上象限）fill: color-primary1
└── [PARAGRAPH "加载中"]
      font: font_size-tip2（12px）/ line_height-tip2（18px）
      fill: color-info2
```

整体无外边距约束，宽度自适应内容；文字内容可自定义（默认"加载中"）。

---

### 组合搭配

> 🔗 **常见搭配**：
> - **页面遮罩**：Loading 居中 + 半透明蒙层（Overlay），覆盖页面主体内容区，阻止用户交互
> - **卡片刷新**：将 medium OLoading 覆盖在 OCard 内容区，等待数据返回后替换
> - **按钮加载态**：OButton 的 `state=loading` 内置了 small 尺寸旋转图标，与 OLoading small 视觉一致
> - **列表底部**：small OLoading 水平居中置于列表末尾，配合"正在加载更多"文字

---

### 设计稿识别指南

> 🔍 **识别特征**：
> - 核心视觉：蓝色弧段（约占 1/4 圆弧）搭配灰色环形轨道，三种尺寸均有轨道环
> - large/medium：垂直自动布局，包含圆形图标层 + 文字层，图标尺寸 96px 或 64px
> - small：水平自动布局，高度固定 18px，宽度 56px 起，图标 16×16px
> - 文字固定为"加载中"（可替换）

> 🔄 **易混淆组件**：
> - 与 **OProgress 进度条** 的区别：Loading 表示不确定时长的等待（无进度值）；Progress 展示可量化的完成百分比
> - 与 **OButton loading 状态** 的区别：Button 内置的旋转图标与 OLoading small 视觉一致，但不可独立插入，应通过 Button 变体切换

---

### 响应式行为

参照 [栅格规范](../SKILL.md#数据资源)，OLoading 无自动断点响应式行为。在移动端（360px 断点）建议优先使用 medium 或 small，页面级加载时居中覆盖内容区即可。

---

## Part B：规格速查参考

### 关联资产

| 资产用途 | 文件路径 |
|---------|---------|
| 加载图标（轨道环 + 活跃弧，viewBox 24×24） | `references/assets/public icons/icon-loading 加载.svg` |

> **生成 HTML 时必须优先内联此 SVG**，通过 `width`/`height` 属性缩放至目标尺寸（large=96px / medium=64px / small=16px），禁止用 CSS 重画。
> Dark 模式覆盖规则：活跃弧第二段 `<path>` 的 `fill` 改为 `rgb(73,122,248)`；轨道环第一段 `<path>` 的 `fill` 改为 `rgb(255,255,255)`，`fill-opacity` 改为 `0.15`。

---

### 变体索引表

| 变体属性 | 可选值 | 默认值 | 视觉差异 |
|---------|--------|--------|---------|
| size | `large` / `medium` / `small` | `large` | 环形尺寸（96/64/16px）、布局方向（垂直/垂直/水平）、文字字号不同 |
| Dark | `off` / `on` | `off` | 活跃弧颜色和文字颜色自动切换深色值 |

---

### 布局规格

| 规格项 | large | medium | small |
|--------|-------|--------|-------|
| 最小宽度 | 96px | 64px | 56px |
| 最小高度 | 136px | 102px | 18px |
| 环形图标直径 | 96px | 64px | — |
| 弧形图标尺寸 | — | — | 16×16px |
| 图标与文字间距 | 16px（padding-top） | 16px（padding-top） | 4px（`gap-1`） |
| 文字行高 | 24px | 22px | 18px |
| 布局方向 | 垂直居中 | 垂直居中 | 水平居中 |
| 文字字号 Token | `font_size-text1`（16px） | `font_size-tip1`（14px） | `font_size-tip2`（12px） |
| 文字行高 Token | `line_height-text1`（24px） | `line_height-tip1`（22px） | `line_height-tip2`（18px） |

> **说明**：环形图标内圈为透明，外轨道为灰色描边，活跃弧为蓝色描边，三者同心。

---

### 颜色 Token 映射

| 视觉区域 | Token 名称 | Light 色值 | Dark 色值 | 说明 |
|---------|-----------|-----------|----------|------|
| 活跃弧（旋转部分） | `--o-color-primary1` | `rgb(0, 47, 167)` | `rgb(73, 122, 248)` | 品牌蓝，构成视觉焦点 |
| 轨道环（背景环） | `--o-color-control4` | `rgba(grey-14, 0.1)` | `rgba(grey-14, 0.15)` | 浅灰轨道，large/medium/small 均有 |
| 文字"加载中" | `--o-color-info2` | `rgba(0, 0, 0, 0.8)` | `rgba(grey-1, 0.8)` | 二级/次强调文字色 |

---

### 字体样式映射

| 尺寸 | 字号 Token | 行高 Token | 字重 Token | 字号值 | 行高值 |
|------|-----------|-----------|-----------|-------|-------|
| large | `font_size-text1` | `line_height-text1` | `font_weight-regular` | 16px | 24px |
| medium | `font_size-tip1` | `line_height-tip1` | `font_weight-regular` | 14px | 22px |
| small | `font_size-tip2` | `line_height-tip2` | `font_weight-regular` | 12px | 18px |

字体族：`font_family`（HarmonyOS / Inter / -apple-system）

---

### 组件层级结构

```
OLoading large（VERTICAL，内容水平居中）
├── [GROUP 96×96px]
│   ├── 轨道环 VECTOR（88×88px，inset 4px）fill: color-control4
│   └── 活跃弧 VECTOR（48×48px，右上象限）fill: color-primary1
└── [Frame]（padding-top: 16px）
    └── PARAGRAPH "加载中"
          font: font_size-text1（16px）/ line_height-text1（24px）/ Regular
          fill: color-info2

OLoading medium（VERTICAL，内容水平居中）
├── [GROUP 64×64px]
│   ├── 轨道环 VECTOR（58.67×58.67px，inset 2.67px）fill: color-control4
│   └── 活跃弧 VECTOR（32×32px，右上象限）fill: color-primary1
└── [Frame]（padding-top: 16px）
    └── PARAGRAPH "加载中"
          font: font_size-tip1（14px）/ line_height-tip1（22px）/ Regular
          fill: color-info2

OLoading small（HORIZONTAL，垂直居中，gap: 4px）
├── [GROUP 16×16px]
│   ├── 轨道环 VECTOR（14.67×14.67px，inset 0.67px）fill: color-control4
│   └── 活跃弧 VECTOR（8×8px，右上象限）fill: color-primary1
└── PARAGRAPH "加载中"
      font: font_size-tip2（12px）/ line_height-tip2（18px）/ Regular
      fill: color-info2
```

---

### 变体 componentKey 速查

| 变体组合 | node_id | component_key |
|---------|---------|---------------|
| size=large, Dark=off | `1042:21479` | `3cf97d0c3c4372322d9068fa097b4bee99e9e729` |
| size=large, Dark=on | `1042:21488` | `3834e879a44214703952da8b12912c23d9a9ddce` |
| size=medium, Dark=off | `1042:21497` | `76461cdd34308e030e2ad8da2a2dd983c3479378` |
| size=medium, Dark=on | `1042:21507` | `ca6f538e6653c1b0563dd7383cd1d70af76aaa37` |
| size=small, Dark=off | `1042:21517` | `af4209f9412c63c5726baf715be8c0d7c482ab68` |
| size=small, Dark=on | `1042:21525` | `e777707fba89508e95436b7da36359d28f050fc4` |

---

### Pixso 操作速查

1. **插入组件**：组件面板搜索「OLoading」，拖入画布；或通过 `create_instance(componentKey)` 插入
2. **切换尺寸**：右侧面板 → size 属性 → 选择 `large` / `medium` / `small`
3. **切换主题**：右侧面板 → Dark 属性 → 选择 `off` / `on`
4. **修改文字**：双击进入组件实例 → 双击文字图层 → 替换"加载中"为自定义文案
5. **应用颜色变量**：`set_bound_variables` 绑定 `--o-color-primary1`（活跃弧）和 `--o-color-info2`（文字）

---

### 注意事项

- **三种尺寸均有轨道环**：large/medium/small 都由轨道环 + 活跃弧两层 VECTOR 组成，small 因尺寸小（16px）视觉上轨道环较细
- **文字不是必须**：可在设计稿中隐藏文字层，仅展示旋转环/弧，用于图标区域或空间极紧凑场景
- **不要嵌套 Loading**：同一区域只放一个 Loading 指示器，避免视觉干扰
- **遮罩配合**：页面级加载时，需单独在 Loading 下层添加半透明蒙层 Frame，OLoading 本身不含遮罩
- **深色模式**：活跃弧颜色在深色模式下为 `rgb(73,122,248)`（亮蓝），确保在深色背景可见

---

## 技术备注

- 组件集合节点：`1042:21478`
- 设计稿 URL：https://pixso.cn/app/design/JZkjW0mhmT61Mtd98dCfBw?item-id=1042:21478
- 生成日期：2026-04-28
- 变体轴：size（large / medium / small）× Dark（off / on）= 6 个变体
- 规格来源：`design_to_code` 三个尺寸变体（large/medium/small Dark=off）实测数据