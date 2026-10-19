> ← [组件索引](../SKILL.md#组件索引) · [README](../README.md)

# OMenu 菜单 · 设计 Skill

> 组件集合节点：`1042:13181` · 组件名：OMenu 菜单 · 变体总数：32

---

## Part A：设计使用卡

### 组件概览

**OMenu 菜单**：侧边栏层级导航组件，由可展开子菜单标题行（level1）和叶子菜单项（level2/level3）多个变体实例垂直堆叠而成。支持多级嵌套、图标前缀、选中高亮、手风琴展开控制以及左/右箭头位置切换。

---

### 适用场景

- ✅ **管理后台侧边栏**：多级功能导航，常宽约 240px，含展开/折叠分组
- ✅ **文档/帮助中心目录**：三级以上深层嵌套导航
- ✅ **数据平台筛选面板**：带图标的菜单项快速切换视图
- ✅ **设置页侧导航**：小尺寸（size=small）紧凑侧边菜单
- ❌ **不适合**：水平导航栏（用 ONavigation）、弹出下拉菜单（用 ODropdown）、内容折叠展示（用 OCollapse）

---

### 变体说明

> ℹ️ 以下为菜单项变体（真正参与导航交互的单元）。分节标题行（Group/Tittle）是纯装饰性版式元素，见下方[组合构建模式](#组合构建模式)。

**size（尺寸）**
- `medium` — 默认尺寸，一级项文字 text1（16px）、上下间距 11px，图标 24×24px
- `small` — 小号，文字 tip1（14px）、上下间距 4px，图标 16×16px（control-xs）

**属性 2（层级）**
- `level1` — 一级菜单行（可展开子菜单的标题行，或顶层叶子项）
- `level2` — 二级菜单行（level1 展开后的子项；medium 有图标/纯文字两种，small 无图标）
- `level3` — 三级菜单行（仅 size=small 有此层级）

**属性 3（状态/图标）**
- `Enabled` — 默认未选中状态
- `Actived` — 选中高亮状态（蓝色背景 + 蓝色文字）
- `ic_Enabled` — 带图标的未选中状态（仅 medium/level2）
- `ic_Actived` — 带图标的选中状态（仅 medium/level2）
- `text_Enabled` — 无图标纯文字未选中（仅 medium/level2）
- `text_Actived` — 无图标纯文字选中（仅 medium/level2）

**Dark（主题）**
- `off` — 浅色模式（默认）
- `on` — 深色模式，背景和文字颜色自动切换深色系

---

### 布局结构

> 🧩 **布局结构**：多个变体实例在 Auto Layout 中垂直堆叠，形成完整菜单列。每个菜单项（level1 / level2 / level3）均为**双层结构**：外层透明 Frame（固定高度，无填充）+ 内层内容行（比外层矮 2px，Actived 时填充 `--o-color-control3-light`，圆角 4px）。选中背景在外层 Frame 内上下各缩进 1px，确保相邻选中项之间始终有 1px 视觉间隔，不粘连。

```
垂直堆叠容器（宽度约 240px）
├── [level1 外层 Frame]（medium: 48px / small: 32px，透明无填充）
│   └── [level1 内容行]（medium: 46px / small: 30px，Actived 时填充 color-control3-light，radius 4px）
│       ├── [箭头图标 left，可选]（16/24px）
│       ├── [前缀图标，可选]（24px medium / 16px small）
│       ├── 标题文字（flex:1）
│       └── [箭头图标 right，默认]（16/24px）
│
├── [level2 外层 Frame]（medium: 40px / small: 32px，透明无填充，展开后插入）
│   └── [level2 内容行]（medium: 38px / small: 30px，Actived 时填充，带层级缩进）
│       ├── [前缀图标，可选，仅 medium ic_* 变体]
│       └── 文字内容（超出省略）
│
├── [level3 外层 Frame]（small 专用，32px，透明无填充）
│   └── [level3 内容行]（30px，Actived 时填充，三级缩进）
│
└── … 继续堆叠更多 level1 / level2
```

---

### 组合构建模式

一个完整的 OMenu 由多个独立变体实例垂直堆叠组合而成。

**基本拼装单元**（按交互角色区分）

| 单元 | 变体 | 角色 |
|------|------|------|
| 一级菜单行 | `level1 Enabled / Actived` | 可展开子菜单的标题行，或顶层叶子项 |
| 二级菜单行 | `level2 text_* / ic_*` | level1 展开后显示的子项 |
| 三级菜单行 | `level3 Enabled / Actived`（small 专用） | level2 展开后的更深一级 |

**分节标题行 Group（可选装饰元素）**

Group/Tittle 变体是纯视觉标签行，插在若干 level1 组之间，用于区分功能分区。它：
- **不参与展开/折叠交互**，无折叠箭头
- **不可被选中**，无 Actived 状态
- 仅呈现分组名 + 可选文字按钮（如「查看全部」「收起」）

| 变体 | 适用场景 |
|------|---------|
| `size=Menu, Tittle` | 纯文字分节标题行（无右侧按钮） |
| `size=medium, Group` | 分节标题 + 右侧可选文字按钮（medium 菜单） |
| `size=small, Group` | 小尺寸版分节标题行 |

**拼装范式示意（medium，两个功能分区）**

```
[Group 分节标题：「功能区 A」]   ← 可选，插入 level1 组之前
[level1 Enabled]                ← 一级菜单项 A-1（收起态）
[level1 Actived]                ← 一级菜单项 A-2（选中态，当前展开）
  [level2 text_Enabled]         ← 二级子项（A-2 展开后显示）
  [level2 text_Actived]         ← 二级子项（选中）
  [level2 ic_Enabled]           ← 带图标的二级子项
[Group 分节标题：「功能区 B」]   ← 可选
[level1 Enabled]                ← 一级菜单项 B-1
  [level2 text_Enabled]
```

**何时插入 Group 行**
- 菜单项超过 6–8 项，需要视觉分区
- 菜单内存在性质不同的功能块（如「基础配置」vs「高级设置」）

---

### 组合搭配

> 🔗 **常见搭配**：
> - **侧边栏布局**：OMenu 堆叠 + 右侧内容区域（布局容器）
> - **面包屑联动**：OMenu 选中项 value 与 OBreadcrumb 同步
> - **手风琴导航**：accordion=true，同级只展开一组
> - **图标导航**：medium level2 配合 OIcon 图标（ic_* 变体）

---

### 设计稿识别指南

> 🔍 **识别特征**：
> - 垂直排列文字列表，有层级缩进，标题行右（或左）侧有展开/折叠箭头图标
> - 选中项有蓝色高亮背景（`--o-color-control3-light`）+ 蓝色文字（`--o-color-primary1`）
> - 整体宽度约 240px，常出现在侧边栏区域
> - medium 一级项字号较大（text1=16px），small 整体字号较小（tip1=14px）
> - 若看到无箭头的纯文字标签行（带或不带右侧文字按钮），即为 Group 分节标题行

> 🔄 **易混淆组件**：
> - 与 **OCollapse**：Menu 有选中高亮和导航语义（ul/li），Collapse 是纯内容展开，无选中态
> - 与 **OTab**：Tab 水平排列选项卡，Menu 垂直层级导航
> - 与 **ODropdown**：Dropdown 是弹出浮层菜单，Menu 是页面内嵌固定侧导航

---

### 响应式行为

参照 [栅格规范](../SKILL.md#数据资源)。OMenu 有断点响应式行为：

| 断点 | medium 表现 | small 表现 |
|------|------------|-----------|
| >1440px | 一级项 padding-v=11px，text1 | tip1，padding-v=4px |
| 841–1440px（laptop） | 一级项 padding-v=8px，tip1，图标 control-s | 标准 |
| ≤840px（pad_v） | 标准 | 一级项保持 tip1，其余 tip2 |

---

## Part B：规格速查参考

### 变体索引表

#### 菜单项变体（参与导航交互）

| 变体属性 | 可选值 | 默认值 | 视觉差异 |
|---------|--------|--------|---------|
| size | `medium` / `small` | `medium` | 文字大小/间距/图标尺寸不同 |
| 属性 2（层级） | `level1` / `level2` / `level3` | `level1` | 缩进深度不同；level3 仅 small |
| 属性 3（状态） | `Enabled` / `Actived` / `ic_Enabled` / `ic_Actived` / `text_Enabled` / `text_Actived` | `Enabled` | 高亮色、图标有无；ic_*/text_* 仅 medium/level2 |
| Dark | `off` / `on` | `off` | 深色主题颜色切换 |

#### 分节标题行变体（纯装饰，不参与交互）

| 变体 | size | 属性 2 | 用途 |
|------|------|--------|------|
| `size=Menu, Tittle` | Menu | Tittle | 纯文字分节标题 |
| `size=medium, Group` | medium | Group | 分节标题 + 可选文字按钮 |
| `size=small, Group` | small | Group | 小尺寸分节标题 |

---

### 布局规格

#### medium 尺寸

| 规格项 | level1（一级） | level2（二级） | 说明 |
|--------|--------------|--------------|------|
| 外层 Frame 高度 | **48px** | **40px** | 透明无填充，撑开行高 |
| 内层内容行高度 | **46px**（11+24+11） | **38px**（8+22+8） | Actived 时有填充色；上下各 1px 缩进于 Frame |
| padding-v（内层） | 11px | 8px | 上下内边距 |
| padding-h | 8px | 8px + level缩进 | 水平内边距 |
| 文字字号 | text1（16px） | tip1（14px） | — |
| 行高 | line_height-text1（24px） | line_height-tip1（22px） | — |
| 图标尺寸 | 24×24px（control-m） | 24×24px（control-m） | ic_* 变体有图标 |
| 箭头图标尺寸 | 24×24px | — | 子菜单折叠箭头 |
| 图标↔文字间距 | 8px | 8px | gap-2 |
| 圆角（内层） | `radius_control-xs`（4px） | `radius_control-xs`（4px） | 填充色所在内容行的圆角 |

#### small 尺寸

| 规格项 | level1 | level2 | level3 |
|--------|--------|--------|--------|
| 外层 Frame 高度 | **32px** | **32px** | **32px** |
| 内层内容行高度 | **30px**（4+22+4） | **30px**（4+22+4） | **30px**（4+22+4） |
| padding-v（内层） | 4px | 4px | 4px |
| padding-h | 4px + level缩进 | — | — |
| 文字字号 | tip1（14px） | tip1（14px） | tip1（14px） |
| 图标尺寸 | 16×16px（control-xs） | 无图标 | 无图标 |
| 箭头图标 | 16×16px | — | — |
| 图标↔文字间距 | 4px | — | — |
| 圆角（内层） | `radius_control-xs`（4px） | `radius_control-xs`（4px） | `radius_control-xs`（4px） |

---

### 颜色 Token 映射

#### 默认状态（Enabled）

| 区域 | Light 模式 | Dark 模式 | Token | 说明 |
|------|-----------|----------|-------|------|
| 菜单项文字 | `--o-color-info2` | `--o-color-info2` | grey-12 | 默认文字色 |
| 菜单项背景 | 透明 | 透明 | — | 无背景 |
| 悬浮背景 | `--o-color-control2-light` | `--o-color-control2-light` | — | hover 态 |
| 图标 | `currentColor` | `currentColor` | — | 跟随文字色 |
| 箭头 | `--o-color-info2` | `--o-color-info2` | — | 折叠箭头色 |
| 指示线 | `--o-color-control4` | `--o-color-control4` | — | 侧边指示线 |

#### 选中状态（Actived）

| 区域 | Light 模式 | Dark 模式 | Token | 说明 |
|------|-----------|----------|-------|------|
| 菜单项文字 | `--o-color-primary1` | `--o-color-primary1` | brand-6 | 选中蓝色文字 |
| 菜单项背景 | `--o-color-control3-light` | `--o-color-control3-light` | — | 浅蓝选中背景 |
| 图标 | `--o-color-primary1` | `--o-color-primary1` | — | 跟随文字色 |
| 箭头 | `--o-color-primary1` | `--o-color-primary1` | — | 展开时箭头色 |

#### 禁用状态（Disabled）

| 区域 | Light 模式 | Dark 模式 | Token | 说明 |
|------|-----------|----------|-------|------|
| 文字 | `--o-color-info4` | `--o-color-info4` | — | 禁用灰色 |
| 图标 | `--o-color-info4` | `--o-color-info4` | — | 禁用灰色 |

---

### 字体样式映射

| 使用场景 | 字号 Token | 行高 Token | 字重 Token | 字号值 | 行高值 |
|---------|-----------|-----------|-----------|-------|-------|
| medium 一级菜单文字（>1440px） | `font_size-text1` | `line_height-text1` | `font_weight-regular` | 16px | 24px |
| medium 其他层级 / laptop 断点 | `font_size-tip1` | `line_height-tip1` | `font_weight-regular` | 14px | 22px |
| small 一级项文字（≤840px） | `font_size-tip1` | `line_height-tip1` | `font_weight-regular` | 14px | 22px |
| small 其余项（≤840px） | `font_size-tip2` | `line_height-tip2` | `font_weight-regular` | 12px | 20px |

字体族：`font_family`（HarmonyOS / HarmonyHeiTi Regular）

---

### 变体 componentKey 速查

#### 菜单项变体（medium）

| 变体组合 | componentKey | node_id | 说明 |
|---------|-------------|---------|------|
| size=medium, level1, Enabled, Dark=off | `11b82fbe2b41e90aa8453330f01038a024718a12` | `1042:13182` | 一级未选中浅色 |
| size=medium, level1, Enabled, Dark=on | `ae0c025d4f28b80edb2b4d8bfbd4e3d8f3f9dd70` | `1042:13186` | 一级未选中深色 |
| size=medium, level1, Actived, Dark=off | `597cd0da9372d4312168aa13e49889587adf8978` | `1042:13190` | 一级选中浅色 |
| size=medium, level1, Actived, Dark=on | `d80ddd07bad9a63d4bcb2e9d407cacee494df6d6` | `1042:13195` | 一级选中深色 |
| size=medium, level2, ic_Enabled, Dark=off | `112d20e3ef4092ccae9ff1b94cf3e2b6a0072236` | `1042:13200` | 二级带图标未选中浅色 |
| size=medium, level2, ic_Enabled, Dark=on | `efb66a36d3a0bcbb6ff020482a01134d87731f56` | `1042:13203` | 二级带图标未选中深色 |
| size=medium, level2, text_Enabled, Dark=off | `347a3164e78f1e4092fca2ffedd114691f2296b5` | `1042:13207` | 二级纯文字未选中浅色 |
| size=medium, level2, text_Enabled, Dark=on | `626c65fd96a780b5465b40a9bf723e4f86c1ea63` | `1042:13211` | 二级纯文字未选中深色 |
| size=medium, level2, text_Actived, Dark=off | `73ac82fb77292789f929fbc56bd19e275c87eddb` | `1042:13215` | 二级纯文字选中浅色 |
| size=medium, level2, text_Actived, Dark=on | `1cc92361e376059aea0cd16de07305e4850b9531` | `1042:13220` | 二级纯文字选中深色 |
| size=medium, level2, ic_Actived, Dark=off | `71dbb482a77ce9c0deb38bc877adcedc30625f69` | `1042:13225` | 二级带图标选中浅色 |
| size=medium, level2, ic_Actived, Dark=on | `284ae5417d818f28e1ea1a1f187b73fd66851c1e` | `1042:13230` | 二级带图标选中深色 |

#### 菜单项变体（small）

| 变体组合 | componentKey | node_id | 说明 |
|---------|-------------|---------|------|
| size=small, level1, Enabled, Dark=off | `a632b4ebac4050f682f756783ed3f54f946b9895` | `1042:13241` | 一级未选中浅色 |
| size=small, level1, Enabled, Dark=on | `c29f2741102e92e656bbeddff0b535f269f5086a` | `1042:13245` | 一级未选中深色 |
| size=small, level2, Enabled, Dark=off | `457163cd4348c9c900ce548d25dd5b3ee7a3c8c4` | `1042:13249` | 二级未选中浅色 |
| size=small, level2, Enabled, Dark=on | `08517a07476b14341e0fbf59f47b23401fd407b2` | `1042:13253` | 二级未选中深色 |
| size=small, level3, Enabled, Dark=off | `7758a248a5b3e6f93c48dd93b1f61529441996b5` | `1042:13257` | 三级未选中浅色 |
| size=small, level3, Enabled, Dark=on | `0d87e047e147c22a417179ab9fb105fa7ea7e73e` | `1042:13261` | 三级未选中深色 |
| size=small, level1, Actived, Dark=off | `2debaebf862877b107493014c3f249898cc9f187` | `1042:13265` | 一级选中浅色 |
| size=small, level1, Actived, Dark=on | `3b16fe15a4bbd57c5fdcd877682978ff80f20978` | `1042:13270` | 一级选中深色 |
| size=small, level2, Actived, Dark=off | `f5cb0602934600204e11f5176608967fe7353a03` | `1042:13275` | 二级选中浅色 |
| size=small, level2, Actived, Dark=on | `fffc27a9c1a20c53db5eb232a375e11022c98cca` | `1042:13280` | 二级选中深色 |
| size=small, level3, Actived, Dark=off | `992433e46997a67cb15acd43296cec4e643b12ef` | `1042:13285` | 三级选中浅色 |
| size=small, level3, Actived, Dark=on | `33c2a03a8b52d4aa765ff872a0659c9dfd234b6d` | `1042:13290` | 三级选中深色 |

#### 分节标题行变体（Group/Tittle，不参与交互）

| 变体组合 | componentKey | node_id | 说明 |
|---------|-------------|---------|------|
| size=Menu, Tittle, Dark=off | `229c232fadaf937836fd34a17e2932214538a1a2` | `1042:13235` | 纯文字分节标题浅色 |
| size=Menu, Tittle, Dark=on | `7bbc7e10306cebf66602bde14fb9837951566ac9` | `1042:13238` | 纯文字分节标题深色 |
| size=medium, Group, Dark=off | `24b2f0b9fb3bd16223f05b189978ed9ecc5f865a` | `1042:13295` | medium 分节标题行浅色 |
| size=medium, Group, Dark=on | `b5e15fde2215253d9d637236722f5239afc49d94` | `1042:13301` | medium 分节标题行深色 |
| size=small, Group, Dark=off | `84673ef051f35a7922ea07335812660f4a5183d7` | `1042:13319` | small 分节标题行浅色 |
| size=small, Group, Dark=on | `48d94b919a52dad9d5166a61c640b313d9fe2576` | `1042:13333` | small 分节标题行深色 |

---

### Pixso 操作速查

1. **拼装菜单**：每个菜单项使用外层透明 Frame（固定高度）+ 内层内容行（高度 = Frame − 2px）的双层结构，依次放入同一 Auto Layout 垂直容器中；Actived 填充背景施加在内层，上下各留 1px 透明间隔
2. **插入分节标题**：在 level1 组之间拖入对应尺寸的 Group/Tittle 变体，作为视觉分区标签
3. **切换尺寸**：右侧面板 → size 属性 → 选择 `medium` / `small`（整组统一修改）
4. **切换状态**：右侧面板 → 属性 3 → 选择 `Enabled` / `Actived` / `ic_Enabled` / `ic_Actived`
5. **切换主题**：右侧面板 → Dark 属性 → 选择 `off` / `on`
6. **修改文字**：双击进入组件 → 双击对应 text 图层修改内容
7. **替换图标**：使用 `create_instance` 插入对应 OIcon 实例，替换 ic_* 变体中的图标占位

---

### 注意事项

- **双层结构（核心规则）**：level1 / level2 / level3 菜单项均由**外层透明 Frame + 内层内容行**组成。外层 Frame 固定高度（medium: level1=48px、level2=40px；small 各级均为 32px），内层内容行比外层矮 2px（各级均减 2px）。Actived 填充背景仅施加在内层内容行上，上下各自与外层 Frame 之间产生 1px 透明间隔，防止相邻选中项背景粘连。
- **图标仅 medium level2 有**：`ic_*` 变体才有前缀图标；small 所有变体、medium level1 均**无**前缀图标，不要额外添加
- **level3 仅 small 有**：三级层级仅在 `size=small` 下存在变体
- **Group 不是菜单项**：Group/Tittle 是装饰性分节标题行，无交互、无选中态，样式（padding/字号/颜色）与 level* 变体不同，不可互换；Group 行**不使用双层结构**
- **箭头位置**：设计稿中默认箭头在右侧（arrowPosition=right），左侧箭头需在代码层面设置 `arrow-position="left"`，Pixso 变体层面不区分
- **选中态背景**：选中背景为纯色（`--o-color-control3-light`），**不支持**在 Pixso 直接设为渐变；如需渐变需通过代码 `:deep()` 覆盖
- **多菜单识别**：设计稿中若同列出现多个独立完整菜单结构（top 差距 >100px），应视为多个独立 OMenu 实例
- **文字溢出**：OMenuItem 文字超出容器宽度时自动截断省略，hover 时通过 Popover 显示完整内容

---

## Part C：交互与状态

### 交互状态

> 📋 **以下为手风琴菜单的完整交互状态规范**，包含一级和二级菜单项在各状态下的详细样式参数。

#### 状态总览

| 状态 | 触发条件 | 说明 |
|------|---------|------|
| 默认（Default） | 初始状态 | 未交互的基础态 |
| 悬浮（Hover） | 鼠标悬浮 | 提示可交互 |
| 按下（Press） | 鼠标按下 | 即时反馈 |
| 选中（Actived） | 点击菜单项 | 当前激活态 |
| 禁用（Disabled） | `disabled=true` | 不可交互 |

---

#### 1️⃣ 默认状态（Default）

**一级菜单项**
- **文字**：一级字号 16px (Regular)
- **一级色值**：`#000000 80%` (`--o-color-info2`)
- **图标**：无前缀图标（仅箭头图标）

**二级菜单项**
- **文字**：二级字号 14px (Regular)
- **二级色值**：`#000000 80%` (`--o-color-info2`)
- **背景**：透明

---

#### 2️⃣ 悬浮状态（Hover）

**二级悬浮**
- **文字**：二级字号 14px (Regular)
- **二级色值**：`#000000 80%` (`--o-color-info2`)
- **背景色值**：`#EBF1FA` (`--o-color-control2-light`)

**一级悬浮**
- **文字**：一级字号 16px (Regular)
- **一级色值**：`#000000 80%` (`--o-color-info2`)
- **背景色值**：`#EBF1FA` (`--o-color-control2-light`)
- **图标**：
  - 大小：24×24px
  - 色值：`#000000 80%`, `rgba(0, 0, 0, 0.8)` (`--o-color-info2`)

---

#### 3️⃣ 按下状态（Press）

**二级按下**
- **文字**：二级字号 14px (**SemiBold**)
- **二级色值**：`#002FA7` (`--o-color-primary1`)
- **背景色值**：`#CEDBF5` (`--o-color-control3-light`)
- **图标**：
  - 大小：24×24px
  - 色值：`#002FA7` (`--o-color-primary1`)

**一级按下**
- **文字**：一级字号 16px (**SemiBold**)
- **一级色值**：`#002FA7` (`--o-color-primary1`)
- **背景色值**：`#CEDBF5` (`--o-color-control3-light`)
- **图标**：
  - 大小：24×24px
  - 色值：`#002FA7` (`--o-color-primary1`)

---

#### 4️⃣ 选中状态（Actived）

**二级选中**
- **文字**：二级字号 14px (**SemiBold**)
- **二级色值**：`#002FA7` (`--o-color-primary1`)
- **背景色值**：`#CEDBF5` (`--o-color-control3-light`)
- **图标**：
  - 大小：24×24px
  - 色值：`#002FA7` (`--o-color-primary1`)

**一级选中**
- **文字**：一级字号 16px (**SemiBold**)
- **一级色值**：`#002FA7` (`--o-color-primary1`)
- **背景色值**：`#CEDBF5` (`--o-color-control3-light`)
- **图标**：
  - 大小：24×24px
  - 色值：`#002FA7` (`--o-color-primary1`)

> ⚠️ **注意**：按下和选中状态的视觉表现一致，区别在于触发时机不同。按下是瞬时反馈，选中是持久状态。

---

#### 5️⃣ 禁用状态（Disabled）

**一级禁用**
- **文字**：一级字号 16px (Regular)
- **一级色值**：`#000000 40%` (`--o-color-info4`)
- **图标**：
  - 大小：24×24px
  - 色值：`#000000 40%`, `rgba(0, 0, 0, 0.4)` (`--o-color-info4`)
- **背景**：透明

**二级禁用**
- **文字**：二级字号 14px (Regular)
- **二级色值**：`#000000 40%` (`--o-color-info4`)
- **背景**：透明

---

### 交互状态速查表

| 状态 | 层级 | 字号 | 字重 | 文字色 | 背景色 | 图标大小 | 图标色 |
|------|------|------|------|--------|--------|---------|--------|
| Default | Level 1 | 16px | Regular | `--o-color-info2` | 透明 | — | — |
| Default | Level 2 | 14px | Regular | `--o-color-info2` | 透明 | — | — |
| Hover | Level 1 | 16px | Regular | `--o-color-info2` | `--o-color-control2-light` | 24×24px | `--o-color-info2` |
| Hover | Level 2 | 14px | Regular | `--o-color-info2` | `--o-color-control2-light` | — | — |
| Press | Level 1 | 16px | **SemiBold** | `--o-color-primary1` | `--o-color-control3-light` | 24×24px | `--o-color-primary1` |
| Press | Level 2 | 14px | **SemiBold** | `--o-color-primary1` | `--o-color-control3-light` | 24×24px | `--o-color-primary1` |
| Actived | Level 1 | 16px | **SemiBold** | `--o-color-primary1` | `--o-color-control3-light` | 24×24px | `--o-color-primary1` |
| Actived | Level 2 | 14px | **SemiBold** | `--o-color-primary1` | `--o-color-control3-light` | 24×24px | `--o-color-primary1` |
| Disabled | Level 1 | 16px | Regular | `--o-color-info4` | 透明 | 24×24px | `--o-color-info4` |
| Disabled | Level 2 | 14px | Regular | `--o-color-info4` | 透明 | — | — |

> Group/Tittle 分节标题行无上述任何交互状态。

### 折叠/展开交互

- **点击 level1 行**：展开/折叠下方 level2 子项，箭头图标旋转 180°
- **手风琴模式**：`accordion=true` 时，同级展开新组自动收起其他组
- **展开动画**：`.o-sub-menu-children` 高度 0 → 内容高度渐变过渡
- **箭头方向**：默认右侧（`arrowPosition=right`），可切换至左侧（`arrowPosition=left`）

### 选中联动

- **selectStrictly=true**：选中叶子项时，其所有父级 level1 同步高亮（关联选中）
- **selectStrictly=false**（默认）：仅叶子项自身高亮，父级不联动

---

## Part D：设计变量绑定

### 推荐绑定变量

| 元素 | 属性 | 推荐变量 Token |
|------|------|---------------|
| 默认文字 | fill | `--o-color-info2` |
| 选中文字 | fill | `--o-color-primary1` |
| 禁用文字 | fill | `--o-color-info4` |
| 悬浮背景 | fill | `--o-color-control2-light` |
| 选中背景 | fill | `--o-color-control3-light` |
| 指示线 | fill | `--o-color-control4` |
| 图标（默认） | fill | `currentColor` |
| 图标（选中） | fill | `--o-color-primary1` |
| medium 一级字号 | fontSize | `font_size-text1`（16px） |
| medium 其他字号 | fontSize | `font_size-tip1`（14px） |
| small 字号 | fontSize | `font_size-tip1`（14px） |
| medium 图标尺寸 | width/height | `icon_size_control-m`（24px） |
| small 图标尺寸 | width/height | `icon_size_control-xs`（16px） |
| medium 图标间距 | autoLayoutItemSpacing | `gap-2`（8px） |
| small 图标间距 | autoLayoutItemSpacing | `gap-1`（4px） |
| 菜单项圆角 | cornerRadius | `radius_control-xs` |
| 菜单宽度 | width | 240px（--menu-width） |

---

## Part E：最佳实践

### 使用建议

1. **尺寸选择**：常规管理后台使用 `medium`，空间紧张的侧边栏使用 `small`
2. **图标使用**：medium level2 项可配图标（`ic_*` 变体），其余层级/尺寸无图标
3. **层级嵌套**：建议不超过三级，过深的嵌套影响可用性
4. **手风琴模式**：侧边栏空间有限时开启 `accordion=true`，避免同时展开多组
5. **分节标题使用时机**：菜单项超过 6–8 项或存在明显功能分区时，插入 Group/Tittle 行辅助视觉组织

### 设计提示

- 菜单宽度通常固定为 240px，文字建议简短（≤10 字）
- 选中背景色（`--o-color-control3-light`）仅支持纯色，如需渐变须通过代码 `:deep()` 实现
- 深色模式下文字颜色和背景色自动切换，无需手动调整
- 箭头图标颜色跟随当前状态（Enabled=info2，Actived=primary1）自动同步
- 多级嵌套时，父级关联高亮（selectStrictly=true）有助于用户感知当前路径

---

## 技术备注

- 组件节点 ID：`1042:13181`（组件集）
- 设计稿 URL：https://pixso.cn/app/design/JZkjW0mhmT61Mtd98dCfBw?item-id=1042:13181
- 生成日期：2026-06-02
- 变体总数：32（size×层级×状态×Dark）
- medium 图标：OpenDesign 内置图标（从 `@opensig/opendesign` 导入），非自定义 SVG
- small 无图标：所有 small 变体均无前缀图标
- level3 限 small：三级层级仅存在于 small 尺寸变体
- 选中背景：`--o-color-control3-light`（纯色），渐变需 `:deep()` 覆盖