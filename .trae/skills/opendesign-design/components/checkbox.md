> ← [组件索引](../SKILL.md#组件索引) · [README](../README.md)

# OCheckbox 复选框 · 设计 Skill

> 组件集合节点：`1042:17361` · 组件名：OCheckbox 复选框 · 变体总数：12

---

## Part A：设计使用卡

### 组件概览

**OCheckbox 复选框**：用于在多个选项中选择多个选项的表单控件（非互斥）。通过方形选择器（带勾号或减号）传达选中/未选中/半选状态，支持禁用状态表达不可操作。可单独使用或以复选框组形式使用，同一组内可选中多个选项。

---

### 适用场景

- ✅ **表单多选**：选择多个选项，如兴趣爱好、服务条款同意、附件选择
- ✅ **列表批量操作**：表格行选择、批量删除、批量下载
- ✅ **设置面板**：多选项配置，如通知设置、显示选项
- ✅ **权限选择**：权限项勾选，多权限组合
- ❌ **不适合**：单选场景（用 ORadio）、开关切换（用 Switch）、二元状态快速切换

---

### 变体说明

**check（选中状态）**
- `Selected` — 已选中，框体使用品牌色填充，显示勾号图标（✓）
- `Unselected` — 未选中，框体使用描边样式，无勾号
- `indeterminate` — 半选状态（部分选中），框体使用品牌色填充，显示减号图标（—）

**state（交互状态）**
- `Enabled` — 正常可点击状态
- `Disabled` — 颜色使用禁用色 Token，不可点击

**Dark（主题）**
- `off` — 浅色模式（默认）
- `on` — 深色模式，品牌色系和背景色自动切换为对应深色值

---

### 布局结构

> 🧩 **布局结构**：复选框水平排列（HORIZONTAL Auto Layout），由方形选择器和文字标签组成。选择器与文字间距 8px，整体高度 24px。

```
OCheckbox（HORIZONTAL，自适应宽度，固定高度 24px）
├── autoLayoutItemSpacing: 8px（选择器↔文字间距）
├── autoLayoutPadding: 0px（上下左右）
│
├── [组合 方形选择器 GROUP]（16×16）
│   ├── [框体 RECTANGLE]（16×16，cornerRadius: 4px）
│   │     fill: 选中/半选=brand-6 / 未选中=white(grey-1)
│   │     stroke: 未选中=grey-14@0.25 / 选中/半选=无
│   └── [勾号/减号图标]（仅选中/半选状态显示）
│         Selected: 勾号（✓），fill: white
│         Indeterminate: 减号（—），fill: white
│
└── [text PARAGRAPH]（自适应宽度，固定高度 24px）
      fill: Enabled=grey-14 / Disabled=grey-14@0.4
      font: 16px / 24px，Regular
```

---

### 组合搭配

> 🔗 **常见搭配**：
> - **复选框组 OCheckboxGroup**：多个 OCheckbox 组合使用，同一组内可多选
> - **表单布局**：与 OInput、OSelect 等表单组件配合，垂直排列
> - **表格行选择**：与 ODataTable 配合，左侧复选框列用于批量选择
> - **权限配置**：与权限树或列表配合，多权限勾选

---

### 设计稿识别指南

> 🔍 **识别特征**：
> - 方形选择器节点（RECTANGLE，16×16px，cornerRadius: 4px），位于文字左侧
> - 选中状态：框体品牌色填充 + 白色勾号图标（约 10×10px）
> - 半选状态：框体品牌色填充 + 白色减号图标（8×8px）
> - 未选中状态：框体白色填充 + 灰色描边（strokeWeight: 1px INSIDE）
> - 整体高度固定 24px，Auto Layout spacing 8px
> - 文字字号 16px，行高 24px，Regular 字重

> 🔄 **易混淆组件**：
> - 与 **ORadio 单选框**：Checkbox 是方形多选，Radio 是圆形单选；Checkbox 选中显示勾号，Radio 选中显示内圈点
> - 与 **OSwitch 开关**：Switch 是滑动开关，用于二元状态快速切换；Checkbox 是选择控件，支持多选和半选

---

### 响应式行为

参照 [栅格规范](../SKILL.md#数据资源) 中的断点定义，OCheckbox 无自动断点响应式行为。选中状态和主题通过变体手动切换。

---

## Part B：规格速查参考

### 变体索引表

| 变体属性 | 可选值 | 默认值 | 视觉差异 |
|---------|--------|--------|---------|
| check | `Selected` / `Unselected` / `indeterminate` | `Unselected` | 框体填充样式 + 勾号/减号/无图标 |
| state | `Enabled` / `Disabled` | `Enabled` | 颜色切换为禁用 Token，不可交互 |
| Dark | `off` / `on` | `off` | 品牌色/背景色切换为深色主题对应值 |

---

### 布局规格

| 规格项 | 值 | Token |
|--------|---|-------|
| 整体高度 | 24px | — |
| 整体宽度 | 自适应（文字宽度 + 24px） | — |
| Auto Layout Spacing | 8px | `gap-2` |
| Auto Layout Padding | 0px（上下左右） | — |
| 框体尺寸 | 16×16px | `icon_size_control-xs` |
| 框体圆角 | 4px | `radius_control-xs` |
| 勾号图标尺寸 | 约 10×10px | — |
| 减号图标尺寸 | 8×8px | — |
| 文字高度 | 24px | `line_height-text1` |
| 文字字号 | 16px | `font_size-text1` |

---

### 颜色 Token 映射

#### Unselected 状态

| 区域 | Light 模式 | Dark 模式 | Token | RGB 值 |
|------|-----------|----------|-------|--------|
| 框体背景（Enabled） | `grey-1` | `grey-4` | `--o-color-fill2` | rgb(255,255,255) → rgb(36,36,39) |
| 框体描边（Enabled） | `grey-14 @ 0.25` | `grey-14 @ 0.25` | `--o-color-control1` | rgba(0,0,0,0.25) → rgba(255,255,255,0.25) |
| 框体背景（Disabled Light） | `brand-1` | — | `--o-color-control2-light` | rgb(235,241,250) |
| 框体背景（Disabled Dark） | — | `grey-5` | `--o-color-control2-light` | rgb(43,43,47) |
| 框体描边（Disabled） | `grey-14 @ 0.1` | `grey-14 @ 0.15` | `--o-color-control4` | rgba(0,0,0,0.1) → rgba(255,255,255,0.15) |
| 文字（Enabled） | `grey-14` | `grey-14` | `--o-color-info1` | rgb(0,0,0) → rgb(255,255,255) |
| 文字（Disabled） | `grey-14 @ 0.4` | `grey-14 @ 0.4` | `--o-color-info4` | rgba(0,0,0,0.4) → rgba(255,255,255,0.4) |

#### Selected 状态

| 区域 | Light 模式 | Dark 模式 | Token | RGB 值 |
|------|-----------|----------|-------|--------|
| 框体背景（Enabled） | `brand-6` | `brand-6` | `--o-color-primary1` | rgb(0,47,167) → rgb(73,122,248) |
| 框体背景（Disabled） | `brand-3` | `brand-3` | `--o-color-primary4` | rgb(132,161,220) → rgb(29,51,120) |
| 勾号图标（Enabled） | `white` | `white` | `--o-color-fill2` | rgb(255,255,255) |
| 勾号图标（Disabled） | `white` | `white` | `--o-color-fill2` | rgb(255,255,255) |
| 文字（Enabled） | `grey-14` | `grey-14` | `--o-color-info1` | rgb(0,0,0) → rgb(255,255,255) |
| 文字（Disabled） | `grey-14 @ 0.4` | `grey-14 @ 0.4` | `--o-color-info4` | rgba(0,0,0,0.4) → rgba(255,255,255,0.4) |

#### Indeterminate 状态（半选）

| 区域 | Light 模式 | Dark 模式 | Token | RGB 值 |
|------|-----------|----------|-------|--------|
| 框体背景（Enabled） | `brand-6` | `brand-6` | `--o-color-primary1` | rgb(0,47,167) → rgb(73,122,248) |
| 框体背景（Disabled） | `brand-3` | `brand-3` | `--o-color-primary4` | rgb(132,161,220) → rgb(29,51,120) |
| 减号图标（Enabled） | `white` | `white` | `--o-color-fill2` | rgb(255,255,255) |
| 减号图标（Disabled） | `white` | `white` | `--o-color-fill2` | rgb(255,255,255) |
| 文字（Enabled） | `grey-14` | `grey-14` | `--o-color-info1` | rgb(0,0,0) → rgb(255,255,255) |
| 文字（Disabled） | `grey-14 @ 0.4` | `grey-14 @ 0.4` | `--o-color-info4` | rgba(0,0,0,0.4) → rgba(255,255,255,0.4) |

> **说明**：选中状态和半选状态框体使用品牌色填充，无描边；勾号和减号图标使用 `--o-color-fill2`（white）。

---

### 字体样式映射

| 使用场景 | 字号 Token | 行高 Token | 字重 Token | 字号值 | 行高值 |
|---------|-----------|-----------|-----------|-------|-------|
| Checkbox 文字 | `font_size-text1` | `line_height-text1` | `font_weight-regular` | 16px | 24px |

字体族：`font_family`（HarmonyOS / HarmonyHeiTi Regular）

---

### 图标资源

本 skill 自带 3 个状态图标 SVG，位于 [`references/assets/checkbox/`](../references/assets/checkbox/)，覆盖复选框全部 check 变体状态。

| 文件 | 对应状态 | SVG 文件尺寸 | 视觉元素尺寸 | 说明 |
|------|---------|------------|------------|------|
| [`Selected.svg`](../references/assets/checkbox/Selected.svg) | `check=Selected` | 24×24 | 16×16（偏移 4,4） | 选中状态，品牌色框体 + 白色勾号 |
| [`Unselected.svg`](../references/assets/checkbox/Unselected.svg) | `check=Unselected` | 24×24 | 16×16（偏移 4,4） | 未选中状态，白色框体 + 灰色描边 |
| [`indeterminate.svg`](../references/assets/checkbox/indeterminate.svg) | `check=indeterminate` | 24×24 | 16×16（偏移 4,4） | 半选状态，品牌色框体 + 白色减号 |

> **HTML 内联使用注意**：SVG 文件尺寸为 24×24，视觉元素（方框）位于内部 `(4,4)` 偏移处。内联时保持 `viewBox="0 0 24 24"`、`width="24" height="24"`，图标与文字之间设置 `gap: 8px` 即可。

---

### 组件层级结构

```
OCheckbox（HORIZONTAL，自适应宽度，固定高度 24px）
│  GUID: 1042:17362（Unselected/Enabled/off）等
│  Width: 自适应（示例 56px）| Height: 24px
│  Auto Layout: HORIZONTAL
│  autoLayoutItemSpacing: 8px
│  autoLayoutPaddingTop/Bottom/Left/Right: 0px
│  autoLayoutPrimaryAlign: flex-start
│  autoLayoutCounterAlign: center
│
├── [容器 FRAME]
│   GUID: 1080:9211（Unselected）/ 1080:9222（Selected）
│   Width: 56px | Height: 24px
│   Auto Layout: HORIZONTAL, spacing: 8px
│   │
│   ├── [组合 方形选择器 GROUP]
│   │   GUID: 1080:9222（Selected）/ 1080:9504（Indeterminate）
│   │   Width: 16px | Height: 16px
│   │   │
│   │   ├── [框体 RECTANGLE]
│   │   │   GUID: 1042:17371（Selected Enabled Light）
│   │   │   Width: 16px | Height: 16px
│   │   │   cornerRadius: 4px
│   │   │   fill: rgb(0,47,167) → rgb(73,122,248) → Token: `--o-color-primary1`
│   │   │         rgb(132,161,220) → rgb(29,51,120) → Token: `--o-color-primary4`
│   │   │   stroke: 无（选中/半选状态）
│   │   │
│   │   └── [勾号图标 GROUP]（仅 Selected 状态显示）
│   │       GUID: 1042:17372
│   │       Width: ~10.23px | Height: ~10.40px
│   │       fill: rgb(255,255,255) → Token: `--o-color-fill2`
│   │       包含 Path 元素（勾号形状）
│   │
│   │   └── [减号图标 RECTANGLE]（仅 Indeterminate 状态显示）
│   │       GUID: 1042:17394
│   │       Width: 8px | Height: 8px
│   │       fill: rgb(255,255,255) → Token: `--o-color-fill2`
│   │
│   └── [text PARAGRAPH]
       GUID: 1080:9210
       Width: 自适应（示例 32px）| Height: 24px
       fill: rgb(0,0,0) → rgb(255,255,255) → Token: `--o-color-info1`
             rgba(0,0,0,0.4) → rgba(255,255,255,0.4) → Token: `--o-color-info4`
       fontFamily: HarmonyHeiTi
       fontStyle: Regular
       fontSize: 16px → Token: `font_size-text1`
       lineHeight: 24px → Token: `line_height-text1`
       nodeText: "选项"
```

---

### 变体 componentKey 速查

| 变体组合 | node_id | 尺寸 | 说明 |
|---------|---------|------|------|
| check=Unselected, state=Enabled, Dark=off | `1042:17362` | 56×24px | 未选中，浅色，正常状态 |
| check=Unselected, state=Disabled, Dark=off | `1042:17366` | 56×24px | 未选中，浅色，禁用状态 |
| check=Selected, state=Enabled, Dark=off | `1042:17370` | 56×24px | 已选中，浅色，正常状态 |
| check=Selected, state=Disabled, Dark=off | `1042:17382` | 56×24px | 已选中，浅色，禁用状态 |
| check=indeterminate, state=Enabled, Dark=off | `1042:17392` | 56×24px | 半选，浅色，正常状态 |
| check=indeterminate, state=Disabled, Dark=off | `1042:17398` | 56×24px | 半选，浅色，禁用状态 |
| check=Unselected, state=Enabled, Dark=on | `1042:17364` | 56×24px | 未选中，深色，正常状态 |
| check=Unselected, state=Disabled, Dark=on | `1042:17368` | 56×24px | 未选中，深色，禁用状态 |
| check=Selected, state=Enabled, Dark=on | `1042:17376` | 56×24px | 已选中，深色，正常状态 |
| check=Selected, state=Disabled, Dark=on | `1042:17387` | 56×24px | 已选中，深色，禁用状态 |
| check=indeterminate, state=Enabled, Dark=on | `1042:17395` | 56×24px | 半选，深色，正常状态 |
| check=indeterminate, state=Disabled, Dark=on | `1042:17401` | 56×24px | 半选，深色，禁用状态 |

> **注意**：componentKey 需从 Pixso 组件面板获取。

---

### Pixso 操作速查

1. **插入组件**：组件面板搜索「OCheckbox」，拖入画布
2. **切换选中状态**：右侧面板 → check 属性 → 选择 `Selected` / `Unselected` / `indeterminate`
3. **切换交互状态**：右侧面板 → state 属性 → 选择 `Enabled` / `Disabled`
4. **切换主题**：右侧面板 → Dark 属性 → 选择 `off` / `on`
5. **修改文字内容**：双击进入组件 → 双击 text 图层修改内容
6. **组合为复选框组**：多个 Checkbox 实例放入同一容器

---

### 注意事项

- **选中状态识别**：选中时框体使用品牌色填充（`--o-color-primary1`），显示白色勾号图标
- **半选状态识别**：半选时框体使用品牌色填充（`--o-color-primary1`），显示白色减号图标（8×8px）
- **未选中状态识别**：未选中时框体使用 `--o-color-fill2` 背景 + `--o-color-control1` 描边，无图标
- **勾号图标**：仅 `Selected` 状态显示，尺寸约 10×10px，白色填充
- **减号图标**：仅 `indeterminate` 状态显示，尺寸 8×8px，白色填充
- **框体描边**：仅 `Unselected` 状态有描边（strokeWeight: 1px INSIDE），`Selected` 和 `indeterminate` 状态无描边
- **禁用状态背景**：Unselected Disabled Light/Dark 使用 `--o-color-control2-light`
- **禁用状态描边**：Unselected Disabled 使用 `--o-color-control4` 描边
- **文字颜色**：Enabled 状态使用 `--o-color-info1`（一级文字色），Disabled 状态使用 `--o-color-info4`（禁用文字色）
- **半选状态语义**：用于表示部分选中（如父级节点下部分子节点已选中），需在交互层面实现
- **多选逻辑**：多个 Checkbox 组合使用时，同一组内可选中多个，无互斥限制

---

## Part C：交互与状态

### 交互状态

| 元素 | 状态 | 视觉表现 |
|------|------|---------|
| 框体 | Unselected Enabled | 背景：`--o-color-fill2`，描边：`--o-color-control1` |
| 框体 | Unselected Disabled Light | 背景：`--o-color-control2-light`，描边：`--o-color-control4` |
| 框体 | Unselected Disabled Dark | 背景：`--o-color-control2-light`，描边：`--o-color-control4` |
| 框体 | Selected Enabled | 背景：`--o-color-primary1`，无描边，显示勾号 |
| 框体 | Selected Disabled | 背景：`--o-color-primary4`，无描边，显示勾号 |
| 框体 | Indeterminate Enabled | 背景：`--o-color-primary1`，无描边，显示减号 |
| 框体 | Indeterminate Disabled | 背景：`--o-color-primary4`，无描边，显示减号 |
| 勾号 | Selected | 显示，fill：`--o-color-fill2` |
| 减号 | Indeterminate | 显示，fill：`--o-color-fill2` |
| 图标 | Unselected | 隐藏 |
| 文字 | Enabled | 颜色：`--o-color-info1` |
| 文字 | Disabled | 颜色：`--o-color-info4` |

---

### 状态切换逻辑

- **Unselected → Selected**：框体背景切换为 `--o-color-primary1`，描边隐藏，勾号图标显示
- **Selected → Unselected**：框体背景切换为 `--o-color-fill2`，描边显示（`--o-color-control1`），勾号图标隐藏
- **Unselected → Indeterminate**：框体背景切换为 `--o-color-primary1`，描边隐藏，减号图标显示
- **Indeterminate → Selected**：减号图标切换为勾号图标
- **Indeterminate → Unselected**：框体背景切换为 `--o-color-fill2`，描边显示，减号图标隐藏
- **Enabled → Disabled**：颜色降级为禁用 Token（Selected/Indeterminate 用 `--o-color-primary4`，Unselected 用 `--o-color-control2-light`），不可交互
- **Light → Dark**：背景色、品牌色、文字色同步切换为深色主题对应值

---

### 半选状态（Indeterminate）说明

- **语义**：表示部分选中，通常用于树形结构父节点或批量选择场景
- **触发条件**：当子项部分选中时，父项显示半选状态
- **点击行为**：点击半选状态的 Checkbox，通常切换为全选或全不选状态
- **视觉特征**：显示减号图标（8×8px），而非勾号

---

### 交互状态完整规格

#### 未选中状态（Unselected）的5种交互态

| 交互状态 | 框体填充 | 框体描边 | 文本样式 |
|---------|---------|---------|---------|
| **默认（Default）** | `--o-color-fill2` | `--o-color-control1` | 鸿蒙黑体 (Regular) 16px, `--o-color-info1` |
| **悬浮（Hover）** | `--o-color-fill2` | `--o-color-control2` | 鸿蒙黑体 (Regular) 16px, `--o-color-info1` |
| **按下（Pressed/Active）** | `--o-color-fill2` | `--o-color-control3` | 鸿蒙黑体 (Regular) 16px, `--o-color-info1` |
| **选中（Focus）** | `--o-color-primary1` | 无 | 鸿蒙黑体 (Regular) 16px, `--o-color-info1` |
| **禁用（Disabled）** | `--o-color-control2-light` | `--o-color-control4` | 鸿蒙黑体 (Regular) 16px, `--o-color-info1` |

#### 已选中状态（Selected）的5种交互态

| 交互状态 | 框体填充 | 文本样式 | 图标显示 |
|---------|---------|---------|---------|
| **默认（Default）** | `--o-color-primary1` | 鸿蒙黑体 (Regular) 16px, `--o-color-info1` | 显示勾号（✓） |
| **悬浮（Hover）** | `--o-color-primary2` | 鸿蒙黑体 (Regular) 16px, `--o-color-info1` | 显示勾号（✓） |
| **按下（Pressed/Active）** | `--o-color-primary3` | 鸿蒙黑体 (Regular) 16px, `--o-color-info1` | 显示勾号（✓） |
| **选中（Focus）** | `--o-color-primary1` | 鸿蒙黑体 (Regular) 16px, `--o-color-info1` | 显示勾号（✓） |
| **禁用（Disabled）** | `--o-color-primary4` | 鸿蒙黑体 (Regular) 16px, `--o-color-info1` | 显示勾号（✓） |

> **交互状态说明**：
> - **默认（Default）**：组件的正常显示状态
> - **悬浮（Hover）**：鼠标悬停在组件上时的状态，用于提供视觉反馈
> - **按下（Pressed/Active）**：鼠标点击按下时的状态，表示正在交互
> - **选中（Focus）**：获得键盘焦点或被选中的状态
> - **禁用（Disabled）**：不可操作的状态，使用降级颜色表达

---

### Hover 状态详细说明

| 元素 | 状态 | Hover 视觉表现 | Token 变化 |
|------|------|---------------|-----------|
| 框体 | Unselected | 描边颜色加深 | `--o-color-control1` → `--o-color-control2` |
| 框体 | Selected | 背景色变浅 | `--o-color-primary1` → `--o-color-primary2` |
| 框体 | Indeterminate | 背景色变浅 | `--o-color-primary1` → `--o-color-primary2` |
| 文字 | 所有状态 | 颜色不变 | 保持 `--o-color-info1` |

---

### Pressed/Active 状态详细说明

| 元素 | 状态 | Pressed 视觉表现 | Token 变化 |
|------|------|-----------------|-----------|
| 框体 | Unselected | 描边颜色进一步加深 | `--o-color-control2` → `--o-color-control3` |
| 框体 | Selected | 背景色进一步变浅 | `--o-color-primary2` → `--o-color-primary3` |
| 文字 | 所有状态 | 颜色不变 | 保持 `--o-color-info1` |

---

## Part D：设计变量绑定

### 推荐绑定变量

| 元素 | 属性 | 推荐变量 Token |
|------|------|---------------|
| 框体背景（Selected/Indeterminate Enabled） | fill | `--o-color-primary1` |
| 框体背景（Selected/Indeterminate Disabled） | fill | `--o-color-primary4` |
| 框体背景（Unselected Enabled） | fill | `--o-color-fill2` |
| 框体背景（Unselected Disabled Light/Dark） | fill | `--o-color-control2-light` |
| 框体描边（Unselected Enabled） | stroke | `--o-color-control1` |
| 框体描边（Unselected Disabled） | stroke | `--o-color-control4` |
| 勾号图标 | fill | `--o-color-fill2` |
| 减号图标 | fill | `--o-color-fill2` |
| 文字（Enabled） | fill | `--o-color-info1` |
| 文字（Disabled） | fill | `--o-color-info4` |
| 文字字号 | fontSize | `font_size-text1`（16px） |
| 文字行高 | lineHeight | `line_height-text1`（24px） |
| 选择器↔文字间距 | autoLayoutItemSpacing | `gap-2`（8px） |
| 框体尺寸 | width/height | `icon_size_control-xs`（16px） |
| 框体圆角 | cornerRadius | `radius_control-xs`（4px） |

---

## Part E：最佳实践

### 使用建议

1. **复选框组布局**：多个 Checkbox 垂直排列时，间距建议 8-12px（gap-2 ~ gap-3）
2. **选项文字简洁**：每个选项文字控制在 2-8 个字，避免过长描述
3. **半选状态使用**：仅在树形结构或批量选择场景使用半选状态
4. **批量操作**：表格批量选择使用 Checkbox，配合「全选」Checkbox 实现半选逻辑

### 设计提示

- 复选框左侧选择器 + 右侧文字是标准布局，不建议反向排列
- 勾号和减号图标使用 `--o-color-fill2`（white），确保在品牌色背景下可识别
- 复选框整体高度固定 24px，与文字行高一致，确保视觉对齐
- 同一组复选框应保持一致的尺寸和间距
- 禁用状态应同时禁用框体和文字的交互
- 半选状态需配合交互逻辑实现，仅视觉设计无法表达完整语义

---

## 技术备注

- 组件节点 ID：`1042:17361`（组件集）
- 设计稿 URL：https://pixso.cn/app/design/JZkjW0mhmT61Mtd98dCfBw?item-id=1042:17361
- 生成日期：2026-04-17
- 变体总数：12（check=Selected/Unselected/indeterminate × state=Enabled/Disabled × Dark=off/on）
- 尺寸数据：来源于 Pixso MCP get_node_dsl
- 字号数据：16px/24px（font_size-text1 / line_height-text1）
- 框体尺寸：16×16px（icon_size_control-xs），圆角 4px（radius_control-xs）
- 勾号图标：约 10×10px
- 减号图标：8×8px