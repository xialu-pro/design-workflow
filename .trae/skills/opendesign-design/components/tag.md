> ← [组件索引](../SKILL.md#组件索引) · [README](../README.md)

# OTag 标签 · 设计 Skill

> 组件集合节点：`1042:19918` · 组件名：OTag 标签 · 变体总数：待确认

---

## Part A：设计使用卡

### 组件概览

**OTag 标签**：用于标记和分类的小型标签组件，通过颜色语义传达不同状态或类别信息。支持实心填充和描边两种外观，预设语义颜色主题及自定义颜色，可选关闭按钮和图标。适用于状态标记、分类筛选、内容标签等场景。

---

### 适用场景

- ✅ **状态标记**：订单状态（待处理、进行中、已完成）、审批状态、任务状态等
- ✅ **分类筛选**：内容分类标签、筛选条件标签
- ✅ **信息标注**：版本号、优先级、类型标识等
- ✅ **可编辑标签**：用户自定义标签、可移除的筛选条件
- ✅ **品牌定制**：使用自定义颜色匹配品牌视觉风格
- ❌ **不适合**：导航切换（用 Tab）、大量文字描述（用卡片或气泡）

---

### 变体说明

**Variant（外观类型）**
- `fill` — 实心填充，背景为语义色，文字和图标为白色，视觉权重强，适合需要突出的标签
- `stroke` — 描边样式，背景透明，边框为语义色，文字和图标为语义色，视觉权重轻，适合辅助标记

**size（尺寸）**
- `large` — 大尺寸，适合需要突出的标签或独立展示场景
- `medium` — 中尺寸，**默认尺寸**，适合大多数场景
- `small` — 小尺寸，适合紧凑布局或行内标签

**Color（颜色主题）**
- `primary` — 品牌色，用于品牌相关、重要标记
- `success` — 成功色，用于成功、完成、通过等状态
- `warning` — 警告色，用于警告、待处理、需关注等状态
- `danger` — 危险色，用于错误、失败、拒绝等状态
- `info` — 信息色，用于信息、辅助、中性状态
- `custom` — 自定义颜色，用户可指定任意颜色值，用于个性化标签、品牌定制等场景

**功能变体**
- `closable` — 可关闭标签，右侧固定显示关闭图标（close icon 与 closable 变体绑定，不可单独控制）
- `icon` — 带占位图标，左侧预留图标位，后期可替换为实际业务图标

> **图标规则**：两类图标（占位图标、关闭图标）的颜色和尺寸均由 OTag 组件统一定义，跟随组件规格自动适配，不需要也不应该在实例层级单独修改。替换占位图标内容时，只替换图标实例，颜色和尺寸保持组件定义不变。

**Dark（主题）**
- `off` — 浅色模式（默认）
- `on` — 深色模式，颜色自动切换为对应深色值

---

### 布局结构

> 🧩 **布局结构**：标签整体水平排列（HORIZONTAL Auto Layout），由图标、文字、关闭按钮组成。

```
OTag（HORIZONTAL，自适应宽度，固定高度）
│  autoLayoutPadding: 见尺寸规格
│  cornerRadius: 见尺寸规格
│  fill: color-primary1（fill 变体）/ 无（stroke 变体）
│  stroke: 无（fill 变体）/ color-primary1（stroke 变体）
│
├── [图标 Icon INSTANCE]（固定尺寸，可选）
│     fill: color-white（fill）/ color-primary1（stroke）
│
├── [文字 PARAGRAPH]（自适应宽，固定行高）
│     font: 见字体规格
│     fill: color-white（fill）/ color-primary1（stroke）
│
└── [关闭按钮 INSTANCE]（固定尺寸，可选）
      fill: color-white（fill）/ color-primary1（stroke）
```

---

### 组合搭配

> 🔗 **常见搭配**：
> - **筛选器**：多个可关闭标签（stroke + closable）横向排列，用于筛选条件展示
> - **状态列表**：不同颜色标签（fill）纵向排列，用于状态标记
> - **表格列**：小尺寸标签（small + fill）嵌入表格单元格，用于状态展示
> - **卡片标签**：带图标标签（medium + icon + fill）置于卡片顶部，用于分类标识

---

### 设计稿识别指南

> 🔍 **识别特征**：
> - 水平自动布局节点，内含文字层 + 可选图标/关闭按钮
> - 高度固定（large / medium / small 三种规格）
> - fill 变体有实心背景填充，文字和图标为白色
> - stroke 变体仅有描边无背景，文字和图标为语义色

> 🔄 **易混淆组件**：
> - 与 **OButton 按钮** 的区别：Tag 是静态标记，无点击交互（关闭按钮除外）；Button 是交互触发器
> - 与 **OTab 标签页** 的区别：Tag 是单个标记元素；Tab 是导航切换控件，多个联动

---

### 响应式行为

参照 [栅格规范](../SKILL.md#数据资源) 中的断点定义，OTag 无自动断点响应式行为。尺寸和样式通过变体手动控制。

---

## Part B：规格速查参考

### 变体索引表

| 变体属性 | 可选值 | 默认值 | 视觉差异 |
|---------|--------|--------|---------|
| Variant | `fill` / `stroke` | `fill` | 实心填充 / 描边无背景 |
| size | `large` / `medium` / `small` | `medium` | 高度和内边距不同 |
| Color | `primary` / `success` / `warning` / `danger` / `info` / `custom` | `primary` | 语义颜色不同 / 自定义颜色 |
| closable | `true` / `false` | `false` | 是否显示关闭按钮 |
| icon | `true` / `false` | `false` | 是否显示左侧图标 |
| Dark | `off` / `on` | `off` | 浅色模式 / 深色模式 |

---

### 布局规格

| 规格项 | large | medium | small |
|--------|-------|--------|-------|
| 高度 | 32px | 24px | 20px |
| Token（高度） | `control_size-m` | — | — |
| 水平内边距 | 12px | 8px | 6px |
| 垂直内边距 | 4px | 2px | 2px |
| 内容容器 gap | 8px | 4px | 4px |
| 圆角 | 4px | 4px | 4px |
| Token（圆角） | `radius_control-m` | `radius_control-m` | `radius_control-m` |
| 字号 | 14px | 12px | 12px |
| Token（字号） | `font_size-tip1` | `font_size-tip2` | `font_size-tip2` |
| 行高 | 22px | 18px | 18px |
| Token（行高） | `line_height-tip1` | `line_height-tip2` | `line_height-tip2` |
| 图标尺寸 | 16px | 14px | 14px |
| 关闭按钮尺寸 | 16px | 14px | 14px |

> **说明**：尺寸规格基于通用 Tag 组件推断，需从设计稿进一步确认。

---

### 颜色 Token 映射

#### Fill 变体

| 颜色主题 | 背景 Token | 文字/图标 Token | Light RGB | Dark RGB |
|---------|-----------|----------------|----------|----------|
| primary | `--o-color-primary1` | `--o-color-white` | rgb(0,47,167) / rgb(255,255,255) | rgb(73,122,248) / rgb(255,255,255) |
| success | `--o-color-success1` | `--o-color-white` | rgb(11,177,81) / rgb(255,255,255) | rgb(51,193,104) / rgb(255,255,255) |
| warning | `--o-color-warning1` | `--o-color-white` | rgb(250,115,5) / rgb(255,255,255) | rgb(251,143,43) / rgb(255,255,255) |
| danger | `--o-color-danger1` | `--o-color-white` | rgb(230,0,18) / rgb(255,255,255) | rgb(235,35,45) / rgb(255,255,255) |
| info | `--o-color-control2-light` | `--o-color-white` | rgb(222,222,227) / rgb(255,255,255) | rgb(43,43,47) / rgb(255,255,255) |

#### Stroke 变体

| 颜色主题 | 边框 Token | 文字/图标 Token | 背景 | Light RGB | Dark RGB |
|---------|-----------|----------------|------|----------|----------|
| primary | `--o-color-primary1` | `--o-color-primary1` | 透明 | rgb(0,47,167) | rgb(73,122,248) |
| success | `--o-color-success1` | `--o-color-success1` | 透明 | rgb(11,177,81) | rgb(51,193,104) |
| warning | `--o-color-warning1` | `--o-color-warning1` | 透明 | rgb(250,115,5) | rgb(251,143,43) |
| danger | `--o-color-danger1` | `--o-color-danger1` | 透明 | rgb(230,0,18) | rgb(235,35,45) |
| info | `--o-color-control4` | `--o-color-info1` | 透明 | rgba(0,0,0,0.1) | rgba(255,255,255,0.15) |

> **说明**：info 颜色主题使用中性色（grey 系列）而非品牌色系，具体 Token 需从设计稿确认。
>
> **custom 自定义颜色**：使用 `custom` 时，用户可指定任意颜色值。fill 变体背景为自定义色，文字/图标为白色；stroke 变体边框和文字均为自定义色。自定义颜色需确保与白色文字形成足够对比度。

---

### 字体样式映射

| 使用场景 | 字号 Token | 行高 Token | 字重 Token | 字号值 | 行高值 |
|---------|-----------|-----------|-----------|-------|-------|
| `large` 标签文字 | `font_size-tip1` | `line_height-tip1` | `font_weight-regular` | 14px | 22px |
| `medium` / `small` 标签文字 | `font_size-tip2` | `line_height-tip2` | `font_weight-regular` | 12px | 18px |

字体族：`font_family`（HarmonyOS / HarmonyHeiTi Regular）

---

### 组件层级结构

```
OTag（HORIZONTAL，自适应宽，固定高）
│  paddingTop/Bottom: 见尺寸规格 | paddingLeft/Right: 见尺寸规格
│  cornerRadius: 4px（radius_control-m）
│  fill: color-primary1（fill）/ 无（stroke）
│  stroke: 无（fill）/ color-primary1（stroke），strokeWeight: 1px INSIDE
│
├── [图标 Icon INSTANCE]（固定尺寸，可选）
│     width/height: 16px（large）/ 14px（medium/small）
│     fill: color-white（fill）/ color-primary1（stroke）
│
├── [文字 PARAGRAPH]（自适应宽，固定行高）
│     font: font_family Regular | font_size-tip1 或 font_size-tip2
│     fill: color-white（fill）/ color-primary1（stroke）
│
└── [关闭按钮 INSTANCE]（固定尺寸，可选）
      width/height: 16px（large）/ 14px（medium/small）
      fill: color-white（fill）/ color-primary1（stroke）
      交互：点击移除标签
```

---

### 变体 componentKey 速查

| 变体组合 | node_id | 尺寸 | 说明 |
|---------|---------|------|------|
| Variant=fill, size=medium, Color=primary, Dark=off | `1042:19918` | 自适应×24px | 实心填充，中尺寸，品牌色，浅色模式（待确认） |
| Variant=stroke, size=medium, Color=primary, Dark=off | 待确认 | 自适应×24px | 描边，中尺寸，品牌色，浅色模式 |
| Variant=fill, size=large, Color=success, Dark=off | 待确认 | 自适应×32px | 实心填充，大尺寸，成功色，浅色模式 |
| Variant=fill, size=small, Color=warning, Dark=off | 待确认 | 自适应×20px | 实心填充，小尺寸，警告色，浅色模式 |
| closable=true | 待确认 | 自适应×24px | 可关闭标签 |
| icon=true | 待确认 | 自适应×24px | 带图标标签 |

> **注意**：componentKey 需从 Pixso 组件面板获取。当前 node_id 为用户提供的设计稿链接中的 item-id。

---

### Pixso 操作速查

1. **插入组件**：组件面板搜索「OTag」，拖入画布
2. **切换外观**：右侧面板 → Variant 属性 → 选择 `fill` / `stroke`
3. **切换尺寸**：右侧面板 → size 属性 → 选择 `large` / `medium` / `small`
4. **切换颜色**：右侧面板 → Color 属性 → 选择 `primary` / `success` / `warning` / `danger` / `info` / `custom`
5. **自定义颜色**：Color 选择 `custom` → 设置自定义颜色值（背景、文字、边框等）
6. **添加关闭按钮**：右侧面板 → closable 属性 → 选择 `true`
7. **添加图标**：右侧面板 → icon 属性 → 选择 `true`
8. **切换主题**：右侧面板 → Dark 属性 → 选择 `off` / `on`
9. **修改文字**：双击进入组件实例 → 双击文字图层修改内容

---

### 注意事项

- **fill vs stroke**：fill 变体视觉权重强（实心背景+白色文字），适合需要突出的标签；stroke 变体视觉权重轻（透明背景+语义色文字），适合辅助标记
- **颜色语义**：严格按照语义使用颜色，primary 用于品牌相关，success 用于成功状态，warning 用于警告，danger 用于错误，info 用于中性信息
- **自定义颜色**：用于品牌定制或特殊场景，需确保自定义色与背景形成足够对比度，深色模式下需同步调整
- **可关闭标签**：仅用于用户可移除的标签，如筛选条件、自定义标签；静态标签不应显示关闭按钮；关闭图标与 closable 变体绑定，不单独控制
- **图标使用**：icon 变体显示占位图标，可替换图标内容；图标颜色和尺寸由 OTag 组件统一定义，跟随组件规格自动适配，不在实例层级单独修改
- **尺寸选择**：large 用于独立展示或需要突出，medium 用于常规场景，small 用于紧凑布局
- **深色模式**：颜色自动切换为深色值，确保在深色背景上可见

---

## Part C：交互与状态

### 状态定义

| 状态 | 说明 |
|------|------|
| 默认 | 标签正常显示，无交互反馈（除关闭按钮） |
| 关闭按钮悬浮 | 关闭按钮背景色加深，提示可点击 |

### 交互行为

- **可关闭标签**：点击关闭按钮移除标签，通常伴随动画过渡（如缩小消失）
- **静态标签**：无交互行为，仅作为静态标记展示

---

## Part D：设计变量绑定

### 推荐绑定变量

| 元素 | 属性 | 推荐变量 Token |
|------|------|---------------|
| 背景（fill 变体） | fill | `--o-color-primary1` 等 |
| 文字/图标颜色（fill 变体） | fill | `--o-color-white` |
| 边框（stroke 变体） | stroke | `--o-color-primary1` 等 |
| 文字/图标颜色（stroke 变体） | fill | `--o-color-primary1` 等 |
| 圆角 | cornerRadius | `radius_control-m`（4px） |
| 高度 | height | `control_size-m`（32px，large） |
| 字号（large） | fontSize | `font_size-tip1`（14px） |
| 字号（medium/small） | fontSize | `font_size-tip2`（12px） |
| 行高（large） | lineHeight | `line_height-tip1`（22px） |
| 行高（medium/small） | lineHeight | `line_height-tip2`（18px） |
| 内容间距 | autoLayoutItemSpacing | `gap-2`（8px）/ `gap-1`（4px） |

---

## Part E：最佳实践

### 使用建议

1. **颜色语义一致**：严格按照语义使用颜色，避免颜色与语义不符造成用户困惑
2. **样式权重匹配**：重要标签使用 fill 变体，辅助标签使用 stroke 变体
3. **尺寸与场景匹配**：独立展示使用 large，行内标签使用 small
4. **可关闭标签控制**：仅用于用户可移除的标签，静态标签不应显示关闭按钮

### 设计提示

- 标签文字应简洁，建议不超过 4 个汉字或 8 个英文字符
- 同一区域使用的标签样式应保持一致（fill 或 stroke）
- 多个标签并列时，间距推荐 8px（gap-2）
- 可关闭标签移除后应有动画过渡，提升用户体验
- 深色模式下 fill 变体背景色切换为对应深色值，文字色保持白色不变
- 自定义颜色需确保与背景形成足够对比度，避免可读性问题

---

## 技术备注

- 组件节点 ID：`1042:19918`（组件集）
- 设计稿 URL：https://pixso.cn/app/design/JZkjW0mhmT61Mtd98dCfBw?item-id=1042:19918
- 生成日期：2026-04-17
- 变体总数：待确认（Variant × size × Color × 功能 × Dark）
- 外观类型：fill（实心填充）、stroke（描边）
- 颜色主题：primary、success、warning、danger、info、custom（自定义）
- 功能变体：可关闭（closable）、带图标（icon）
- 数据来源：用户提供的设计规范信息，部分规格需从设计稿确认