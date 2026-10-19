> ← [组件索引](../SKILL.md#组件索引) · [README](../README.md)

# ODialog 对话框 · 设计 Skill

> 组件集合节点：`1042:20396` · 组件名：ODialog 对话框 · 变体总数：24

---

## Part A：设计使用卡

### 组件概览

**ODialog 对话框**：用于展示重要信息或需要用户交互的模态窗口，通过浮层遮罩和居中弹窗吸引用户注意力。支持多种尺寸、类型和状态变体，可包含标题、内容区、操作按钮等模块化区域。

---

### 适用场景

- ✅ **确认操作**：删除确认、提交确认、重要操作二次确认
- ✅ **信息提示**：系统通知、操作结果反馈、警告提示
- ✅ **表单收集**：用户输入、快速编辑、简单表单填写
- ✅ **详情展示**：查看详细信息、预览内容、说明文档
- ✅ **多步骤流程**：向导式操作、分步引导、渐进式任务
- ❌ **不适合**：非模态提示（用 OMessage）、页面级内容（用 Drawer/OCard）、轻量提示（用 Tooltip）

---

### 变体说明

**size（尺寸）**
- `large` — 宽 720px，适合复杂表单、多步骤向导、大量内容展示
- `medium` — 宽 480px，**默认尺寸**，适合大多数场景（确认框、信息展示）
- `small` — 宽 360px，适合简单确认、简短提示、紧凑布局
- `fullscreen` — 全屏宽度，移动端或特殊场景使用

**type（类型）**
- `alert` — 警告对话框，带警告图标（⚠️），用于重要提醒或危险操作确认
- `confirm` — 确认对话框，标准样式，用于一般性确认操作
- `info` — 信息对话框，带信息图标（ℹ️），用于展示说明性内容
- `form` — 表单对话框，内容区包含表单控件，用于数据收集

**state（交互状态）**
- `normal` — 正常显示状态（默认）
- `loading` — 加载中状态，内容区显示加载动画或骨架屏
- `disabled` — 禁用状态，操作按钮置灰不可点击

**Dark（主题）**
- `off` — 浅色模式（默认）
- `on` — 深色模式，背景色、文字色、边框色自动切换为对应深色值

---

### 布局结构

> 🧩 **布局结构**：对话框垂直排列（VERTICAL Auto Layout），由遮罩层、弹窗容器、标题区、内容区、底部操作区组成。

```
ODialog（OVERLAY，全屏覆盖）
├── [遮罩层 RECTANGLE]（100vw × 100vh）
│     fill: rgba(0,0,0,0.45) → Token: `--o-color-mask`
│     点击关闭对话框（可选配置）
│
└── [弹窗容器 FRAME]（居中定位）
      Width: 360px（small）/ 480px（medium）/ 720px（large）/ 100%（fullscreen）
      Height: 自适应（最大高度 80vh）
      cornerRadius: 8px → Token: `radius_control-sm`
      fill: rgb(255,255,255) → Token: `--o-color-fill2`
      boxShadow: DIALOG_SHADOW x=0 y=16 blur=48 spread=-8 rgba(0,0,0,0.12)
      autoLayout: VERTICAL
      │
      ├── [标题区 HEADER]（固定高度 56px）
      │     autoLayoutPadding: 上下 16px，左右 24px
      │     borderBottom: 1px solid → Token: `--o-color-control3` @ 0.15
      │     ├── [图标 INSTANCE]（24×24，仅 alert/info 类型显示）
      │     │     alert: ⚠️ 警告图标 fill: `--o-color-warning1`
      │     │     info: ℹ️ 信息图标 fill: `--o-color-primary1`
      │     └── [标题文本 PARAGRAPH]
      │           fontFamily: HarmonyHeiTi
      │           font: 18px / 26px，SemiBold
      │           fill: `--o-color-info1`
      │
      ├── [内容区 BODY]（自适应高度）
      │     autoLayoutPadding: 24px（四周）
      │     minHeight: 64px
      │     maxHeight: 60vh（超出滚动）
      │     ├── [内容文本 PARAGRAPH]（confirm/alert 类型）
     │     │     fontFamily: HarmonyHeiTi
     │     │     font: 16px / 24px，Regular
     │     │     fill: `--o-color-info2`
      │     │     textAlign: left / center（根据 type 决定）
      │     └── [表单控件 × N]（form 类型）
      │           OInput / OSelect / OCheckbox 等
      │
      └── [底部操作区 FOOTER]（固定高度 64px）
            autoLayoutPadding: 上下 16px，左右 24px
            borderTop: 1px solid → Token: `--o-color-control3` @ 0.15
            autoLayout: HORIZONTAL，justifyContent: flex-end
            gap: 12px → Token: `gap-3`
            ├── [取消按钮 OButton]（variant="outline"）
            │     text: "取消"
            │     onClick: 关闭对话框
            └── [确认按钮 OButton]（variant="solid" color="brand"）
                  text: "确定" / "确认" / "删除"（根据场景）
                  onClick: 执行确认操作
```

---

### 组合搭配

> 🔗 **常见搭配**：
> - **OButton 触发**：通过按钮 click 事件打开对话框
> - **表单组件**：form 类型中配合 OInput、OSelect、OTextarea、ORadio、OCheckbox
> - **OMessage 反馈**：操作完成后用 OMessage 展示成功/失败提示
> - **OTable 操作**：表格中的「删除」「编辑」操作触发 confirm 类型对话框
> - **OStep 向导**：多步骤流程中使用 dialog 作为每个步骤的操作容器
> - **OLoading 加载**：loading 状态下在内容区显示 OLoading 组件

---

### 设计稿识别指南

> 🔍 **识别特征**：
> - 浮层容器（FRAME），圆角 8px，阴影明显（blur 48px, spread -8px）
> - 遮罩层（RECTANGLE），半透明黑色背景（rgba(0,0,0,0.45)），100% 尺寸覆盖
> - 三段式结构：顶部标题区（56px 高，底部分割线）+ 中间内容区（padding 24px）+ 底部操作区（64px 高，顶部分割线）
> - 底部右侧对齐的两个 OButton 实例（取消 + 确认），间距 12px
> - 标题左侧可选图标（alert 类型为警告图标 ⚠️，info 类型为信息图标 ℹ️）
> - 整体宽度为 360px / 480px / 720px 三种之一，最大高度 80vh

> 🔄 **易混淆组件**：
> - 与 **Drawer 抽屉**：Dialog 是居中模态窗口，Drawer 从侧边滑出；Dialog 用于重要操作，Drawer 用于辅助信息或不打断主流程的内容
> -与 **Popover 气泡卡片**：Popover 是非模态轻量浮层，无遮罩层；Dialog 有遮罩层且强制聚焦
> - 与 **OMessage 消息提示**：Message 是自动消失的非阻塞提示，Dialog 需要用户主动交互才能关闭
> - 与 **Tooltip 文字提示**：Tooltip 仅展示简短文字，无操作按钮；Dialog 可承载复杂内容和交互

---

### 响应式行为

参照 [栅格规范](../SKILL.md#数据资源) 中的断点定义，ODialog 的响应式行为如下：

| 断点 | 行为 |
|------|------|
| **Desktop (≥1200px)** | 保持设定尺寸（small/medium/large），居中显示 |
| **Tablet (768-1199px)** | medium ↔ small 自动降级，large ↔ medium 自动降级 |
| **Mobile (<768px)** | 强制 fullscreen 或 small 尺寸，宽度 100%，左右 padding 16px |

> **注意**：Dialog 的响应式主要通过 size 变体切换实现，不支持弹性缩放。移动端建议使用 fullscreen 变体以获得更好的用户体验。

---

## Part B：规格速查参考

### 变体索引表

| 变体属性 | 可选值 | 默认值 | 视觉差异 |
|---------|--------|--------|---------|
| size | `small` / `medium` / `large` / `fullscreen` | `medium` | 弹窗宽度：360px / 480px / 720px / 100% |
| type | `alert` / `confirm` / `info` / `form` | `confirm` | 图标、内容区布局、按钮文案差异 |
| state | `normal` / `loading` / `disabled` | `normal` | loading 显示加载动画，disabled 按钮置灰 |
| Dark | `off` / `on` | `off` | 背景色、文字色、边框色切换深色主题 |

---

### 布局规格

#### 弹窗容器规格

| 规格项 | Small | Medium | Large | Fullscreen | Token |
|--------|-------|--------|-------|------------|-------|
| 宽度 | 360px | 480px | 720px | 100% | — |
| 最大高度 | 80vh | 80vh | 80vh | 100vh | — |
| 圆角 | 8px | 8px | 8px | 0px | `radius_control-sm` |
| 阴影 | x=0 y=16 blur=48 spread=-8 rgba(0,0,0,0.12) | 同左 | 同左 | 无 | — |
| 内边距（整体） | 0px | 0px | 0px | 0px | — |

#### 标题区（Header）

| 规格项 | 值 | Token |
|--------|---|-------|
| 高度 | 56px | — |
| 内边距（上/下） | 16px | — |
| 内边距（左/右） | 24px | — |
| 底部分割线 | 1px solid | `--o-color-control3` @ 0.15 |
| 标题字号 | 18px | `font_size-text3` |
| 标题行高 | 26px | `line_height-text3` |
| 标题字重 | SemiBold | `font_weight-semibold` |
| 标题颜色 | Light: rgb(0,0,0) / Dark: rgb(255,255,255) | `--o-color-info1` |
| 图标尺寸 | 24×24px | `icon_size_control-default` |

#### 内容区（Body）

| 规格项 | 值 | Token |
|--------|---|-------|
| 内边距（四周） | 24px | — |
| 最小高度 | 64px | — |
| 最大高度 | 60vh（超出滚动） | — |
| 内容字号 | 16px | `font_size-text2` |
| 内容行高 | 24px | `line_height-text2` |
| 内容字重 | Regular | `font_weight-regular` |
| 内容颜色 | Light: rgb(78,89,104) / Dark: rgb(167,172,181) | `--o-color-info2` |
| 文本对齐 | left（form） / center（confirm/alert） | — |

#### 底部操作区（Footer）

| 规格项 | 值 | Token |
|--------|---|-------|
| 高度 | 64px | — |
| 内边距（上/下） | 16px | — |
| 内边距（左/右） | 24px | — |
| 顶部分割线 | 1px solid | `--o-color-control3` @ 0.15 |
| 按钮间距 | 12px | `gap-3` |
| 按钮对齐 | flex-end（右对齐） | — |
| 取消按钮 | variant="outline" | — |
| 确认按钮 | variant="solid", color="brand" | — |

#### 遮罩层（Overlay）

| 规格项 | 值 | Token |
|--------|---|-------|
| 尺寸 | 100vw × 100vh | — |
| 背景颜色 | rgba(0,0,0,0.45) | `--o-color-mask` |
| 点击行为 | 关闭对话框（可配置） | — |
| z-index | 1000（高于其他元素） | — |

---

### 颜色 Token 映射

#### 弹窗容器

| 区域 | Light 模式 | Dark 模式 | Token | RGB 值 |
|------|-----------|----------|-------|--------|
| 容器背景 | `white` | `grey-4` | `--o-color-fill2` | rgb(255,255,255) → rgb(36,36,39) |
| 容器圆角 | 8px | 8px | `radius_control-sm` | — |
| 容器阴影 | rgba(0,0,0,0.12) | rgba(0,0,0,0.35) | — | x=0 y=16 blur=48 spread=-8 |

#### 标题区

| 区域 | Light 模式 | Dark 模式 | Token | RGB 值 |
|------|-----------|----------|-------|--------|
| 标题文字 | `grey-14` (rgb(0,0,0)) | `grey-14` (rgb(255,255,255)) | `--o-color-info1` | — |
| 分割线 | `grey-14 @ 0.15` | `grey-14 @ 0.15` | `--o-color-control3` | — |
| Alert 图标 | `warning-6` (rgb(234,135,50)) | `warning-5` (rgb(239,163,82)) | `--o-color-warning1` | — |
| Info 图标 | `brand-6` (rgb(0,47,167)) | `brand-5` (rgb(73,122,248)) | `--o-color-primary1` | — |

#### 内容区

| 区域 | Light 模式 | Dark 模式 | Token | RGB 值 |
|------|-----------|----------|-------|--------|
| 正文文字 | `grey-10` (rgb(78,89,104)) | `grey-8` (rgb(167,172,181)) | `--o-color-info2` | — |
| 辅助说明文字 | `grey-9` (rgb(134,142,156)) | `grey-7` (rgb(117,123,139)) | `--o-color-info3` | — |

#### 底部操作区

| 区域 | Light 模式 | Dark 模式 | Token | RGB 值 |
|------|-----------|----------|-------|--------|
| 分割线 | `grey-14 @ 0.15` | `grey-14 @ 0.15` | `--o-color-control3` | — |
| 取消按钮文字 | `brand-6` (rgb(0,47,167)) | `brand-5` (rgb(73,122,248)) | `--o-color-primary1` | — |
| 取消按钮描边 | `brand-6 @ 1px` | `brand-5 @ 1px` | `--o-color-primary1` | — |
| 确认按钮背景 | `brand-6` (rgb(0,47,167)) | `brand-5` (rgb(73,122,248)) | `--o-color-primary1` | — |
| 确认按钮文字 | `white` | `white` | `--o-color-fill2` | — |
| Disabled 按钮 | `brand-3` (rgb(132,161,220)) | `brand-3` (rgb(29,51,120)) | `--o-color-primary4` | — |

#### 遮罩层

| 区域 | Light/Dark 模式 | Token | RGB 值 |
|------|----------------|-------|--------|
| 遮罩背景 | rgba(0,0,0,0.45) | `--o-color-mask` | — |

---

### 字体样式映射

| 使用场景 | 字号 Token | 行高 Token | 字重 Token | 字号值 | 行高值 |
|---------|-----------|-----------|-----------|-------|-------|
| 标题文字 | `font_size-text3` | `line_height-text3` | `font_weight-semibold` | 18px | 26px |
| 正文内容 | `font_size-text2` | `line_height-text2` | `font_weight-regular` | 16px | 24px |
| 辅助说明 | `font_size-text2` | `line_height-text2` | `font_weight-regular` | 16px | 24px |
| 按钮文字 | `font_size-text2` | `line_height-text2` | `font_weight-semibold` | 16px | 24px |

字体族：`HarmonyHeiTi`（Regular/SemiBold）

---

### 图标资源

本 skill 使用 OpenDesign 图标库中的标准图标，位于 [`references/assets/dialog/`](../references/assets/dialog/)（如需自定义图标可从此路径引用）。

| 文件 | 对应类型 | SVG 文件尺寸 | 说明 |
|------|---------|------------|------|
| [`alert-icon.svg`](../references/assets/dialog/alert-icon.svg) | `type=alert` | 24×24 | 警告图标（⚠️），fill: `--o-color-warning1` |
| [`info-icon.svg`](../references/assets/dialog/info-icon.svg) | `type=info` | 24×24 | 信息图标（ℹ️），fill: `--o-color-primary1` |
| [`close-icon.svg`](../references/assets/dialog/close-icon.svg) | 关闭按钮 | 24×24 | 关闭图标（✕），fill: `--o-color-info3` |

> **HTML 内联使用注意**：SVG 文件尺寸为 24×24，内联时保持 `viewBox="0 0 24 24"`、`width="24" height="24"`。Alert 和 Info 图标仅在对应对话框类型中显示于标题左侧。

---

### 组件层级结构

```
ODialog（OVERLAY，fixed 定位，z-index: 1000）
│  GUID: 1042:20396（基础组件节点）
│  Width: 100vw | Height: 100vh
│  Position: fixed, top: 0, left: 0
│
├── [遮罩层 Overlay RECTANGLE]
│   GUID: （遮罩层子节点）
│   Width: 100vw | Height: 100vh
│   fill: rgba(0,0,0,0.45) → Token: `--o-color-mask`
│   onClick: onClose（可配置是否点击关闭）
│
└── [弹窗容器 Dialog FRAME]
    GUID: 1042:20396（主容器）
    Width: 480px（medium 默认）| Height: auto（max 80vh）
    cornerRadius: 8px → Token: `radius_control-sm`
    fill: rgb(255,255,255) → Token: `--o-color-fill2`
    boxShadow: x=0 y=16 blur=48 spread=-8 rgba(0,0,0,0.12)
    Position: absolute, top: 50%, left: 50%, transform: translate(-50%, -50%)
    Auto Layout: VERTICAL
    │
    ├── [标题区 Header FRAME]
    │   GUID: （Header 子节点）
    │   Width: 100% | Height: 56px
    │   Auto Layout: HORIZONTAL, alignItems: center
    │   autoLayoutPadding: 16px（top/bottom）, 24px（left/right）
    │   borderBottom: 1px solid → Token: `--o-color-control3` @ 0.15
    │   │
    │   ├── [图标 Icon INSTANCE]（仅 alert/info 类型）
    │   │   GUID: （图标实例）
    │   │   Width: 24px | Height: 24px
    │   │   marginRight: 8px
    │   │   fill: alert=`--o-color-warning1` / info=`--o-color-primary1`
    │   │
    │   └── [标题文本 Title PARAGRAPH]
    │       GUID: （标题文本节点）
    │       Width: auto | Height: auto
    │       fontFamily: HarmonyHeiTi
    │       fontStyle: SemiBold
    │       fontSize: 18px → Token: `font_size-text3`
    │       lineHeight: 26px → Token: `line_height-text3`
    │       fill: rgb(0,0,0) → Token: `--o-color-info1`
    │       nodeText: "标题"
    │
    ├── [内容区 Body FRAME]
    │   GUID: （Body 子节点）
    │   Width: 100% | Height: auto（maxHeight: 60vh）
    │   Auto Layout: VERTICAL
    │   autoLayoutPadding: 24px（四周）
    │   overflow: auto（超出滚动）
    │   │
    │   ├── [内容文本 Content PARAGRAPH]（confirm/alert 类型）
    │   │   GUID: （内容文本节点）
    │   │   Width: 100% | Height: auto
    │   │   fontFamily: HarmonyHeiTi
    │   │   fontStyle: Regular
    │   │   fontSize: 16px → Token: `font_size-text2`
    │   │   lineHeight: 24px → Token: `line_height-text2`
    │   │   fill: rgb(78,89,104) → Token: `--o-color-info2`
    │   │   textAlign: center（confirm/alert）/ left（form）
    │   │   nodeText: "对话框内容..."
    │   │
    │   └── [表单控件 × N]（form 类型）
    │       OInput / OSelect / OCheckbox / ORadio / OTextarea 等
    │       具体规格见各组件 Skill
    │
    └── [底部操作区 Footer FRAME]
        GUID: （Footer 子节点）
        Width: 100% | Height: 64px
        Auto Layout: HORIZONTAL, justifyContent: flex-end, alignItems: center
        autoLayoutPadding: 16px（top/bottom）, 24px（left/right）
        borderTop: 1px solid → Token: `--o-color-control3` @ 0.15
        gap: 12px → Token: `gap-3`
        │
        ├── [取消按钮 Cancel OButton INSTANCE]
        │   GUID: （取消按钮实例）
        │   Width: auto | Height: 32px
        │   variant: "outline"
        │   nodeText: "取消"
        │   onClick: onCancel
        │
        └── [确认按钮 Confirm OButton INSTANCE]
            GUID: （确认按钮实例）
            Width: auto | Height: 32px
            variant: "solid"
            color: "brand"
            nodeText: "确定"
            onClick: onConfirm
```

---

### 变体 componentKey 速查

| 变体组合 | node_id | 尺寸 | 说明 |
|---------|---------|------|------|
| size=small, type=confirm, state=normal, Dark=off | `1042:20396-001` | 360×auto | 小型确认框，浅色 |
| size=medium, type=confirm, state=normal, Dark=off | `1042:20396` | 480×auto | 中型确认框，浅色（默认） |
| size=large, type=confirm, state=normal, Dark=off | `1042:20396-002` | 720×auto | 大型确认框，浅色 |
| size=medium, type=alert, state=normal, Dark=off | `1042:20396-010` | 480×auto | 警告对话框，浅色 |
| size=medium, type=info, state=normal, Dark=off | `1042:20396-020` | 480×auto | 信息对话框，浅色 |
| size=medium, type=form, state=normal, Dark=off | `1042:20396-030` | 480×auto | 表单对话框，浅色 |
| size=medium, type=confirm, state=loading, Dark=off | `1042:20396-100` | 480×auto | 加载状态，浅色 |
| size=medium, type=confirm, state=disabled, Dark=off | `1042:20396-200` | 480×auto | 禁用状态，浅色 |
| size=medium, type=confirm, state=normal, Dark=on | `1042:20396-off` | 480×auto | 中型确认框，深色 |

> **注意**：componentKey 需从 Pixso 组件面板获取实际值，上述为示例编号。完整变体组合共 24 种（4 size × 4 type × 3 state × 2 theme，部分组合可能不存在）。

---

### Pixso 操作速查

1. **插入组件**：组件面板搜索「ODialog」或「对话框」，拖入画布
2. **切换尺寸**：右侧面板 → size 属性 → 选择 `small` / `medium` / `large` / `fullscreen`
3. **切换类型**：右侧面板 → type 属性 → 选择 `confirm` / `alert` / `info` / `form`
4. **切换状态**：右侧面板 → state 属性 → 选择 `normal` / `loading` / `disabled`
5. **切换主题**：右侧面板 → Dark 属性 → 选择 `off` / `on`
6. **修改标题**：双击进入 Header → 双击标题文本图层修改内容
7. **修改内容**：双击进入 Body → 修改文本或添加/编辑表单控件
8. **修改按钮文案**：双击进入 Footer → 双击按钮实例修改文字
9. **调整内容区高度**：选中 Body → 拖拽边缘或设置 maxHeight 约束
10. **配置关闭方式**：设置遮罩层 onClick 事件或添加关闭按钮

---

### 注意事项

- **模态特性**：Dialog 打开时，页面其他区域被遮罩层覆盖且不可交互，直到 Dialog 关闭
- **焦点管理**：Dialog 打开后，焦点应自动移至 Dialog 内的第一个可交互元素（通常是确认按钮）；关闭后恢复到触发元素
- **ESC 键关闭**：支持按 ESC 键关闭 Dialog（可通过配置禁用）
- **点击遮罩关闭**：默认支持点击遮罩层关闭 Dialog（可通过配置禁用）
- **滚动处理**：当内容区超过 maxHeight 时，仅在 Body 区域内部滚动，Header 和 Footer 固定不动
- **Body 最小高度**：即使内容很少，Body 也应保持至少 64px 高度以保证视觉平衡
- **按钮顺序**：Footer 区按钮从右到左依次为「确认」→「取消」（符合 F 型视觉动线）
- **按钮文案**：根据 type 和场景动态调整，如 delete 场景确认按钮应为「删除」，danger 样式
- **Loading 状态**：state=loading 时，Body 区显示 OLoading 组件，Footer 按钮设为 disabled
- **Disabled 状态**：state=disabled 时，所有交互元素（按钮、表单控件）均不可点击，降低透明度
- **Alert 类型视觉强化**：type=alert 时，标题区显示警告图标（⚠️），确认按钮可选用 danger 色（`color="danger"`）
- **Info 类型辅助说明**：type=info 时，标题区显示信息图标（ℹ️），适合展示帮助文档或操作指引
- **Form 类型校验**：type=form 时，建议在 Footer 左侧增加「重置」按钮，并集成表单校验逻辑
- **Fullscreen 移动端适配**：size=fullscreen 时，容器圆角改为 0px，左右 padding 增加至 16px
- **Z-index 层级**：确保 Dialog 的 z-index（1000）高于页面上其他浮动元素（Dropdown、Popover 等）
- **动画效果**：打开/关闭时建议添加淡入淡出+轻微缩放动画（opacity 0→1, scale 0.95→1, duration 200ms）
- **无障碍访问**：需设置 role="dialog"，aria-modal="true"，aria-labelledby 指向标题，aria-describedby 指向内容描述

---

## Part C：交互与状态

### 交互状态

| 元素 | 状态 | 视觉表现 |
|------|------|---------|
| 弹窗容器 | Normal | 正常显示，阴影完整，圆角 8px |
| 弹窗容器 | Loading | 内容区显示加载动画，Footer 按钮 disabled |
| 弹窗容器 | Disabled | 整体透明度降至 0.6，所有交互元素 disabled |
| 遮罩层 | Normal | 半透明黑色背景（rgba(0,0,0,0.45)），可点击关闭 |
| 遮罩层 | 不可关闭 | 同上视觉效果，但点击不触发关闭 |
| 确认按钮 | Hover | 背景色加深（`--o-color-primary1` → `--o-color-primary2`） |
| 确认按钮 | Active | 背景色进一步加深（`--o-color-primary2` → `--o-color-primary3`） |
| 确认按钮 | Disabled | 背景变为 `--o-color-primary4`，cursor: not-allowed |
| 取消按钮 | Hover | 描边和文字色加深（`--o-color-primary1` → `--o-color-primary2`） |
| 取消按钮 | Active | 描边和文字色进一步加深 |
| 取消按钮 | Disabled | 描边和文字色变为 `--o-color-primary4`, cursor: not-allowed |
| 关闭按钮 | Hover | 图标颜色加深（`--o-color-info3` → `--o-color-info2`） |
| 表单控件 | Normal/Focus/Error | 见各组件 Skill（OInput/ORadio 等） |

---

### 状态切换逻辑

- **Normal → Loading**：Body 内容替换为 OLoading 组件，Footer 所有按钮变为 disabled 态
- **Loading → Normal**：恢复 Body 原始内容，Footer 按钮恢复正常态
- **Normal → Disabled**：整体透明度降至 0.6，所有交互元素 disabled，遮罩层仍可点击关闭（除非特别配置）
- **Disabled → Normal**：恢复正常透明度和交互能力
- **打开动画**：遮罩层 opacity 0→1（150ms），弹窗容器 opacity 0→1 + scale 0.95→1（200ms），ease-out 缓动
- **关闭动画**：反向执行（遮罩层 opacity 1→0，弹窗容器 opacity 1→0 + scale 1→0.95），动画结束后从 DOM 移除
- **Focus 管理**：打开时 Focus 移入 Dialog 内，关闭时 Focus 返回触发元素（Trap Focus）

---

### 键盘交互

| 按键 | 行为 | 条件 |
|------|------|------|
| `ESC` | 关闭 Dialog | 默认启用（可配置禁用） |
| `Tab` | 在 Dialog 内可交互元素间循环切换 | Focus Trap 生效 |
| `Shift + Tab` | 反向循环切换 Focus | Focus Trap 生效 |
| `Enter` | 触发确认按钮点击事件 | Focus 在确认按钮上时 |
| `Space` | 触发当前 Focus 元素点击 | 按钮/复选框/单选框等 |

> **Focus 顺序**（Tab 键循环路径）：
> 关闭按钮（如有）→ 标题链接（如有）→ Body 内第一个表单控件 → ... → Body 内最后一个表单控件 → 取消按钮 → 确认按钮 → 回到关闭按钮

---

### 不同类型的交互细节

#### Confirm 类型（标准确认框）

- **典型流程**：用户触发操作 → 弹出 Confirm Dialog → 用户阅读内容 → 点击「确定」或「取消」
- **按钮默认文案**：「取消」+「确定」
- **内容区文本对齐**：居中对齐（textAlign: center）
- **使用场景**：删除确认、提交确认、退出确认等二选一决策

#### Alert 类型（警告框）

- **视觉增强**：标题左侧显示 ⚠️ 警告图标（24×24px，fill: `--o-color-warning1`）
- **按钮样式**：确认按钮可选用 `color="danger"` 突出风险操作
- **按钮文案建议**：「取消」+「删除」/「继续」/「确认」
- **内容区文本对齐**：居中对齐
- **使用场景**：危险操作确认（删除数据、不可逆操作）、重要警告通知

#### Info 类型（信息展示框）

- **视觉增强**：标题左侧显示 ℹ️ 信息图标（24×24px，fill: `--o-color-primary1`）
- **按钮配置**：通常仅需「知道了」或「确定」按钮（单个按钮居中显示）
- **内容区文本对齐**：左对齐（适合长文本阅读）
- **使用场景**：功能说明、操作指引、版本更新提示、帮助文档

#### Form 类型（表单对话框）

- **内容区**：包含表单控件（OInput、OSelect、OTextarea、ORadio、OCheckbox 等）
- **按钮配置**：「取消」+「确定」（确定按钮触发表单提交和校验）
- **可选额外按钮**：Footer 左侧增加「重置」按钮清空表单
- **内容区文本对齐**：左对齐
- **表单校验**：点击「确定」时先校验，校验失败显示错误提示，不关闭 Dialog
- **使用场景**：快速新建/编辑、收集用户输入、配置设置等

---

### 交互状态完整规格

#### 弹窗容器的 4 种状态

| 交互状态 | 容器表现 | 遮罩层 | 内容区 | 操作按钮 |
|---------|---------|--------|--------|---------|
| **Normal（默认）** | 正常显示，完整阴影 | 半透明黑色，可点击关闭 | 显示正常内容 | 正常可点击 |
| **Loading（加载中）** | 正常显示 | 半透明黑色，可点击关闭 | 显示 OLoading 动画 | Disabled 态 |
| **Disabled（禁用）** | 整体透明度 0.6 | 半透明黑色，可点击关闭 | 显示正常内容但不可交互 | Disabled 态 |
| **Closing（关闭中）** | 执行关闭动画 | 渐隐至透明 | 保持内容不变 | 不可交互 |

#### 确认按钮的 5 种交互态

| 交互状态 | 背景色 | 文字色 | 是否可点击 |
|---------|--------|--------|-----------|
| **Default** | `--o-color-primary1` | white（`--o-color-fill2`） | ✅ |
| **Hover** | `--o-color-primary2` | white（`--o-color-fill2`） | ✅ |
| **Pressed/Active** | `--o-color-primary3` | white（`--o-color-fill2`） | ✅ |
| **Focus** | `--o-color-primary1` + focus ring | white（`--o-color-fill2`） | ✅ |
| **Disabled** | `--o-color-primary4` | white（`--o-color-fill2`） | ❌ |

#### 取消按钮的 5 种交互态

| 交互状态 | 背景 | 描边/文字色 | 是否可点击 |
|---------|------|------------|-----------|
| **Default** | transparent | `--o-color-primary1` | ✅ |
| **Hover** | `--o-color-control2-light` | `--o-color-primary2` | ✅ |
| **Pressed/Active** | `--o-color-control3-light` | `--o-color-primary3` | ✅ |
| **Focus** | transparent + focus ring | `--o-color-primary1` | ✅ |
| **Disabled** | transparent | `--o-color-primary4` | ❌ |

> **交互状态说明**：
> - **Default**：组件的正常显示状态
> - **Hover**：鼠标悬停时的状态，提供视觉反馈
> - **Pressed/Active**：鼠标点击按下时的状态
> - **Focus**：获得键盘焦点时的状态（显示 focus ring 轮廓）
> - **Disabled**：不可操作的状态，使用降级颜色

---

## Part D：设计变量绑定

### 推荐绑定变量

#### 容器与布局

| 元素 | 属性 | 推荐变量 Token |
|------|------|---------------|
| 弹窗容器背景 | fill | `--o-color-fill2` |
| 弹窗容器圆角 | cornerRadius | `radius_control-sm` |
| 弹窗容器阴影 | boxShadow | 自定义 DIALOG_SHADOW（非全局 Token） |
| 遮罩层背景 | fill | `--o-color-mask` |
| 标题区分割线 | stroke/borderColor | `--o-color-control3` @ 0.15 |
| 底部分割线 | stroke/borderColor | `--o-color-control3` @ 0.15 |
| Footer 按钮间距 | gap | `gap-3` |

#### 文字颜色

| 元素 | 属性 | 推荐变量 Token |
|------|------|---------------|
| 标题文字 | fill | `--o-color-info1` |
| 正文内容 | fill | `--o-color-info2` |
| 辅助说明 | fill | `--o-color-info3` |
| 确认按钮文字 | fill | `--o-color-fill2`（白色） |
| 取消按钮文字 | fill | `--o-color-primary1` |
| Disabled 按钮文字 | fill | `--o-color-fill2`（白色） |

#### 图标颜色

| 元素 | 属性 | 推荐变量 Token |
|------|------|---------------|
| Alert 警告图标 | fill | `--o-color-warning1` |
| Info 信息图标 | fill | `--o-color-primary1` |
| 关闭图标 | fill | `--o-color-info3` |

#### 按钮颜色

| 元素 | 属性 | 推荐变量 Token |
|------|------|---------------|
| 确认按钮背景（Enabled） | fill | `--o-color-primary1` |
| 确认按钮背景（Hover） | fill | `--o-color-primary2` |
| 确认按钮背景（Active） | fill | `--o-color-primary3` |
| 确认按钮背景（Disabled） | fill | `--o-color-primary4` |
| 取消按钮描边/文字（Enabled） | stroke/fill | `--o-color-primary1` |
| 取消按钮描边/文字（Hover） | stroke/fill | `--o-color-primary2` |
| 取消按钮描边/文字（Disabled） | stroke/fill | `--o-color-primary4` |
| Danger 确认按钮背景（Alert 类型） | fill | `--o-color-danger1` |

#### 字体样式

| 元素 | 属性 | 推荐变量 Token | 值 |
|------|------|---------------|-----|
| 标题文字 | fontSize | `font_size-text3` | 18px |
| 标题文字 | lineHeight | `line_height-text3` | 26px |
| 标题文字 | fontStyle | `font_weight-medium` | Medium |
| 正文/按钮文字 | fontSize | `font_size-text2` | 14px |
| 正文/按钮文字 | lineHeight | `line_height-text2` | 22px |
| 正文文字 | fontStyle | `font_weight-regular` | Regular |
| 按钮文字 | fontStyle | `font_weight-medium` | Medium |

#### 尺寸规格

| 元素 | 属性 | 推荐值 | Token |
|------|------|--------|-------|
| Small 宽度 | width | 360px | — |
| Medium 宽度 | width | 480px | — |
| Large 宽度 | width | 720px | — |
| Fullscreen 宽度 | width | 100% | — |
| 标题区高度 | height | 56px | — |
| 底部区高度 | height | 64px | — |
| 内容区最小高度 | minHeight | 64px | — |
| 内容区最大高度 | maxHeight | 60vh | — |
| 标题区内边距（上下） | paddingTop/Bottom | 16px | — |
| 标题区内边距（左右） | paddingLeft/Right | 24px | — |
| 内容区内边距（四周） | padding | 24px | — |
| 底部区内边距（上下） | paddingTop/Bottom | 16px | — |
| 底部区内边距（左右） | paddingLeft/Right | 24px | — |
| 图标尺寸 | width/height | 24px | `icon_size_control-default` |
| 图标与标题间距 | marginRight | 8px | `gap-2` |

---

### 特殊场景变量绑定

#### Alert 类型的 Danger 按钮

当 Dialog 用于危险操作（如删除确认）时，确认按钮应使用 danger 色系：

| 元素 | 属性 | 推荐变量 Token |
|------|------|---------------|
| 危险确认按钮背景 | fill | `--o-color-danger1` |
| 危险确认按钮 Hover | fill | `--o-color-danger2` |
| 危险确认按钮 Active | fill | `--o-color-danger3` |
| 危险确认按钮 Disabled | fill | `--o-color-danger4` |

#### Loading 状态变量

| 元素 | 属性 | 推荐变量 Token |
|------|------|---------------|
| 加载指示器 | 使用 OLoading 组件 | 参考 OLoading Skill |
| 加载遮罩 | fill | `rgba(255,255,255,0.8)` |
| 整体透明度 | opacity | 0.6（Disabled 状态） |

#### 深色主题变量映射

当 `Dark=on` 时，以下变量自动切换：

| Light 模式 Token | Dark 模式替代值 |
|-----------------|----------------|
| `--o-color-fill2` (rgb(255,255,255)) | rgb(36,36,39) |
| `--o-color-info1` (rgb(0,0,0)) | rgb(255,255,255) |
| `--o-color-info2` (rgb(78,89,104)) | rgb(167,172,181) |
| `--o-color-info3` (rgb(134,142,156)) | rgb(117,123,139) |
| `--o-color-primary1` (rgb(0,47,167)) | rgb(73,122,248) |
| `--o-color-primary2` | rgb(54,96,225) |
| `--o-color-primary3` | rgb(107,141,245) |
| `--o-color-primary4` (rgb(132,161,220)) | rgb(29,51,120) |
| `--o-color-warning1` (rgb(234,135,50)) | rgb(239,163,82) |
| `--o-color-danger1` | rgb(235,87,87)（Light）→ rgb(244,99,99)（Dark）|

---

### 注意事项

- **Token 权威性**：所有颜色、字号、间距必须匹配上游 Token 定义，禁止硬编码 RGB 值
- **阴影特殊性**：Dialog 阴影（DIALOG_SHADOW）不是全局 Token，属于组件专属样式，保持固定值即可
- **遮罩层 Token**：`--o-color-mask` 为专用 Token，值为 rgba(0,0,0,0.45)，Light/Dark 模式相同
- **分割线透明度**：Header/Footer 的分割线使用 `--o-color-control3` 并设置 15% 透明度（@ 0.15）
- **按钮复用**：Footer 区的按钮应直接使用 OButton 组件实例（通过 componentKey 创建），确保样式一致性和可维护性
- **图标一致性**：Alert/Info 图标应使用 OpenDesign 图标库中的标准图标，保持与其他组件图标风格统一
- **响应式断点**：虽然 Dialog 主要通过 size 变体控制尺寸，但在 Tablet 断点下可考虑自动降级（large→medium, medium→small）
- **无障碍绑定**：确保 ARIA 属性正确绑定（role、aria-modal、aria-labelledby、aria-describedby），这对屏幕阅读器用户至关重要
- **动画性能**：打开/关闭动画仅针对 opacity 和 transform（scale）属性，避免触发 layout 重排，保证流畅性