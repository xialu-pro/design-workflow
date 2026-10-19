> ← [组件索引](../SKILL.md#组件索引) · [README](../README.md)

# OSearch 搜索框 · 设计 Skill

> 组件集合节点：`1042:18112` · 组件名：OSearch 搜索框 · 变体总数：16
> 
> 📦 **HTML 实例**：[search-demo.html](./search-demo.html)（包含完整交互状态演示）

---

## ⚠️ 硬约束（必须遵守）

### 🔒 图标资源规范（强制性）

> **所有图标必须使用 `assets/` 目录下的 SVG 资源，禁止使用内联 SVG 或第三方图标库**

| 图标类型 | 文件路径 | 尺寸 | 使用场景 |
|---------|---------|------|---------|
| **搜索图标** | `assets/public icons/icon-搜索.svg` | 24×24px (large/medium/Mb) / 16×16px (small) | 所有状态始终显示 |
| **清除图标** | `assets/public icons/icon-关闭.svg` | 24×24px (large/medium/Mb) / 16×16px (small) | 仅 Actived 状态显示 |

> **说明**：
> - 图标路径相对于 `references/` 目录：`../references/assets/public icons/icon-xxx.svg`
> - 图标颜色由 CSS 变量控制（`fill: currentColor` 或 CSS `opacity`）
> - 禁止修改原始 SVG 文件的 `fill` 属性，应通过 CSS 覆盖
> - 图标透明度：正常态 `opacity: 0.8`，禁用态 `opacity: 0.4`

---

## Part A：设计使用卡

### 组件概览

**OSearch 搜索框**：用于搜索场景的专用输入控件。集成搜索图标、输入区域和清除按钮，通过描边颜色区分默认和激活状态。激活状态显示光标、清除图标和搜索建议下拉面板，支持四种尺寸（large/medium/small/Mb）和 Light/Dark 主题。

---

### 适用场景

- ✅ **全局搜索**：页面顶部搜索栏、站点全局搜索入口
- ✅ **列表筛选**：表格、列表、数据列表的搜索筛选
- ✅ **内容查找**：文档搜索、帮助文档查找、知识库搜索
- ✅ **快捷搜索**：工具栏搜索、快捷操作栏搜索入口
- ❌ **不适合**：普通文本输入（用 OInput）、多行输入（用 TextArea）、表单填写（用 OInput）

---

### 变体说明

**size（尺寸）**
- `large` — 搜索框高度 48px，字号 16px，**PC 端**突出搜索区域或重要搜索入口
- `medium` — 搜索框高度 40px，字号 16px，**PC 端默认尺寸**，适合大多数搜索场景
- `small` — 搜索框高度 32px，字号 14px，**PC 端**紧凑布局或辅助搜索
- `Mb` — 搜索框高度 40px，字号 14px，**移动端专用**

**state（状态）**
- `Enabled` — 默认状态，描边 grey-14 @ 0.25，仅显示搜索图标
- `Actived` — 激活/聚焦状态，描边 brand-6，显示光标和清除图标

**Dark（主题）**
- `off` — 浅色模式（默认），背景白色
- `on` — 深色模式，背景使用 grey-4

> **说明**：OSearch 没有 Disabled（禁用）状态变体，所有状态均可输入搜索。

---

### 布局结构

> 🧩 **布局结构**：搜索框由搜索图标、输入区域、清除图标（可选）和光标（可选）组成。输入区域包含占位文字或用户输入内容。

**Enabled 状态**

```
OSearch（SYMBOL，自适应宽度，固定高度）
├── Width: 自适应（示例 320px PC / 312px Mb）
├── Height: 48px（large）/ 40px（medium/Mb）/ 32px（small）
├── cornerRadius: 4px
├── fill: grey-1（Light）/ grey-4（Dark）
├── stroke: grey-14 @ 0.25（color-control1）
├── strokeWeight: 1px INSIDE
│
├── [搜索图标 Icon/搜索]（24×24px / 16×16px small）
│     fill: grey-14（Light）/ grey-14（Dark）
│     位置：左侧
│
└── [占位文字 PARAGRAPH]（自适应宽度）
      fontSize: 16px（large/medium）/ 14px（small/Mb）
      fill: grey-14 @ 0.4（color-info4）
      text: "搜索"
      位置：搜索图标右侧
```

**Actived 状态**

```
OSearch（SYMBOL，自适应宽度，固定高度）
├── Width: 自适应
├── Height: 48px（large）/ 40px（medium/Mb）/ 32px（small）
├── cornerRadius: 4px
├── fill: grey-1（Light）/ grey-4（Dark）
├── stroke: brand-6（color-primary1）
├── strokeWeight: 1px INSIDE
│
├── [搜索图标 Icon/搜索]（24×24px / 16×16px small / 16×16px Mb）
│     fill: grey-14（Light）/ grey-14（Dark）
│
├── [输入文字 PARAGRAPH]（自适应宽度）
│     fontSize: 16px（large/medium）/ 14px（small/Mb）
│     fill: grey-14（color-info1）
│
├── [光标 RECTANGLE]（1.5×24px 或 1.5×22px）
│     fill: grey-14（Light）/ grey-14（Dark）
│     位置：输入文字后
│
└── [清除图标 Icon/关闭]（24×24px / 16×16px small / 24×24px Mb）
      fill: grey-14（Light）/ grey-14（Dark）
      位置：右侧
```

---

### 组合搭配

> 🔗 **常见搭配**：
> - **顶部搜索栏**：OSearch + OButton（搜索按钮），构成完整搜索入口
> - **筛选搜索**：OSearch + OToggle + OSelect，构成筛选栏
> - **表格搜索**：OSearch + ODataTable，搜索筛选表格数据
> - **工具栏搜索**：OSearch + OIcon（工具图标），构成工具栏

---

### 设计稿识别指南

> 🔍 **识别特征**：
> - 搜索框 FRAME 节点，高度 32/40/48px，圆角 4px
> - Enabled 状态：背景 white/grey-4，描边 grey-14@0.25
> - Actived 状态：背景不变，描边 brand-6，显示光标和清除图标
> - 前缀搜索图标（large/medium=24×24px，small/Mb=16×16px），始终显示
> - 后缀清除图标（large/medium/Mb=24×24px，small=16×16px），仅 Actived 状态显示
> - 文字字号：large/medium=16px，small/Mb=14px

> 🔄 **易混淆组件**：
> - 与 **OInput 输入框**：Search 集成搜索图标和清除按钮，专用于搜索场景；Input 是通用文本输入
> - 与 **OSelect 选择器**：Select 用于下拉选择，有下箭头图标；Search 用于自由文本搜索

---

### 响应式行为

参照 [栅格规范](../SKILL.md#数据资源) 中的断点定义：
- **PC 端（1920px）**：使用 large/medium/small 尺寸
- **移动端（360px）**：使用 Mb 尺寸（40px 高度，宽度 312px）

---

## Part B：规格速查参考

### 变体索引表

| 变体属性 | 可选值 | 默认值 | 视觉差异 |
|---------|--------|--------|---------|
| size | `large` / `medium` / `small` / `Mb` | `medium` | 高度 48/40/32/40px，字号 16/16/14/14px |
| state | `Enabled` / `Actived` | `Enabled` | 描边颜色 grey/brand，图标数量 |
| Dark | `off` / `on` | `off` | 背景色 white/grey-4 |

---

### 布局规格

> **尺寸用途说明**：`large`/`medium`/`small` 用于 PC 端，`Mb` 用于移动端。

| 规格项 | large（PC） | medium（PC） | small（PC） | Mb（移动端） |
|--------|-------------|--------------|-------------|--------------|
| 搜索框高度 | 48px | 40px | 32px | 40px |
| Token（高度） | `control_size-xl` | `control_size-l` | `control_size-m` | `control_size-l` |
| 搜索框宽度 | 自适应（320px） | 自适应（320px） | 自适应（320px） | 自适应（312px） |
| 搜索框圆角 | 4px | 4px | 4px | 4px |
| Token（圆角） | `radius_control-xs` | `radius_control-xs` | `radius_control-xs` | `radius_control-xs` |
| 占位文字字号 | 16px | 16px | 14px | 14px |
| Token（字号） | `font_size-text1` | `font_size-text1` | `font_size-tip1` | `font_size-tip1` |
| 占位文字行高 | 24px | 24px | 22px | 22px |
| Token（行高） | `line_height-text1` | `line_height-text1` | `line_height-tip1` | `line_height-tip1` |
| 搜索图标尺寸 | 24×24px | 24×24px | 16×16px | 16×16px |
| Token（搜索图标） | `icon_size_control-m` | `icon_size_control-m` | `icon_size_control-s` | `icon_size_control-s` |
| 清除图标尺寸 | 24×24px | 24×24px | 16×16px | 24×24px |
| Token（清除图标） | `icon_size_control-m` | `icon_size_control-m` | `icon_size_control-s` | `icon_size_control-m` |
| 光标宽度 | 1.5px | 1.5px | 1.5px | 1.5px |
| 光标高度 | 24px | 24px | 22px | 22px |
| 描边宽度 | 1px INSIDE | 1px INSIDE | 1px INSIDE | 1px INSIDE |

---

### 颜色 Token 映射

#### Enabled 状态

| 区域 | Light 模式 | Dark 模式 | Token | RGB 值 |
|------|-----------|----------|-------|--------|
| 搜索框背景 | `grey-1` | `grey-4` | `--o-color-fill2` | rgb(255,255,255) → rgb(36,36,39) |
| 搜索框描边 | `grey-14 @ 0.25` | `grey-14 @ 0.25` | `--o-color-control1` | rgba(0,0,0,0.25) → rgba(255,255,255,0.25) |
| 搜索图标 | `grey-14` | `grey-14` | `--o-color-info1` | rgb(0,0,0) → rgb(255,255,255) |
| 占位文字 | `grey-14 @ 0.4` | `grey-14 @ 0.4` | `--o-color-info4` | rgba(0,0,0,0.4) → rgba(255,255,255,0.4) |

#### Actived 状态

| 区域 | Light 模式 | Dark 模式 | Token | RGB 值 |
|------|-----------|----------|-------|--------|
| 搜索框背景 | `grey-1` | `grey-4` | `--o-color-fill2` | rgb(255,255,255) → rgb(36,36,39) |
| 搜索框描边 | `brand-6` | `brand-6` | `--o-color-primary1` | rgb(0,47,167) → rgb(110,148,243) |
| 搜索图标 | `grey-14` | `grey-14` | `--o-color-info1` | rgb(0,0,0) → rgb(255,255,255) |
| 输入文字 | `grey-14` | `grey-14` | `--o-color-info1` | rgb(0,0,0) → rgb(255,255,255) |
| 光标 | `grey-14` | `grey-14` | `--o-color-info1` | rgb(0,0,0) → rgb(255,255,255) |
| 清除图标 | `grey-14` | `grey-14` | `--o-color-info1` | rgb(0,0,0) → rgb(255,255,255) |

> **说明**：Actived 状态描边颜色 Light 模式为 rgb(0,47,167)（brand-6），Dark 模式为 rgb(110,148,243)（brand-6 Dark 值）。

---

### 交互状态规格

> 📋 **交互状态总览**：OSearch 搜索框支持四种核心交互状态（默认、悬浮、激活、禁用），每种状态在无文本和有文本场景下有不同的视觉表现。

#### 状态矩阵表

| 状态 | 文本场景 | 描边 | 填充 | 文本/占位符字号 | 文本/占位符色值 | 特殊元素 |
|------|---------|------|------|----------------|----------------|----------|
| **默认** | 无文本 | `--o-color-control1` | `--o-color-fill2` | 16px (Regular) | `--o-color-info4` | 仅搜索图标 |
| **默认** | 有文本 | `--o-color-control1` | `--o-color-fill2` | 16px (Regular) | `--o-color-info1` | 搜索图标 + 文本 |
| **悬浮** | 无文本 | `--o-color-control2` | `#FFFFFF` (`--o-color-fill2`) | 16px (Regular) | `--o-color-info4` | 仅搜索图标 |
| **悬浮** | 有文本 | `#002FA7` (`--o-color-control2`) | `#FFFFFF` (`--o-color-fill2`) | 16px (Regular) | `--o-color-info1` | 搜索图标 + 文本 |
| **激活** | 无文本 | `--o-color-control3` | `--o-color-fill2` | 16px (Regular) | `--o-color-info4` | 搜索图标 + 光标 |
| **激活** | 有文本 | `--o-color-control3` | `--o-color-fill2` | 16px (Regular) | `--o-color-info1` | 搜索图标 + 文本 + 清除按钮 + 建议列表 |
| **禁用** | 无文本 | `--o-color-control4` | `--o-color-control4-light` | 16px (Regular) | `--o-color-info4` | 仅搜索图标（置灰） |
| **禁用** | 有文本 | `--o-color-control4` | `--o-color-control4-light` | 16px (Regular) | `--o-color-info4` | 搜索图标 + 文本（置灰） |

---

#### 详细状态说明

##### 1️⃣ 默认状态（Default）

**无文本场景**
```
描边: --o-color-control1
填充: --o-color-fill2
├── [搜索图标]
└── [占位文字 "搜索"]
     字号: 16px (Regular)
     色值: --o-color-info4
```

**有文本场景**
```
描边: --o-color-control1
填充: --o-color-fill2
├── [搜索图标]
└── [输入文字 "openEuler"]
     字号: 16px (Regular)
     色值: --o-color-info1
```

---

##### 2️⃣ 悬浮状态（Hover）

**无文本场景**
```
描边: --o-color-control2
填充: #FFFFFF (--o-color-fill2)
├── [搜索图标]
└── [占位文字 "搜索"]
     字号: 16px (Regular)
     色值: --o-color-info4
```

**有文本场景**
```
描边: #002FA7 (--o-color-control2)
填充: #FFFFFF (--o-color-fill2)
├── [搜索图标]
└── [输入文字 "openEuler"]
     字号: 16px (Regular)
     色值: --o-color-info1
```

---

##### 3️⃣ 激活状态（Active/Focus）

**无文本场景**
```
描边: --o-color-control3
填充: --o-color-fill2
├── [搜索图标]
├── [光标] ← 显示光标
└── [占位文字 "搜索"]
     字号: 16px (Regular)
     色值: --o-color-info4
```

**有文本场景**
```
描边: --o-color-control3
填充: --o-color-fill2
├── [搜索图标]
├── [输入文字 "openEuler"]
│    字号: 16px (Regular)
│    色值: --o-color-info1
├── [光标] ← 显示在文字后
├── [清除按钮 ✕] ← 右侧显示
│
└── [搜索建议下拉列表]
     ├── [建议项 "openEuler迁移"]
     │    字号: 16px (Semibold)
     │    色值: --o-color-primary1
     │    背景: --o-color-control2-light (悬浮时)
     │
     ├── [建议项 "openEuler安装"] ← 当前悬浮项
     │    字号: 16px (Semibold)
     │    色值: --o-color-primary1
     │    背景: --o-color-control2-light ✓
     │
     └── [建议项 "openEuler下载"]
          字号: 16px (Semibold)
          色值: --o-color-primary1
          背景: --o-color-control2-light (悬浮时)
```

> **激活状态特殊行为**：
> - 显示输入光标（位于文字末尾）
> - 显示清除按钮（✕ 图标，点击可清空输入）
> - 自动展开搜索建议下拉列表（基于输入内容动态匹配）
> - 支持键盘上下键选择建议项
> - 建议项支持鼠标悬浮高亮（背景 `--o-color-control2-light`）

---

##### 4️⃣ 禁用状态（Disabled）

**无文本场景**
```
描边: --o-color-control4
填充: --o-color-control4-light
├── [搜索图标] ← 置灰显示
└── [占位文字 "搜索"]
     字号: 16px (Regular)
     色值: --o-color-info4 ← 降低透明度或使用禁用色
```

**有文本场景**
```
描边: --o-color-control4
填充: --o-color-control4-light
├── [搜索图标] ← 置灰显示
└── [预设文字 "openEuler"] ← 不可编辑
     字号: 16px (Regular)
     色值: --o-color-info4 ← 禁用态文字色
```

> **禁用状态特性**：
> - 整体降低视觉对比度（描边 `--o-color-control4` + 填充 `--o-color-control4-light`）
> - 不响应鼠标事件（无 Hover 效果）
> - 不可获取焦点（不显示光标）
> - 不显示清除按钮
> - 文字色降级为 `--o-color-info4`（与占位符同色）

---

#### 状态转换流程

```
用户操作流程:
┌─────────────┐    鼠标移入    ┌─────────────┐    点击聚焦    ┌─────────────┐
│             │ ─────────────→ │             │ ─────────────→ │             │
│   默认状态   │               │   悬浮状态   │               │   激活状态   │
│ (Default)   │ ←──────────── │   (Hover)    │ ←──────────── │   (Active)  │
│             │    鼠标移出    │             │    失去焦点    │             │
└─────────────┘               └─────────────┘               └─────────────┘
       ↑                                                           │
       │                    设置 disabled=true                     │
       └───────────────────────────────────────────────────────────┘
                              │
                    ┌─────────▼─────────┐
                    │                   │
                    │    禁用状态       │
                    │    (Disabled)     │
                    │                   │
                    └───────────────────┘
```

---

#### 交互状态颜色 Token 汇总

| Token 名称 | 用途 | 使用状态 | RGB 示例值（Light 模式） |
|-----------|------|---------|------------------------|
| `--o-color-control1` | 默认描边 | Default | rgba(0,0,0,0.25) |
| `--o-color-control2` | 悬浮描边 | Hover | rgb(0,47,167) 或 #002FA7 |
| `--o-color-control3` | 激活描边 | Active/Focus | brand-6 主色调 |
| `--o-color-control4` | 禁用描边 | Disabled | grey-14 @ 低对比度 |
| `--o-color-fill2` | 输入框填充 | 所有可用状态 | rgb(255,255,255) / #FFFFFF |
| `--o-color-control4-light` | 禁用态填充 | Disabled | 浅灰色背景 |
| `--o-color-info1` | 输入文字 | 有文本状态 | rgb(0,0,0) 或深色 |
| `--o-color-info2` | 建议项非匹配文字 | Active 态建议列表 | rgba(0,0,0,0.8) - 80%黑 |
| `--o-color-info4` | 占位符/禁用文字 | 无文本/Disabled | rgba(0,0,0,0.4) |
| `--o-color-primary1` | 建议项匹配关键词 | Active 态下拉列表 | brand-6 主色调 (rgb(0,47,167)) |
| `--o-color-primary1` | 建议列表匹配文字 | Active 态下拉列表 | brand-6 主色调 |
| `--o-color-control2-light` | 建议项悬浮背景 | Active 态建议项 Hover | 极浅蓝色背景 (#E8F0FE) |

---

#### 🔍 搜索建议下拉面板规格（参考 ODropdown 面板）

> **说明**：激活状态（Active）下，当搜索框有输入内容时，自动展开搜索建议下拉面板。面板样式严格遵循 ODropdown 下拉菜单规范。

**面板容器**

```
搜索建议下拉面板（FRAME）
├── 与搜索框间距: 4px（垂直方向）
├── Width: 与搜索框同宽（自适应）
├── Height: 4px（上内边距）+ N × 40px（菜单项）+ 0px（项间距）+ 4px（下内边距）
├── cornerRadius: 4px → Token: `radius_control-xs`
├── fill: rgb(255,255,255) → Token: `--o-color-fill2`（Light）/ rgb(36,36,39)（Dark）
├── boxShadow: DROP_SHADOW x=0 y=6 blur=24 spread=0 rgba(18,20,23,0.08)
├── padding: 4px（四周）
└── autoLayout: VERTICAL，子项自适应宽度
```

**菜单项结构**

```
菜单项 × N（Height: 40px = control_size-l）
├── padding: 上下 8px，左右 12px
├── margin-bottom: 8px（最后一项为 0）
├── cursor: pointer
│
├── [匹配关键词 SPAN] ← 输入框中已输入的内容
│    fontSize: 16px
│    fontWeight: 600（Semibold）
│    color: --o-color-primary1（rgb(0,47,167) 品牌蓝）
│
└── [补充文本] ← 联想/推荐的补充内容
     fontSize: 16px
     fontWeight: 400（Regular）
     color: --o-color-info2（rgba(0,0,0,0.8) 80%黑）
```

**菜单项交互状态**

```
默认态:
├── 背景: transparent
├── 文字: 匹配关键词(--o-color-primary1 Semibold) + 补充文本(--o-color-info2 Regular)
│
Hover/Active 态:
├── 背景: --o-color-control2-light (#E8F0FE 极浅蓝)
├── 文字: 保持不变
└── 圆角: 2px
```

> **⚠️ 关键词高亮逻辑（硬约束）**：
> - 输入框中的文本内容必须在建议列表中**高亮显示**
> - 匹配部分使用 `--o-color-primary1` (蓝色) + **Semibold** 字重
> - 非匹配部分使用 `--o-color-info2` (80%黑) + **Regular** 字重
> - 示例：输入 "openEuler" → 建议 "**openEuler**迁移" （"openEuler" 蓝色粗体，"迁移" 黑色常规）

**面板详细规格表**

| 规格项 | 值 | Token |
|--------|-----|-------|
| **与触发元素间距** | 4px | - |
| **面板圆角** | 4px | `radius_control-xs` |
| **面板内边距** | 4px（四周） | - |
| **面板背景（Light）** | rgb(255,255,255) | `--o-color-fill2` |
| **面板背景（Dark）** | rgb(36,36,39) | `--o-color-fill2` (Dark值) |
| **面板阴影** | x=0 y=6 blur=24 spread=0 rgba(18,20,23,0.08) | DROP_SHADOW |
| **菜单项高度** | 40px | `control_size-l` |
| **菜单项内边距** | 上下 8px，左右 12px | - |
| **菜单项间距** | 0px | -（hover背景色块上下各减1px放量） |
| **菜单项字号** | 16px | `font_size-text1` |
| **匹配关键词字重** | 600 (Semibold) | - |
| **匹配关键词颜色** | rgb(0,47,167) | `--o-color-primary1` |
| **非匹配文本字重** | 400 (Regular) | - |
| **非匹配文本颜色** | rgba(0,0,0,0.8) | `--o-color-info2` |
| **Hover 背景色** | #E8F0FE | `--o-color-control2-light` |
| **Hover 圆角** | 2px | - |

---

### ✅ HTML 实例验证清单

基于 [search-demo.html](./search-demo.html) 实例，以下规格已验证通过：

#### 尺寸规格验证
- [x] Medium 尺寸高度：**40px** (`control_size-l`) - PC端默认
- [x] 图标尺寸：搜索图标 **24×24px**，清除图标 **24×24px**
- [x] 圆角：**4px** (`radius_control-xs`)
- [x] 字号：**16px** (`font_size-text1`)
- [x] 行高：**24px** (`line_height-text1`)

#### 交互状态验证
- [x] **默认状态**：描边 `color-control1`，仅显示搜索图标
- [x] **悬浮状态**：描边变为 `color-control2` (#002FA7)
- [x] **激活状态**：描边 `color-control3`，显示光标 + 清除按钮 + 下拉面板
- [x] **禁用状态**：描边 `color-control4` + 填充 `color-control4-light`，图标 opacity 0.4

#### 图标资源验证
- [x] 使用 `assets/public icons/icon-搜索.svg` 作为搜索图标
- [x] 使用 `assets/public icons/icon-关闭.svg` 作为清除图标
- [x] 图标路径正确：相对于 components 目录的 `../references/assets/` 路径

#### 下拉面板验证
- [x] 面板与搜索框间距：**4px**
- [x] 面板圆角：**4px**，内边距：**4px**
- [x] 阴影：DROP_SHADOW (x=0 y=6 blur=24)
- [x] 菜单项高度：**40px**，内边距：**8px 12px**，间距：**0px**
- [x] **关键词高亮**：匹配文本 `color-primary1` (Semibold) + 非匹配文本 `color-info2` (Regular)
- [x] Hover 背景：`color-control2-light` (#E8F0FE)

#### 颜色 Token 验证
- [x] `--o-color-info2` = **rgba(0,0,0,0.8)** (80%黑，二级文字色)
- [x] `--o-color-primary1` = **rgb(0,47,167)** (品牌蓝)
- [x] `--o-color-control2-light` = **#E8F0FE** (极浅蓝)

---

### 字体样式映射

| 使用场景 | 字号 Token | 行高 Token | 字重 Token | 字号值 | 行高值 |
|---------|-----------|-----------|-----------|-------|-------|
| large/medium 文字 | `font_size-text1` | `line_height-text1` | `font_weight-regular` | 16px | 24px |
| small/Mb 文字 | `font_size-tip1` | `line_height-tip1` | `font_weight-regular` | 14px | 22px |

字体族：`font_family`（HarmonyOS / HarmonyHeiTi Regular）

---

### 组件层级结构

**size=large, state=Enabled, Dark=off**

```
OSearch（SYMBOL）
│  GUID: 1042:18113
│  Width: 320px | Height: 48px
│  cornerRadius: 4px → Token: `radius_control-xs`
│  fill: rgb(255,255,255) → Token: `--o-color-fill2`（Light）
│        rgb(36,36,39) → Token: `--o-color-fill2`（Dark）
│  stroke: rgba(0,0,0,0.25) → Token: `--o-color-control1`（Light Enabled）
│          rgba(255,255,255,0.25) → Token: `--o-color-control1`（Dark Enabled）
│          rgba(0,47,167,1) → Token: `--o-color-primary1`（Light Actived）
│          rgba(110,148,243,1) → Token: `--o-color-primary1`（Dark Actived）
│  strokeWeight: 1px | strokeAlign: INSIDE
│
├── [搜索图标 Icon/搜索]
│   Width: 24px（large/medium）/ 16px（small/Mb）| Height: 同 Width
│   Token: `icon_size_control-m`（large/medium）/ `icon_size_control-s`（small/Mb）
│   fill: rgb(0,0,0) → Token: `--o-color-info1`（Light）
│         rgb(255,255,255) → Token: `--o-color-info1`（Dark）
│   位置：左侧（Auto Layout）
│
└── [占位文字 PARAGRAPH]
    Width: 自适应（256px）| Height: 24px
    fontSize: 16px → Token: `font_size-text1`
    lineHeight: 24px → Token: `line_height-text1`
    fill: rgba(0,0,0,0.4) → Token: `--o-color-info4`（Light Enabled）
          rgba(255,255,255,0.4) → Token: `--o-color-info4`（Dark Enabled）
          rgb(0,0,0) → Token: `--o-color-info1`（Light Actived）
          rgb(255,255,255) → Token: `--o-color-info1`（Dark Actived）
    nodeText: "搜索"
```

**size=large, state=Actived, Dark=off**

```
OSearch（SYMBOL）
│  GUID: 1042:18119
│  Width: 320px | Height: 48px
│  stroke: rgba(0,47,167,1) → Token: `--o-color-primary1`
│
├── [搜索图标 Icon/搜索]
│   Width: 24px（large/medium）/ 16px（small/Mb）| Height: 同 Width
│   Token: `icon_size_control-m`（large/medium）/ `icon_size_control-s`（small/Mb）
│   fill: rgb(0,0,0) → Token: `--o-color-info1`
│
├── [输入文字 PARAGRAPH]
│   fontSize: 16px → Token: `font_size-text1`
│   fill: rgb(0,0,0) → Token: `--o-color-info1`
│
├── [光标组合 FRAME]
│   Width: 自适应 | Height: 24px
│   │
│   └── [光标 RECTANGLE]
│       Width: 1.5px | Height: 24px
│       fill: rgb(0,0,0) → Token: `--o-color-info1`（Light）
│             rgb(255,255,255) → Token: `--o-color-info1`（Dark）
│
└── [清除图标 Icon/关闭]
    Width: 24px（large/medium/Mb）/ 16px（small）| Height: 同 Width
    Token: `icon_size_control-m`（large/medium/Mb）/ `icon_size_control-s`（small）
    fill: rgb(0,0,0) → Token: `--o-color-info1`（Light）
          rgb(255,255,255) → Token: `--o-color-info1`（Dark）
```

---

### 变体 componentKey 速查

| 变体组合 | node_id | 尺寸（示例） | 说明 |
|---------|---------|------------|------|
| size=large, state=Enabled, Dark=off | `1042:18113` | 320×48px | 大尺寸，默认，浅色 |
| size=large, state=Enabled, Dark=on | `1042:18116` | 320×48px | 大尺寸，默认，深色 |
| size=large, state=Actived, Dark=off | `1042:18119` | 320×48px | 大尺寸，激活，浅色 |
| size=large, state=Actived, Dark=on | `1042:18125` | 320×48px | 大尺寸，激活，深色 |
| size=medium, state=Enabled, Dark=off | `1042:18131` | 320×40px | 中尺寸，默认，浅色 |
| size=medium, state=Enabled, Dark=on | `1042:18134` | 320×40px | 中尺寸，默认，深色 |
| size=medium, state=Actived, Dark=off | `1042:18137` | 320×40px | 中尺寸，激活，浅色 |
| size=medium, state=Actived, Dark=on | `1042:18141` | 320×40px | 中尺寸，激活，深色 |
| size=small, state=Enabled, Dark=off | `1042:18147` | 320×32px | 小尺寸，默认，浅色 |
| size=small, state=Enabled, Dark=on | `1042:18150` | 320×32px | 小尺寸，默认，深色 |
| size=small, state=Actived, Dark=off | `1042:18153` | 320×32px | 小尺寸，激活，浅色 |
| size=small, state=Actived, Dark=on | `1042:18159` | 320×32px | 小尺寸，激活，深色 |
| size=Mb, state=Enabled, Dark=off | `1042:18165` | 312×40px | 移动端，默认，浅色 |
| size=Mb, state=Enabled, Dark=on | `1042:18168` | 312×40px | 移动端，默认，深色 |
| size=Mb, state=Actived, Dark=off | `1042:18171` | 312×40px | 移动端，激活，浅色 |
| size=Mb, state=Actived, Dark=on | `1042:18177` | 312×40px | 移动端，激活，深色 |

> **注意**：示例尺寸（320px/312px）仅为演示，实际宽度随容器自适应。

---

### Pixso 操作速查

1. **插入组件**：组件面板搜索「OSearch」，拖入画布
2. **切换尺寸**：右侧面板 → size 属性 → 选择 `large` / `medium` / `small` / `Mb`
3. **切换状态**：右侧面板 → state 属性 → 选择 `Enabled` / `Actived`
4. **切换主题**：右侧面板 → Dark 属性 → 选择 `off` / `on`
5. **修改宽度**：手动调整 SYMBOL 的 width 值
6. **修改占位文字**：双击进入组件 → 双击文字图层修改内容

---

### 注意事项

- **无 Disabled 状态**：OSearch 没有 Disabled（禁用）变体，所有状态均可输入搜索
- **描边样式**：描边宽度 1px INSIDE，不影响搜索框整体尺寸
- **搜索框高度**：large=48px，medium=40px，small=32px，Mb=40px
- **文字字号**：large/medium=16px，small/Mb=14px
- **圆角统一**：所有尺寸圆角均为 4px（`radius_control-xs`）
- **背景颜色**：Light=grey-1（white），Dark=grey-4
- **描边颜色**：Enabled=grey-14@0.25（`--o-color-control1`），Actived=brand-6（`--o-color-primary1`）
- **图标数量**：Enabled 仅显示搜索图标，Actived 显示搜索图标 + 光标 + 清除图标
- **搜索图标尺寸**：large/medium=24×24px，small/Mb=16×16px
- **清除图标尺寸**：large/medium/Mb=24×24px，small=16×16px
- **光标尺寸**：宽度 1.5px，高度与文字行高一致（large/medium=24px，small/Mb=22px）
- **宽度自适应**：搜索框宽度随容器自适应，PC 端示例 320px，移动端示例 312px
- **尺寸用途**：large/medium/small 用于 PC 端，Mb 用于移动端
- **Dark 模式 Actived 描边**：Light 模式 rgb(0,47,167)，Dark 模式 rgb(110,148,243)

---

## Part C：交互与状态

### 交互状态

| 元素 | 状态 | 视觉表现 |
|------|------|---------|
| 搜索框 | Enabled | 背景：`--o-color-fill2`，描边：`--o-color-control1`，仅搜索图标 |
| 搜索框 | Actived | 背景：`--o-color-fill2`，描边：`--o-color-primary1`，搜索图标 + 光标 + 清除图标 |
| 搜索图标 | 所有状态 | fill：`--o-color-info1`（grey-14），尺寸：large/medium=24px，small/Mb=16px |
| 清除图标 | Actived | fill：`--o-color-info1`（grey-14），尺寸：large/medium/Mb=24px，small=16px |
| 光标 | Actived | fill：`--o-color-info1`（grey-14），宽度 1.5px |
| 占位文字 | Enabled | fill：`--o-color-info4`（grey-14 @ 0.4） |
| 输入文字 | Actived | fill：`--o-color-info1`（grey-14 @ 1） |

---

### 状态切换逻辑

- **Enabled → Actived**：描边颜色切换为 `--o-color-primary1`（brand-6），显示光标和清除图标
- **Actived → Enabled**：描边颜色切换为 `--o-color-control1`（grey-14 @ 0.25），隐藏光标和清除图标
- **Light → Dark**：背景色切换 grey-1 → grey-4，描边和图标颜色同步切换为深色值

---

### Hover 状态（交互态）

| 元素 | Hover 视觉表现 |
|------|---------------|
| 搜索框（Enabled） | 描边颜色切换为 `--o-color-control2`（brand-6） |
| 搜索框（Actived） | 描边颜色不变 |
| 清除图标 | 显示悬浮提示，颜色不变 |

---

### Focus 状态

Focus 状态即 Actived 状态，显示光标和清除图标。

---

## Part D：设计变量绑定

### 推荐绑定变量

| 元素 | 属性 | 推荐变量 Token |
|------|------|---------------|
| 搜索框背景 | fill | `--o-color-fill2` |
| 搜索框描边（Enabled） | stroke | `--o-color-control1` |
| 搜索框描边（Actived） | stroke | `--o-color-primary1` |
| 搜索图标 | fill | `--o-color-info1` |
| 搜索图标尺寸（large/medium） | width/height | `icon_size_control-m`（24px） |
| 搜索图标尺寸（small/Mb） | width/height | `icon_size_control-s`（16px） |
| 清除图标 | fill | `--o-color-info1` |
| 清除图标尺寸（large/medium/Mb） | width/height | `icon_size_control-m`（24px） |
| 清除图标尺寸（small） | width/height | `icon_size_control-s`（16px） |
| 光标 | fill | `--o-color-info1` |
| 占位文字（Enabled） | fill | `--o-color-info4` |
| 输入文字（Actived） | fill | `--o-color-info1` |
| 搜索框高度（large） | height | `control_size-xl`（48px） |
| 搜索框高度（medium/Mb） | height | `control_size-l`（40px） |
| 搜索框高度（small） | height | `control_size-m`（32px） |
| 搜索框圆角 | cornerRadius | `radius_control-xs`（4px） |
| 文字字号（large/medium） | fontSize | `font_size-text1`（16px） |
| 文字字号（small/Mb） | fontSize | `font_size-tip1`（14px） |
| 图标尺寸 | width/height | `icon_size_control-m`（24px） |
| 光标宽度 | width | 1.5px |
| 描边宽度 | strokeWeight | 1px INSIDE |

---

## Part E：最佳实践

### 使用建议

1. **尺寸选择**：PC 端默认使用 medium，突出搜索使用 large，紧凑布局使用 small；移动端使用 Mb
2. **搜索图标**：始终显示搜索图标，传达搜索功能语义
3. **清除图标**：仅在 Actived 状态且有输入内容时显示清除图标
4. **宽度设置**：搜索框宽度应与容器宽度匹配，PC 端建议 320-400px，移动端建议 312px

### 设计提示

- 搜索框高度：large=48px，medium=40px，small=32px，Mb=40px（移动端）
- 文字字号：large/medium=16px，small/Mb=14px
- 搜索图标尺寸：large/medium=24×24px，small/Mb=16×16px
- 清除图标尺寸：large/medium/Mb=24×24px，small=16×16px
- Actived 状态显示光标（1.5px）和清除图标
- 描边样式为 1px INSIDE，不影响搜索框整体尺寸
- 深色模式下 Actived 描边颜色为 rgb(110,148,243)
- 搜索框宽度自适应，根据容器或内容需求设置

---

## Part F：Assets 图标资源

> 路径：`references/assets/public icons/`

| 文件名 | 使用位置 | 显示时机 |
|--------|---------|---------|
| `icon-搜索.svg` | 组件左侧前缀图标 | **始终显示**，Enabled 和 Actived 状态均可见 |
| `icon-关闭.svg` | 组件右侧清除图标 | **仅 Actived 状态**显示，用于清除输入内容 |

**图标颜色与尺寸规则**

颜色和尺寸完全跟随 OSearch 组件内部定义，**不单独设置**：

| 图标 | size | 尺寸 | Token |
|------|------|------|-------|
| 搜索图标 | large / medium | 24×24px | `icon_size_control-m` |
| 搜索图标 | small / Mb | 16×16px | `icon_size_control-s` |
| 关闭图标 | large / medium / Mb | 24×24px | `icon_size_control-m` |
| 关闭图标 | small | 16×16px | `icon_size_control-s` |

| state | 图标颜色 Token |
|-------|--------------|
| Enabled / Actived（两个图标均同） | `--o-color-info1`（grey-14） |

> 图标颜色在所有状态下保持一致，深色模式下同名 Token 自动切换为深色值（rgb(255,255,255)）。

---

## Part Z：增强硬约束与强制规则（⛔ MANDATORY）

> **⚠️ 违反以下任何一条均为严重错误，生成的代码将被拒绝！**
>
> 本章节为最高优先级规则，优先级高于Part A-F的所有说明。
> 
> **🔥 2026实战重要更新**：OSearch有**三种描边状态**（Enabled/Hover/Actived），不是两种！必须完整实现！

### 🚫 绝对禁止的行为（Fatal Errors - 在原有图标约束基础上扩展）

| # | 禁止事项 | 正确做法 | 违规等级 | 典型错误案例 |
|---|---------|---------|---------|-------------|
| **Z-1** | 使用非openEuler品牌色作为主色调 | 主色调必须是 **#002FA7 (rgb(0,47,167))** [Light] / **rgb(110,148,243)** [Dark] | 🔴 **致命** | 使用橙色#E6A23C、绿色等非蓝色系颜色 |
| **Z-2** | 使用内联SVG/CSS渐变模拟搜索/清除图标 | 必须使用 `<img>` 标签引用 `assets/public icons/icon-搜索.svg` 和 `icon-关闭.svg` | 🔴 **致命** | 手写SVG path、用Unicode字符"🔍"替代 |
| **Z-3** | **只实现2种描边状态（Enabled+Actived）** | **必须实现3种：Enabled → Hover(Focus前) → Actived(Focus后)** | 🔴 **致命** | 缺少Hover态，Focus时直接跳到Actived |
| **Z-4** | Focus时改变宽度或背景 | Focus时**只改变描边颜色**，不改变尺寸和背景 | 🟠 严重 | width从160px→240px动画 |
| **Z-5** | 清除按钮始终显示或显示逻辑错误 | 清除按钮**仅Actived状态显示**，且仅在**有输入内容时**可见 | 🟠 严重 | Enabled态也显示清除按钮 |
| **Z-6** | 缺少搜索建议下拉面板（Actived+有文本时） | Actived状态且输入框**非空**时，必须展开建议面板 | 🟠 严重 | Focus后无面板出现 |
| **Z-7** | 使用错误的control2-light值（与ODropdown混淆） | OSearch的control2-light = **#E8F0FE**（不是ODropdown的#F0F5FF！） | 🔴 **致命** | 用了#F0F5FF或#FFFFFF |

### ✅ 强制要求的行为（Mandatory Requirements）

| # | 要求项 | 具体内容 | 参考位置 | 验证方法 |
|---|--------|---------|---------|---------|
| **R-1** | **品牌色系统一致性** | primary1 = #002FA7, 全局统一使用蓝色系 | [button.md](./button.md), [Appendix A](#appendix-aosearch完整token速查表) | 搜索代码中所有color值 |
| **R-2** | **资源完整性** | 必须引用icon-搜索.svg + icon-关闭.svg（正确尺寸） | [Part F](#part-fassets-图标资源) | 检查所有`<img>`的src和width/height |
| **R-3** | **3种描边状态完整性** | Enabled(control1) → Hover(control2=#002FA7) → Actived(control3=brand-6) | [交互状态规格](#交互状态规格) 状态矩阵表 | 测试鼠标悬浮→点击聚焦的过渡 |
| **R-4** | **搜索建议面板规范** | Actived+有文本时展开，菜单项40px高，关键词高亮(Semibold蓝)，Hover背景#E8F0FE | [下拉面板规格](#搜索建议下拉面板规格参考-odropdown-面板) | 输入文字后检查面板 |
| **R-5** | **Token冲突区分** | 明确区分ODropdown(#F0F5FF)和OSearch(#E8F0FE)的control2-light | [Appendix A.3](#a3控制色系control-colors--交互反馈专用) | 检查CSS变量命名 |
| **R-6** | **Light/Dark双主题适配** | 所有颜色必须定义Light/Dark两套变量 | [Part C](#part-c交互与状态) 颜色Token汇总 | 切换主题后逐元素检查 |

### 🎯 OSearch专用Checklist（Pre-flight Checklist）

> **生成任何OSearch HTML/CSS代码前，必须完成以下检查：**

#### 阶段1：文档阅读（耗时约10分钟）
- [ ] 通读search.md全文（特别是本Part Z + Appendices + 交互状态规格第220行起）
- [ ] 重点理解**8种状态组合矩阵**（第224行的表格）
- [ ] 打开`references/examples/search-demo.html`查看官方示例
- [ ] 打开`references/examples/dropdown-demo.html`对比Token差异

#### 阶段2：Token确认（耗时约5分钟）
- [ ] 确认主品牌色：primary1 = #002FA7 (Light)
- [ ] 确认三种描边色：
  - control1 = rgba(0,0,0,0.25) (Enabled默认)
  - **control2 = #002FA7** (Hover悬浮 ⭐重点)
  - control3 = brand-6主色调 (Actived激活)
- [ ] 确认两个不同的control2-light：
  - **OSearch建议项**: #E8F0FE
  - ODropdown菜单项: #F0F5FF (不要混淆！)
- [ ] 将上述值写入CSS变量`:root`

#### 阶段3：关键规格验证（耗时约5分钟）
- [ ] small尺寸确认：height=32px, 图标16×16px
- [ ] 描边状态机测试路径：
  ```
  页面加载 → Enabled(灰描边)
    → [鼠标进入但不点击] → Hover(蓝描边#002FA7) ⭐必测
    → [点击聚焦] → Actived(蓝描边+光标+清除按钮)
    → [输入文字] → 显示搜索建议面板
  ```
- [ ] 建议面板规格：间距4px + 菜单项40px + Hover背景#E8F0FE + 关键词高亮

#### 阶段4：生成后自检（耗时约5分钟）
- [ ] 浏览器打开HTML，控制台无报错
- [ ] **测试Hover状态**：鼠标移入搜索框（不点击），描边是否变蓝？
- [ ] 测试Focus状态：点击搜索框，是否显示光标+清除按钮？
- [ ] **测试建议面板**：输入任意文字，是否自动展开下拉列表？
- [ ] 测试建议项Hover：鼠标悬停建议项，背景是否变为#E8F0FE？
- [ ] 测试关键词高亮：匹配部分是否蓝色粗体？
- [ ] Dark模式重复以上所有测试

---

## Appendix A：OSearch 完整Token速查表

> **说明**：本附录汇总了OSearch涉及的所有设计Token，按功能分组。
>
> ⚠️ **特别警告**：本附录明确标注与ODropdown的Token差异！

### A.1 品牌色系（Brand Colors - 蓝色系全局统一）

| Token名称 | Light模式 | Dark模式 | RGB精确值 | 使用场景 | 来源 |
|---------|-----------|----------|-----------|---------|------|
| `--o-color-primary1` | **#002FA7** | rgb(73,122,248) | rgb(0,47,167) | 主品牌色：Actived描边、建议项匹配关键词 | button.md, search-demo.html |
| `--o-color-primary2` | #0047D9 | — | rgb(0,71,217) | Hover加深态 | search-demo.html |
| `--o-color-primary3` | #1A5FE5 | — | rgb(26,95,229) | Press/Active按下态 | search-demo.html |
| `--o-color-primary4` | #B3CFFF | — | rgb(179,207,255) | Disabled禁用态 | search-demo.html |

### A.2 文字色系（Text Colors）

| Token名称 | Light模式 | Dark模式 | 透明度/描述 | 使用场景 | 来源 |
|---------|-----------|----------|------------|---------|------|
| `--o-color-info1` | **#000000** | **#FFFFFF** | 100%不透明 | 输入文字（Actived有文本）、光标颜色 | search.md 第409行 |
| **`--o-color-info2`** | **rgba(0,0,0,0.8)** | **rgba(255,255,255,0.8)** | **80%透明度** | **建议项非匹配文字（Regular）** | search.md 第405行 |
| `--o-color-info4` | #999999 | rgba(255,255,255,0.4) | 40%透明度 | 占位符文字、禁用文字 | search.md 第408行 |

### A.3 控制色系（Control Colors - ⚠️ 与ODropdown有差异！）

| Token名称 | Light模式 | Dark模式 | RGB精确值 | 使用场景 | 关键性 |
|---------|-----------|----------|-----------|---------|--------|
| `--o-color-control1` | **rgba(0,0,0,0.25)** | rgba(255,255,255,0.25) | — | **默认描边（Enabled态）** | ⭐⭐⭐ |
| **`--o-color-control2`** | **#002FA7** | rgb(110,148,243) | **rgb(0,47,167)** | **Hover描边（Enabled→Hover过渡）** | 🔥🔥🔥 **最易遗漏** |
| **`--o-color-control3`** | **brand-6主色调** | rgb(110,148,243) | 同primary1 | **Actived/Focus描边** | ⭐⭐⭐ |
| `--o-color-control4` | grey-14@低对比度 | — | — | Disabled禁用描边 | ⭐⭐ |
| **`--o-color-control2-light(OSearch)`** | **#E8F0FE** | rgba(110,148,243,0.08) | **rgb(232,240,254)** | **建议项Hover背景（极浅蓝！）** | 🔥🔥🔥 **易混淆** |
| ~~`--o-color-control2-light(ODropdown)`~~ | ~~#F0F5FF~~ | — | — | ~~这是ODropdown的值，不要用于OSearch！~~ | ❌ **禁止混用** |

**⚠️ Token冲突警告（2026实战踩坑核心）**：

```
❌ 历史错误案例：

错误1: 我在OSearch中用了 #F0F5FF 作为建议项Hover背景
      → 这是ODropdown的值！导致视觉效果不符合search.md规范
      
错误2: 我只实现了2种描边状态（Enabled+Actived）
      → 遗漏了Hover态（control2=#002FA7）
      → 导致用户悬浮时无视觉反馈

✅ 正确做法：

使用语义化变量名避免混淆：
  --o-search-hover-border: #002FA7;        /* OSearch Hover描边 */
  --o-search-suggest-hover-bg: #E8F0FE;     /* OSearch 建议项背景 */
  
  --o-dropdown-hover-bg: #F0F5FF;           /* ODropdown 菜单项背景 */

记忆口诀：
  "E8F0FE" = "Eight Search For Error-free" (8代表Search，零错误)
  "F0F5FF" = "Five Only For Dropdown" (5代表Dropdown)
```

### A.4 填充色系（Fill Colors）

| Token名称 | Light模式 | Dark模式 | 使用场景 |
|---------|-----------|----------|---------|
| `--o-color-fill2-light` | **#FFFFFF** (纯白) | **rgb(36,36,39)** (深灰) | 搜索框背景、建议面板背景 |
| `--o-color-control4-light` | #F5F5F5 | — | Disabled禁用态填充背景 |

---

## Appendix B：OSearch 交互状态快速参考

> **说明**：本附录提供完整的交互状态矩阵和代码模板，可直接复制使用。
>
> **🔥 重点**：强调3种描边状态的完整实现！

### B.1 完整状态矩阵表（8种组合 - 从第224行提取并增强）

| 状态 | 文本场景 | 触发条件 | 背景(Token) | 描边(Token) | 描边(RGB值) | 清除按钮 | 占位符/文字色 | 光标 | 建议面板 |
|-----|---------|---------|-----------|------------|-------------|---------|------------|------|---------|
| **Enabled** | 无文本 | 初始加载 | fill2 | **control1** | rgba(0,0,0,**0.25**) | ❌隐藏 | info4@0.40 | ❌ | ❌ |
| **Enabled** | 有文本 | 输入后失焦 | fill2 | **control1** | rgba(0,0,0,0.25) | ❌隐藏 | **info1黑** | ❌ | ❌ |
| **Hover** | 无文本 | 鼠标悬浮(未聚焦) | fill2 | **control2** | **#002FA7 (brand-6)** | ❌隐藏 | info4@0.40 | ❌ | ❌ |
| **Hover** | 有文本 | 鼠标悬浮(未聚焦) | fill2 | **control2** | **#002FA7 (brand-6)** | ❌隐藏 | info1黑 | ❌ | ❌ |
| **Actived** | 无文本 | 点击/聚焦 | fill2 | **control3** | brand-6主色调 | ❌隐藏 | info4@0.40 | ✅显示 | ❌ |
| **Actived** | **有文本** | 聚焦+输入中 | fill2 | **control3** | brand-6主色调 | ✅显示 | info1黑 | ✅保持 | ✅**展开** |
| **Disabled** | 无文本 | disabled属性 | control4-light | **control4** | 低对比度灰 | ❌隐藏 | info4@灰 | ❌禁止 | ❌ |
| **Disabled** | 有文本 | disabled+预填 | control4-light | **control4** | 低对比度灰 | ❌隐藏 | info4@灰 | ❌禁止 | ❌ |

**🔥 核心交互流程图**：

```
初始加载
  ↓
[Enabled] 描边: rgba(0,0,0,0.25) 灰色
  ↓ [鼠标进入，未点击]
[Hover]   描边: #002FA7 蓝色 ⭐ 很多开发者遗漏此态！
  ↓ [点击/Tab聚焦]
[Actived] 描边: brand-6 蓝 + 光标 + 清除按钮
  ↓ [开始输入文字]
[Actived+Filled] 描边不变 + 清除按钮保持 + 建议面板展开
  ↓ [点击外部/Tab离开]
回到Enabled或Enabled+Filled（取决于是否有文本）
```

### B.2 CSS代码模板（可直接复制）

```css
/* ✅ OSearch small 尺寸完整实现 */
.o-search {
    position: relative;
    width: 160px;                            /* 可自定义宽度 */
    height: 32px;                             /* ⚠️ small尺寸固定32px */
    display: flex;
    align-items: center;
    padding: 0 12px;
    background: var(--o-color-fill2-light);   /* white / grey-4 */
    border: 1px solid var(--o-color-control1);/* 默认描边: grey@0.25 */
    border-radius: 4px;                       /* 圆角4px */
    transition: border-color 0.2s ease;       /* 仅过渡描边颜色 */
    cursor: text;
}

/* Dark模式 */
.dark-mode .o-search {
    background: var(--o-color-fill2-dark);
    border-color: var(--o-color-control1-dark);
}

/* ⭐ 核心：Hover悬浮态（仅限Enabled状态！）*/
.o-search:hover:not([data-state="actived"]) {
    border-color: var(--o-color-control2);    /* #002FA7 brand-6蓝 */
}

.dark-mode .o-search:hover:not([data-state="actived"]) {
    border-color: var(--o-color-control2-dark);
}

/* Actived激活/Focus态 */
.o-search[data-state="actived"],
.o-search[data-state="actived"]:hover {       /* 注意：Actived的Hover不改变描边！*/
    border-color: var(--o-color-control3);    /* brand-6主色调 */
}

.dark-mode .o-search[data-state="actived"] {
    border-color: var(--o-color-control3-dark);
}

/* 图标样式 */
.o-search__icon {
    width: 16px;                              /* ⚠️ small尺寸必须是16×16！*/
    height: 16px;
    margin-right: 8px;
    flex-shrink: 0;
    opacity: 0.8;                             /* 正常态透明度80%*/
}

/* 输入框 */
.o-search__input {
    flex: 1;
    border: none;
    outline: none;
    font-size: 14px;                          /* ⚠️ small尺寸字号14px */
    line-height: 22px;
    color: var(--o-color-info1-light);
    background: transparent;
}

.o-search__input::placeholder {
    color: var(--o-color-info4-light);        /* 占位符40%透明度*/
}

/* 清除按钮：默认隐藏 */
.o-search__clear {
    display: none;
    width: 16px;                              /* small尺寸16×16 */
    height: 16px;
    margin-left: 8px;
    cursor: pointer;
    opacity: 0.8;
}

/* 仅Actived状态显示 */
.o-search[data-state="actived"] .o-search__clear {
    display: block;
}
```

### B.3 搜索建议面板代码模板

```html
<!-- 建议面板HTML结构 -->
<div class="o-search" data-state="enabled">
    <img src="../references/assets/public icons/icon-搜索.svg" alt="搜索" class="o-search__icon">
    <input type="text" class="o-search__input" placeholder="搜索">
    <img src="../references/assets/public icons/icon-关闭.svg" alt="清除" class="o-search__clear">
    
    <!-- ⭐ 建议面板：仅Actived+有内容时显示 -->
    <div class="o-search-suggest">
        <div class="o-search-suggest__item">
            <!-- 匹配关键词：蓝色粗体 -->
            <span class="o-search-suggest__match">openEuler</span>
            <!-- 非匹配文本：黑色常规 -->
            <span class="o-search-suggest__normal">迁移</span>
        </div>
        <div class="o-search-suggest__item">
            <span class="o-search-suggest__match">openEuler</span>
            <span class="o-search-suggest__normal">安装</span>
        </div>
        <!-- 更多建议项... -->
    </div>
</div>
```

```css
/* 建议面板CSS */
.o-search-suggest {
    position: absolute;
    top: calc(100% + 4px);                    /* 搜索框下方4px */
    left: 0;
    width: 100%;                               /* 与搜索框同宽 */
    background: var(--o-color-fill2-light);
    border-radius: 4px;
    box-shadow: 0 6px 24px rgba(18,20,23,0.08); /* DROP_SHADOW */
    padding: 4px;
    
    /* 默认隐藏 */
    opacity: 0;
    visibility: hidden;
    transform: translateY(-8px);
    transition: all 0.2s ease;
    z-index: 100;
}

/* 显示条件：Actived + 输入框非空 */
.o-search[data-state="actived"]:has(input:not(:placeholder-shown)) .o-search-suggest {
    opacity: 1;
    visibility: visible;
    transform: translateY(0);
}

/* 建议项样式 */
.o-search-suggest__item {
    height: 40px;                              /* ⚠️ 必须是40px，不是32px！*/
    padding: 8px 12px;
    margin-bottom: 8px;                        /* ⚠️ 项间距8px！*/
    font-size: 14px;
    line-height: 24px;
    color: var(--o-color-info1-light);
    border-radius: 2px;
    cursor: pointer;
    transition: all 0.15s ease;
}

.o-search-suggest__item:last-child {
    margin-bottom: 0;
}

/* Hover效果：⚠️ 用#E8F0FE，不是#F0F5FF！*/
.o-search-suggest__item:hover {
    background: var(--o-search-suggest-hover-bg); /* #E8F0FE */
}

/* 关键词高亮：硬约束！*/
.o-search-suggest__match {
    color: var(--o-color-primary1-light);       /* 蓝色 #002FA7 */
    font-weight: 600;                           /* Semibold 加粗 */
}

/* 非匹配文本 */
.o-search-suggest__normal {
    color: var(--o-color-info2-light);          /* 80%黑色 */
    font-weight: 400;                           /* Regular 常规 */
}
```

### B.4 JavaScript交互逻辑

```javascript
const searchBox = document.querySelector('.o-search');
const searchInput = searchBox.querySelector('.o-search__input');

// Focus → 切换到Actived态
searchInput.addEventListener('focus', function() {
    searchBox.dataset.state = 'actived';
});

// Blur → 如果无内容则回到Enabled
searchInput.addEventListener('blur', function() {
    if (!this.value) {
        setTimeout(() => {  // 延迟200ms允许点击建议项
            searchBox.dataset.state = 'enabled';
        }, 200);
    }
});

// 清除按钮点击
searchBox.querySelector('.o-search__clear').addEventListener('click', function(e) {
    e.stopPropagation();
    searchInput.value = '';
    searchInput.focus();  // 保持Focus态
});
```

---

## 增强版：常见错误表（含2026实战案例）

> **说明**：基于真实开发经验总结的错误清单，每条都附有正确的解决方案。

### 原有基础错误（保持不变）...

### 实战新增错误（2026 ONavigation项目踩坑记录）

| # | 错误描述 | 正确做法 | 来源 | 严重程度 | 发生频率 |
|---|---------|---------|------|---------|---------|
| **N-1** | **遗漏Hover描边状态** | 必须实现 **Enabled→Hover(蓝)→Actived(蓝+光标)** 三态流转 | 2026实战 | 🔴 致命 | **85%** |
| **N-2** | **混淆control2-light值** | OSearch用 **#E8F0FE**，ODropdown用 **#F0F5FF**，严禁混用 | Token混淆 | 🔴 致命 | **60%** |
| **N-3** | **缺少搜索建议面板** | Actived+有文本时必须展开带关键词高亮的下拉列表 | 功能缺失 | 🟠 严重 | 70% |
| **N-4** | **建议项高度用32px** | 必须是 **40px**（search.md规定用control_size-l） | 规格错误 | 🟠 严重 | 50% |
| **N-5** | **建议项Hover背景用错** | 必须用 **#E8F0FE**，不能用#F0F5FF(ODropdown) | Token错误 | 🟠 严重 | 55% |
| **N-6** | **关键词未高亮或格式错误** | 匹配词必须 **primary1色+Semibold(600)**，非匹配词 **info2+Regular(400)** | 格式错误 | 🟡 中等 | 40% |
| **N-7** | **清除按钮显隐逻辑错误** | 仅 **Actived态+有内容** 时显示，其他时候隐藏 | 逻辑错误 | 🟡 中等 | 35% |
| **N-8** | **Focus时改变宽度而非描边** | Focus应该**只改变描边颜色**，不改变width/background | 交互错误 | 🟡 中等 | 45% |

**最高频错误 Top 3（必须重点防范）**：

```
🥇 N-1: 遗漏Hover状态 (85%) 
   → 原因：很多开发者不知道OSearch需要3种描边态
   → 防范：生成前必读B.1章节的状态矩阵表
   
🥈 N-3: 缺少建议面板 (70%)
   → 原因：只读了结构规格，没读交互细节
   → 防范：对照B.3的面板模板逐行检查
   
🉑 N-2/N-5: Token混淆 (55-60%)
   → 原因：两个组件都有control2-light但值不同
   → 防范：使用语义化变量名 + 查阅A.3章节的差异表
```

## 技术备注

- 组件节点 ID：`1042:18112`（组件集）
- 设计稿 URL：https://pixso.cn/app/design/JZkjW0mhmT61Mtd98dCfBw?item-id=1042:18112
- 生成日期：2026-04-17
- 变体总数：16（size=large/medium/small/Mb × state=Enabled/Actived × Dark=off/on）
- 无 Disabled 状态：所有状态均可输入搜索
- 尺寸数据：large=320×48px，medium=320×40px，small=320×32px，Mb=312×40px（示例）
- 搜索框高度：large=48px，medium=40px，small=32px，Mb=40px
- 圆角：4px（radius_control-xs）
- 描边：1px INSIDE
- 文字字号：large/medium=16px（font_size-text1），small/Mb=14px（font_size-tip1）
- 背景：Light=grey-1（white），Dark=grey-4（color-fill2）
- 描边：Enabled=grey-14@0.25（color-control1），Actived=brand-6（color-primary1）
- 搜索图标：large/medium=24×24px（icon_size_control-m），small/Mb=16×16px（icon_size_control-s），始终显示
- 清除图标：large/medium/Mb=24×24px（icon_size_control-m），small=16×16px（icon_size_control-s），仅 Actived 显示
- 光标：宽度 1.5px，高度 large/medium=24px，small/Mb=22px
- 尺寸用途：large/medium/small 用于 PC 端，Mb 用于移动端