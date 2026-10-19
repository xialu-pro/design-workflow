> ← [组件索引](../SKILL.md#组件索引) · [README](../README.md)

# OScrollbar 滚动条 · 设计 Skill

> 组件集合节点：`1042:17346` · 组件名：OScrollbar 滚动条 · 变体总数：8

---

## Part A：设计使用卡

### 组件概览

**OScrollbar 滚动条**：用于展示和控制可滚动内容的当前位置。通过滑块（thumb）在轨道中滑动，传达当前可见区域在整体内容中的位置比例。支持水平和垂直方向，以及两种尺寸（small/medium）。轨道透明，滑块使用 `--o-color-control1`（grey-14 @ 0.25）填充。

---

### 适用场景

- ✅ **长列表滚动**：表格、列表、树形结构等长内容区域的滚动指示
- ✅ **页面滚动**：整体页面内容超出可视区域时的滚动控制
- ✅ **文本编辑器**：代码编辑器、富文本编辑器的滚动条
- ✅ **侧边栏滚动**：导航菜单、目录等侧边栏内容的滚动
- ❌ **不适合**：固定高度的短内容区域、无需滚动的界面

---

### 变体说明

**size（尺寸）**
- `medium` — 轨道宽度/高度 16px，滑块宽度/高度 6px，适合大多数场景
- `small` — 轨道宽度/高度 12px，滑块宽度/高度 4px，适合紧凑布局或精细内容

**Direction（方向）**
- `Y` — 垂直滚动条，轨道宽度固定（12/16px），高度自适应
- `X` — 水平滚动条，轨道高度固定（12/16px），宽度自适应

**Dark（主题）**
- `off` — 浅色模式（默认），滑块使用 grey-14 @ 0.25 填充
- `on` — 深色模式，滑块使用 grey-14 @ 0.25 填充

> **说明**：OScrollbar 没有 Disabled（禁用）状态变体，所有变体均为 Enabled 状态。滚动条轨道是透明的，不显示背景色。

---

### 布局结构

> 🧩 **布局结构**：滚动条由轨道（SYMBOL 本体）和滑块（RECTANGLE）组成。轨道是透明的，滑块显示当前滚动位置。滑块长度根据内容比例自适应。

**Direction=Y（垂直）**

```
OScrollbar（SYMBOL，宽度固定，高度自适应）
├── Width: 16px（medium）/ 12px（small）
├── Height: 自适应（示例 240px）
├── fill: transparent（轨道透明）
│
└── [编组 GROUP]（滑块容器）
    ├── Width: 6px（medium）/ 4px（small）
    ├── Height: 自适应（根据内容比例）
    │
    ├── [轨道占位 RECTANGLE]（透明）
    │     fill: none
    └
    └── [滑块 RECTANGLE]（显示滚动位置）
          Width: 6px（medium）/ 4px（small）
          Height: 自适应（示例 120px medium / 80px small）
          fill: `--o-color-control1`（Light/Dark）
          位置：根据滚动比例在轨道中定位
```

**Direction=X（水平）**

```
OScrollbar（SYMBOL，高度固定，宽度自适应）
├── Width: 自适应（示例 600px）
├── Height: 16px（medium）/ 12px（small）
├── fill: transparent（轨道透明）
│
└── [滑块 RECTANGLE]（进度线条）
      Width: 自适应（根据内容比例）
      Height: 6px（medium）/ 3px（small）
      fill: `--o-color-control1`（Light/Dark）
      位置：根据滚动比例在轨道中定位
```

---

### 组合搭配

> 🔗 **常见搭配**：
> - **表格滚动**：ODataTable + OScrollbar Y，纵向滚动表格数据
> - **侧边栏滚动**：侧边栏容器 + OScrollbar Y，纵向滚动导航菜单
> - **代码编辑器**：代码编辑区域 + OScrollbar Y + OScrollbar X，双方向滚动
> - **页面滚动**：页面容器 + OScrollbar Y，整体页面滚动控制

---

### 设计稿识别指南

> 🔍 **识别特征**：
> - 轨道节点（SYMBOL）无填充色（transparent），宽度/高度固定
> - 滑块节点（RECTANGLE）显示填充色，宽度/高度根据尺寸变体而定
> - Y 方向：轨道宽度 12/16px，滑块宽度 3/6px
> - X 方向：轨道高度 12/16px，滑块高度 3/6px
> - Dark=off：滑块 rgba(0,0,0,0.25)（grey-14 @ 0.25）
> - Dark=on：滑块 rgba(255,255,255,0.25)（grey-14 @ 0.25）

> 🔄 **易混淆组件**：
> - 与 **OProgress 进度条**：Scrollbar 用于滚动控制，滑块位置表示可见区域；Progress 用于进度展示，填充表示完成比例
> - 与 **OSlider 滑块**：Slider 用于值选择，有刻度和交互；Scrollbar 用于滚动指示，无刻度

---

### 响应式行为

参照 [栅格规范](../SKILL.md#数据资源) 中的断点定义，OScrollbar 滑块长度根据内容比例自适应，无固定断点响应式行为。

---

## Part B：规格速查参考

### 变体索引表

| 变体属性 | 可选值 | 默认值 | 视觉差异 |
|---------|--------|--------|---------|
| size | `medium` / `small` | `medium` | 轨道宽度/高度 16/12px，滑块宽度/高度 6/4px |
| Direction | `Y` / `X` | `Y` | 垂直/水平方向 |
| Dark | `off` / `on` | `off` | 滑块颜色 `--o-color-control1` |

---

### 布局规格

#### Direction=Y（垂直）

| 规格项 | medium | small |
|--------|--------|-------|
| 轨道宽度 | 16px | 12px |
| 轨道高度 | 自适应 | 自适应 |
| 滑块宽度 | 6px | 4px |
| 滑块高度 | 自适应（根据内容比例） | 自适应 |
| 滑块最小高度 | 建议不低于 40px | 建议不低于 30px |

#### Direction=X（水平）

| 规格项 | medium | small |
|--------|--------|-------|
| 轨道宽度 | 自适应 | 自适应 |
| 轨道高度 | 16px | 12px |
| 滑块宽度 | 自适应（根据内容比例） | 自适应 |
| 滑块高度 | 6px | 3px |
| 滑块最小宽度 | 建议不低于 40px | 建议不低于 30px |

---

### 颜色 Token 映射

#### 滑块（thumb）

| 区域 | Light 模式 | Dark 模式 | Token | RGB 值 |
|------|-----------|----------|-------|--------|
| 滑块 | `grey-14 @ 0.25` | `grey-14 @ 0.25` | `--o-color-control1` | rgba(0,0,0,0.25) → rgba(255,255,255,0.25) |

#### 轨道（track）

| 区域 | Light 模式 | Dark 模式 | Token | 说明 |
|------|-----------|----------|-------|------|
| 轨道背景 | transparent | transparent | — | 无填充色，透明 |

> **说明**：滚动条轨道是透明的，不显示背景色，仅通过滑块传达滚动位置。滑块颜色使用 `--o-color-control1`（grey-14 @ 0.25），Light 和 Dark 模式下均一致。

---

### 组件层级结构

**Direction=Y，size=medium**

```
OScrollbar（SYMBOL，垂直）
│  GUID: 1042:17347（Dark=off）/ 1042:17350（Dark=on）
│  Width: 16px | Height: 自适应（示例 240px）
│  fill: transparent（visible=False, opacity=0）
│
└── [编组 GROUP]
    GUID: 1042:17348
    Width: 6px | Height: 自适应（示例 120px）
    │
    └── [滑块 RECTANGLE]
        GUID: 1042:17349
        Width: 6px | Height: 自适应（示例 120px）
        fill: rgba(0,0,0,0.25) → Token: `--o-color-control1`（Light）
              rgba(255,255,255,0.25) → Token: `--o-color-control1`（Dark）
```

**Direction=Y，size=small**

```
OScrollbar（SYMBOL，垂直）
│  GUID: 1042:17353（Dark=off）/ 1042:17357（Dark=on）
│  Width: 12px | Height: 自适应（示例 240px）
│  fill: transparent
│
└── [编组 GROUP]
    Width: 4px | Height: 自适应
    │
    ├── [轨道占位 RECTANGLE]
    │   Width: 4px | Height: 自适应（示例 240px）
    │   fill: none（透明）
    │
    └── [滑块 RECTANGLE]
        Width: 4px | Height: 自适应（示例 80px）
        fill: `--o-color-control1`
```

**Direction=X，size=medium**

```
OScrollbar（SYMBOL，水平）
│  GUID: 1042:17342（Dark=off）/ 1042:17344（Dark=on）
│  Width: 自适应（示例 600px）| Height: 16px
│  fill: transparent
│
└── [滑块 RECTANGLE]
    GUID: 1042:17343
    Width: 自适应（示例 600px）| Height: 6px
    fill: `--o-color-control1`
```

**Direction=X，size=small**

```
OScrollbar（SYMBOL，水平）
│  GUID: 1042:17336（Dark=off）/ 1042:17339（Dark=on）
│  Width: 自适应（示例 600px）| Height: 12px
│  fill: transparent
│
└── [进度线条 GROUP]
    Width: 自适应 | Height: 3px
    │
    └── [滑块 RECTANGLE]
        Width: 自适应（示例 600px）| Height: 3px
        fill: `--o-color-control1`
```

---

### 变体 componentKey 速查

| 变体组合 | node_id | 尺寸（示例） | 说明 |
|---------|---------|------------|------|
| size=medium, Dark=off, Direction=Y | `1042:17347` | 16×240px | 垂直，中尺寸，浅色 |
| size=medium, Dark=on, Direction=Y | `1042:17350` | 16×240px | 垂直，中尺寸，深色 |
| size=small, Dark=off, Direction=Y | `1042:17353` | 12×240px | 垂直，小尺寸，浅色 |
| size=small, Dark=on, Direction=Y | `1042:17357` | 12×240px | 垂直，小尺寸，深色 |
| size=medium, Dark=off, Direction=X | `1042:17342` | 600×16px | 水平，中尺寸，浅色 |
| size=medium, Dark=on, Direction=X | `1042:17344` | 600×16px | 水平，中尺寸，深色 |
| size=small, Dark=off, Direction=X | `1042:17336` | 600×12px | 水平，小尺寸，浅色 |
| size=small, Dark=on, Direction=X | `1042:17339` | 600×12px | 水平，小尺寸，深色 |

> **注意**：示例尺寸（240px/600px）仅为演示，实际使用时轨道和滑块长度根据内容自适应。

---

### Pixso 操作速查

1. **插入组件**：组件面板搜索「OScrollbar」，拖入画布
2. **切换尺寸**：右侧面板 → size 属性 → 选择 `medium` / `small`
3. **切换方向**：右侧面板 → Direction 属性 → 选择 `Y` / `X`
4. **切换主题**：右侧面板 → Dark 属性 → 选择 `off` / `on`
5. **调整轨道长度**：手动调整 SYMBOL 的 width（X 方向）或 height（Y 方向）
6. **调整滑块长度**：进入组件 → 调整滑块 RECTANGLE 的长度

---

### 注意事项

- **无 Disabled 状态**：OScrollbar 没有 Disabled（禁用）变体，所有状态均为 Enabled
- **轨道透明**：轨道背景是透明的（visible=False, opacity=0），无填充色
- **滑块颜色**：Light/Dark 模式均使用 `--o-color-control1`（grey-14 @ 0.25）
- **滑块长度自适应**：滑块长度根据内容比例计算，示例值（120px/80px）仅为演示
- **滑块最小尺寸**：建议滑块最小不低于 30-40px，确保可识别和可操作
- **尺寸选择**：medium 适合大多数场景，small 适合紧凑布局或精细内容（如代码编辑器）
- **方向选择**：Y 方向用于纵向滚动内容，X 方向用于横向滚动内容
- **滚动比例计算**：滑块长度 = 轨道长度 × (可见区域 / 总内容长度)
- **Dark 模式差异**：滑块颜色使用 `--o-color-control1`（grey-14 @ 0.25），Light 和 Dark 模式下颜色值不同但透明度一致

---

## Part C：交互与状态

### 交互状态

| 元素 | 状态 | 视觉表现 |
|------|------|---------|
| 滑块 | Light | fill：`--o-color-control1`（rgba(0,0,0,0.25)） |
| 滑块 | Dark | fill：`--o-color-control1`（rgba(255,255,255,0.25)） |
| 轨道 | 所有状态 | transparent（透明，无填充） |

---

### Hover 状态（交互态）

| 元素 | Hover 视觉表现 |
|------|---------------|
| 滑块 | 滑块宽度/高度略微放大（可选动效） |
| 滑块 | 颜色不变 |

---

### 滚动比例计算

- **滑块长度计算公式**：
  - Direction=Y：滑块高度 = 轨道高度 × (可见区域高度 / 总内容高度)
  - Direction=X：滑块宽度 = 轨道宽度 × (可见区域宽度 / 总内容宽度)

- **滑块位置计算公式**：
  - Direction=Y：滑块 top = 轨道高度 × (当前滚动位置 / 总可滚动距离)
  - Direction=X：滑块 left = 轨道宽度 × (当前滚动位置 / 总可滚动距离)

---

## Part D：设计变量绑定

### 推荐绑定变量

| 元素 | 属性 | 推荐变量 Token |
|------|------|---------------|
| 滑块 | fill | `--o-color-control1` |
| 轨道宽度（Y medium） | width | 16px |
| 轨道宽度（Y small） | width | 12px |
| 轨道高度（X medium） | height | 16px |
| 轨道高度（X small） | height | 12px |
| 滑块宽度（Y medium） | width | 6px |
| 滑块宽度（Y small） | width | 4px |
| 滑块高度（X medium） | height | 6px |
| 滑块高度（X small） | height | 3px |

---

## Part E：最佳实践

### 使用建议

1. **尺寸选择**：默认使用 medium 尺寸，紧凑布局或精细内容使用 small 尺寸
2. **最小滑块尺寸**：确保滑块可识别，建议最小不低于 30-40px
3. **双方向滚动**：内容超出可视区域时，同时显示 X 和 Y 方向滚动条
4. **自动隐藏**：在支持的场景下，滚动条可在不滚动时自动隐藏，节省空间

### 设计提示

- 滚动条轨道透明，滑块颜色使用 `--o-color-control1`（grey-14 @ 0.25）
- small 尺寸适合代码编辑器、侧边栏等精细内容滚动
- medium 尺寸适合表格、列表等常规内容滚动
- 滚动条应与容器边缘保持一定间距（通常 4-8px）
- 滚动时滑块位置和长度应实时更新，反映当前滚动状态

---

## 技术备注

- 组件节点 ID：`1042:17346`（组件集）
- 设计稿 URL：https://pixso.cn/app/design/JZkjW0mhmT61Mtd98dCfBw?item-id=1042:17346
- 生成日期：2026-04-17
- 变体总数：8（size=medium/small × Dark=off/on × Direction=Y/X）
- 无 Disabled 状态：所有变体均为 Enabled
- 轨道尺寸：Y medium=16px宽，Y small=12px宽；X medium=16px高，X small=12px高
- 滑块尺寸：Y medium=6px宽，Y small=4px宽；X medium=6px高，X small=3px高
- 轨道背景：transparent（透明）
- 滑块颜色：使用 `--o-color-control1`（grey-14 @ 0.25）