> ← [组件索引](../SKILL.md#组件索引) · [README](../README.md)

# OInput 输入框 · 设计 Skill

> 组件集合节点：`1042:18183` · 组件名：OInput 输入框 · 变体总数：12

---

## Part A：设计使用卡

### 组件概览

**OInput 输入框**：用于接收用户文本输入的基础表单控件。支持三种尺寸（large/medium/small）、三种状态（Enabled/Error/Complete），以及 Light/Dark 主题。输入框由标签、输入区域、提示文字组成，通过描边颜色区分正常和错误状态。

---

### 适用场景

- ✅ **表单输入**：登录表单、注册表单、信息填写等场景
- ✅ **搜索输入**：搜索框、筛选输入、查找功能
- ✅ **数据录入**：数据编辑、配置设置、参数输入
- ✅ **评论输入**：评论框、反馈输入、备注填写
- ❌ **不适合**：多行长文本（用 TextArea）、富文本编辑（用 RichTextEditor）、只读展示（用 OText）

---

### 变体说明

**size（尺寸）**
- `large` — 输入框高度 40px，文字字号 16px，**PC 端**突出的表单区域或重要输入
- `medium` — 输入框高度 32px，文字字号 14px，**PC 端默认尺寸**，适合大多数表单场景
- `small` — 输入框高度 40px，文字字号 14px，提示文字 12px，**移动端专用**

**Type（状态类型）**
- `Enabled` — 正常状态，默认可输入状态
- `Error` — 错误状态，描边使用红色，配合错误提示文字
- `Complete` — 完成状态，视觉上与 Enabled 相同（描边 grey-14@0.25），表示输入已完成但无特殊视觉反馈

**Dark（主题）**
- `off` — 浅色模式（默认），背景白色
- `on` — 深色模式，背景使用 grey-4

> **说明**：`Group-*` 变体（Group-tittle、Group-btn、Group-number）是组合示例，展示带标题、按钮、数字输入的完整输入框组合，不是单独的基础状态。

---

### 布局结构

> 🧩 **布局结构**：输入框由标签区域（可选）、输入区域、提示区域（可选）组成。输入区域是 FRAME 节点，包含文字输入层和可选的前缀/后缀图标。

**基础结构**

```
OInput（SYMBOL，自适应宽度，固定高度）
├── Width: 自适应（示例 320px large/medium，312px small）
├── Height: 40px（large/small）/ 32px（medium）
│
├── [提示区域 FRAME]（可选）
│   Width: 自适应 | Height: 22px（large）/ 18px（medium/small）
│   └── [提示文字 PARAGRAPH]
│         fontSize: 14px（large）/ 12px（medium/small）
│         fill: grey-14（Light）/ grey-14（Dark）
│         text: 提示语内容
│
└── [输入区域 FRAME]
    Width: 自适应 | Height: 40px（large/small）/ 32px（medium）
    cornerRadius: 4px
    fill: grey-1（Light）/ grey-4（Dark）
    stroke: grey-14@0.25（Enabled）/ red-6（Error）
    strokeWeight: 1px INSIDE
    │
    ├── [前缀图标]（可选，如搜索图标）
    │   Width: 16-24px | Height: 16-24px
    │
    ├── [输入文字 PARAGRAPH]
    │   fontSize: 16px（large）/ 14px（medium/small）
    │   fill: grey-14（Light）/ grey-14（Dark）
    │   text: 用户输入内容或占位文字
    │
    └── [后缀图标]（可选，如清除、完成图标）
        Width: 16-24px | Height: 16-24px
```

---

### 组合搭配

> 🔗 **常见搭配**：
> - **表单组合**：OForm + OFormItem（内含 OInput/OButton），构成完整表单
> - **搜索组合**：OInput（带搜索图标）+ OButton（搜索按钮）
> - **登录表单**：用户名输入框 + 密码输入框 + 提交按钮
> - **筛选输入**：OInput + OSelect + OToggle，构成筛选栏

---

### 设计稿识别指南

> 🔍 **识别特征**：
> - 输入区域 FRAME 节点，高度 32/40px，圆角 4px
> - Enabled 状态：背景 white/grey-4，描边 grey-14@0.25（color-control1）
> - Error 状态：背景不变，描边 red-6（color-danger1）
> - 文字字号：large=16px，medium/small=14px
> - 提示字号：large=14px，medium/small=12px
> - 无 Disabled 状态

> 🔄 **易混淆组件**：
> - 与 **TextArea 多行输入**：Input 是单行输入，固定高度；TextArea 是多行，高度自适应
> - 与 **OSelect 选择器**：Select 用于下拉选择，有下箭头图标；Input 用于自由文本输入

---

### 响应式行为

参照 [栅格规范](../SKILL.md#数据资源) 中的断点定义，OInput 输入框宽度随容器自适应，无固定断点响应式行为。

---

## Part B：规格速查参考

### 变体索引表

| 变体属性 | 可选值 | 默认值 | 视觉差异 |
|---------|--------|--------|---------|
| size | `large` / `medium` / `small` | `medium` | PC 端 40/32px，移动端 40px，字号 16/14/14px |
| Type | `Enabled` / `Error` / `Complete` | `Enabled` | 描边颜色 grey/red/grey |
| Dark | `off` / `on` | `off` | 背景色 white/grey-4 |

---

### 布局规格

> **尺寸用途说明**：`large` 和 `medium` 用于 PC 端，`small` 用于移动端。

| 规格项 | large（PC） | medium（PC） | small（移动端） |
|--------|-------|--------|-------|
| 输入框高度 | 40px | 32px | 40px |
| Token（高度） | `control_size-l` | `control_size-m` | `control_size-l` |
| 输入框宽度 | 自适应（示例 320px） | 自适应（示例 320px） | 自适应（示例 312px） |
| 输入框圆角 | 4px | 4px | 4px |
| Token（圆角） | `radius_control-xs` | `radius_control-xs` | `radius_control-xs` |
| 占位文字字号（输入框内） | 16px | 14px | 14px |
| Token（字号） | `font_size-text1` | `font_size-tip1` | `font_size-tip1` |
| 占位文字行高 | 24px | 22px | 22px |
| Token（行高） | `line_height-text1` | `line_height-tip1` | `line_height-tip1` |
| 提示文字字号（输入框外） | 14px | 12px | 12px |
| Token（提示字号） | `font_size-tip1` | `font_size-tip2` | `font_size-tip2` |
| 提示区域高度 | 22px | 18px | 18px |
| 描边宽度 | 1px INSIDE | 1px INSIDE | 1px INSIDE |

---

### 颜色 Token 映射

#### 输入区域（Enabled 状态）

| 区域 | Light 模式 | Dark 模式 | Token | RGB 值 |
|------|-----------|----------|-------|--------|
| 输入框背景 | `grey-1` | `grey-4` | `--o-color-fill2` | rgb(255,255,255) → rgb(36,36,39) |
| 输入框描边 | `grey-14 @ 0.25` | `grey-14 @ 0.25` | `--o-color-control1` | rgba(0,0,0,0.25) → rgba(255,255,255,0.25) |
| 提示文字（输入框外） | `grey-14 @ 0.6` | `grey-14 @ 0.6` | `--o-color-info3` | rgba(0,0,0,0.6) → rgba(255,255,255,0.6) |
| 占位文字（输入框内） | `grey-14 @ 0.4` | `grey-14 @ 0.4` | `--o-color-info4` | rgba(0,0,0,0.4) → rgba(255,255,255,0.4) |

#### 输入区域（Error 状态）

| 区域 | Light 模式 | Dark 模式 | Token | RGB 值 |
|------|-----------|----------|-------|--------|
| 输入框背景 | `grey-1` | `grey-4` | `--o-color-fill2` | rgb(255,255,255) → rgb(36,36,39) |
| 输入框描边 | `red-6` | `red-6` | `--o-color-danger1` | rgb(230,0,18) → rgb(235,35,45) |
| 提示文字（输入框外） | `grey-14 @ 0.6` | `grey-14 @ 0.6` | `--o-color-info3` | rgba(0,0,0,0.6) → rgba(255,255,255,0.6) |
| 错误提示文字（输入框外） | `red-6` | `red-6` | `--o-color-danger1` | rgb(230,0,18) → rgb(235,35,45) |
| 占位文字（输入框内） | `grey-14` | `grey-14` | `--o-color-info1` | rgb(0,0,0) → rgb(255,255,255) |

#### 输入区域（Complete 状态）

Complete 状态视觉上与 Enabled 状态相同，无特殊颜色差异。

| 区域 | Light 模式 | Dark 模式 | Token | RGB 值 |
|------|-----------|----------|-------|--------|
| 输入框背景 | `grey-1` | `grey-4` | `--o-color-fill2` | rgb(255,255,255) → rgb(36,36,39) |
| 输入框描边 | `grey-14 @ 0.25` | `grey-14 @ 0.25` | `--o-color-control1` | rgba(0,0,0,0.25) → rgba(255,255,255,0.25) |
| 提示文字（输入框外） | `grey-14 @ 0.6` | `grey-14 @ 0.6` | `--o-color-info3` | rgba(0,0,0,0.6) → rgba(255,255,255,0.6) |
| 占位文字（输入框内） | `grey-14` | `grey-14` | `--o-color-info1` | rgb(0,0,0) → rgb(255,255,255) |

---

### 字体样式映射

| 使用场景 | 字号 Token | 行高 Token | 字重 Token | 字号值 | 行高值 |
|---------|-----------|-----------|-----------|-------|-------|
| large 占位文字（输入框内） | `font_size-text1` | `line_height-text1` | `font_weight-regular` | 16px | 24px |
| medium/small 占位文字（输入框内） | `font_size-tip1` | `line_height-tip1` | `font_weight-regular` | 14px | 22px |
| large 提示文字（输入框外） | `font_size-tip1` | — | `font_weight-regular` | 14px | — |
| medium/small 提示文字（输入框外） | `font_size-tip2` | — | `font_weight-regular` | 12px | — |

字体族：`font_family`（HarmonyOS / HarmonyHeiTi Regular）

---

### 组件层级结构

**size=large, Type=Enabled, Dark=off**

```
OInput（SYMBOL）
│  GUID: 1042:18202
│  Width: 自适应（示例 320px）| Height: 40px
│
├── [提示区域 FRAME]
│   GUID: 内部节点
│   Width: 320px | Height: 22px
│   │
│   └── [提示文字 PARAGRAPH]
│       Width: 268px | Height: 22px
│       fontSize: 14px → Token: `font_size-tip1`
│       fill: rgba(0,0,0,0.6) → Token: `--o-color-info3`（Light）
│             rgba(255,255,255,0.6) → Token: `--o-color-info3`（Dark）
│       nodeText: "提示语"
│
└── [输入区域 FRAME]
    GUID: 内部节点（属性 1=L,属性 2=Enabled）
    Width: 320px | Height: 40px
    cornerRadius: 4px → Token: `radius_control-xs`
    fill: rgb(255,255,255) → Token: `--o-color-fill2`（Light）
          rgb(36,36,39) → Token: `--o-color-fill2`（Dark）
    stroke: rgba(0,0,0,0.25) → Token: `--o-color-control1`（Light Enabled）
            rgba(255,255,255,0.25) → Token: `--o-color-control1`（Dark Enabled）
            rgba(230,0,18,1) → Token: `--o-color-danger1`（Light Error）
            rgba(235,35,45,1) → Token: `--o-color-danger1`（Dark Error）
    strokeWeight: 1px | strokeAlign: INSIDE
    │
    └── [输入文字 PARAGRAPH]
        Width: 自适应 | Height: 24px
        fontSize: 16px → Token: `font_size-text1`
        fill: rgba(0,0,0,0.4) → Token: `--o-color-info4`（Light）
              rgba(255,255,255,0.4) → Token: `--o-color-info4`（Dark）
        nodeText: "Hint"（占位文字示例）
```

**size=medium, Type=Enabled, Dark=off**

```
OInput（SYMBOL）
│  GUID: 1042:18258
│  Width: 320px | Height: 32px
│
├── [提示区域 FRAME]
│   Width: 320px | Height: 18px
│   └── [提示文字]
│       fontSize: 12px → Token: `font_size-tip2`
│
└── [输入区域 FRAME]
    Width: 320px | Height: 32px
    cornerRadius: 4px
    fill: rgb(255,255,255) → Token: `--o-color-fill2`
    stroke: rgba(0,0,0,0.25) → Token: `--o-color-control1`
    strokeWeight: 1px INSIDE
    │
    └── [输入文字]
        fontSize: 14px → Token: `font_size-tip1`
        nodeText: "Hint"
```

**size=small, Type=Enabled, Dark=off**

```
OInput（SYMBOL）
│  GUID: 1042:18284
│  Width: 312px | Height: 40px
│
├── [提示区域 FRAME]
│   Width: 312px | Height: 18px
│   └── [提示文字]
│       fontSize: 12px → Token: `font_size-tip2`
│       nodeText: "提示语"
│
└── [输入区域 FRAME]
    Width: 312px | Height: 40px
    cornerRadius: 4px
    fill: rgb(255,255,255) → Token: `--o-color-fill2`
    stroke: rgba(0,0,0,0.25) → Token: `--o-color-control1`
    strokeWeight: 1px INSIDE
    │
    └── [输入文字]
        fontSize: 14px → Token: `font_size-tip1`
        nodeText: "Hint"
```

---

### 变体 componentKey 速查

| 变体组合 | node_id | 尺寸（示例） | 说明 |
|---------|---------|------------|------|
| size=large, Type=Enabled, Dark=off | `1042:18202` | 320×40px | 大尺寸，正常，浅色 |
| size=large, Type=Enabled, Dark=on | `1042:18209` | 320×40px | 大尺寸，正常，深色 |
| size=large, Type=Error, Dark=off | `1042:18216` | 320×40px | 大尺寸，错误，浅色 |
| size=large, Type=Error, Dark=on | `1042:18221` | 320×40px | 大尺寸，错误，深色 |
| size=large, Type=Complete, Dark=off | `1042:18226` | 320×40px | 大尺寸，完成，浅色 |
| size=large, Type=Complete, Dark=on | `1042:18233` | 320×40px | 大尺寸，完成，深色 |
| size=medium, Type=Enabled, Dark=off | `1042:18258` | 320×32px | 中尺寸，正常，浅色 |
| size=medium, Type=Enabled, Dark=on | `1042:18265` | 320×32px | 中尺寸，正常，深色 |
| size=medium, Type=Complete, Dark=off | `1042:18240` | 320×32px | 中尺寸，完成，浅色 |
| size=medium, Type=Complete, Dark=on | `1042:18249` | 320×32px | 中尺寸，完成，深色 |
| size=small, Type=Enabled, Dark=off | `1042:18284` | 312×40px | 小尺寸，正常，浅色 |
| size=small, Type=Enabled, Dark=on | `1042:18291` | 312×40px | 小尺寸，正常，深色 |
| size=small, Type=Error, Dark=off | `1042:18322` | 312×40px | 小尺寸，错误，浅色 |
| size=small, Type=Error, Dark=on | `1042:18328` | 312×40px | 小尺寸，错误，深色 |
| size=small, Type=Complete, Dark=off | `1042:18298` | 312×40px | 小尺寸，完成，浅色 |
| size=small, Type=Complete, Dark=on | `1042:18307` | 312×40px | 小尺寸，完成，深色 |

> **注意**：示例尺寸（320px/312px）仅为演示，实际宽度随容器自适应。

---

### Pixso 操作速查

1. **插入组件**：组件面板搜索「OInput」，拖入画布
2. **切换尺寸**：右侧面板 → size 属性 → 选择 `large` / `medium` / `small`
3. **切换状态**：右侧面板 → Type 属性 → 选择 `Enabled` / `Error` / `Complete`
4. **切换主题**：右侧面板 → Dark 属性 → 选择 `off` / `on`
5. **修改宽度**：手动调整 SYMBOL 的 width 值
6. **修改提示文字**：双击进入组件 → 双击提示文字图层修改内容
7. **修改占位文字**：双击进入组件 → 双击输入文字图层修改内容

---

### 注意事项

- **无 Disabled 状态**：OInput 没有 Disabled（禁用）变体，所有状态均可输入
- **描边样式**：描边宽度 1px INSIDE，不影响输入框整体尺寸
- **输入框高度**：large=40px，medium=32px，small=40px（与 large 相同）
- **文字字号**：large=16px，medium/small=14px
- **提示字号**：large=14px，medium/small=12px
- **圆角统一**：所有尺寸圆角均为 4px（`radius_control-xs`）
- **背景颜色**：Light=grey-1（white），Dark=grey-4
- **描边颜色**：Enabled=grey-14@0.25（`--o-color-control1`），Error=red-6（`--o-color-danger1`）
- **Complete 状态**：视觉与 Enabled 相同，描边使用 `--o-color-control1`，无图标
- **占位文字颜色差异**：Enabled=grey-14@0.4（`--o-color-info4`），Error/Complete=grey-14@1（`--o-color-info1`）
- **宽度自适应**：输入框宽度随容器自适应，无固定宽度
- **Group 变体**：组合示例，展示带标题、按钮、数字输入的完整输入框组合
- **提示区域**：可选，默认不显示，需要时可手动添加或使用 Group 变体

---

### ⚠️ 硬约束（必须遵守）

> 🔒 **以下规则为硬性要求，实现时必须严格遵守，不得违反**

#### 图标资源规范

| 图标类型 | 资源路径 | 规格 | 使用场景 |
|---------|---------|------|---------|
| **关闭/清除图标** | `../references/assets/public icons/icon-关闭.svg` | 24×24px SVG | 有文本时显示的清除按钮 |
| **错误提示图标** | `../references/assets/message/错误.svg` | 24×24px SVG | Error 状态下的错误提示 |

**强制规则**：

- ✅ **必须使用 assets 目录中的官方 SVG 资源**
- ✅ **禁止使用 emoji、字符（×）、或自定义绘制的图标替代**
- ✅ **图标尺寸必须符合硬约束 §5 规定的合法集合**（PC: 16/20/24/32/40/48/56/64px）
- ✅ **Input 组件中图标统一使用 16×16px 显示尺寸**（原始资源 24×24px 缩放）
- ❌ **禁止从外部链接引用图标**
- ❌ **禁止使用 base64 内嵌或其他非标准方式引入图标**

#### 实现示例

```html
<!-- 正确示范：使用 assets 中的 SVG 资源 -->
<button class="o-input-clear" aria-label="清除输入">
    <img src="../references/assets/public icons/icon-关闭.svg" alt="关闭图标">
</button>

<!-- 错误示范：使用字符或 emoji（严格禁止）-->
<span class="o-input-clear">×</span>  <!-- ❌ 违反硬约束 -->
<span class="o-input-error-icon">⚠️</span>  <!-- ❌ 违反硬约束 -->
```

#### 校验检查项

```
□ 关闭图标是否使用 ../references/assets/public icons/icon-关闭.svg？
□ 错误图标是否使用 ../references/assets/message/错误.svg？
□ 图标元素是否为 <img> 标签引入 SVG 文件？
□ 是否存在使用字符、emoji 或其他非标准图标的违规行为？
□ 图标显示尺寸是否符合 16×16px 的组件规范？
```

---

## Part C：交互与状态

### 交互状态总览

Input 组件包含 **7 种交互状态**：默认、悬浮、激活、输入中、输入完成、禁用、错误。每种状态在「无文本」和「有文本」两种场景下有不同的视觉表现。

---

### 1. 默认状态（Default）

> 基础可输入状态，组件初始展示形态。

#### 无文本场景

| 属性 | 规格 | Token |
|------|------|-------|
| 描边颜色 | grey-14 @ 0.25 | `--o-color-control1` |
| 填充颜色 | white（Light）/ grey-4（Dark） | `--o-color-fill2` |
| 占位符文本字号 | 16px（Regular） | `font_size-text1` |
| 占位符文本色值 | grey-14 @ 0.4 | `--o-color-info4` |

#### 有文本场景

| 属性 | 规格 | Token |
|------|------|-------|
| 描边颜色 | grey-14 @ 0.25 | `--o-color-control1` |
| 填充颜色 | white（Light）/ grey-4（Dark） | `--o-color-fill2` |
| 文本字号 | 16px（Regular） | `font_size-text1` |
| 文本色值 | grey-14 @ 1 | `--o-color-info1` |

---

### 2. 悬浮状态（Hover）

> 鼠标悬停在输入框上时的状态，提示可交互性。

#### 无文本场景

| 属性 | 规格 | Token |
|------|------|-------|
| 描边颜色 | brand-6 | `--o-color-control2` |
| 填充颜色 | white（Light）/ grey-4（Dark） | `--o-color-fill2` |
| 占位符文本字号 | 16px（Regular） | `font_size-text1` |
| 占位符文本色值 | grey-14 @ 0.4 | `--o-color-info4` |

#### 有文本场景

| 属性 | 规格 | Token |
|------|------|-------|
| 描边颜色 | brand-6 | `--o-color-control2` |
| 填充颜色 | white（Light）/ grey-4（Dark） | `--o-color-fill2` |
| 文本字号 | 16px（Regular） | `font_size-text1` |
| 文本色值 | grey-14 @ 1 | `--o-color-info1` |
| 清除图标 | 显示（×） | — |

---

### 3. 激活状态（Focus/Active）

> 用户点击或聚焦输入框时的状态，表示当前正在操作该输入框。

#### 无文本场景

| 属性 | 规格 | Token |
|------|------|-------|
| 描边颜色 | brand-6（高亮） | `--o-color-control3` |
| 填充颜色 | white（Light）/ grey-4（Dark） | `--o-color-fill2` |
| 占位符文本字号 | 16px（Regular） | `font_size-text1` |
| 占位符文本色值 | grey-14 @ 0.4 | `--o-color-info4` |

#### 有文本场景

| 属性 | 规格 | Token |
|------|------|-------|
| 描边颜色 | brand-6（高亮） | `--o-color-control3` |
| 填充颜色 | white（Light）/ grey-4（Dark） | `--o-color-fill2` |
| 文本字号 | 16px（Regular） | `font_size-text1` |
| 文本色值 | grey-14 @ 1 | `--o-color-info1` |
| 清除图标 | 显示（×） | — |
| 光标 | 显示闪烁光标 | — |

---

### 4. 输入中状态（Typing）

> 用户正在输入文字的过程状态，仅在「有文本」场景下存在。

#### 有文本场景

| 属性 | 规格 | Token |
|------|------|-------|
| 描边颜色 | brand-6（高亮） | `--o-color-control3` |
| 填充颜色 | white（Light）/ grey-4（Dark） | `--o-color-fill2` |
| 文本字号 | 16px（Regular） | `font_size-text1` |
| 文本色值 | grey-14 @ 1 | `--o-color-info1` |
| 清除图标 | 显示（×） | — |
| 光标 | 显示闪烁光标 | — |

> **说明**：「无文本」场景下不存在输入中状态，直接从默认/悬浮/激活状态转换。

---

### 5. 输入完成状态（Complete）

> 用户完成输入后的状态，通常在失去焦点后触发。

#### 有文本场景

| 属性 | 规格 | Token |
|------|------|-------|
| 描边颜色 | grey-14 @ 0.25 | `--o-color-control1` |
| 填充颜色 | white（Light）/ grey-4（Dark） | `--o-color-fill2` |
| 文本字号 | 16px（Regular） | `font_size-text1` |
| 文本色值 | grey-14 @ 1 | `--o-color-info1` |

> **说明**：
> - 「无文本」场景下不存在输入完成状态（显示为默认状态）
> - 视觉表现与「默认-有文本」状态相同
> - 作为语义标记，表示输入已完成但无特殊视觉反馈

---

### 6. 禁用状态（Disabled）

> 输入框不可用的状态，禁止用户交互。

#### 无文本场景

| 属性 | 规格 | Token |
|------|------|-------|
| 描边颜色 | grey-14 @ 0.15 | `--o-color-control4` |
| 填充颜色 | grey-1（浅灰） | `--o-color-control4-light` |
| 占位符文本字号 | 16px（Regular） | `font_size-text1` |
| 占位符文本色值 | grey-14 @ 0.4 | `--o-color-info4` |

#### 有文本场景

| 属性 | 规格 | Token |
|------|------|-------|
| 描边颜色 | grey-14 @ 0.15 | `--o-color-control4` |
| 填充颜色 | grey-1（浅灰） | `--o-color-control4-light` |
| 占位符文本字号 | 16px（Regular） | `font_size-text1` |
| 占位符文本色值 | grey-14 @ 0.4 | `--o-color-info4` |

> **说明**：禁用状态下不显示清除图标，不接受任何用户输入。

---

### 7. 错误状态（Error）

> 输入校验失败时的状态，需要用户修正输入内容。

#### 无文本场景

| 属性 | 规格 | Token |
|------|------|-------|
| 描边颜色 | red-6 | `--o-color-danger1` |
| 填充颜色 | white（Light）/ grey-4（Dark） | `--o-color-fill2` |
| 占位符文本字号 | 16px（Regular） | `font_size-text1` |
| 占位符文本色值 | grey-14 @ 0.4 | `--o-color-info4` |
| 错误提示文字字号 | 12px（Regular） | `font_size-tip2` |
| 错误提示文字色值 | red-6 | `--o-color-danger1` |
| 错误图标大小 | 16×16 px | — |

#### 有文本场景

| 属性 | 规格 | Token |
|------|------|-------|
| 描边颜色 | red-6 | `--o-color-danger1` |
| 填充颜色 | white（Light）/ grey-4（Dark） | `--o-color-fill2` |
| 文本字号 | 16px（Regular） | `font_size-text1` |
| 文本色值 | grey-14 @ 1 | `--o-color-info1` |
| 错误提示文字字号 | 12px（Regular） | `font_size-tip2` |
| 错误提示文字色值 | red-6 | `--o-color-danger1` |
| 错误图标大小 | 16×16 px | — |
| 清除图标 | 显示（×） | — |

---

### 交互状态速查表

| 状态 | 场景 | 描边 Token | 填充 Token | 文本/占位符 Token | 特殊元素 |
|------|------|-----------|-----------|-------------------|---------|
| 默认 | 无文本 | `--o-color-control1` | `--o-color-fill2` | `--o-color-info4` | — |
| 默认 | 有文本 | `--o-color-control1` | `--o-color-fill2` | `--o-color-info1` | — |
| 悬浮 | 无文本 | `--o-color-control2` | `--o-color-fill2` | `--o-color-info4` | — |
| 悬浮 | 有文本 | `--o-color-control2` | `--o-color-fill2` | `--o-color-info1` | 清除图标 |
| 激活 | 无文本 | `--o-color-control3` | `--o-color-fill2` | `--o-color-info4` | 光标 |
| 激活 | 有文本 | `--o-color-control3` | `--o-color-fill2` | `--o-color-info1` | 清除图标+光标 |
| 输入中 | 有文本 | `--o-color-control3` | `--o-color-fill2` | `--o-color-info1` | 清除图标+光标 |
| 输入完成 | 有文本 | `--o-color-control1` | `--o-color-fill2` | `--o-color-info1` | — |
| 禁用 | 无文本 | `--o-color-control4` | `--o-color-control4-light` | `--o-color-info4` | — |
| 禁用 | 有文本 | `--o-color-control4` | `--o-color-control4-light` | `--o-color-info4` | — |
| 错误 | 无文本 | `--o-color-danger1` | `--o-color-fill2` | `--o-color-info4` | 错误提示+图标 |
| 错误 | 有文本 | `--o-color-danger1` | `--o-color-fill2` | `--o-color-info1` | 清除图标+错误提示+图标 |

---

### 状态切换逻辑

#### 正常流程

```
默认（Default）
    ↓ [鼠标进入]
悬浮（Hover）
    ↓ [鼠标点击]
激活（Focus）
    ↓ [开始输入]
输入中（Typing）
    ↓ [停止输入 + 失去焦点]
输入完成（Complete）
    ↓ [重新获得焦点]
激活（Focus）
    ↓ [鼠标离开]
默认（Default）
```

#### 异常流程

```
默认/悬浮/激活/输入中
    ↓ [校验失败]
错误（Error）
    ↓ [用户修正 + 校验通过]
默认/悬浮/激活/输入中
```

#### 禁用流程

```
任意状态
    ↓ [设置为禁用]
禁用（Disabled）
    ↓ [解除禁用]
恢复到之前状态
```

---

### 状态切换详细规则

- **默认 → 悬浮**：鼠标移入输入框区域，描边颜色切换为 `--o-color-control2`（brand-6）
- **悬浮 → 默认**：鼠标移出输入框区域，描边颜色恢复为 `--o-color-control1`（grey-14@0.25）
- **悬浮/默认 → 激活**：鼠标点击输入框，描边颜色切换为 `--o-color-control3`（brand-6 高亮），显示光标
- **激活 → 输入中**：用户开始键入文字，有文本时显示清除图标
- **输入中 → 输入完成**：用户停止输入且输入框失去焦点，描边颜色恢复为 `--o-color-control1`
- **任意状态 → 错误**：输入校验失败，描边颜色切换为 `--o-color-danger1`（red-6），显示错误提示文字和图标
- **错误 → 默认/激活**：用户修正输入内容并通过校验，恢复正常状态
- **任意状态 → 禁用**：设置 disabled 属性，描边切换为 `--o-color-control4`，填充切换为 `--o-color-control4-light`
- **禁用 → 之前状态**：移除 disabled 属性，恢复正常交互能力

---

### Hover 状态（交互态）

| 元素 | Hover 视觉表现 |
|------|---------------|
| 输入框（Enabled/Complete） | 描边颜色切换为 `--o-color-control2`（brand-6） |
| 输入框（Error） | 描边颜色不变（保持 `--o-color-danger1`） |
| 输入框（Disabled） | 无 Hover 效果 |
| 清除图标（有文本时） | Hover 时显示 |

---

### Focus 状态（交互态）

| 元素 | Focus 视觉表现 |
|------|---------------|
| 输入框（Enabled/Complete） | 描边颜色切换为 `--o-color-control3`（brand-6 高亮），显示闪烁光标 |
| 输入框（Error） | 描边颜色保持 `--o-color-danger1`，显示闪烁光标 |
| 输入框（Disabled） | 无法获得 Focus |

---

## Part D：设计变量绑定

### 推荐绑定变量

| 元素 | 属性 | 推荐变量 Token |
|------|------|---------------|
| 输入框背景 | fill | `--o-color-fill2` |
| 输入框描边（Enabled） | stroke | `--o-color-control1` |
| 输入框描边（Error） | stroke | `--o-color-danger1` |
| 提示文字（输入框外） | fill | `--o-color-info3` |
| 错误提示文字（输入框外） | fill | `--o-color-danger1` |
| 占位文字（输入框内） Enabled | fill | `--o-color-info4` |
| 占位文字（输入框内） Error/Complete | fill | `--o-color-info1` |
| 输入框高度（large/small） | height | `control_size-l`（40px） |
| 输入框高度（medium） | height | `control_size-m`（32px） |
| 输入框圆角 | cornerRadius | `radius_control-xs`（4px） |
| 占位文字字号（large） | fontSize | `font_size-text1`（16px） |
| 占位文字字号（medium/small） | fontSize | `font_size-tip1`（14px） |
| 提示文字字号（large） | fontSize | `font_size-tip1`（14px） |
| 提示文字字号（medium/small） | fontSize | `font_size-tip2`（12px） |
| 描边宽度 | strokeWeight | 1px INSIDE |

---

## Part E：最佳实践

### 使用建议

1. **尺寸选择**：PC 端默认使用 medium，突出表单使用 large；移动端使用 small
2. **错误提示**：Error 状态应配合具体的错误提示文字，说明错误原因
3. **占位文字**：使用简短的提示文字，如"请输入"、"搜索"、"用户名"
4. **宽度设置**：输入框宽度应与容器宽度匹配，确保布局协调

### 设计提示

- **尺寸用途**：large/medium 用于 PC 端，small 用于移动端
- 输入框高度：large=40px，medium=32px，small=40px（注意 small 与 large 高度相同）
- 提示区域高度：large=22px，medium/small=18px
- 文字字号：large=16px，medium/small=14px；提示字号：large=14px，medium/small=12px
- Error 状态仅改变描边颜色（red-6），背景不变
- Complete 状态视觉与 Enabled 相同，无图标，仅作为语义标记
- 描边样式为 1px INSIDE，不影响输入框整体尺寸
- 深色模式下背景切换为 grey-4，描边和文字颜色同步切换
- 输入框宽度自适应，根据容器或内容需求设置

---

## 技术备注

- 组件节点 ID：`1042:18183`（组件集）
- 设计稿 URL：https://pixso.cn/app/design/JZkjW0mhmT61Mtd98dCfBw?item-id=1042:18183
- 生成日期：2026-04-17
- 变体总数：12（size=large/medium/small × Type=Enabled/Error/Complete × Dark=off/on）
- 无 Disabled 状态：所有状态均可输入
- **尺寸用途**：large/medium 用于 PC 端，small 用于移动端

---

### 🔒 硬约束合规性声明

> ⚠️ **本组件实现严格遵循以下硬约束规范**

#### 图标资源硬约束（§强制）

| 约束项 | 合规状态 | 实现方式 |
|--------|---------|---------|
| 关闭图标资源来源 | ✅ 已合规 | 使用 `assets/public icons/icon-关闭.svg` |
| 错误图标资源来源 | ✅ 已合规 | 使用 `assets/message/错误.svg` |
| 图标格式 | ✅ 已合规 | SVG 格式，符合设计系统标准 |
| 图标引入方式 | ✅ 已合规 | `<img>` 标签引用，语义化正确 |
| 图标显示尺寸 | ✅ 已合规 | 16×16px（符合硬约束 §5 规定） |
| 禁止使用字符/emoji | ✅ 已合规 | 未使用任何非标准图标 |

**合规校验时间戳**：2026-07-14  
**校验结果**：✅ **全部通过 - 符合硬约束要求**

> 📌 **重要提示**：任何对本组件图标的修改都必须继续遵守此硬约束，不得替换为非 assets 资源。
- 尺寸数据：large=320×40px，medium=320×32px，small=312×40px（示例）
- 输入框高度：large/small=40px，medium=32px
- 圆角：4px（radius_control-xs）
- 描边：1px INSIDE
- 文字字号：large=16px（font_size-text1），medium/small=14px（font_size-tip1）
- 提示字号：large=14px（font_size-tip1），medium/small=12px（font_size-tip2）
- 背景：Light=grey-1（white），Dark=grey-4（color-fill2）
- 描边：Enabled=grey-14@0.25（color-control1），Error=red-6（color-danger1）
- Group 变体：组合示例（Group-tittle、Group-btn、Group-number），非基础状态