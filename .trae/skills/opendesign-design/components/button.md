> ← [组件索引](../SKILL.md#组件索引) · [README](../README.md)

# OButton 按钮 · 设计 Skill

> 组件集合节点：`1042:17008` · 组件名：OButton 按钮 · 变体总数：36

---

## Part A：设计使用卡

### 组件概览

**OButton 按钮**：触发一个操作或跳转，是页面中最常见的交互入口。通过外观（实心/描边/文字）和尺寸传达操作的重要程度，通过禁用状态表达不可操作。

---

### 适用场景

- ✅ **主操作按钮**：表单提交、确认弹窗、引导用户完成核心流程，使用 `solid` 变体突出视觉权重
- ✅ **次要操作**：与主操作并排放置的取消、返回等操作，使用 `outline` 降低视觉权重
- ✅ **文字链式操作**：列表行内的编辑、删除等轻量操作，使用 `text` 减少视觉噪声
- ✅ **工具栏与紧凑布局**：空间有限时使用 `small` 尺寸
- ❌ **不适合**：纯导航跳转（用链接组件）、大段说明文字中的行内操作（用 text 链接）

---

### 变体说明

**Variant（外观类型）**
- `solid` — 实心填充，背景为品牌主色，适合最重要的主操作，每个区域最多一个
- `outline` — 描边样式，背景透明，适合与 solid 搭配的次要操作
- `text` — 无背景无边框，水平内边距为 0，点击区域保留但视觉极轻，适合列表行内操作或工具栏

**size（尺寸）**
- `large` — 高 40px，水平内边距 24px，全圆角（pill 胶囊型），字号 16px，适合突出的主操作区域
- `medium` — 高 32px，水平内边距 16px，圆角 16px，字号 14px，**默认尺寸**，适合大多数场景
- `small` — 高 28px，水平内边距 16px，全圆角（pill 胶囊型），字号 14px，适合工具栏、筛选标签

**state（状态）**
- `Enabled` — 正常可点击状态
- `Disabled` — 颜色使用禁用色 Token，不可点击；`text` 变体文字透明度降至 40%

**Dark（主题）**
- `off` — 浅色模式（默认）
- `on` — 深色模式，品牌色系在 dark 主题下自动切换为对应深色值

---

### 布局结构

> 🧩 **布局结构**：按钮整体水平排列（HORIZONTAL Auto Layout），内容区由文字标签和可选图标组成，图标和文字之间间距 8px，内容区与按钮边缘通过 padding 控制距离。

```
OButton（HORIZONTAL，自适应宽度，固定高度）
├── [浮层图标 INSTANCE]（绝对定位，靠右，16×16 或 24×24，可选）
└── [内容容器 FRAME]（HORIZONTAL，gap 8px，自适应宽高）
    ├── [图标 Icon/占位符号 INSTANCE]（固定 16×16 / 24×24，可选）
    └── [text PARAGRAPH]（自适应宽，固定行高）
```

- **整体**：水平自动布局，宽度随内容自适应（large / medium 有最小宽度约束）
- **内边距**：决定按钮在不同尺寸下的视觉胖瘦感（见规格表）
- **圆角**：`large` 和 `small` 为全圆角（100px），`medium` 为 16px 圆角
- **text 变体**：水平内边距为 0（无最小宽度约束），视觉上极轻量

---

### 组合搭配

> 🔗 **常见搭配**：
> - **主次按钮组**：`solid` + `outline` 并排，solid 在右（主操作），outline 在左（取消）
> - **操作栏**：多个 `text` 按钮横向排列，用于表格行操作（编辑 / 删除 / 查看）
> - **表单底部**：`solid`（提交） + `outline`（重置）组合，按钮组间距通常 8–12px
> - **弹窗底部**：`solid`（确认） + `outline`（取消），large 或 medium 尺寸

---

### 设计稿识别指南

> 🔍 **识别特征**：
> - 水平自动布局节点，内含文字层 + 可选图标，整体高度为 28 / 32 / 40px 之一
> - 背景为纯色填充（solid）或无填充仅描边（outline）或两者皆无（text）
> - `large` / `medium` 有最小宽度约束（96px / 80px），`text` 无约束
> - 文字颜色：solid 为白色，outline 与 text Enabled 态文字颜色不同（outline 为品牌色，text 为黑色）

> 🔄 **易混淆组件**：
> - 与 **OTab 标签页** 的区别：Tab 是导航切换控件，多个并排且有选中态联动；Button 是独立触发操作
> - 与 **文字链接** 的区别：Button 保留点击区域 padding（text 变体除外）且有统一高度约束；链接行内流动无固定高度

---

### 响应式行为

参照 [栅格规范](../SKILL.md#数据资源) 中的断点定义，OButton 无自动断点响应式行为。尺寸通过 `size` 变体手动控制，深色模式通过 `Dark` 变体手动切换。

---

## Part B：规格速查参考

### 变体索引表

| 变体属性 | 可选值 | 默认值 | 视觉差异 |
|---------|--------|--------|---------|
| Variant | `solid` / `outline` / `text` | `solid` | 背景填充 / 描边无背景 / 无装饰无水平内边距 |
| size | `large` / `medium` / `small` | `medium` | 高度 40/32/28px，圆角和字号有差异 |
| state | `Enabled` / `Disabled` | `Enabled` | 禁用时色值切换为禁用 Token，不可交互 |
| Dark | `off` / `on` | `off` | 品牌色系切换为深色主题对应值 |

---

### 布局规格

| 规格项 | large | medium | small |
|--------|-------|--------|-------|
| 高度 | 40px | 32px | 28px |
| Token（高度） | `control_size-l` | `control_size-m` | — |
| 水平内边距（左/右） | 24px | 16px | 16px |
| 垂直内边距（上/下） | 8px | 5px | 3px |
| 内容容器 gap（图标↔文字） | 8px | 8px | — |
| 最小宽度 | 96px | 80px | 无 |
| 圆角 | 100px（全圆角） | 16px | 100px（全圆角） |
| 字号 | 16px | 14px | 14px |
| Token（字号） | `font_size-text1` | `font_size-tip1` | `font_size-tip1` |
| 行高 | 24px | 22px | 22px |
| Token（行高） | `line_height-text1` | `line_height-tip1` | `line_height-tip1` |
| 图标尺寸 | 24×24px | 16×16px | 16×16px |
| Token（图标尺寸） | `icon_size_control-m` | `icon_size_control-xs` | `icon_size_control-xs` |

> **text 变体特殊规格**：水平内边距为 0（左右均为 0px），无最小宽度，高度与同 size 的其他变体相同。

---

### 颜色 Token 映射

#### Solid 变体

| 区域 | 状态 | Light 模式 Token | Dark 模式 Token | 说明 |
|------|------|-----------------|----------------|------|
| 背景 | Enabled | `--o-color-primary1` | `--o-color-primary1` | brand-6：rgb(0,47,167) → rgb(73,122,248) |
| 背景 | Disabled | `--o-color-primary4` | `--o-color-primary4` | brand-3：rgb(132,161,220) → rgb(29,51,120) |
| 文字 / 图标 | Enabled & Disabled | `white` | `white` | 始终为白色 rgb(255,255,255) |

#### Outline 变体

| 区域 | 状态 | Light 模式 Token | Dark 模式 Token | 说明 |
|------|------|-----------------|----------------|------|
| 边框 | Enabled | `--o-color-primary1` | `--o-color-primary1` | brand-6：rgb(0,47,167) → rgb(73,122,248) |
| 边框 | Disabled | `--o-color-primary4` | `--o-color-primary4` | brand-3：rgb(132,161,220) → rgb(29,51,120) |
| 文字 / 图标 | Enabled | `--o-color-primary1` | `--o-color-primary1` | 同边框色 |
| 文字 / 图标 | Disabled | `--o-color-primary4` | `--o-color-primary4` | 同边框色 |
| 背景 | — | 透明 | 透明 | 无填充 |

#### Text 变体

| 区域 | 状态 | Light 模式 Token | Dark 模式 Token | 说明 |
|------|------|-----------------|----------------|------|
| 文字 | Enabled | `grey-14` | `grey-14` | Light: rgb(0,0,0)；Dark: rgb(255,255,255) |
| 文字 | Disabled | `grey-14` @ 40% | `grey-14` @ 40% | 透明度降至 40%，不使用单独 token |
| 背景 / 边框 | — | 无 | 无 | — |

### 交互状态详细规格

#### 强调按钮 (Solid) - 以L尺寸为例

| 状态 | 填充色 | 文本样式 | 文字颜色 |
|------|--------|---------|----------|
| 默认 | `--o-color-primary1` | 鸿蒙黑体 (Regular)，16px | `--o-color-white` |
| 悬浮 | `--o-color-primary2` | 鸿蒙黑体 (Regular)，16px | `--o-color-white` |
| 按下 | `--o-color-primary3` | 鸿蒙黑体 (Regular)，16px | `--o-color-white` |
| 选中 | `--o-color-primary1` | 鸿蒙黑体 (Regular)，16px | `--o-color-white` |
| 禁用 | `--o-color-primary4` | 鸿蒙黑体 (Regular)，16px | `--o-color-white` |

#### 普通按钮 (Outline) - 以L尺寸为例

| 状态 | 描边色 | 文本样式 | 文字颜色 |
|------|--------|---------|----------|
| 默认 | `--o-color-primary1` | 鸿蒙黑体 (Regular)，16px | `--o-color-primary1` |
| 悬浮 | `--o-color-primary2` | 鸿蒙黑体 (Regular)，16px | `--o-color-primary2` |
| 按下 | `--o-color-primary3` | 鸿蒙黑体 (Regular)，16px | `--o-color-primary3` |
| 选中 | `--o-color-primary1` | 鸿蒙黑体 (Regular)，16px | `--o-color-primary1` |
| 禁用 | `--o-color-primary4` | 鸿蒙黑体 (Regular)，16px | `--o-color-primary4` |

#### 文本按钮 (Text) - 以L尺寸为例

| 状态 | 图标颜色/尺寸 | 文本样式 | 文字颜色 |
|------|-------------|---------|----------|
| 默认 | 24×24px，`--o-color-info1` | 鸿蒙黑体 (Regular)，16px | `--o-color-info1` |
| 悬浮 | 24×24px，`--o-color-primary2` | 鸿蒙黑体 (Regular)，16px | `--o-color-primary2` |
| 按下 | 24×24px，`--o-color-primary3` | 鸿蒙黑体 (Regular)，16px | `--o-color-primary3` |
| 选中 | 24×24px，`--o-color-primary1` | 鸿蒙黑体 (Regular)，16px | `--o-color-primary1` |
| 禁用 | 24×24px，`--o-color-info4` | 鸿蒙黑体 (Regular)，16px | `--o-color-info4` |

#### 图标按钮 (Icon Only) - 以L尺寸为例

| 状态 | 图标颜色/尺寸 | 背景色/尺寸 | 说明 |
|------|-------------|------------|------|
| 默认 | 24×24px，`--o-color-info2` | 无背景 | 仅图标显示 |
| 悬浮 | 24×24px，`--o-color-primary2` | 24×24px，`--o-color-control2-light` | 显示浅色背景浮层 |
| 按下 | 24×24px，`--o-color-primary3` | 24×24px，`--o-color-control2-light` | 显示浅色背景浮层 |
| 选中 | 24×24px，`--o-color-primary1` | 无背景 | 高亮品牌色 |
| 禁用 | 24×24px，`--o-color-info4` | 无背景 | 使用禁用色 |

---

### 字体样式映射

| 使用场景 | 字号 Token | 行高 Token | 字重 Token | 字号值 | 行高值 |
|---------|-----------|-----------|-----------|-------|-------|
| `large` 按钮文字 | `font_size-text1` | `line_height-text1` | `font_weight-regular` | 16px | 24px |
| `medium` / `small` 按钮文字 | `font_size-tip1` | `line_height-tip1` | `font_weight-regular` | 14px | 22px |

字体族：`font_family`（HarmonyOS / HarmonyHeiTi Regular）

---

### 组件层级结构

```
OButton（HORIZONTAL，自适应宽，固定高）
│  paddingTop/Bottom: 见尺寸规格 | paddingLeft/Right: 见尺寸规格
│  cornerRadius: 100px（large/small）/ 16px（medium）
│  fill: color-primary1（solid Enabled）/ color-primary4（solid Disabled）/ 无（outline/text）
│  stroke: color-primary1（outline Enabled）/ color-primary4（outline Disabled）/ 无（solid/text），strokeWeight: 1px INSIDE
│
├── [浮层图标 INSTANCE]（绝对定位，靠右，icon_size_control-m 或 icon_size_control-xs，可选）
│
└── [内容容器 FRAME]（HORIZONTAL，gap 8px，自适应宽高）
    ├── [图标 Icon/占位符号 INSTANCE]（固定尺寸，可选）
    │     fill: white（solid）/ color-primary1（outline Enabled）/ grey-14（text Enabled）
    └── [text PARAGRAPH]（自适应宽，固定行高）
          font: font_family Regular | font_size-text1 or font_size-tip1
          fill: white（solid）/ color-primary1（outline Enabled）/ grey-14（text Enabled）
               color-primary4（solid/outline Disabled）/ grey-14 @ 40%（text Disabled）
```

---

### 变体 componentKey 速查

| 变体组合 | node_id | componentKey |
|---------|---------|-------------|
| size=large, state=Enabled, Dark=off, Variant=solid | `1042:17060` | `1a36bbfd374263c7b4644d4490e8bb65450c2f57` |
| size=medium, state=Enabled, Dark=off, Variant=solid | `1042:17034` | `eab0e6c545d27376ba67aef7dd233fc58c476234` |
| size=small, state=Enabled, Dark=off, Variant=solid | `1042:17044` | `a6bd542b1e147d36d0c84bae58270b3f3eacc4ec` |
| size=large, state=Enabled, Dark=off, Variant=outline | `1042:17066` | `e60861de10fad85d4d138b3d0b14eaee30d76b42` |
| size=medium, state=Enabled, Dark=off, Variant=outline | `1042:17076` | `3cd0c2f583c60180725e297d3529d67ed62fcad8` |
| size=medium, state=Enabled, Dark=off, Variant=text | `1042:17133` | `2b717cf5c4d422ff4385f6f215d52fa4dcc1c04a` |

---

### Pixso 操作速查

1. **插入组件**：组件面板搜索「OButton」，拖入画布；或 AI 调用 `create_instance("1a36bbfd374263c7b4644d4490e8bb65450c2f57")`（以 large/solid 为例）
2. **切换外观**：右侧面板 → Variant 属性 → 选择 `solid` / `outline` / `text`
3. **切换尺寸**：Variant 属性 → size 选择 `large` / `medium` / `small`
4. **切换主题**：Variant 属性 → Dark 选择 `on` / `off`
5. **修改文字**：双击进入组件实例 → 双击 text 图层修改内容
6. **隐藏图标**：双击进入实例 → 选中图标层 → 设为不可见

---

### 注意事项

- `large` 和 `small` 均为全圆角（cornerRadius: 100px），视觉上是胶囊形；`medium` 是 16px 圆角矩形——三者圆角**不同**，切换 size 时注意视觉一致性
- 同一区域使用 `solid` 按钮不超过 1 个，多个主操作并列时降级为 `outline`
- `Disabled` 状态仅改变颜色 Token（切换为 `--o-color-primary4`），不改变尺寸和圆角
- `text` 变体水平内边距为 0，宽度完全由内容决定，无最小宽度约束——不适合用于需要固定宽度的布局
- 内部图标层为占位符（Icon/占位符号），替换时进入实例修改图标内容，不要直接绑定外部图标组件
- 按钮宽度默认随文字自适应，如需固定宽度需手动覆盖 Auto Layout 的自适应规则

---

## Part C：Assets 图标资源

> 路径：`references/assets/public icons/`

| 文件名 | 使用场景 | 说明 |
|--------|---------|------|
| `icon-占位.svg` | 按钮内左侧图标（默认占位） | 可替换为任意业务图标，设计稿中作为占位符存在 |
| `icon-右箭头.svg` | `text` 变体按钮的右侧图标，表示继续/跳转 | 主要配合 Variant=text 使用，引导用户进入下一步或查看详情 |
| `icon-外链.svg` | 按钮右侧浮层图标 / 表示跳转到外部链接 | 用于打开新标签页的操作按钮 |

**图标颜色与尺寸规则**

图标颜色和尺寸完全跟随 OButton 组件内部定义，**不单独设置**：

| size | 图标尺寸 | Token |
|------|---------|-------|
| `large` | 24×24px | `icon_size_control-m` |
| `medium` | 16×16px | `icon_size_control-xs` |
| `small` | 16×16px | `icon_size_control-xs` |

| Variant / state | 图标颜色 Token |
|----------------|--------------|
| solid · Enabled & Disabled | `white` |
| outline · Enabled | `--o-color-primary1` |
| outline · Disabled | `--o-color-primary4` |
| text · Enabled | `grey-14` |
| text · Disabled | `grey-14` @ 40% |