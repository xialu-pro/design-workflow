> ← [组件索引](../SKILL.md#组件索引) · [README](../README.md)

# OPagination 分页 · 设计 Skill

> 组件集合节点：`1042:13347` · 组件名：OPagination 分页 · 变体总数：4

---

## Part A：设计使用卡

### 组件概览

**OPagination 分页**：用于内容分页展示，帮助用户快速浏览和定位大量数据。提供页码翻页、跳转、每页条数选择和总数统计功能。

---

### 适用场景

- ✅ **数据表格分页**：表格底部标配组件，展示数据分页信息并提供翻页控制
- ✅ **列表内容分页**：卡片列表、图片列表等需要分页展示的内容区域
- ✅ **搜索结果分页**：搜索引擎、筛选结果等大量数据的分页导航
- ✅ **后台管理系统**：数据管理界面的标准分页控件
- ❌ **不适合**：无分页需求的单页内容、移动端无限滚动列表

---

### 变体说明

**show（显示模式）**
- `All` — 完整模式，显示页码、跳转器、每页条数选择器和数据统计，适合后台管理系统
- `Simple` — 简约模式，仅显示页码和总条数，适合前台展示页面

**Dark（主题）**
- `off` — 浅色模式（默认）
- `on` — 深色模式，品牌色系和背景色自动切换为对应深色值

**visible_5_60（数据统计）**
- `true` — 显示数据统计部分（show=All 时有效）
- `false` — 隐藏数据统计部分

---

### 布局结构

> 🧩 **布局结构**：分页整体水平排列（HORIZONTAL Auto Layout），由 Pager Ctrl（页码区）、Jumper Ctrl（跳转区）和数据区组成，各区域间距 24px。

**show=All 模式**

```
OPagination（HORIZONTAL，自适应宽度，固定高度 32px）
├── [Pager Ctrl FRAME]（HORIZONTAL，gap 8px，自适应宽度）
│   ├── [左箭头 FRAME]（32×32，含图标 INSTANCE 24×24）
│   ├── [页码按钮 FRAME]（32×32，多个）
│   │   ├── 选中态：背景 brand-6，文字白色，圆角 4px
│   │   └── 常态：背景透明，文字 info1，无圆角
│   └── [右箭头 FRAME]（32×32，含图标 INSTANCE 24×24）
├── [Jumper Ctrl FRAME]（HORIZONTAL，gap 8px，固定宽度 100px）
│   ├── [输入框 + 页码跳转]
│   └── [页文字]
└── [数据 FRAME]（HORIZONTAL，自适应宽度）
│   ├── [Pagesize Ctrl]（每页条数下拉选择器，含下箭头图标）
│   └── [Total Label]（总条数统计）
```

**show=Simple 模式**

```
OPagination（HORIZONTAL，自适应宽度，固定高度 32px）
├── [左箭头 FRAME]（32×32）
├── [页 FRAME]（当前页/总页数）
└── [右箭头 FRAME]（32×32）
```

---

### 组合搭配

> 🔗 **常见搭配**：
> - **数据表格底部**：`show=All` 模式，配合 ODataTable 组件，提供完整的分页控制
> - **卡片列表**：`show=Simple` 模式，简洁的翻页控制
> - **搜索结果**：`show=All` 模式，快速跳转和每页条数调整
> - **移动端适配**：`show=Simple` 模式，减少控件数量

---

### 设计稿识别指南

> 🔍 **识别特征**：
> - 水平自动布局节点，高度固定为 32px，包含页码按钮和箭头图标
> - 选中页码有品牌色（brand-6）背景填充，圆角 4px
> - 未选中页码背景透明，仅显示数字文字
> - 包含下箭头图标（每页条数选择器）和总条数文字（show=All 模式）

> 🔄 **易混淆组件**：
> - 与 **OBreadcrumb 面包屑** 的区别：分页用于数据翻页导航，面包屑用于层级路径导航
> - 与 **OStep 步骤条** 的区别：分页切换数据内容，步骤条引导流程进度

---

### 响应式行为

参照 [栅格规范](../SKILL.md#数据资源) 中的断点定义：
- **PC（1920px）**：推荐使用 `show=All` 模式，完整展示所有控件
- **移动端（360px）**：推荐切换为 `show=Simple` 模式，隐藏跳转器和每页条数下拉选择器

---

## Part B：规格速查参考

### 变体索引表

| 变体属性 | 可选值 | 默认值 | 视觉差异 |
|---------|--------|--------|---------|
| show | `All` / `Simple` | `All` | 完整模式含跳转器/每页条数下拉选择器/数据统计；简约模式仅页码 |
| Dark | `off` / `on` | `off` | 品牌色切换为深色主题对应值 |
| visible_5_60 | `true` / `false` | `true` | 显示/隐藏数据统计部分（show=All 时有效） |

---

### 布局规格

| 规格项 | show=All | show=Simple |
|--------|----------|-------------|
| 整体高度 | 32px | 32px |
| Token（高度） | `control_size-m` | `control_size-m` |
| 区域间距 | 24px | — |
| Token（间距） | `gap-6` | — |
| Pager Ctrl 内部间距 | 8px | — |
| Token（内部间距） | `gap-2` | — |
| 页码按钮尺寸 | 32×32px | — |
| Token（按钮尺寸） | `control_size-m` | — |
| 图标尺寸 | 24×24px | 24×24px |
| Token（图标尺寸） | `icon_size_control-m` | `icon_size_control-m` |

---

### 颜色 Token 映射

#### 选中页码

| 区域 | Light 模式 Token | Dark 模式 Token | 说明 |
|------|-----------------|----------------|------|
| 背景 | `--o-color-primary1` | `--o-color-primary1` | brand-6：rgb(0,47,167) → rgb(73,122,248) |
| 文字 | `white` | `white` | 始终为白色 rgb(255,255,255) |

#### 未选中页码/箭头按钮

| 区域 | Light 模式 Token | Dark 模式 Token | 说明 |
|------|-----------------|----------------|------|
| 背景 | 透明 | 透明 | 无填充 |
| 线框 | `--o-color-control1` | `--o-color-control1` | 箭头按钮默认线框 |
| 文字/图标 | `--o-color-info1` | `--o-color-info1` | 一级文字色 |

#### 禁用箭头

| 区域 | Light 模式 Token | Dark 模式 Token | 说明 |
|------|-----------------|----------------|------|
| 线框 | `--o-color-control4` | `--o-color-control4` | 禁用线框色 |
| 文字/图标 | `--o-color-info4` | `--o-color-info4` | 禁用色，透明度 40% |

#### 箭头悬浮状态

| 区域 | Light 模式 Token | Dark 模式 Token | 说明 |
|------|-----------------|----------------|------|
| 线框 | `--o-color-control2` | `--o-color-control2` | 悬浮线框色 |
| 图标 | `--o-color-primary1` | `--o-color-primary1` | 悬浮图标色 |

#### 数据统计文字

| 区域 | Light 模式 Token | Dark 模式 Token | 说明 |
|------|-----------------|----------------|------|
| 总条数/每页条数 | `--o-color-info1` | `--o-color-info1` | 一级文字色，强调信息 |

#### 页码输入框

**输入框线框与填充**

| 状态 | 线框 Token | 填充 Token | 说明 |
|------|-----------|-----------|------|
| 默认 | `--o-color-control1` | `--o-color-fill2` | 常规线框，二级填充背景 |
| 悬浮 | `--o-color-primary2` | `--o-color-fill2` | 悬浮线框（brand-4） |
| 选中 | `--o-color-primary1` | `--o-color-fill2` | 选中线框（brand-6） |
| 禁用 | `--o-color-control4` | `--o-color-control4-light` | 禁用线框和背景 |

**输入框文字**

| 状态 | 文字 Token | 说明 |
|------|-----------|------|
| 默认/悬浮 | `--o-color-info1` | 一级文字色 |
| 禁用 | `--o-color-info4` | 禁用文字色 |

#### 每页条数下拉选择器

**文字**

| 状态 | 文字 Token | 说明 |
|------|-----------|------|
| 默认/悬浮 | `--o-color-info1` | 一级文字色 |
| 禁用 | `--o-color-info4` | 禁用文字色 |

**图标**

| 状态 | 图标 Token | 说明 |
|------|-----------|------|
| 默认/悬浮 | `--o-color-info2` | 二级文字色 |
| 激活 | `--o-color-info2` | 二级文字色，垂直翻转 |
| 禁用 | `--o-color-info4` | 禁用图标色 |

**下拉选择框**

| 状态 | 线框 Token | 填充 Token | 说明 |
|------|-----------|-----------|------|
| 默认 | `--o-color-control1` | `--o-color-fill2` | 常规线框，二级填充背景 |
| 悬浮 | `--o-color-control2` | `--o-color-fill2` | 悬浮线框（brand-6） |
| 激活 | `--o-color-control3` | `--o-color-fill2` | 激活线框（brand-7） |
| 禁用 | `--o-color-control4` | `--o-color-control4-light` | 禁用线框和背景 |

---

### 字体样式映射

| 使用场景 | 字号 Token | 行高 Token | 字重 Token | 字号值 | 行高值 |
|---------|-----------|-----------|-----------|-------|-------|
| 页码数字 | `font_size-tip1` | `line_height-tip1` | `font_weight-regular` | 14px | 22px |
| 页码输入框文字 | `font_size-tip1` | `line_height-tip1` | `font_weight-regular` | 14px | 22px |
| 数据统计 | `font_size-tip1` | `line_height-tip1` | `font_weight-regular` | 14px | 22px |

字体族：`font_family`（HarmonyOS / HarmonyHeiTi Regular）

---

### 组件层级结构

```
OPagination（HORIZONTAL，自适应宽度，固定高 32px）
│  autoLayoutItemSpacing: 24px | autoLayoutPadding: 0
│
├── [Pager Ctrl FRAME]（HORIZONTAL，gap 8px）
│   │
│   ├── [左箭头 FRAME]（32×32）
│   │     fill: transparent
│   │     stroke: color-control1（默认）/ color-control4（禁用）/ color-control2（悬浮）
│   │     └── [展示类/图标 Icon/左箭头 INSTANCE]（24×24）
│   │           fill: color-info1（默认）/ color-info4（禁用）/ color-primary1（悬浮）
│   │
│   ├── [页码按钮 FRAME]（32×32，多个）
│   │     ├── 未选中态：
│   │     │     fill: transparent | cornerRadius: 0
│   │     │     └── [页码数字 PARAGRAPH]
│   │     │           font: font_size-tip1 | fill: color-info1
│   │     │
│   │     └── 选中态：
│   │     │     fill: color-primary1 | cornerRadius: 4px（radius_control-xs）
│   │     │     └── [页码数字 PARAGRAPH]
│   │     │           font: font_size-tip1 | fill: white
│   │
│   └── [右箭头 FRAME]（32×32）
│   │     fill: transparent
│   │     stroke: color-control1（默认）/ color-control4（禁用）/ color-control2（悬浮）
│   │     └── [展示类/图标 Icon/右箭头 INSTANCE]（24×24）
│   │           fill: color-info1（默认）/ color-info4（禁用）/ color-primary1（悬浮）
│   │
│   └── [省略号 PARAGRAPH]（可选，超出页数时显示）
│         font: font_size-tip1 | fill: color-info1
│
├── [Jumper Ctrl FRAME]（HORIZONTAL，gap 8px，宽度 100px）
│   ├── [页码输入框 FRAME]
│   │     fill: color-fill2（默认/悬浮/选中）/ color-control4-light（禁用）
│   │     stroke: color-control1（默认）/ color-primary2（悬浮）/ color-primary1（选中）/ color-control4（禁用）
│   │     ├── [输入框数字]
│   │     │     fill: color-info1（默认/悬浮/选中）/ color-info4（禁用）
│   │     └── [text PARAGRAPH]（"跳转"）
│   │           font: font_size-tip1 | fill: color-info1
│   └── [页 PARAGRAPH]
│         font: font_size-tip1 | fill: color-info1
│
└── [数据 FRAME]（HORIZONTAL，自适应宽度）
│   ├── [Pagesize Ctrl FRAME]（每页条数下拉选择器）
│   │     fill: color-fill2（默认/悬浮/激活）/ color-control4-light（禁用）
│   │     stroke: color-control1（默认）/ color-control2（悬浮）/ color-control3（激活）/ color-control4（禁用）
│   │     ├── [每页条数 PARAGRAPH]
│   │     │     font: font_size-tip1 | fill: color-info1（默认/悬浮）/ color-info4（禁用）
│   │     └── [图标 Icon/下箭头 INSTANCE]（24×24）
│   │           fill: color-info2（默认/悬浮/激活）/ color-info4（禁用）
│   │           激活时：垂直翻转
│   │
│   └── [Total Label PARAGRAPH]（"共 × 条"）
│         font: font_size-tip1 | fill: color-info1
```

---

### 变体 componentKey 速查

| 变体组合 | node_id | componentKey |
|---------|---------|-------------|
| show=All, Dark=off | `1042:13348` | 待补充（需从 Pixso 组件面板获取） |
| show=All, Dark=on | `1042:13379` | 待补充 |
| show=Simple, Dark=off | `1042:13414` | 待补充 |
| show=Simple, Dark=on | `1042:13423` | 待补充 |

---

### Pixso 操作速查

1. **插入组件**：组件面板搜索「OPagination」，拖入画布；或 AI 调用 `create_instance(componentKey)`
2. **切换显示模式**：右侧面板 → show 属性 → 选择 `All` / `Simple`
3. **切换主题**：show 属性 → Dark 选择 `on` / `off`
4. **隐藏数据统计**：show=All 时，切换 visible_5_60 属性
5. **修改页码数量**：进入 Pager Ctrl → 复制/删除页码按钮实例
6. **修改选中页码**：进入 Pager Ctrl → 修改选中态按钮的背景色和文字色

---

### 注意事项

- 选中页码使用品牌色背景（brand-6）+ 白色文字，圆角 4px，与其他页码视觉区分明显
- 未选中页码背景透明，避免视觉干扰，仅文字显示
- 禁用箭头使用 `--o-color-info4`（禁用色）图标 + `--o-color-control4` 禁用线框，点击区域保留但不可交互
- 左/右箭头按钮默认有 `--o-color-control1` 线框，尺寸 32×32px，悬浮时线框切换为 `--o-color-control2`
- 省略号显示在页码超出范围时，与其他页码文字颜色保持一致，使用一级文字色 `--o-color-info1`
- `show=Simple` 模式适合移动端或空间有限的场景，隐藏复杂控件
- 页码按钮尺寸固定 32×32px，与图标（24×24px）保持 4px 的内边距对齐
- 页码输入框宽度自适应，建议设置最小宽度约束避免变形
- 深色模式下品牌色自动切换为 rgb(73,122,248)，其他色值同步切换

---

## Part C：交互与状态

### 交互状态

| 元素 | 状态 | 视觉表现 |
|------|------|---------|
| 页码按钮 | 默认 | 背景：透明，文字：`--o-color-info1` |
| 页码按钮 | 选中 | 背景：`--o-color-primary1`（brand-6），文字：`white`，圆角：4px |
| 页码按钮 | 悬浮 | 背景：`--o-color-primary2`（brand-4），文字：`--o-color-info1` |
| 左/右箭头 | 默认 | 线框：`--o-color-control1`，图标：`--o-color-info1`，尺寸：32×32px |
| 左/右箭头 | 禁用 | 线框：`--o-color-control4`，图标：`--o-color-info4`，透明度 40% |
| 左/右箭头 | 悬浮 | 线框：`--o-color-control2`，图标：`--o-color-primary1` |
| 页码输入框文字 | 默认 | 文字：`--o-color-info1` |
| 页码输入框文字 | 悬浮 | 文字：`--o-color-info1` |
| 页码输入框 | 默认 | 线框：`--o-color-control1`，填充：`--o-color-fill2` |
| 页码输入框 | 悬浮 | 线框：`--o-color-primary2`，填充：`--o-color-fill2` |
| 页码输入框 | 选中 | 线框：`--o-color-primary1`，填充：`--o-color-fill2` |
| 页码输入框 | 禁用 | 线框：`--o-color-control4`，填充：`--o-color-control4-light`，文字：`--o-color-info4` |
| 每页条数下拉选择器文字 | 默认 | 文字：`--o-color-info1` |
| 每页条数下拉选择器文字 | 悬浮 | 文字：`--o-color-info1` |
| 每页条数下拉选择器文字 | 禁用 | 文字：`--o-color-info4` |
| 每页条数下拉选择器图标 | 默认 | 图标：`--o-color-info2` |
| 每页条数下拉选择器图标 | 悬浮 | 图标：`--o-color-info2` |
| 每页条数下拉选择器图标 | 激活 | 图标：`--o-color-info2`，垂直翻转 |
| 每页条数下拉选择器图标 | 禁用 | 图标：`--o-color-info4` |
| 每页条数下拉选择框 | 默认 | 线框：`--o-color-control1`，填充：`--o-color-fill2` |
| 每页条数下拉选择框 | 悬浮 | 线框：`--o-color-control2`，填充：`--o-color-fill2` |
| 每页条数下拉选择框 | 激活 | 线框：`--o-color-control3`，填充：`--o-color-fill2` |
| 每页条数下拉选择框 | 禁用 | 线框：`--o-color-control4`，填充：`--o-color-control4-light` |

---

### 状态切换逻辑

- **选中页码**：仅一个页码处于选中状态，切换时移除上一个选中态
- **禁用箭头**：首页时左箭头禁用，末页时右箭头禁用
- **省略号显示**：页码超过 7 页时，中间页码显示省略号

---

## Part D：设计变量绑定

### 推荐绑定变量

| 元素 | 属性 | 推荐变量 Token |
|------|------|---------------|
| 选中页码背景 | fill | `--o-color-primary1` |
| 选中页码文字 | text color | `white` |
| 未选中页码文字 | text color | `--o-color-info1` |
| 禁用箭头图标 | fill | `--o-color-info4` |
| 禁用箭头线框 | stroke | `--o-color-control4` |
| 默认箭头线框 | stroke | `--o-color-control1` |
| 悬浮箭头线框 | stroke | `--o-color-control2` |
| 数据统计文字 | text color | `--o-color-info1` |
| 每页条数下拉选择器文字 | text color | `--o-color-info1`（默认/悬浮）/ `--o-color-info4`（禁用） |
| 每页条数下拉选择器图标 | fill | `--o-color-info2`（默认/悬浮/激活）/ `--o-color-info4`（禁用） |
| 每页条数下拉选择框默认线框 | stroke | `--o-color-control1` |
| 每页条数下拉选择框默认填充 | fill | `--o-color-fill2` |
| 每页条数下拉选择框悬浮线框 | stroke | `--o-color-control2` |
| 每页条数下拉选择框激活线框 | stroke | `--o-color-control3` |
| 每页条数下拉选择框禁用线框 | stroke | `--o-color-control4` |
| 每页条数下拉选择框禁用填充 | fill | `--o-color-control4-light` |
| 页码输入框默认线框 | stroke | `--o-color-control1` |
| 页码输入框默认填充 | fill | `--o-color-fill2` |
| 页码输入框悬浮线框 | stroke | `--o-color-primary2` |
| 页码输入框选中线框 | stroke | `--o-color-primary1` |
| 页码输入框禁用线框 | stroke | `--o-color-control4` |
| 页码输入框禁用填充 | fill | `--o-color-control4-light` |
| 页码输入框禁用文字 | text color | `--o-color-info4` |
| 页码按钮圆角 | cornerRadius | `radius_control-xs`（4px） |
| 区域间距 | autoLayoutItemSpacing | `gap-6`（24px） |
| 按钮间距 | autoLayoutItemSpacing | `gap-2`（8px） |

---

## Part E：最佳实践

### 使用建议

1. **页码数量控制**：推荐显示 5-7 个页码按钮，超出时使用省略号
2. **简约模式优先**：前台展示页面优先使用 `show=Simple`，减少视觉负担
3. **默认选中首页**：组件初始化时，第一个页码默认选中
4. **数据统计重要性**：数据统计帮助用户了解总数据量，建议保留（visible_5_60=true）

### 设计提示

- 分页组件通常置于内容区域底部，与上方内容间距建议 16-24px
- 页码按钮宽度固定，避免因页码数字位数差异导致宽度不一致
- 页码输入框有四种交互状态：默认（control1 线框 + fill2 填充）、悬浮（primary2 线框）、选中（primary1 线框）、禁用（control4 线框 + control4-light 填充 + info4 文字）
- 页码输入框建议限制输入数字范围，避免用户输入超出页码范围的值
- 每页条数下拉选择器图标默认使用二级文字色 info2，激活时垂直翻转指示下拉面板已展开
- 每页条数下拉选择框有四种状态：默认（control1 线框）、悬浮（control2 线框）、激活（control3 线框）、禁用（control4 线框 + control4-light 填充）
- 深色模式下注意背景色切换，确保所有元素色值同步

---

## 技术备注

- 组件节点 ID：`1042:13347`（组件集）
- 设计稿 URL：https://pixso.cn/app/design/JZkjW0mhmT61Mtd98dCfBw?item-id=1042:13347
- 生成日期：2026-04-16
- 变体总数：4（show=All/Simple × Dark=off/on）