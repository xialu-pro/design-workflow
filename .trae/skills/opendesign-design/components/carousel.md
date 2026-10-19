> ← [组件索引](../SKILL.md#组件索引) · [README](../README.md)

# OCarousel 幻灯片 · 设计 Skill

> 组件集合节点：`1042:21288` · 组件名：OCarousel 幻灯片指示器 · 变体总数：4（size × Dark）

---

## Part A：设计使用卡

### 组件概览

**OCarousel 幻灯片指示器**：用于幻灯片（轮播图）底部的位置指示组件，由多个横向排列的矩形条（指示块）组成。当前活跃项以品牌色高亮显示，其余项以半透明色示意，直观呈现当前所在位置及总项数。提供 `small` 和 `medium` 两种尺寸，支持浅色 / 深色主题。

---

### 适用场景

- ✅ **图片轮播**：Banner 图、营销海报轮播底部位置指示
- ✅ **内容卡片切换**：多张卡片横向滑动时的进度指示
- ✅ **引导步骤**：新手引导/启动页横向步骤指示
- ✅ **数据概览面板**：Dashboard 多组数据横向切换的位置标记
- ❌ **不适合**：纵向滚动内容（用 OScrollbar）、步骤引导带文字说明（用 OStep）

---

### 变体说明

**size（尺寸）**
- `small` — 小尺寸，指示块高度 2px，宽度较窄，适合紧凑场景或移动端
- `medium` — 中尺寸，指示块高度 3px，宽度较宽，适合 PC 端或大屏展示

**state（状态）**
- `normal` — 正常展示状态（当前设计稿仅提供此状态）

**Dark（主题）**
- `off` — 浅色模式（默认），活跃色 rgb(0,47,167)，非活跃色 rgba(0,0,0,0.25)
- `on` — 深色模式，活跃色 rgb(73,122,248)，非活跃色 rgba(255,255,255,0.25)

> **指示块数量**：设计稿中固定显示 8 个指示块，第 1 个（最左侧）为活跃状态。实际使用时根据内容总数动态调整数量，由开发实现。

---

### 布局结构

> 🧩 **布局结构**：指示器整体水平排列（HORIZONTAL Auto Layout），由多个等高矩形条依序排列。

```
OCarousel 指示器（HORIZONTAL，自适应宽度，固定高度）
│  autoLayoutItemSpacing: 4px（small）/ 12px（medium）
│  counterAlign: center
│
├── [活跃指示块 RECTANGLE]（第一个，品牌色填充）
│     width: 24px（small）/ 48px（medium）
│     height: 2px（small）/ 3px（medium）
│     cornerRadius: 3px（small）/ 6px（medium）
│     fill: color-primary1
│
└── [非活跃指示块 × N RECTANGLE]（其余项，半透明填充）
      width: 24–26px（small）/ 48px（medium）
      height: 2px（small）/ 3px（medium）
      cornerRadius: 3px（small）/ 6px（medium）
      fill: color-control1（半透明）
```

---

### 组合搭配

> 🔗 **常见搭配**：
> - **图片轮播容器**：图片/卡片区域下方居中放置指示器，与轮播内容垂直间距 12–16px
> - **引导页**：与 OButton（下一步）组合，指示器居中，按钮置于底部
> - **Banner 区域**：叠加在图片底部，指示器使用深色模式（Dark=on）保证可见性

---

### 设计稿识别指南

> 🔍 **识别特征**：
> - 水平自动布局，内含多个等高矩形条，其中一个为品牌色，其余半透明
> - 指示块高度极小（2–3px），宽高比大（约 12:1 至 16:1）
> - 整体无文字、无图标，仅由矩形色块组成

> 🔄 **易混淆组件**：
> - 与 **OStep 步骤条** 的区别：OCarousel 指示器为纯图形无文字，OStep 带序号和说明文字
> - 与 **OPagination 分页** 的区别：OCarousel 指示器为横向色条，OPagination 为数字/箭头分页控件
> - 与 **OScrollbar 滚动条** 的区别：OScrollbar 为纵向/横向滚动轨道，OCarousel 指示器为位置圆点/色条

---

### 响应式行为

参照 [栅格规范](../SKILL.md#数据资源) 中的断点定义：
- **PC 端**：推荐使用 `medium` 尺寸，居中对齐
- **移动端 / 紧凑布局**：推荐使用 `small` 尺寸，居中对齐

---

## Part B：规格速查参考

### 变体索引表

| 变体属性 | 可选值 | 默认值 | 视觉差异 |
|---------|--------|--------|---------|
| size | `small` / `medium` | `medium` | 指示块高度和宽度不同 |
| state | `normal` | `normal` | 正常展示 |
| Dark | `off` / `on` | `off` | 活跃/非活跃颜色随主题切换 |

---

### 布局规格

| 规格项 | small | medium |
|--------|-------|--------|
| 组件高度 | 2px | 3px |
| 活跃指示块宽度 | 24px | 48px |
| 非活跃指示块宽度 | 24–26px | 48px |
| 指示块间距（gap） | 4px | 12px |
| 圆角 | 3px | 6px |
| 设计稿示例总宽（8块） | 228px | 468px |

> **说明**：`small` 尺寸中非活跃指示块宽度有 24px / 26px 两种（前 4 个 24px，后 4 个 26px），为设计稿当前实测值。实际实现中统一为 24px 即可。

---

### 颜色 Token 映射

| 状态 | Token 名称 | Light RGB | Dark RGB |
|------|-----------|----------|----------|
| 活跃指示块 | `--o-color-primary1` | rgb(0, 47, 167) | rgb(73, 122, 248) |
| 非活跃指示块 | `--o-color-control1` | rgba(0, 0, 0, 0.251) | rgba(255, 255, 255, 0.251) |

---

### 组件层级结构

```
OCarousel（HORIZONTAL，自适应宽，固定高）
│  autoLayoutItemSpacing: 4px（small）/ 12px（medium）
│  counterAlign: center（light）/ flex-start（dark，注意对齐差异）
│
├── [活跃指示块]
│     type: RECTANGLE
│     width: 24px（small）/ 48px（medium）
│     height: 2px（small）/ 3px（medium）
│     cornerRadius: 3px（small）/ 6px（medium）
│     fill: color-primary1（Light: rgb(0,47,167) / Dark: rgb(73,122,248)）
│
└── [非活跃指示块 × 7]
      type: RECTANGLE
      width: 24–26px（small）/ 48px（medium）
      height: 2px（small）/ 3px（medium）
      cornerRadius: 3px（small）/ 6px（medium）
      fill: color-control1（Light: rgba(0,0,0,0.251) / Dark: rgba(255,255,255,0.251)）
```

---

### 变体 node_id 速查

| 变体组合 | node_id | 尺寸（宽×高） | 说明 |
|---------|---------|-------------|------|
| size=small, state=normal, Dark=off | `1042:21289` | 228×2px | 小尺寸，浅色模式 |
| size=small, state=normal, Dark=on | `1042:21298` | 228×2px | 小尺寸，深色模式 |
| size=medium, state=normal, Dark=off | `1042:21307` | 468×3px | 中尺寸，浅色模式 |
| size=medium, state=normal, Dark=on | `1042:21316` | 468×3px | 中尺寸，深色模式 |

> **组件集合节点**：`1042:21288`（父节点，包含以上 4 个变体）

---

### Pixso 操作速查

1. **插入组件**：组件面板搜索「OCarousel」，拖入画布
2. **切换尺寸**：右侧面板 → size 属性 → 选择 `small` / `medium`
3. **切换主题**：右侧面板 → Dark 属性 → 选择 `off` / `on`
4. **调整指示块数量**：进入组件实例 → 增减矩形子层，保持 gap 一致
5. **切换活跃位置**：修改目标矩形的 fill 为 `--o-color-primary1`，原活跃项改为 `--o-color-control1`

---

### 注意事项

- **尺寸选择**：`small` 适合移动端或紧凑场景（2px 高，视觉极轻），`medium` 适合 PC 端或需要较强存在感的场景
- **指示块数量**：设计稿固定 8 块；实际使用时与开发对齐，按实际轮播项数动态生成
- **活跃位置**：活跃块默认在最左侧（第 1 项），布局时可根据当前页码调整位置
- **深色模式对齐**：设计稿中 Dark=on 的 counterAlign 为 flex-start，与 Dark=off 的 center 略有差异，实际使用时统一为 center
- **叠加在深色背景上**：选用 Dark=on 变体，确保非活跃指示块在深色背景上仍可见（白色半透明）
- **间距规范**：指示器与上方轮播内容的垂直间距推荐 12–16px，与页面边距对齐

---

## Part C：交互与状态

### 状态定义

| 状态 | 说明 |
|------|------|
| 默认（normal） | 一个活跃块（primary 色）+ 其余非活跃块（半透明） |
| 切换中 | 活跃状态从当前块过渡到相邻块，通常伴随宽度/透明度动画 |

### 交互行为

- **自动轮播**：轮播内容切换时活跃指示块随之移动
- **点击指示块**：点击非活跃块跳转到对应内容项（由开发实现，设计稿不体现）
- **拖拽手势**：移动端支持左右拖拽，释放后活跃块更新（由开发实现）

---

## Part D：设计变量绑定

### 推荐绑定变量

| 元素 | 属性 | 推荐变量 Token |
|------|------|---------------|
| 活跃指示块填充 | fill | `--o-color-primary1` |
| 非活跃指示块填充 | fill | `--o-color-control1` |
| 指示块圆角（small） | cornerRadius | 3px（无对应 Token，直接填值） |
| 指示块圆角（medium） | cornerRadius | 6px（无对应 Token，直接填值） |
| 指示块高度（small） | height | 2px |
| 指示块高度（medium） | height | 3px |
| 间距（small） | autoLayoutItemSpacing | `gap-1`（4px） |
| 间距（medium） | autoLayoutItemSpacing | `gap-3`（12px） |

---

## Part E：最佳实践

### 使用建议

1. **主题匹配**：叠加于深色图片/背景时使用 Dark=on，浅色背景使用 Dark=off
2. **数量克制**：指示块过多（>10）时建议改用数字分页或仅显示 x/n 文字
3. **居中放置**：指示器始终水平居中于轮播容器，避免左对齐或右对齐
4. **间距统一**：指示器与轮播内容的距离全局统一，推荐 12px

### 设计提示

- 小尺寸指示器（small）在视觉上极为克制，适合不希望抢占焦点的场景
- 如果设计要求活跃块与非活跃块宽度不同（active 更宽），需自行扩展组件变体
- 深色模式下非活跃块为白色半透明，确保最低 25% 不透明度以保证可见性
- 横向多个指示块时，总宽度应不超过内容区宽度的 30%

---

## 技术备注

- 组件集合节点 ID：`1042:21288`
- 设计稿 URL：https://pixso.cn/app/design/JZkjW0mhmT61Mtd98dCfBw?item-id=1042:21288
- 生成日期：2026-06-02
- 变体总数：4（size=small/medium × Dark=off/on）
- 状态：normal（仅当前设计稿提供的状态）
- 数据来源：Pixso MCP DSL 实测数据，颜色值与 Token 映射均经设计稿确认