> ← [组件索引](../SKILL.md#组件索引) · [README](../README.md)

# OSelect 选择器 · 设计 Skill

> 组件集合节点：`1042:17664` · 组件名：OSelect 选择器 · 变体总数：20
> 
> 📦 **HTML 实例**：[select-demo.html](../references/examples/select-demo.html)（包含完整交互状态演示）

---

## ⚠️ 硬约束（必须遵守）

### 🔒 图标资源规范（强制性）

> **所有图标必须使用 `assets/` 目录下的 SVG 资源，禁止使用内联 SVG 或第三方图标库**

| 图标类型 | 文件路径 | 尺寸 | 使用场景 |
|---------|---------|------|---------|
| **下箭头图标** | `assets/public icons/icon-下箭头.svg` | 24×24px (所有尺寸) | Enabled/Complete/Actived 状态显示，指示可展开 |
| **上箭头图标** | `assets/public icons/icon-上箭头.svg` | 24×24px (所有尺寸) | Actived-menu 状态显示，指示已展开可收起 |
| **加载图标** | `assets/public icons/icon-loading 加载.svg` | 24×24px | Loading 状态显示，替代箭头图标位置 |
| **关闭图标** | `assets/public icons/icon-关闭.svg` | 12×12px | 多选标签的删除按钮（×） |
| **复选框-未选中** | `assets/checkbox/Unselected.svg` | 16×16px | 多选模式未选中选项的复选框 |
| **复选框-选中** | `assets/checkbox/Selected.svg` | 16×16px | 多选模式已选中选项的复选框 |

> **说明**：
> - 图标路径相对于 `references/` 目录：`../references/assets/public icons/icon-xxx.svg` 或 `../references/assets/checkbox/xxx.svg`
> - 图标颜色由 CSS 变量控制（`fill: currentColor` 或 CSS `color` 属性）
> - **禁止修改原始 SVG 文件的 `fill` 属性**，应通过 CSS 覆盖控制颜色
> - 箭头图标透明度：正常态 `opacity: 1.0`，禁用态 `opacity: 0.4`
> - 加载图标需添加 CSS 动画：`animation: spin 1s linear infinite`（360°旋转）
> - 复选框图标在多选模式下使用，单选模式不显示复选框

### 🔒 图标使用示例代码

```html
<!-- ✅ 正确：使用外部 SVG 资源 -->
<svg class="arrow-icon" style="width: 24px; height: 24px;">
    <use xlink:href="../references/assets/public icons/icon-下箭头.svg#icon" />
</svg>

<!-- ❌ 错误：禁止内联 SVG -->
<svg class="arrow-icon" viewBox="0 0 24 24">
    <path d="M7.41 8.59L12 13.17l4.59-4.58L18 10l-6 6-6-6z" fill="currentColor"/>
</svg>
```

### 🔒 图标状态映射表

| 组件状态 | 显示图标 | 图标资源 | 颜色控制 | 特殊效果 |
|---------|---------|---------|---------|---------|
| **Enabled** | 下箭头 | `icon-下箭头.svg` | `--o-color-info1` | 无 |
| **Complete** | 下箭头 | `icon-下箭头.svg` | `--o-color-info1` | 无 |
| **Actived** | 下箭头 | `icon-下箭头.svg` | `--o-color-info1` | 无 |
| **Actived-menu** | 上箭头 | `icon-上箭头.svg` | `--o-color-info1` | 无 |
| **Disabled** | 下箭头 | `icon-下箭头.svg` | `--o-color-control4-light` | opacity: 0.4 |
| **Loading** | 加载动画 | `icon-loading 加载.svg` | `--o-color-info1` | spin 动画 |
| **多选-未选中** | 复选框 | `Unselected.svg` | `--o-color-control1` + border | 无 |
| **多选-已选中** | 复选框勾选 | `Selected.svg` | 白色 (`white`) | 背景 `--o-color-primary1` |
| **多选标签-删除** | 关闭按钮 | `icon-关闭.svg` | `--o-color-info1` | hover 时 opacity: 1 |

---

## Part A：设计使用卡

### 组件概览

**OSelect 选择器**：用于在预设选项中选择单个值的表单控件。外观类似输入框，右侧显示下箭头图标；点击后展开下拉菜单（Actived-menu 状态），选中后显示所选值（Complete 状态）。支持三种尺寸（large/medium/small）和 Light/Dark 主题。

---

### 适用场景

- ✅ **表单选择**：省份、城市、分类等固定选项的下拉选择
- ✅ **筛选条件**：数据筛选栏中的类型、状态、范围筛选
- ✅ **配置设置**：语言、主题、排序方式等设置项选择
- ✅ **表格操作**：分页每页条数选择、批量操作类型选择
- ❌ **不适合**：自由文本输入（用 OInput）、搜索场景（用 OSearch）、多选（用 OCheckbox 组合）

---

### 变体说明

**size（尺寸）**
- `large` — 选择器高度 40px，字号 16px，**PC 端**突出表单区域或重要选择入口
- `medium` — 选择器高度 32px，字号 14px，**PC 端默认尺寸**，适合大多数表单场景
- `small` — 选择器高度 40px，字号 14px，**移动端专用**

**type（状态类型）**
- `Enabled` — 默认状态，显示占位文字 + 下箭头，描边 grey-14@0.25
- `Actived` — 聚焦状态，描边切换为 brand-6，无下拉菜单展开
- `Complete` — 已选中状态，显示所选值 + 下箭头，描边 grey-14@0.25
- `Actived-menu` — **设计示意变体**，将 Actived 触发器（描边 brand-6、上箭头）与菜单面板合为一体，仅供原型展示展开效果；触发器高度不变（40px/32px），菜单面板紧排其下（实际开发为浮层）

**OSelect-Dropdown（独立浮层）**
- 节点 `1042:17681`，与 `Actived` 触发器配对，构成完整交互展开层；浮层定位于触发器正下方，**间距 4px**，宽度与触发器一致，高度随选项数量动态伸缩

**Dark（主题）**
- `off` — 浅色模式（默认），背景白色
- `on` — 深色模式，背景使用 grey-4

> **说明**：`small` 尺寸仅有 `Enabled` 和 `Complete` 两种 type，无 `Actived` 和 `Actived-menu`，为移动端简化变体。

---

### 布局结构

> 🧩 **布局结构**：选择器由输入区域（FRAME）组成，水平排列占位/选中文字和后缀下箭头图标。`Actived-menu` 是设计示意变体，将触发器与菜单面板组合在同一 FRAME 内方便展示；实际开发中菜单面板以浮层形式渲染在触发器下方，不改变触发器本身的尺寸。

**Enabled / Complete 状态**

```
OSelect（FRAME，自适应宽度，固定高度）
├── Width: 自适应（示例 320px）
├── Height: 40px（large/small）/ 32px（medium）
├── cornerRadius: 4px
├── fill: grey-1（Light）/ grey-4（Dark）
├── stroke: grey-14 @ 0.25（Enabled/Complete）
├── strokeWeight: 1px INSIDE
│
├── [占位文字 / 选中文字 PARAGRAPH]（自适应宽度）
│     Enabled  — fontSize: 16px（large）/ 14px（medium/small），fill: grey-14@0.4（color-info4）
│     Complete — fontSize: 16px（large）/ 14px（medium/small），fill: grey-14（color-info1）
│     位置：左侧
│
└── [下箭头图标 Icon/下箭头 INSTANCE]（24×24px）
      fill: grey-14（color-info1）
      位置：右侧后缀
```

**Actived 状态**

```
OSelect（FRAME，自适应宽度，固定高度）
├── stroke: brand-6（color-primary1）       ← 仅描边颜色变化
├── strokeWeight: 1px INSIDE
│
├── [占位文字 PARAGRAPH]（同 Enabled）
│
└── [下箭头图标 Icon/下箭头]（24×24px）
      fill: grey-14（color-info1）
```

**Actived-menu 状态（设计示意变体）**

> ⚠️ `Actived-menu` 是专为设计稿展示准备的合并变体，触发器自身高度仍为 40px/32px，菜单面板在设计稿中位于触发器正下方 **4px** 处（示意总高 large=252px / medium=194px）。实际开发中菜单面板为浮层，不影响页面布局高度。

```
OSelect-Actived-menu（FRAME，示意组合）
├── 示意总高: 252px（large）/ 194px（medium）  ← 仅设计稿尺寸，非实际高度
│
├── [触发器 FRAME]（Height: 40px / 32px）← 与 Actived 状态相同
│     ├── stroke: brand-6（color-primary1）
│     ├── [占位文字 PARAGRAPH]（fill: color-info4）
│     └── [上箭头图标 Icon/上箭头]（24×24px）← 展开状态切换为上箭头
│
├── [间距 4px]  ← 触发器与菜单面板垂直间距，与 OSelect-Dropdown 独立浮层保持一致
│
└── [菜单面板 FRAME]（Height: 208px / 158px）← 实际为浮层（large: 4+5×40+4=208px）
      ├── cornerRadius: 4px
      ├── fill: grey-1（Light）/ grey-4（Dark）
      ├── boxShadow: 卡片投影
      └── [菜单项 OSelect 选择器/Menu × N]（每项 Height: 40px / 32px）
            ├── [文字 PARAGRAPH]（fill: color-info1）
            └── hover 态背景: --o-color-control2-light
```

**OSelect-Dropdown（独立浮层）**

> 实际交互中使用的独立菜单浮层节点（`1042:17681`）。与 Actived 触发器配对，定位于触发器正下方，间距 4px；宽度与触发器一致（自适应），高度随选项数量动态伸缩。

```
OSelect-Dropdown（FRAME，浮层）
│  GUID: 1042:17681
│  Width: 与触发器同宽（自适应，示例 320px）
│  Height: 4px（上内边距）+ N × 40px（large）/ N × 32px（medium）+ 4px（下内边距）
│          示例 5 项 × large = 208px
│  cornerRadius: 4px → Token: `radius_control-xs`
│  fill: rgb(255,255,255) → Token: `--o-color-fill2`（Light）
│  boxShadow: DROP_SHADOW x=0 y=6 blur=24 spread=0 rgba(18,20,23,0.08)
│  padding: 4px（四周）
│  autoLayout: VERTICAL，子项自适应宽度
│
│  ← 与 Actived 触发器的间距：4px（垂直方向）
│
└── [菜单项 × N]（OSelect 选择器/Menu INSTANCE）
      ├── Width: 自适应（填充浮层内容宽度）
      ├── Height: 40px（large）/ 32px（medium）
      ├── padding: 上下 8px，左右 12px，item spacing: 0px
      ├── [文字 PARAGRAPH]（fill: color-info1）
      └── hover 态背景: --o-color-control2-light（上下各减1px放量）
```

---

### 组合搭配

> 🔗 **常见搭配**：
> - **表单行**：OSelect + OLabel，构成带标签的选择表单项
> - **筛选栏**：OSearch + OSelect + OToggle，构成完整筛选条件组
> - **分页组**：OPagination + OSelect（每页条数），构成带条数选择的分页
> - **配置页**：多个 OSelect 竖排，构成配置选项列表

---

### 设计稿识别指南

> 🔍 **识别特征**：
> - FRAME 节点，高度 32/40px，圆角 4px，宽度自适应
> - 右侧必有下箭头图标（24×24px），是区分 OSelect 的关键特征
> - Enabled 状态：描边 grey-14@0.25，占位文字 grey-14@0.4
> - Complete 状态：描边 grey-14@0.25，文字颜色为 grey-14（实色）
> - Actived 状态：描边 brand-6，箭头图标不变
> - Actived-menu 状态：箭头切换为上箭头，下方出现菜单面板

> 🔄 **易混淆组件**：
> - 与 **OInput 输入框**：Select 用于从选项中选择，有下箭头图标且不可自由输入；Input 用于自由文本输入
> - 与 **ODropdown 下拉按钮**：Select 是**表单控件**，呈现输入框外观；Dropdown 是**按钮**，呈现胶囊形（圆角 100px），用于触发下拉菜单操作
> - 与 **OSearch 搜索框**：Select 有下箭头且选项固定；Search 有搜索图标且用于自由文本搜索

---

### 响应式行为

参照 [栅格规范](../SKILL.md#数据资源) 中的断点定义：
- **PC 端（1920px）**：使用 `large`（40px）或 `medium`（32px）尺寸
- **移动端（360px）**：使用 `small` 尺寸（40px 高度，宽度随容器自适应）

---

## Part B：规格速查参考

### 变体索引表

| 变体属性 | 可选值 | 默认值 | 视觉差异 |
|---------|--------|--------|---------|
| size | `large` / `medium` / `small` | `medium` | 高度 40/32/40px，字号 16/14/14px |
| type | `Enabled` / `Actived` / `Complete` / `Actived-menu` | `Enabled` | 描边颜色、文字颜色、箭头方向；`Actived-menu` 为示意变体（含菜单面板） |
| Dark | `off` / `on` | `off` | 背景色 white/grey-4 |

> **注意**：`small` 仅有 `Enabled` 和 `Complete`，无 `Actived` 和 `Actived-menu`。

---

### 布局规格

> **尺寸用途说明**：`large` 和 `medium` 用于 PC 端，`small` 用于移动端。

| 规格项 | large（PC） | medium（PC） | small（移动端） |
|--------|-------------|--------------|----------------|
| 选择器高度 | 40px | 32px | 40px |
| Token（高度） | `control_size-l` | `control_size-m` | `control_size-l` |
| 选择器宽度 | 自适应（示例 320px） | 自适应（示例 320px） | 自适应（示例 320px） |
| 选择器圆角 | 4px | 4px | 4px |
| Token（圆角） | `radius_control-xs` | `radius_control-xs` | `radius_control-xs` |
| 文字字号 | 16px | 14px | 14px |
| Token（字号） | `font_size-text1` | `font_size-tip1` | `font_size-tip1` |
| 文字行高 | 24px | 22px | 22px |
| Token（行高） | `line_height-text1` | `line_height-tip1` | `line_height-tip1` |
| 下箭头图标尺寸 | 24×24px | 24×24px | 24×24px |
| Token（箭头图标） | `icon_size_control-m` | `icon_size_control-m` | `icon_size_control-m` |
| 描边宽度 | 1px INSIDE | 1px INSIDE | 1px INSIDE |
| Actived-menu 示意总高 | 252px | 194px | — |

---

### OSelect-Dropdown 浮层规格

| 规格项 | large（PC） | medium（PC） |
|--------|------------|-------------|
| 与触发器间距 | 4px | 4px |
| 浮层圆角 | 4px（`radius_control-xs`） | 4px |
| 浮层内边距（四周） | 4px | 4px |
| 阴影 | DROP_SHADOW x=0 y=6 blur=24 spread=0 rgba(18,20,23,0.08) | 同 large |
| 每菜单项高度 | 40px（`control_size-l`） | 32px（`control_size-m`） |
| 菜单项内边距（上/下） | 8px | 8px |
| 菜单项内边距（左/右） | 12px | 12px |
| 菜单项 item spacing | 0px | 0px |
| 示例高度（5 项） | 208px | 168px |
| node_id（示例） | `1042:17681` | — |

---

### 颜色 Token 映射

#### Enabled 状态

| 区域 | Light 模式 | Dark 模式 | Token | RGB 值 |
|------|-----------|----------|-------|--------|
| 选择器背景 | `grey-1` | `grey-4` | `--o-color-fill2` | rgb(255,255,255) → rgb(36,36,39) |
| 选择器描边 | `grey-14 @ 0.25` | `grey-14 @ 0.25` | `--o-color-control1` | rgba(0,0,0,0.25) → rgba(255,255,255,0.25) |
| 占位文字 | `grey-14 @ 0.4` | `grey-14 @ 0.4` | `--o-color-info4` | rgba(0,0,0,0.4) → rgba(255,255,255,0.4) |
| 下箭头图标 | `grey-14` | `grey-14` | `--o-color-info1` | rgb(0,0,0) → rgb(255,255,255) |

#### Actived 状态

| 区域 | Light 模式 | Dark 模式 | Token | RGB 值 |
|------|-----------|----------|-------|--------|
| 选择器背景 | `grey-1` | `grey-4` | `--o-color-fill2` | rgb(255,255,255) → rgb(36,36,39) |
| 选择器描边 | `brand-6` | `brand-6` | `--o-color-primary1` | rgb(0,47,167) → rgb(110,148,243) |
| 占位文字 | `grey-14 @ 0.4` | `grey-14 @ 0.4` | `--o-color-info4` | rgba(0,0,0,0.4) → rgba(255,255,255,0.4) |
| 下箭头图标 | `grey-14` | `grey-14` | `--o-color-info1` | rgb(0,0,0) → rgb(255,255,255) |

#### Complete 状态

| 区域 | Light 模式 | Dark 模式 | Token | RGB 值 |
|------|-----------|----------|-------|--------|
| 选择器背景 | `grey-1` | `grey-4` | `--o-color-fill2` | rgb(255,255,255) → rgb(36,36,39) |
| 选择器描边 | `grey-14 @ 0.25` | `grey-14 @ 0.25` | `--o-color-control1` | rgba(0,0,0,0.25) → rgba(255,255,255,0.25) |
| 选中文字 | `grey-14` | `grey-14` | `--o-color-info1` | rgb(0,0,0) → rgb(255,255,255) |
| 下箭头图标 | `grey-14` | `grey-14` | `--o-color-info1` | rgb(0,0,0) → rgb(255,255,255) |

#### Actived-menu 状态

| 区域 | Light 模式 | Dark 模式 | Token | RGB 值 |
|------|-----------|----------|-------|--------|
| 选择器描边 | `brand-6` | `brand-6` | `--o-color-primary1` | rgb(0,47,167) → rgb(110,148,243) |
| 上箭头图标 | `grey-14` | `grey-14` | `--o-color-info1` | rgb(0,0,0) → rgb(255,255,255) |
| 菜单背景 | `grey-1` | `grey-4` | `--o-color-fill2` | rgb(255,255,255) → rgb(36,36,39) |
| 菜单项文字 | `grey-14` | `grey-14` | `--o-color-info1` | rgb(0,0,0) → rgb(255,255,255) |
| 菜单项 hover 背景 | — | — | `--o-color-control2-light` | — |

---

### 字体样式映射

| 使用场景 | 字号 Token | 行高 Token | 字重 Token | 字号值 | 行高值 |
|---------|-----------|-----------|-----------|-------|-------|
| large 文字 | `font_size-text1` | `line_height-text1` | `font_weight-regular` | 16px | 24px |
| medium/small 文字 | `font_size-tip1` | `line_height-tip1` | `font_weight-regular` | 14px | 22px |

字体族：`font_family`（HarmonyOS / HarmonyHeiTi Regular）

---

### 组件层级结构

**size=large, type=Enabled, Dark=off**

```
OSelect（FRAME）
│  GUID: 1042:17665
│  Width: 320px | Height: 40px
│  cornerRadius: 4px → Token: `radius_control-xs`
│  fill: rgb(255,255,255) → Token: `--o-color-fill2`（Light）
│  stroke: rgba(0,0,0,0.25) → Token: `--o-color-control1`
│  strokeWeight: 1px | strokeAlign: INSIDE
│
├── [占位文字 PARAGRAPH]（自适应宽度）
│   fontSize: 16px → Token: `font_size-text1`
│   lineHeight: 24px → Token: `line_height-text1`
│   fill: rgba(0,0,0,0.4) → Token: `--o-color-info4`
│   nodeText: "请选择"
│
└── [下箭头图标 Icon/下箭头 INSTANCE]
    Width: 24px | Height: 24px → Token: `icon_size_control-m`
    fill: rgb(0,0,0) → Token: `--o-color-info1`
```

**size=large, type=Actived-menu, Dark=off**

```
OSelect（FRAME）
│  GUID: 1042:17677
│  Width: 320px | Height: 252px
│  stroke: rgba(0,47,167,1) → Token: `--o-color-primary1`
│
├── [选择器头部]（Height: 40px）
│   ├── [占位文字 PARAGRAPH]
│   │   fill: rgba(0,0,0,0.4) → Token: `--o-color-info4`
│   └── [上箭头图标 Icon/上箭头 INSTANCE]（24×24px）
│       fill: rgb(0,0,0) → Token: `--o-color-info1`
│
└── [菜单面板 FRAME]（Height: 212px）
    cornerRadius: 4px
    fill: rgb(255,255,255) → Token: `--o-color-fill2`
    └── [菜单项 × N]（Height: 40px each）
        └── [文字 PARAGRAPH]
            fill: rgb(0,0,0) → Token: `--o-color-info1`
```

**size=medium, type=Complete, Dark=off**

```
OSelect（FRAME）
│  GUID: 1042:17712
│  Width: 320px | Height: 32px
│  cornerRadius: 4px
│  fill: rgb(255,255,255) → Token: `--o-color-fill2`
│  stroke: rgba(0,0,0,0.25) → Token: `--o-color-control1`
│
├── [选中文字 PARAGRAPH]
│   fontSize: 14px → Token: `font_size-tip1`
│   lineHeight: 22px → Token: `line_height-tip1`
│   fill: rgb(0,0,0) → Token: `--o-color-info1`
│
└── [下箭头图标 Icon/下箭头 INSTANCE]（24×24px）
    fill: rgb(0,0,0) → Token: `--o-color-info1`
```

---

### 变体 node_id 速查

| 变体组合 | node_id | 尺寸（示例） | 说明 |
|---------|---------|------------|------|
| size=large, type=Enabled, Dark=off | `1042:17665` | 320×40px | 大尺寸，默认，浅色 |
| size=large, type=Enabled, Dark=on | `1042:17668` | 320×40px | 大尺寸，默认，深色 |
| size=large, type=Complete, Dark=off | `1042:17671` | 320×40px | 大尺寸，已选，浅色 |
| size=large, type=Complete, Dark=on | `1042:17674` | 320×40px | 大尺寸，已选，深色 |
| size=large, type=Actived, Dark=off | `1042:17756` | 320×40px | 大尺寸，聚焦，浅色 |
| size=large, type=Actived, Dark=on | `1042:17759` | 320×40px | 大尺寸，聚焦，深色 |
| size=large, type=Actived-menu, Dark=off | `1042:17677` | 320×252px | 大尺寸，菜单展开，浅色 |
| size=large, type=Actived-menu, Dark=on | `1042:17687` | 320×252px | 大尺寸，菜单展开，深色 |
| size=medium, type=Enabled, Dark=off | `1042:17703` | 320×32px | 中尺寸，默认，浅色 |
| size=medium, type=Enabled, Dark=on | `1042:17709` | 320×32px | 中尺寸，默认，深色 |
| size=medium, type=Complete, Dark=off | `1042:17712` | 320×32px | 中尺寸，已选，浅色 |
| size=medium, type=Complete, Dark=on | `1042:17715` | 320×32px | 中尺寸，已选，深色 |
| size=medium, type=Actived, Dark=off | `1042:17706` | 320×32px | 中尺寸，聚焦，浅色 |
| size=medium, type=Actived, Dark=on | `1042:17762` | 320×32px | 中尺寸，聚焦，深色 |
| size=medium, type=Actived-menu, Dark=off | `1042:17718` | 320×194px | 中尺寸，菜单展开，浅色 |
| size=medium, type=Actived-menu, Dark=on | `1042:17728` | 320×194px | 中尺寸，菜单展开，深色 |
| size=small, type=Enabled, Dark=off | `1042:17744` | 320×40px | 移动端，默认，浅色 |
| size=small, type=Enabled, Dark=on | `1042:17747` | 320×40px | 移动端，默认，深色 |
| size=small, type=Complete, Dark=off | `1042:17750` | 320×40px | 移动端，已选，浅色 |
| size=small, type=Complete, Dark=on | `1042:17753` | 320×40px | 移动端，已选，深色 |
| OSelect-Dropdown（5 项 × large，Light） | `1042:17681` | 320×208px | 独立浮层，与 Actived 触发器配对使用，间距 4px |

---

### Pixso 操作速查

1. **插入组件**：组件面板搜索「OSelect」，拖入画布
2. **切换尺寸**：右侧面板 → size 属性 → 选择 `large` / `medium` / `small`
3. **切换状态**：右侧面板 → type 属性 → 选择 `Enabled` / `Actived` / `Complete` / `Actived-menu`
4. **切换主题**：右侧面板 → Dark 属性 → 选择 `off` / `on`
5. **修改宽度**：手动调整 FRAME 的 width 值
6. **修改文字**：双击进入组件 → 双击文字图层修改占位文字或选中值
7. **展示下拉**：切换 type 为 `Actived-menu` 可展示下拉菜单展开效果

---

## Part C：交互状态详细说明

### 一、默认态（Default）

#### 单选选择器

| 区域 | 属性 | Token | 说明 |
|------|------|-------|------|
| 选择器描边 | 颜色 | `--o-color-control1` | 默认描边颜色 |
| 选择器背景 | 颜色 | `--o-color-fill2` | 背景填充 |
| 占位符文字 | 字号 | 16px (`font_size-text1`) | large 尺寸 |
| 占位符文字 | 字重 | Regular (`font_weight-regular`) | — |
| 占位符文字 | 颜色 | `--o-color-info4` | 占位提示文字 |
| 已选文字 | 字号 | 16px (`font_size-text1`) | large 尺寸 |
| 已选文字 | 字重 | Regular (`font_weight-regular`) | — |
| 已选文字 | 颜色 | `--o-color-info1` | 实际选中值 |

#### 多选选择器

| 区域 | 属性 | Token | 说明 |
|------|------|-------|------|
| 选择器描边 | 颜色 | `--o-color-control1` | 默认描边颜色 |
| 选择器背景 | 颜色 | `--o-color-fill2` | 背景填充 |
| 占位符文字 | 字号 | 16px (`font_size-text1`) | large 尺寸 |
| 占位符文字 | 颜色 | `--o-color-info4` | 占位提示文字 |
| 标签文字 | 字号 | 12px (`font_size-tip2`) | 已选标签文字 |
| 标签文字 | 字重 | Regular (`font_weight-regular`) | — |
| 标签文字 | 颜色 | `--o-color-info1` | 标签文字颜色 |

---

### 二、悬浮态（Hover）

#### 单选选择器

| 区域 | 属性 | Token | 说明 |
|------|------|-------|------|
| 选择器描边 | 颜色 | `--o-color-control2` | 悬浮时描边加深 |
| 选择器背景 | 颜色 | `--o-color-fill2` | 背景不变 |
| 文字 | 字号 | 16px (`font_size-text1`) | large 尺寸 |
| 文字 | 颜色 | `--o-color-info1` | 文字颜色不变 |

#### 多选选择器

| 区域 | 属性 | Token | 说明 |
|------|------|-------|------|
| 选择器描边 | 颜色 | `--o-color-control2` | 悬浮时描边加深 |
| 选择器背景 | 颜色 | `--o-color-fill2` | 背景不变 |
| 标签文字 | 字号 | 12px (`font_size-tip2`) | 标签文字 |
| 标签文字 | 颜色 | `--o-color-info1` | 标签文字颜色不变 |

---

### 三、激活态-展开菜单（Active/Expanded）

> **说明**：下拉面板规格严格遵循 **ODropdown** 和 **OSearch** 组件的下拉面板规范，确保跨组件视觉一致性。

#### 触发器区域

| 区域 | 属性 | Token | 说明 |
|------|------|-------|------|
| 选择器描边 | 颜色 | `--o-color-control3` | 聚焦描边 |
| 选择器背景 | 颜色 | `--o-color-fill2` | 背景不变 |
| 占位符/文字 | 字号 | 16px (`font_size-text1`) | large 尺寸 |
| 占位符文字 | 颜色 | `--o-color-info4` | 占位提示 |
| 已选文字 | 颜色 | `--o-color-info1` | 选中值 |

#### 下拉面板容器（参考 ODropdown/OSearch）

> 📐 **面板布局规格**：与 ODropdown 下拉面板、OSearch 搜索建议面板完全一致。

```
OSelect-Dropdown 浮层（FRAME）
├── 与触发器间距: 4px（垂直方向）
├── Width: 与触发器同宽（自适应）
├── Height: 4px（上内边距）+ N × 40px（large）/ N × 32px（medium）+ 0px（项间距）+ 4px（下内边距）
├── cornerRadius: 4px → Token: `radius_control-xs`
├── fill: rgb(255,255,255) → Token: `--o-color-fill2`（Light）/ rgb(36,36,39)（Dark）
├── boxShadow: DROP_SHADOW x=0 y=6 blur=24 spread=0 rgba(18,20,23,0.08)
├── padding: 4px（四周）
└── autoLayout: VERTICAL，子项自适应宽度
```

**面板详细规格表**

| 规格项 | large（PC） | medium（PC） | Token |
|--------|------------|-------------|-------|
| **与触发器间距** | 4px | 4px | — |
| **面板圆角** | 4px | 4px | `radius_control-xs` |
| **面板内边距（四周）** | 4px | 4px | — |
| **面板背景（Light）** | rgb(255,255,255) | rgb(255,255,255) | `--o-color-fill2` |
| **面板背景（Dark）** | rgb(36,36,39) | rgb(36,36,39) | `--o-color-fill2` (Dark值) |
| **面板阴影** | DROP_SHADOW | DROP_SHADOW | x=0 y=6 blur=24 spread=0 rgba(18,20,23,0.08) |

#### 下拉菜单项（参考 ODropdown/OSearch）

> 📐 **菜单项规格**：与 ODropdown 菜单项、OSearch 建议项完全一致。
> 
> ⚠️ **重要规范**：Hover 和 Selected 的背景色块必须**上下各缩 1px 放量**，避免相邻项粘连。

```
菜单项 × N（Height: 40px large / 32px medium）
├── padding: 上下 8px，左右 12px
├── item spacing: 0px（相邻菜单项无间距）
├── cursor: pointer
│
├── [文字 PARAGRAPH / 复选框 + 文字]
│    fontSize: 16px (large) / 14px (medium)
│    fontWeight: Regular (`font_weight-regular`)
│    color: --o-color-info1（默认态）
│
└── [背景色块 ::before 伪元素] ← 关键实现细节
     position: absolute
     top: 1px              ← 上缩 1px
     left: 0
     right: 0
     bottom: 1px           ← 下缩 1px
     borderRadius: 2px
     z-index: -1           ← 在内容层下方
     
     Hover 态:
       background: --o-color-control2-light  （浅色提示）
       
     Selected 态:
       background: --o-color-control3-light  （深色强调）
```

**菜单项详细规格表**

| 规格项 | large（PC） | medium（PC） | Token | 说明 |
|--------|------------|-------------|-------|------|
| **菜单项高度** | 40px | 32px | `control_size-l` / `control_size-m` | 整体高度 |
| **菜单项内边距（上/下）** | 8px | 8px | — | 内容区内边距 |
| **菜单项内边距（左/右）** | 12px | 12px | — | 内容区左右边距 |
| **菜单项间距（item spacing）** | 0px | 0px | — | 相邻项无间距 |
| **菜单项字号** | 16px | 14px | `font_size-text1` / `font_size-tip1` | 文字大小 |
| **菜单项字重** | Regular | Regular | `font_weight-regular` | 默认字重 |
| **默认文字颜色** | `--o-color-info1` | `--o-color-info1` | rgb(0,0,0) / rgb(255,255,255) | 正常文字 |
| **Hover 背景色** | `--o-color-control2-light` | `--o-color-control2-light` | #E8F0FE (Light) / rgba(110,148,243,0.15) (Dark) | 浅色提示背景 |
| **Selected 背景色** | `--o-color-control3-light` | `--o-color-control3-light` | #D2E3FC (Light) / rgba(110,148,243,0.25) (Dark) | 深色强调背景 |
| **背景色块圆角** | 2px | 2px | — | 圆角半径 |
| **背景色块缩放** | 上下各缩 1px | 上下各缩 1px | — | 防止粘连的关键 |
| **选中项文字颜色** | `--o-color-primary1` | `--o-color-primary1` | brand-6 主色调 | 品牌蓝色高亮 |

#### 菜单项交互状态详情

| 状态 | 背景 Token | Light 模式值 | Dark 模式值 | 文字颜色 | 特殊效果 |
|------|-----------|-------------|-------------|---------|---------|
| **默认态** | transparent | transparent | transparent | `--o-color-info1` | 正常显示 |
| **Hover 态** | `--o-color-control2-light` | #E8F0FE | rgba(110,148,243,0.15) | `--o-color-info1` | 浅色背景高亮，圆角 2px |
| **选中态** | `--o-color-control3-light` | #D2E3FC | rgba(110,148,243,0.25) | `--o-color-primary1` | 深色背景强调 + 文字品牌蓝 |
| **当前聚焦** | `--o-color-control2-light` | #E8F0FE | rgba(110,148,243,0.15) | `--o-color-info1` | 键盘导航时浅色高亮 |

> **⚠️ 视觉层级说明**：
> - **Hover 态**：使用 `--o-color-control2-light`（较浅），表示"可被选中"的提示状态
> - **Selected 态**：使用 `--o-color-control3-light`（较深），表示"已选中"的确认状态
> - 两种状态有明显视觉差异，避免用户混淆

##### 多选菜单项特殊状态

| 状态 | 属性 | Token | Light 值 | Dark 值 | 说明 |
|------|------|-------|----------|---------|------|
| 复选框-选中 | 图标 | 勾选图标（✓） | — | — | 已选项标识 |
| 复选框-未选中 | 图标 | 空白方框（□） | — | — | 未选项标识 |
| 选中项背景 | 颜色 | `--o-color-control3-light` | #D2E3FC | rgba(110,148,243,0.25) | 多选深色选中背景 |
| Hover 未选中项背景 | 颜色 | `--o-color-control2-light` | #E8F0FE | rgba(110,148,243,0.15) | 悬停浅色背景 |
| 复选框与文字间距 | 间距 | 8px | — | — | 图标与文字的水平间距 |

---

### 四、激活态-可搜索（Active/Searchable）

> **说明**：可搜索选择器的下拉面板规格同样严格遵循 **OSearch** 搜索建议面板规范。

#### 触发器区域

| 区域 | 属性 | Token | 说明 |
|------|------|-------|------|
| 选择器描边 | 颜色 | `--o-color-control3` | 聚焦描边 |
| 输入框背景 | 颜色 | `--o-color-fill2` | 输入区域背景 |
| 输入文字 | 字号 | 16px (`font_size-text1`) | large 尺寸 |
| 输入文字 | 颜色 | `--o-color-info1` | 用户输入文字 |

#### 搜索下拉面板容器（参考 OSearch）

> 📐 **面板布局规格**：与 OSearch 搜索建议面板完全一致。

```
可搜索 Select 下拉面板（FRAME）
├── 与触发器间距: 4px（垂直方向）
├── Width: 与触发器同宽（自适应）
├── Height: 4px（上内边距）+ N × 40px（菜单项）+ 0px（项间距）+ 4px（下内边距）
├── cornerRadius: 4px → Token: `radius_control-xs`
├── fill: rgb(255,255,255) → Token: `--o-color-fill2`（Light）/ rgb(36,36,39)（Dark）
├── boxShadow: DROP_SHADOW x=0 y=6 blur=24 spread=0 rgba(18,20,23,0.08)
├── padding: 4px（四周）
└── autoLayout: VERTICAL，子项自适应宽度
```

**面板详细规格表**

| 规格项 | 值 | Token | 说明 |
|--------|-----|-------|------|
| **与触发元素间距** | 4px | — | 垂直方向间距 |
| **面板圆角** | 4px | `radius_control-xs` | 统一圆角 |
| **面板内边距** | 4px（四周） | — | 内边距 |
| **面板背景（Light）** | rgb(255,255,255) | `--o-color-fill2` | 浅色模式 |
| **面板背景（Dark）** | rgb(36,36,39) | `--o-color-fill2` (Dark值) | 深色模式 |
| **面板阴影** | DROP_SHADOW | — | x=0 y=6 blur=24 spread=0 rgba(18,20,23,0.08) |

#### 搜索下拉菜单项（参考 OSearch）

> 📐 **菜单项规格**：与 OSearch 搜索建议项完全一致，支持关键词高亮。

```
菜单项 × N（Height: 40px = control_size-l）
├── padding: 上下 8px，左右 12px
├── item spacing: 0px
├── cursor: pointer
│
├── [匹配关键词 SPAN] ← 与输入内容匹配的部分
│    fontSize: 16px
│    fontWeight: 600（Semibold）
│    color: --o-color-primary1（rgb(0,47,167) 品牌蓝）
│
└── [补充文本] ← 选项的其他内容
     fontSize: 16px
     fontWeight: 400（Regular）
     color: --o-color-info2（rgba(0,0,0,0.8) 80%黑）
     
└── [Hover 态背景]
     background: --o-color-control2-light (#E8F0FE 极浅蓝)
     borderRadius: 2px
```

**菜单项详细规格表**

| 规格项 | 值 | Token | Light 值 | Dark 值 | 说明 |
|--------|-----|-------|----------|---------|------|
| **菜单项高度** | 40px | `control_size-l` | 40px | 40px | 统一高度 |
| **菜单项内边距** | 上下 8px，左右 12px | — | 同左 | 同左 | 内边距 |
| **菜单项间距** | 0px | — | 0px | 0px | 无间距 |
| **匹配关键词字重** | 600 (Semibold) | — | Semibold | Semibold | 粗体高亮 |
| **匹配关键词颜色** | rgb(0,47,167) | `--o-color-primary1` | brand-6 | brand-6 Dark | 品牌蓝色 |
| **非匹配文本字重** | 400 (Regular) | — | Regular | Regular | 常规字体 |
| **非匹配文本颜色** | rgba(0,0,0,0.8) | `--o-color-info2` | 80%黑 | 80%白 | 次要文字 |
| **Hover 背景色** | 浅蓝 | `--o-color-control2-light` | #E8F0FE | rgba(110,148,243,0.15) | 浅色提示背景 |
| **Selected 背景色** | 中蓝 | `--o-color-control3-light` | #D2E3FC | rgba(110,148,243,0.25) | 深色强调背景 |
| **Hover/Selected 圆角** | 2px | — | 2px | 2px | 小圆角（上下缩1px后） |
| **选中项文字颜色** | 品牌蓝 | `--o-color-primary1` | brand-6 | brand-6 Dark | 文字高亮 |

> **⚠️ 背景色块规范**：与单选选择器一致，Hover 和 Selected 背景色块必须**上下各缩 1px 放量**

> **⚠️ 关键词高亮逻辑（硬约束）**：
> - 输入框中的文本内容必须在选项列表中**高亮显示**
> - 匹配部分使用 `--o-color-primary1` (蓝色) + **Semibold** 字重
> - 非匹配部分使用 `--o-color-info2` (80%黑) + **Regular** 字重
> - 示例：输入 "选项" → "**选项**一" （"选项" 蓝色粗体，"一" 黑色常规）

---

### 五、禁用态（Disabled）

| 区域 | 属性 | Token | 说明 |
|------|------|-------|------|
| 选择器描边 | 颜色 | `--o-color-control4-light` | 禁用描边（浅色） |
| 选择器背景 | 颜色 | `--o-color-fill2` | 背景不变 |
| 文字/占位符 | 字号 | 16px (`font_size-text1`) | large 尺寸 |
| 文字/占位符 | 颜色 | `--o-color-info4` | 禁用文字颜色 |
| 占位符文字颜色 | 颜色 | `--o-color-info4` | 提示文字 |
| 箭头图标 | 颜色 | `--o-color-control4-light` | 禁用图标颜色 |

> **注意**：禁用状态下选择器不可交互，鼠标悬停无反馈，点击无效。

---

### 六、加载态（Loading）

| 区域 | 属性 | Token | 说明 |
|------|------|-------|------|
| 选择器描边 | 颜色 | `--o-color-control3` | 同激活态 |
| 选择器背景 | 颜色 | `--o-color-fill2` | 背景不变 |
| 加载图标 | 类型 | Spinner | 旋转加载动画 |
| 加载图标位置 | 位置 | 右侧 | 替代箭头图标位置 |
| 文字/占位符 | 颜色 | `--o-color-info1` / `--o-color-info4` | 根据状态显示 |

> **注意**：加载态通常出现在异步加载选项数据时，加载完成后切换至正常状态。

---

### 七、交互状态转换流程

```
默认态（Default）
    ↓ [鼠标进入]
悬浮态（Hover）
    ↓ [鼠标离开]
默认态（Default）
    ↓ [点击/聚焦]
激活态-展开（Active/Expanded）
    ↓ [选择选项]
完成态（Complete）→ 默认态（显示选中值）
    ↓ [再次点击]
激活态-展开（Active/Expanded）
    ↓ [按 Esc / 点击外部]
默认态（Default）

特殊情况：
├── 禁用态（Disabled）：不可交互，保持在禁用状态
└── 加载态（Loading）：自动过渡，加载完成后变为默认/激活态
```

---

### 八、多选选择器特殊交互

#### 标签显示规则

| 状态 | 显示内容 | 行为 |
|------|---------|------|
| 未选择 | 占位符文字 | 显示 "请选择" 提示 |
| 已选 1 项 | 1 个标签 | 显示单个选项标签 |
| 已选 N 项 | N 个标签 | 标签横向排列，超出隐藏或折叠显示 |

#### 标签样式

| 属性 | Token | 说明 |
|------|-------|------|
| 标签背景 | `--o-color-control2-light` | 标签背景色 |
| 标签文字 | `--o-color-info1` | 标签文字颜色 |
| 标签文字字号 | 12px (`font_size-tip2`) | 小字号 |
| 关闭按钮 | × 图标 | 可删除已选项 |
| 标签圆角 | 2px (`radius_control-xs`) | 小圆角 |

---

### 九、可搜索选择器特殊交互

#### 搜索输入框

| 属性 | Token | 说明 |
|------|-------|------|
| 输入框背景 | `--o-color-fill2` | 透明背景 |
| 输入框描边 | 无 | 无额外描边 |
| 输入文字 | `--o-color-info1` | 用户输入文字 |
| 占位符文字 | `--o-color-info4` | "请搜索" 提示 |

#### 搜索过滤行为

1. **实时过滤**：输入即时过滤选项列表
2. **匹配高亮**：匹配文字高亮显示（可选）
3. **无结果提示**：过滤后无匹配项时显示 "无匹配结果"
4. **清除按钮**：输入内容后显示清除按钮（×）
5. **键盘导航**：支持上下键选择，Enter 确认，Esc 关闭

---

### 十、颜色变量速查表（o-color-*）

> **⚠️ 2026-07-15 更新**：优化菜单项 Hover/Selected 背景色区分，新增 `--o-color-control3-light` 变量

#### 选择器主体状态

| 状态 | 描边 Token | 背景 Token | 文字 Token | 说明 |
|------|-----------|-----------|-----------|------|
| **默认态** | `--o-color-control1` | `--o-color-fill2` | `--o-color-info1` / `--o-color-info4` | 正常显示（占位符/已选值） |
| **悬浮态** | `--o-color-control2` | `--o-color-fill2` | `--o-color-info1` | 鼠标悬停选择器 |
| **激活态** | `--o-color-control3` | `--o-color-fill2` | `--o-color-info1` | 聚焦/展开下拉 |
| **禁用态** | `--o-color-control4-light` | `--o-color-fill2` | `--o-color-info4` | 不可交互 |

#### 下拉菜单项状态（关键更新）

| 状态 | 背景 Token | Light 模式值 | Dark 模式值 | 文字 Token | 说明 |
|------|-----------|-------------|-------------|-----------|------|
| **默认态** | transparent | transparent | transparent | `--o-color-info1` | 正常显示 |
| **Hover 态** | `--o-color-control2-light` | #E8F0FE | rgba(110,148,243,0.15) | `--o-color-info1` | 浅色提示背景 |
| **Selected 态** | `--o-color-control3-light` | #D2E3FC | rgba(110,148,243,0.25) | `--o-color-primary1` | 深色强调背景 + 品牌蓝文字 |

#### 多选特殊状态

| 元素 | 背景 Token | Light 值 | Dark 值 | 说明 |
|------|-----------|----------|---------|------|
| **标签背景** | `--o-color-control2-light` | #E8F0FE | rgba(110,148,243,0.15) | 已选标签背景 |
| **多选选中项** | `--o-color-control3-light` | #D2E3FC | rgba(110,148,243,0.25) | 复选框选中项深色背景 |

#### 背景色块实现规范

> **🔧 技术实现要求**：
> - 使用 CSS `::before` 伪元素实现背景色块
> - 色块位置：`top: 1px; bottom: 1px; left: 0; right: 0`
> - 色块圆角：`border-radius: 2px`
> - 层级控制：`z-index: -1`（在文字内容下方）
> - 目的：确保相邻菜单项的背景不粘连，保持 2px 间隙

---

## Part D：注意事项与最佳实践

### 注意事项

- **Actived-menu 是示意变体**：将触发器（Actived 态）+ 菜单面板合为一体，仅用于设计稿展示；实际开发中菜单为浮层，触发器高度始终为 40px/32px
- **small 尺寸限制**：`small`（移动端）仅有 `Enabled` 和 `Complete`，无 `Actived` / `Actived-menu` 变体
- **无 Disabled 状态**：OSelect 无 Disabled 变体（注：根据最新规范，已补充禁用态定义）
- **箭头图标方向**：Enabled/Actived/Complete 使用「下箭头」，Actived-menu 使用「上箭头」
- **描边样式**：描边宽度 1px INSIDE，不影响选择器整体尺寸
- **选择器高度**：large=40px，medium=32px，small=40px（移动端同 large 高度）
- **文字字号**：large=16px，medium/small=14px
- **圆角统一**：所有尺寸圆角均为 4px（`radius_control-xs`）
- **背景颜色**：Light=grey-1（white），Dark=grey-4
- **描边颜色**：Enabled/Complete=grey-14@0.25（`--o-color-control1`），Actived/Actived-menu=brand-6（`--o-color-primary1`）
- **⚠️ 菜单项背景色块规范（2026-07-15 更新）**：
  - Hover 态使用 `--o-color-control2-light`（浅色提示）：Light=#E8F0FE, Dark=rgba(110,148,243,0.15)
  - Selected 态使用 `--o-color-control3-light`（深色强调）：Light=#D2E3FC, Dark=rgba(110,148,243,0.25)
  - 背景色块必须**上下各缩 1px 放量**（使用 `::before` 伪元素实现）
  - 目的：避免相邻菜单项背景粘连，保持视觉清晰度
  - 实现方式：`position: absolute; top: 1px; bottom: 1px; z-index: -1`

### 最佳实践

1. **交互一致性**：确保所有状态的过渡平滑，避免突兀的视觉跳变
2. **可访问性**：提供清晰的焦点指示器（Focus Indicator），支持键盘导航
3. **性能优化**：大量选项时使用虚拟滚动（Virtual Scroll），避免渲染性能问题
4. **响应式适配**：移动端考虑触摸区域大小，适当增大点击热区
5. **国际化**：支持 RTL（从右到左）布局，文字方向自适应
6. **背景色块实现**：使用 CSS `::before` 伪元素而非直接 background 属性，确保缩放规范正确执行
- **占位文字颜色**：`--o-color-info4`（grey-14@0.4），选中值颜色：`--o-color-info1`（grey-14）
- **Actived-menu 总高**：包含选择器头部 + 菜单面板（large=252px，medium=194px）
- **宽度自适应**：选择器宽度随容器自适应，示例宽度 320px
- **尺寸用途**：large/medium 用于 PC 端，small 用于移动端

---

## Part C：交互与状态

### 交互状态

| 元素 | 状态 | 视觉表现 |
|------|------|---------|
| 选择器 | Enabled | 背景 `--o-color-fill2`，描边 `--o-color-control1`，占位文字 `--o-color-info4`，下箭头 |
| 选择器 | Actived | 背景 `--o-color-fill2`，描边 `--o-color-primary1`，占位文字 `--o-color-info4`，下箭头 |
| 选择器 | Complete | 背景 `--o-color-fill2`，描边 `--o-color-control1`，选中值 `--o-color-info1`，下箭头 |
| 选择器 | Actived-menu | 背景 `--o-color-fill2`，描边 `--o-color-primary1`，上箭头，菜单展开 |
| 下箭头图标 | Enabled / Actived / Complete | fill：`--o-color-info1`（grey-14），24×24px |
| 上箭头图标 | Actived-menu | fill：`--o-color-info1`（grey-14），24×24px |
| 菜单项 | 默认 | 文字 `--o-color-info1`，背景透明 |
| 菜单项 | Hover | 背景 `--o-color-control2-light` |

---

### 状态切换逻辑

> `Actived-menu` 是设计稿示意变体，不参与真实交互流转，下列描述对应真实的触发器状态变化。

- **Enabled → Actived**：点击 / hover 选择器，描边颜色切换为 `--o-color-primary1`（brand-6）
- **Actived → Complete**：从下拉菜单选中一个选项，菜单收起，触发器显示选中值，描边恢复为 `--o-color-control1`
- **Complete → Actived**：再次点击，触发器重新进入 Actived 态（下拉菜单重新展开）
- **Light → Dark**：背景色切换 grey-1 → grey-4，描边和文字颜色同步切换

**设计稿使用 Actived-menu**：在原型 / 交互说明中，切换 type 为 `Actived-menu` 即可展示完整展开效果，无需手动拼合触发器与菜单面板。

**实际交互组合（OSelect-Dropdown）**：实现完整展开层时，使用 `Actived` 状态的触发器 + `OSelect-Dropdown`（独立浮层，间距 4px），两者垂直对齐，浮层宽度与触发器一致。

---

### Hover 状态

| 元素 | Hover 视觉表现 |
|------|---------------|
| 选择器（Enabled / Complete） | 描边颜色切换为 `--o-color-control2` |
| 选择器（Actived / Actived-menu） | 描边颜色不变 |
| 菜单项 | 背景 `--o-color-control2-light` |

---

### Focus 状态

Focus 状态即 Actived 状态，描边切换为 `--o-color-primary1`。

---

## Part D：设计变量绑定

### 推荐绑定变量

| 元素 | 属性 | 推荐变量 Token |
|------|------|---------------|
| 选择器背景 | fill | `--o-color-fill2` |
| 选择器描边（Enabled/Complete） | stroke | `--o-color-control1` |
| 选择器描边（Actived/Actived-menu） | stroke | `--o-color-primary1` |
| 占位文字 | fill | `--o-color-info4` |
| 选中文字 | fill | `--o-color-info1` |
| 箭头图标 | fill | `--o-color-info1` |
| 箭头图标尺寸 | width/height | `icon_size_control-m`（24px） |
| 菜单背景 | fill | `--o-color-fill2` |
| 菜单项文字 | fill | `--o-color-info1` |
| 菜单项 hover 背景 | fill | `--o-color-control2-light` |
| 选择器高度（large/small） | height | `control_size-l`（40px） |
| 选择器高度（medium） | height | `control_size-m`（32px） |
| 选择器圆角 | cornerRadius | `radius_control-xs`（4px） |
| 文字字号（large） | fontSize | `font_size-text1`（16px） |
| 文字字号（medium/small） | fontSize | `font_size-tip1`（14px） |
| 文字行高（large） | lineHeight | `line_height-text1`（24px） |
| 文字行高（medium/small） | lineHeight | `line_height-tip1`（22px） |
| 描边宽度 | strokeWeight | 1px INSIDE |

---

## Part E：最佳实践

### 使用建议

1. **尺寸选择**：PC 端默认使用 `medium`（32px），重要选择项使用 `large`（40px）；移动端使用 `small`（40px）
2. **状态演示**：原型演示时，通过 `Actived-menu` 变体展示下拉菜单展开效果
3. **占位文字**：占位文字建议使用"请选择"或具体的指引，如"请选择城市"
4. **宽度设置**：选择器宽度应与同行表单元素对齐，PC 端建议 160-320px，移动端全宽

### 设计提示

- 选择器高度：large=40px，medium=32px，small=40px（移动端）
- 文字字号：large=16px，medium/small=14px
- 箭头图标：所有尺寸统一 24×24px
- Actived-menu 展开总高：large=252px，medium=194px
- 描边样式为 1px INSIDE，不影响整体尺寸
- 深色模式下 Actived 描边颜色为 rgb(110,148,243)（brand-6 Dark 值）
- `small` 尺寸（移动端）仅支持 Enabled 和 Complete，无展开菜单变体

---

## Part F：Assets 图标资源

> 路径：`references/assets/public icons/`

| 文件名 | 语义 | 结构性质 |
|--------|------|---------|
| `icon-下箭头.svg` | 下拉展开指示图标 | **固定结构**，与 `icon-上箭头.svg` 成对使用 |
| `icon-上箭头.svg` | 收起指示图标 | 与 `icon-下箭头.svg` 成对使用 |

**使用逻辑**
- **菜单收起**（Enabled / Actived / Complete）：显示 `icon-下箭头.svg`，表示下拉展开
- **菜单展开**（Actived-menu）：切换为 `icon-上箭头.svg`，表示收起

**图标颜色与尺寸规则**

颜色和尺寸完全跟随 OSelect 组件内部定义，**不单独设置，不随交互状态变化**：

| 尺寸（所有 size） | 图标尺寸 | Token |
|-----------------|---------|-------|
| large / medium / small | 24×24px | `icon_size_control-m` |

| state | 使用图标 | 图标颜色 Token |
|-------|---------|--------------|
| Enabled / Actived / Complete | `icon-下箭头.svg` | `--o-color-info1`（grey-14） |
| Actived-menu | `icon-上箭头.svg` | `--o-color-info1`（grey-14） |

> 图标颜色与尺寸由 OSelect 组件统一定义，在所有 type 和 size 下保持不变。深色模式下同名 Token 自动切换为深色值（rgb(255,255,255)）。

---

## 技术备注

- 组件节点 ID：`1042:17664`（组件集）
- 设计稿 URL：https://pixso.cn/app/design/JZkjW0mhmT61Mtd98dCfBw?item-id=1042:17664
- 生成日期：2026-06-02
- 变体总数：20（size=large/medium × type=Enabled/Actived/Complete/Actived-menu × Dark=off/on 各 8；size=small × type=Enabled/Complete × Dark=off/on 共 4）
- small 尺寸无 Actived / Actived-menu 变体
- 无 Disabled 状态
- 尺寸数据：large=320×40px，medium=320×32px，small=320×40px（示例，宽度自适应）
- Actived-menu 总高：large=252px，medium=194px
- 菜单子组件：`OSelect 选择器/large/Menu/off`（1042:17697，148×40px）、`OSelect 选择器/medium/Menu/off`（1042:17738，112×32px）
- OSelect-Dropdown 独立浮层节点：`1042:17681`（320×208px，示例 5 项 × large，Light）；浮层圆角 4px，内边距 4px，阴影 DROP_SHADOW x=0 y=6 blur=24 rgba(18,20,23,0.08)；与 Actived 触发器间距 4px
- 圆角：4px（radius_control-xs）
- 描边：1px INSIDE
- 文字字号：large=16px（font_size-text1），medium/small=14px（font_size-tip1）
- 背景：Light=grey-1（white），Dark=grey-4（color-fill2）
- 描边：Enabled/Complete=grey-14@0.25（color-control1），Actived/Actived-menu=brand-6（color-primary1）
- 箭头图标：所有尺寸 24×24px（icon_size_control-m），Enabled/Actived/Complete=下箭头，Actived-menu=上箭头
- 尺寸用途：large/medium 用于 PC 端，small 用于移动端