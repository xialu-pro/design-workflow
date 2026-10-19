> ← [组件索引](../SKILL.md#组件索引) · [README](../README.md)

# OBreadcrumb 面包屑

## 组件概述

面包屑导航，用于展示页面层级路径，帮助用户了解当前位置并快速返回上级页面。

- **节点 ID**：`1042:13137`
- **Pixso 链接**：https://pixso.cn/app/design/JZkjW0mhmT61Mtd98dCfBw?item-id=1042:13137
- **组件类型**：ComponentSet（StateGroup）
- **尺寸**：454 × 24（单行面包屑）

---

## Props 变体

| Prop | 可选值 | 说明 |
|------|--------|------|
| `Dark` | `off`（默认）、`on` | 浅色 / 深色模式 |

对应变体节点：

| 变体 | 节点 ID |
|------|---------|
| `Dark=off` | `1042:13138` |
| `Dark=on` | `1042:13159` |

---

## 层级结构

面包屑由多个「层级项」组成，从左到右排列，末尾为「当前页面」文本：

```
OBreadcrumb (HORIZONTAL auto-layout, gap=4)
├── 层级一 (Group)
│   └── 容器 (Frame, HORIZONTAL, gap=4)
│       ├── Text "首页"
│       └── Icon/右箭头 (24×24)
├── 层级二 (Group)
│   └── 容器 (Frame, HORIZONTAL, gap=4)
│       ├── Text "一级页面"
│       └── Icon/右箭头 (24×24)
├── 层级三 (Group)
│   └── 容器 (Frame, HORIZONTAL, gap=4)
│       ├── Text "二级页面"
│       └── Icon/右箭头 (24×24)
├── 层级四 (Group)
│   └── 容器 (Frame, HORIZONTAL, gap=4)
│       ├── Text "三级页面"
│       └── Icon/右箭头 (24×24)
├── 层级五 (Frame, HORIZONTAL, gap=4)
│   ├── Text "四级页"
│   └── Icon/右箭头 (24×24)
└── Text "当前页面"（当前页，加粗蓝色）
```

---
## 交互状态

| 状态 | 非当前层级 | 当前层级 |
|------|-----------|---------|
| **默认** | 首页 > 一级页面 > **当前页面**<br>字号: 14px (Regular)<br>色值: --o-color-info3 | 首页 > 一级页面 > **当前页面**<br>字号: 14px (SemiBold)<br>色值: --o-color-primary1 |
| **悬浮** | 首页 > <u>一级页面</u> > **当前页面**<br>字号: 14px (Regular)<br>色值: --o-color-primary2 | 首页 > 一级页面 > **当前页面**<br>字号: 14px (SemiBold)<br>色值: --o-color-primary1 |
| **按下** | 首页 > 一级页面 > **当前页面**<br>字号: 14px (Regular)<br>色值: --o-color-primary3 | 首页 > 一级页面 > **当前页面**<br>字号: 14px (SemiBold)<br>色值: --o-color-primary1 |
| **选中** | 首页 > **一级页面** > **当前页面**<br>字号: 14px (SemiBold)<br>色值: --o-color-primary1 | 首页 > 一级页面 > **当前页面**<br>字号: 14px (SemiBold)<br>色值: --o-color-primary1 |
| **禁用** | 首页 > 一级页面 > **当前页面**<br>字号: 14px (Regular)<br>色值: --o-color-info4 | 首页 > 一级页面 > ~~当前页面~~<br>字号: 14px (SemiBold)<br>色值: --o-color-primary4 |

### 状态说明

- **非当前层级**：可点击的导航项（首页、一级页面等）
- **当前层级**：当前位置项（不可点击）
- **悬浮**：鼠标悬停时显示下划线，颜色变为主色调
- **按下**：点击时颜色加深
- **选中**：被选中的导航项加粗并高亮
- **禁用**：文字变灰，当前层级文字添加删除线效果

### 交互状态字体规范

| 状态 | 元素 | 字号 | 字重 |
|------|------|------|------|
| 默认 | 导航层级 | 14px | Regular (400) |
| 默认 | 当前页面 | 14px | SemiBold (600) |
| 悬浮/按下 | 导航层级 | 14px | Regular (400) |
| 选中 | 导航层级 | 14px | SemiBold (600) |
| 禁用 | 导航层级 | 14px | Regular (400) |
| 禁用 | 当前页面 | 14px | SemiBold (600) |

### 交互状态颜色 Token

| 状态 | 元素 | 色值 Token |
|------|------|-----------|
| 默认 | 导航层级 | --o-color-info3 |
| 默认 | 当前页面 | --o-color-primary1 |
| 悬浮 | 导航层级 | --o-color-primary2 |
| 按下 | 导航层级 | --o-color-primary3 |
| 选中 | 导航层级/当前页面 | --o-color-primary1 |
| 禁用 | 导航层级 | --o-color-info4 |
| 禁用 | 当前页面 | --o-color-primary4 |

---

## 样式规范

### 字体

| 元素 | Token | 字体 | 字重 | 字号 | 行高 |
|------|-------|------|------|------|------|
| 导航层级文字 | — | HarmonyHeiTi | `var(--o-font_weight-regular)` (400) | `var(--o-font_size-tip1)` (14px) | `var(--o-line_height-tip1)` (22px) |
| 当前页面文字 | — | HarmonyHeiTi | `var(--o-font_weight-bold)` (600) | `var(--o-font_size-tip1)` (14px) | `var(--o-line_height-tip1)` (22px) |

### 颜色

| 元素 | Token | Dark=off（浅色） | Dark=on（深色） |
|------|-------|-----------------|-----------------|
| 导航层级文字 | `--o-color-info3` | `rgba(var(--o-grey-14), 0.6)` | `rgba(var(--o-grey-1), 0.6)` |
| 当前页面文字 | `--o-color-primary1` | `rgb(var(--o-brand-6))` | `rgb(var(--o-brand-6))` |
| 分隔图标（右箭头） | `--o-color-info3` | 继承父层颜色 | 继承父层颜色 |

### 布局

- **方向**：水平排列（HORIZONTAL）
- **间距**：层级项之间 `var(--o-gap-1)` (4px)
- **内边距**：无（padding=0）
- **尺寸**：宽高自适应内容（RESIZE_TO_FIT）
- **图标尺寸**：`var(--o-icon_size-m)` (24px)

---

## 使用示例

### 基础用法（浅色模式，5 级路径）

```
首页 > 一级页面 > 二级页面 > 三级页面 > 四级页 > 当前页面
```

### 深色模式（Dark=on）

导航文字改为白色半透明，当前页面文字改为亮蓝色。

---

## 设计注意事项

1. **层级数量**：默认展示 5 个导航层级 + 1 个当前页面，可根据实际层级删减中间层级项
2. **分隔符**：使用「右箭头」图标（Icon/右箭头，24×24）作为层级间的分隔符，位于每个层级项的右侧
3. **当前页面**：始终位于最右侧，使用 SemiBold 加粗 + 蓝色高亮，不带分隔箭头
4. **深色适配**：Dark=on 时文字颜色自动切换，无需额外修改

---

## 调用方式

使用 `mcp__pixso-desktop__create_instance` 插入组件实例：

- `Dark=off`（浅色）componentKey 对应节点 `1042:13138`（版本：`1057:7920`）
- `Dark=on`（深色）componentKey 对应节点 `1042:13159`（版本：`1057:7921`）

插入后可通过 `get_node_dsl` 验证结构，通过 `get_image` 确认视觉效果。