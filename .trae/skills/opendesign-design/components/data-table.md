← [组件索引](../SKILL.md#组件索引) · [README](../README.md)

# ODataTable 数据表格 · 设计 Skill

> 组件帧节点：`1370:887` · 组件名：ODataTable 数据表格 · 类型：复合组件（表头 + 数据行堆叠）

---

## Part A：设计使用卡

### 组件概览

**ODataTable 数据表格**：用于展示二维结构化数据。由表头行（`表头`）和若干数据行（`表格N`）垂直堆叠构成，支持排序、筛选图标、行选择复选框、悬停高亮以及深浅主题切换。表格不是单一变体组件，而是**表头 + 数据行**的自由组合，设计时按需拼装行数。

---

### 适用场景

- ✅ **数据列表展示**：用户列表、订单列表、配置项列表等结构化数据
- ✅ **操作型表格**：每行附带"文字按钮"操作列（编辑、删除、查看详情）
- ✅ **可排序/可筛选列**：标题列附带排序或筛选图标
- ✅ **批量选择**：首列放置复选框，支持行级勾选与全选
- ❌ **不适合**：树形层级数据（暂无对应组件）、卡片式布局、移动端小屏（行高 48px 适合 PC）

---

### 结构说明

ODataTable 由两类行单元组合而成：

| 行类型 | 节点名 | 说明 |
|--------|--------|------|
| 表头行 | `表头` | 固定在顶部，含列标题、排序/筛选图标，顶部圆角 12px |
| 普通数据行 | `表格1` / `表格N` | 正常状态，背景近透明，顶部内阴影分隔线 |
| 斑马纹 / 悬停行 | `表格3`（示例） | 使用 `--o-color-control2-light` 填充，交替或悬停时应用 |
| 带复选框行 | `表格N`（含多选框/未选） | 首列以多选框替换纯文字 |
| 嵌入组件行 | `表格N`（含组件实例） | 单元格内嵌 OSelect / OInput / OButton 等组件，操作组件统一放最右侧列 |

---

### 布局结构

**表头（表头 节点）**

```
表头 FRAME（HORIZONTAL，1416×48px）
├── autoLayoutPaddingTop/Bottom: 12px
├── autoLayoutPaddingLeft/Right: 32px
├── autoLayoutItemSpacing: 32px
├── cornerRadius: 12px（仅左上/右上）
├── fill: color-fill2（白色/深色模式自动切换）
├── stroke: color-main2（渐变底边 1px）
│
├── [标题1 PARAGRAPH]（463px 自适应宽度，SemiBold 16/24，color-info2）
├── [画板152 FRAME]（200×24，标题2 + 排序图标）
│   ├── autoLayoutItemSpacing: 4px，paddingRight: 131px
│   ├── [标题2 text]（42px，SemiBold 16/24，color-info2）
│   └── [排序图标]（24×24，内联 icon-排序.svg，颜色/尺寸跟随组件定义不变）
├── [标题3 PARAGRAPH]（42px，SemiBold 16/24，color-info2）
├── [筛选图标]（24×24，内联 icon-筛选.svg，颜色/尺寸跟随组件定义不变）
├── [标题4 PARAGRAPH]（固定宽，SemiBold 16/24，color-info2）
└── [标题5 PARAGRAPH]（160px，SemiBold 16/24，color-info2）
```

**普通数据行（表格1 节点）**

```
表格N FRAME（HORIZONTAL，1416×48px）
├── autoLayoutPaddingTop/Bottom: 12px
├── autoLayoutPaddingLeft/Right: 32px
├── autoLayoutItemSpacing: 32px
├── effects: INNER_SHADOW y=-1（分隔线，Light: rgba(0,0,0,0.1) / Dark: rgba(255,255,255,0.15)）
├── fill: rgba(255,255,255,0.01) ≈ 透明（Light）/ rgba(0,0,0,0.01)（Dark）
│
├── [col1 PARAGRAPH]（464px 自适应宽度，Regular 16/24，color-info1）
├── [col2 PARAGRAPH]（200px，Regular 16/24，color-info1）
├── [col3 PARAGRAPH]（200px，Regular 16/24，color-info1）
├── [col4 PARAGRAPH]（200px，Regular 16/24，color-info1）
├── [col5 PARAGRAPH]（200px，Regular 16/24，color-info1）
├── [操作1 PARAGRAPH]（64px，Regular 16/24，center，color-link1）
└── [操作2 PARAGRAPH]（64px，Regular 16/24，center，color-link1）
```

> 单元格内容不限于纯文本，可替换为组件实例：
> - **OSelect / OInput**：用于行内可编辑场景，置于对应数据列
> - **OButton（文字按钮）**：用于操作列，统一放最右侧一列或多列（即"操作区"）
> - 嵌入组件时，该单元格宽度需与组件最小宽度对齐；行高 48px 对 OSelect / OInput 单行高度仍适用

**带复选框数据行（首列结构）**

```
[画板103 FRAME]（404px 自适应，HORIZONTAL，spacing=16px）
├── [多选框/未选 INSTANCE]（24×24，无文字标签）
│     componentKey: c2a73d66aa548a344859d72b97acd4c23e21dacd
│     来源：本文件 JZkjW0mhmT61Mtd98dCfBw（node_id: 1327:892）
│     Light: fill=color-fill2（白）, stroke=color-control1（rgba(0,0,0,0.25)）
│     Dark:  fill=color-fill2-dark（rgb(36,36,39)）, stroke=color-control1-dark（rgba(255,255,255,0.25)）
└── [col1 PARAGRAPH]（364px 自适应，Regular 16/24，color-info1）
```

---

### 组合搭配

- **分页 OPagination**：放置在表格底部，分页控制
- **搜索 OSearch / OInput**：放置在表格上方，作为筛选区
- **多选框/未选**：内嵌于首列，实现行选择（本文件本地组件，非 OCheckbox）
- **下拉 ODropdown**：列头筛选弹出层
- **加载 OLoading**：数据加载中时覆盖表格区域
- **行内嵌入（数据行单元格）**：OSelect、OInput 可嵌入数据列实现行内编辑；OButton（文字/图标按钮）嵌入最右侧操作列；操作列宽度需与嵌入组件对齐

---

### 设计稿识别指南

> 🔍 **识别特征**：
> - 宽度 1416px，行高固定 48px（表头和数据行一致）
> - 表头：顶部圆角 12px，底部渐变描边（`--o-color-main2`）
> - 数据行：内阴影分隔线（y=-1），无独立边框
> - 列间距 32px，左右内边距各 32px
> - 标题文字 SemiBold 16px；内容文字 Regular 16px
> - 操作列文字使用 `--o-color-link1`（蓝色），居中对齐
> - 悬停行：背景填充 `--o-color-control2-light`

> 🔄 **易混淆组件**：
> - 与 **OCard**：Card 是卡片容器，无表头/行分隔结构；Table 是行列网格
> - 与 **ODataTable（单列模式）**：单列 Table 为垂直列表，多列 Table 为结构化数据

---

### 响应式行为

ODataTable 当前设计稿宽度为 1416px（PC 宽幅），适用于 PC 端大屏。移动端需根据断点调整列宽或改为卡片式布局，设计稿中无 Mb 变体。

---

## Part B：规格速查参考

### 行类型对照表

| 行类型 | 背景填充 | 分隔线效果 | 适用场景 |
|--------|---------|-----------|---------|
| 表头 | `--o-color-fill2` | 底部渐变描边 | 固定顶部 |
| 普通行 | ~透明 | 内阴影 y=-1 | 默认数据行 |
| 悬浮/斑马纹行 | `--o-color-control2-light` | 内阴影 y=-1 | Hover 或交替色 |
| 带复选框行 | 同普通行 | 同普通行 | 行选择模式 |

---

### 布局规格

| 规格项 | 值 | Token |
|--------|---|-------|
| 行高 | 48px | — |
| 列内边距（左/右） | 32px | `gap-8` |
| 列内边距（上/下） | 12px | `gap-3` |
| 列间距 | 32px | `gap-8` |
| 表头圆角（左上/右上） | 12px | `radius_control-xl` 附近 |
| 表头底边描边 | 1px 渐变 | `--o-color-main2` |
| 行分隔线（内阴影） | y=-1，spread=0，radius=0 | — |
| 操作按钮列宽 | 64px | — |
| 复选框容器列宽 | 404px（含 checkbox 24 + spacing 16 + text） | — |
| 字号 | 16px | `font_size-text1` |
| 行高（文字） | 24px | `line_height-text1` |

---

### 颜色 Token 映射

#### 表头

| 区域 | Light | Dark | Token | RGB |
|------|-------|------|-------|-----|
| 背景 | white | dark-fill2 | `--o-color-fill2` | Light: rgb(255,255,255) / Dark: rgb(33,35,39) |
| 标题文字 | rgba(0,0,0,0.8) | rgba(255,255,255,0.8) | `--o-color-info2` | — |
| 底部描边 | solid 品牌蓝 | 同 | `--o-color-main2` | rgb(0,47,167) |
| 图标（排序/筛选） | rgba(0,0,0,0.8) | rgba(255,255,255,0.8) | `--o-color-info2` | — |

#### 数据行

| 区域 | Light | Dark | Token | RGB |
|------|-------|------|-------|-----|
| 普通行背景 | rgba(255,255,255,0.01) | rgba(0,0,0,0.01) | — | ≈ 透明 |
| 悬停/斑马纹背景 | rgb(235,241,250) | rgb(40,42,47) | `--o-color-control2-light` | — |
| 分隔线（内阴影） | rgba(0,0,0,0.1) | rgba(255,255,255,0.15) | `--o-color-control4` / `--o-color-control4-dark` | — |
| 内容文字 | rgb(0,0,0) | rgb(255,255,255) | `--o-color-info1` | — |
| 操作链接文字 | rgb(0,47,167) | rgb(73,122,248) | `--o-color-link1` | — |

---

### 字体样式映射

| 使用场景 | 字号 Token | 行高 Token | 字重 Token | 字号值 | 行高值 |
|---------|-----------|-----------|-----------|-------|-------|
| 表头标题 | `font_size-text1` | `line_height-text1` | `font_weight-semibold` | 16px | 24px |
| 单元格内容 | `font_size-text1` | `line_height-text1` | `font_weight-regular` | 16px | 24px |
| 操作链接 | `font_size-text1` | `line_height-text1` | `font_weight-regular` | 16px | 24px |

字体族：`font_family`（HarmonyHeiTi）

---

### 组件层级结构（完整）

```
ODataTable 数据表格（VERTICAL 堆叠）
│
├── 表头 FRAME（1416×48）
│   GUID: 1370:889（Light）
│   fill: color-fill2
│   stroke bottom: color-main2（渐变 1px）
│   cornerRadius: 12px（左上/右上）
│   padding: 12/12/32/32px，spacing: 32px
│   │
│   ├── [标题1 PARAGRAPH]（1370:890，463px）
│   ├── [画板152 FRAME]（1370:891，200px）
│   │   ├── [标题2 text] + [排序图标]（内联 icon-排序.svg，24×24）
│   ├── [标题3 PARAGRAPH]（1370:895，42px）
│   ├── [筛选图标]（内联 icon-筛选.svg，24×24，1370:896）
│   └── [标题5 PARAGRAPH]（1370:900，160px）
│
├── 表格1 FRAME（1416×48）— 普通行 Light
│   GUID: 1370:901
│   fill: rgba(255,255,255,0.01)
│   innerShadow: rgba(0,0,0,0.1) y=-1
│   padding: 12/12/32/32px，spacing: 32px
│   │
│   ├── [col1]（1370:902，464px，color-info1）
│   ├── [col2]（1370:903，200px，color-info1）
│   ├── [col3]（1370:904，200px，color-info1）
│   ├── [col4]（1370:905，200px，color-info1）
│   ├── [操作1]（1370:906，64px，color-link1，center）
│   └── [操作2]（1370:907，64px，color-link1，center）
│
├── 表格3 FRAME（1416×48）— 悬停/斑马纹行 Light
│   GUID: 1370:915
│   fill: color-control2-light（rgb(237,239,242)）
│   innerShadow: rgba(0,0,0,0.1) y=-1
│   [其余结构同 表格1]
│
├── 表格N FRAME（1416×48）— 带复选框行
│   [画板103 FRAME]（404px，spacing=16px）
│   ├── [多选框/未选 INSTANCE]（24×24，componentKey: c2a73d66aa548a344859d72b97acd4c23e21dacd）
│   └── [col1 text]（364px，color-info1）
│
├── 表格4 FRAME（1416×48）— 普通行 Dark
│   GUID: 1370:970
│   fill: rgba(0,0,0,0.01)
│   innerShadow: rgba(255,255,255,0.15) y=-1
│   text: color-info1 dark（白）
│   link: color-link1 dark（rgb(84,120,251)）
│   [其余结构同 表格1]
│
└── 表格2 FRAME（1416×48）— 带复选框行 Dark
    GUID: 1370:1150
    innerShadow: rgba(255,255,255,0.15) y=-1
    多选框/未选: props override → fill=color-fill2-dark, stroke=color-control1-dark
    text: color-info1 dark（白）
```

---

### 表头图标速查

| 图标 | 用途 | Asset 路径 |
|------|------|-----------|
| 排序 | 列头排序触发 | `references/assets/public icons/icon-排序.svg` |
| 筛选 | 列头筛选触发 | `references/assets/public icons/icon-筛选.svg` |

> ⚠️ **图标颜色与尺寸由 ODataTable 组件定义，不可手动修改**：排序/筛选图标颜色（静态 `--o-color-info2` / 激活 `--o-color-link1`）和尺寸（24×24px）跟随 ODataTable 组件定义；生成 HTML 时直接内联上述 SVG，不得单独覆盖 fill 或修改尺寸。

---

### 行内多选框 componentKey 速查

表格行选择列中使用的多选框（无文字标签，24×24）。该组件为**本文件本地组件**（非外部 OCheckbox），Light / Dark 通过 props overrides 切换，只有一个 componentKey：

| 组件名 | componentKey | node_id | 来源文件 |
|--------|-------------|---------|---------|
| 多选框/未选 | `c2a73d66aa548a344859d72b97acd4c23e21dacd` | `1327:892` | `JZkjW0mhmT61Mtd98dCfBw`（本文件） |

**Light 模式（默认）**

| 属性 | Token | RGB |
|------|-------|-----|
| 矩形 fill | `--o-color-fill2` | rgb(255,255,255) |
| 矩形 stroke | `--o-color-control1` | rgba(0,0,0,0.25) |

**Dark 模式（props override）**

| 属性 | Token | RGB |
|------|-------|-----|
| 矩形 fill | `--o-color-fill2`（Dark） | rgb(36,36,39) |
| 矩形 stroke | `--o-color-control1`（Dark） | rgba(255,255,255,0.25) |

**内部结构**

```
多选框/未选 SYMBOL（24×24，node_id: 1327:892）
└── 矩形 RECTANGLE（16×16，top: 4px，left: 4px）
      cornerRadius: 4px（四角一致）
      strokeWeight: 1px（INSIDE）
      fill: --o-color-fill2
      stroke: --o-color-control1
```

---

### Pixso 操作速查

1. **搭建表格**：先放置表头（`1370:889`），再垂直堆叠数据行，各行上下间距为 0（紧密排列）
2. **添加排序图标**：列头标题旁内联 `icon-排序.svg`（assets），尺寸 24×24，颜色跟随 ODataTable 组件定义不手动覆盖
3. **添加筛选图标**：内联 `icon-筛选.svg`（assets），同上
4. **行选择多选框**：`create_instance(c2a73d66aa548a344859d72b97acd4c23e21dacd)`，放入 画板103 结构（spacing=16px），Dark 模式通过 props override 切换 fill/stroke
5. **悬停/斑马纹**：将对应行填充改为 `--o-color-control2-light`
6. **切换深色模式**：将所有 Token 切换为 Dark 对应值（fill/text/link/icon 颜色）

---

### 注意事项

- **行间距为 0**：表头与数据行、数据行彼此之间无间距，仅靠内阴影分隔线视觉分隔
- **表头圆角仅上方**：`cornerRadius` 仅设置左上 + 右上 12px，左下右下为 0
- **表头底边为渐变描边**：非普通 border，使用 `--o-color-main2` 渐变；数据行分隔线为内阴影（INNER_SHADOW），非描边
- **表头图标颜色/尺寸不可修改**：排序/筛选图标颜色（`--o-color-info2` / `--o-color-link1`）和尺寸（24×24px）由 ODataTable 组件定义，使用实例时不单独覆盖
- **操作按钮宽度固定 64px（纯文字链接）**：文字居中对齐，使用 `--o-color-link1`，不加下划线；若操作列改用 OButton 实例，列宽需按 OButton 规格调整
- **操作列始终在最右侧**：无论是文字链接、OButton 还是其他可交互组件，操作列固定放置在最右一列或最右若干列
- **行内嵌入组件**：OSelect / OInput 可替换数据列的纯文本单元格，实现行内编辑；OButton 替换操作列；嵌入组件后该列宽需与组件最小宽度对齐
- **复选框列结构**：须用 画板103 容器（spacing=16px）将 `多选框/未选` 实例与文字包裹，不能直接平铺；Dark 模式通过 props overrides 而非换组件实现
- **多选框来源**：表格内行选择框为本文件本地组件（`1327:892`），不是 OCheckbox；仅提供"未选"视觉基态，Selected/Disabled 状态需在开发层实现
- **斑马纹逻辑**：奇数/偶数行交替填充，或仅 Hover 时应用 `--o-color-control2-light`
- **行宽固定 1416px**：PC 宽幅设计稿，如需适配其他宽度需等比缩放列宽

---

## Part C：交互与状态

### 行交互状态

| 状态 | 视觉表现 |
|------|---------|
| 普通 | 背景透明（~rgba 0.01），分隔线内阴影 |
| Hover | 背景切换为 `--o-color-control2-light` |
| 已选中（Checkbox） | 复选框显示 Selected 状态，行背景可搭配浅蓝高亮 |
| 禁用行 | 文字颜色降级为 `--o-color-info4`（rgba 0.4） |

### 列头交互状态

| 状态 | 视觉表现 |
|------|---------|
| 排序-未激活 | 排序图标 `--o-color-info2`（80% 透明） |
| 排序-升序激活 | 排序图标品牌色 `--o-color-link1` |
| 排序-降序激活 | 排序图标品牌色 `--o-color-link1`，方向翻转 |
| 筛选-未激活 | 筛选图标 `--o-color-info2` |
| 筛选-已筛选 | 筛选图标品牌色 `--o-color-link1` |

---

## Part D：设计变量绑定

### 推荐绑定变量

| 元素 | 属性 | 推荐变量 Token |
|------|------|---------------|
| 表头背景 | fill | `--o-color-fill2` |
| 表头底边描边 | stroke | `--o-color-main2` |
| 表头标题文字 | fill | `--o-color-info2` |
| 排序/筛选图标 | fill | `--o-color-info2` |
| 数据行内阴影（Light） | effectColor | `--o-color-control4` |
| 数据行内阴影（Dark） | effectColor | `--o-color-control4-dark` |
| 悬停/斑马纹背景 | fill | `--o-color-control2-light` |
| 内容文字 | fill | `--o-color-info1` |
| 操作链接 | fill | `--o-color-link1` |
| 文字字号 | fontSize | `font_size-text1`（16px） |
| 文字行高 | lineHeight | `line_height-text1`（24px） |
| 列内边距（左/右） | padding | `gap-8`（32px） |
| 列内边距（上/下） | padding | `gap-3`（12px） |
| 列间距 | itemSpacing | `gap-8`（32px） |

---

## Part E：最佳实践

### 使用建议

1. **列宽分配**：主内容列（第一列）使用"自适应宽度"（Fill），其余列固定宽度；纯文字操作列固定 64px
2. **操作列数量**：建议不超过 3 个操作，超出时用"更多"下拉
3. **操作列位置**：无论是文字链接还是嵌入 OButton，操作列统一放最右侧；嵌入 OSelect/OInput 的编辑列则就近放在对应数据列
4. **行内嵌入组件列宽**：嵌入 OSelect/OInput 的列宽需 ≥ 组件最小宽度（参考各组件规格），不能沿用 64px 操作列宽
5. **斑马纹使用**：行数较多（>8 行）时建议开启斑马纹，提升扫读体验
6. **排序列优先级**：最多 1 列同时激活排序状态，避免歧义
7. **复选框表头**：行选择模式下，表头首列也应放置 Mix（全选）状态 Checkbox

### 设计提示

- 数据行与表头行高统一为 48px，视觉整齐
- 若数据行内容可能换行，行高应相应加大，分隔线保持内阴影方式
- 操作链接文字不超过 4 个字，避免列宽撑开
- 深色模式下分隔线用白色内阴影（15% 透明度），不要用描边替代

---

## 技术备注

- 组件帧节点 ID：`1370:887`（所有表格示例的父框架）
- 表头节点：`1370:889`（Light）
- 数据行节点（Light 普通）：`1370:901`
- 数据行节点（Light 悬停）：`1370:915`
- 数据行节点（Dark 普通）：`1370:970`
- 带 Checkbox 数据行（Light）：`1370:1120`（结构参考）
- 带 Checkbox 数据行（Dark）：`1370:1150`
- 设计稿 URL：https://pixso.cn/app/design/JZkjW0mhmT61Mtd98dCfBw?item-id=1370:887
- 生成日期：2026-06-02（组件刷新于 2026-06-02）
- 图标来源文件：`ZuTufm9KC1JTIM-KwWz52A`（排序/筛选图标）
- 行内多选框来源：本文件本地组件 `多选框/未选`（`1327:892`），componentKey: `c2a73d66aa548a344859d72b97acd4c23e21dacd`；原外部 12 变体 Checkbox 已废弃，Light/Dark 改由 props overrides 实现