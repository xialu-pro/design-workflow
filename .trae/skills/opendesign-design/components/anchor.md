> ← [组件索引](../SKILL.md#组件索引) · [README](../README.md)

# OAnchor 锚点 · 设计 Skill

> 组件集合节点：`1042:16476` · 组件名：OAnchor 锚点 · 变体总数：30

---

## Part A：设计使用卡

### 组件概览

**OAnchor 锚点**：页面内导航定位组件，用于长页面内容区的快速跳转导航。通过层级区分标题与子项，支持横向和纵向两种布局形态，帮助用户快速定位页面内容区块。

---

### 适用场景

- ✅ **长文档导航**：技术文档、帮助中心等长页面，使用纵向布局配合分隔线，帮助用户快速跳转到对应章节
- ✅ **产品详情页**：商品详情页的商品介绍、规格参数、用户评价等内容区块导航
- ✅ **表单/配置页侧边导航**：复杂表单或配置页面左侧固定的目录导航
- ✅ **页面顶部导航条**：横向布局用于页面顶部的区块切换，如产品页的「概述/功能/定价」切换
- ❌ **不适合**：跨页面导航（用 OTab 或 OBreadcrumb）、需要下拉展开的复杂目录结构（用树形组件）

---

### 变体说明

**size（尺寸）**
- `medium` — 一级锚点高度 40px（纵向）或 48px（横向），字号 16px，适合大多数场景，**默认尺寸**
- `small` — 一级锚点高度 32px，字号 14px，适合空间紧凑的侧边栏或工具栏

**level（层级）**
- `level1` — 一级锚点，主要章节标题，左内边距 12px，视觉权重最高
- `level2` — 二级锚点，子章节标题，左内边距 24px，视觉层级次于一级
- `Group` — 一级分组容器（纵向），包含分隔线区域和锚点项，高度 200px（medium）
- `Group2` — 二级分组容器（纵向），包含分隔线和二级锚点列表，高度 256px（medium）/ 208px（small）
- `Group3` — 横向分组容器，多个一级锚点横向排列，间距 32px

**state（状态）**
- `Enabled` — 未选中状态，文字使用辅助信息色
- `Actived` — 选中状态，文字使用品牌主色，字重加粗（SemiBold）

**Dark（主题）**
- `off` — 浅色模式（默认），未选中文字为 `rgba(0,0,0,0.8)`（`--o-color-info2`）
- `on` — 深色模式，未选中文字为 `rgba(255,255,255,0.8)`（`--o-color-info2`），选中文字为 `rgb(73,122,248)`（`--o-color-primary1`）

**Layout（布局方向）**
- `h` — 横向布局，锚点项水平排列，间距 32px，适合页面顶部导航条
- `v` — 纵向布局，锚点项垂直排列，配合左侧分隔线，适合侧边栏导航

---

### 布局结构

> 🧩 **布局结构**：锚点组件根据布局方向呈现不同形态。横向布局为水平排列的锚点项组；纵向布局为带左侧分隔线的垂直锚点列表，支持一级/二级层级嵌套。

**横向布局（Layout=h）**
```
OAnchor（HORIZONTAL，自适应宽，固定高 48px）
│  itemSpacing: 32px | paddingTop/Bottom: 0px
│
├── [一级锚点项 INSTANCE]（自适应宽，固定高 48px）
│   └── [文字 PARAGRAPH]（字号 16px，居中对齐）
│
├── [一级锚点项 INSTANCE]（Actived 状态）
│   └── [文字 PARAGRAPH]（字号 16px SemiBold，品牌色）
│
└── ...
```

**纵向布局（Layout=v）**
```
OAnchor（VERTICAL，固定宽，自适应高）
│  itemSpacing: 0px | padding: 0px
│
├── [容器行 FRAME]（HORIZONTAL，固定高 40px/32px/28px）
│   ├── [分隔线区域 FRAME]（固定宽 4.5px，stroke 分隔线）
│   │   └── [矩形分隔线 RECTANGLE]（stroke 色值）
│   └── [锚点项实例 INSTANCE]
│       └── [文字 PARAGRAPH]
│
├── [容器行 FRAME]
│   └── ...
│
└── ...
```

- **整体**：横向布局采用 HORIZONTAL Auto Layout，间距 32px；纵向布局采用 VERTICAL，间距 0px
- **分隔线**：纵向布局左侧有 4.5px 宽的分隔线区域，用于视觉层级指示
- **层级嵌套**：纵向布局支持一级/二级层级，二级锚点左内边距增加 12px（总计 24px）
- **选中指示**：选中态通过文字颜色变化和字重加粗体现，无背景高亮

---

### 组合搭配

> 🔗 **常见搭配**：
> - **与内容区块配合**：锚点导航 + 对应内容区块，点击锚点滚动定位到目标区域
> - **与 OBreadcrumb 搭配**：面包屑提供页面层级定位，锚点提供页面内区块导航
> - **在长文档页中**：左侧固定锚点导航 + 右侧内容区，锚点跟随滚动高亮当前章节
> - **在产品详情页中**：顶部横向锚点导航 + 下方分段内容，锚点切换不同产品信息区块

---

### 设计稿识别指南

> 🔍 **识别特征**：
> - 横向布局：水平排列的文字节点组，间距 32px，高度 48px，无背景装饰
> - 纵向布局：垂直排列的文字节点 + 左侧 4.5px 分隔线区域，支持一级/二级层级区分
> - 选中态：文字颜色为品牌色、字重 SemiBold；未选中态：文字颜色为辅助信息色 60% 透明度
> - 层级特征：二级锚点左内边距比一级多 12px（一级 12px，二级 24px）

> 🔄 **易混淆组件**：
> - 与 **OTab 标签页** 的区别：Tab 是切换控件，点击切换内容区显示；Anchor 是导航定位，点击滚动到目标区域，不切换显示
> - 与 **OBreadcrumb 面包屑** 的区别：面包屑是跨页面层级导航路径；锚点是同一页面内内容区块定位

---

### 响应式行为

参照 [栅格规范](../SKILL.md#数据资源) 中的断点定义：

- **PC（1920px）**：纵向布局锚点导航通常固定在左侧边栏（宽度 200–280px），横向布局锚点导航可在顶部内容区内自适应宽度
- **移动端（360px）**：纵向布局锚点可能收起为抽屉式导航或折叠显示，横向布局锚点间距缩小，可能需要滑动查看

---

## Part B：规格速查参考

### 变体索引表

| 变体属性 | 可选值 | 默认值 | 视觉差异 |
|---------|--------|--------|---------|
| size | `medium` / `small` | `medium` | 高度 40/48px vs 32px，字号 16px vs 14px |
| level | `level1` / `level2` / `Group` / `Group2` / `Group3` | `level1` | 层级深度、左内边距、是否为容器组 |
| state | `Enabled` / `Actived` | `Enabled` | 选中态品牌色+SemiBold，未选中辅助色+Regular |
| Dark | `off` / `on` | `off` | 浅色模式 vs 深色模式色值切换 |
| Layout | `h` / `v` | `v` | 横向排列 vs 纵向排列+分隔线 |

---

### 布局规格

#### 单个锚点项规格

| 规格项 | medium level1 (v) | medium level2 (v) | small level1 (v) | small level2 (v) | medium (h) |
|--------|-------------------|-------------------|------------------|------------------|------------|
| 高度 | 40px | 32px | 32px | 28px | 48px |
| 左内边距 | 12px | 24px | 12px | 24px | 0px（居中） |
| 上内边距 | 8px | 5px | 6px | 4px | 12px |
| 下内边距 | 8px | 5px | 6px | 4px | 12px |
| 字号 | 16px | 16px | 14px | 14px | 16px |
| 字号 Token | `font_size-text1` | `font_size-text1` | `font_size-tip1` | `font_size-tip1` | `font_size-text1` |
| 行高 | 24px | 24px | 22px | 22px | 24px |
| 行高 Token | `line_height-text1` | `line_height-text1` | `line_height-tip1` | `line_height-tip1` | `line_height-text1` |
| 字重（选中） | SemiBold | SemiBold | SemiBold | SemiBold | SemiBold |
| 字重（未选中） | Regular | Regular | Regular | Regular | Regular |

#### 横向布局规格

| 规格项 | 值 | 说明 |
|--------|-----|------|
| 方向 | HORIZONTAL | 水平排列 |
| Item Spacing | 32px | 锚点项间距 |
| Padding Top/Bottom | 0px | 无垂直内边距 |
| Padding Left/Right | 0~148px | 可变水平边距 |
| Primary Align | center | 内容居中 |
| 容器高度 | 48px | 固定高度 |

#### 纵向布局规格

| 规格项 | 值 | 说明 |
|--------|-----|------|
| 方向 | VERTICAL | 垂直排列 |
| Item Spacing | 0px | 容器行无间距 |
| Padding | 0px | 无内边距 |
| 分隔线宽度 | 4.5px | 左侧分隔线区域 |

---

### 颜色 Token 映射

| 视觉区域 | Token 名称 | Light 模式色值 | Dark 模式色值 | 说明 |
|---------|-----------|----------------|---------------|------|
| 未选中文字 | `--o-color-info2` | `rgba(0, 0, 0, 0.8)` | `rgba(255, 255, 255, 0.8)` | 二级次强调色 |
| 选中文字 | `--o-color-primary1` | `rgb(0, 47, 167)` | `rgb(73, 122, 248)` | 品牌色 brand-6 |
| 容器背景 | `--o-color-fill2` | `rgb(255, 255, 255)` | `rgb(36, 36, 39)` | 二级填充色 |
| 分隔线 | `--o-color-control4` | `rgba(0, 0, 0, 0.1)` | `rgba(255, 255, 255, 0.15)` | 浅色分隔线（禁用边框色） |

---

### 字体样式映射

| 文字区域 | 状态 | 字号 | 字重 | 行高 | 字体 |
|---------|------|------|------|------|------|
| medium 一级锚点 | 选中 | 16px | SemiBold (600) | 24px | HarmonyHeiTi |
| medium 一级锚点 | 未选中 | 16px | Regular (400) | 24px | HarmonyHeiTi |
| small 一级锚点 | 选中 | 14px | SemiBold (600) | 22px | HarmonyHeiTi |
| small 一级锚点 | 未选中 | 14px | Regular (400) | 22px | HarmonyHeiTi |

---

### 交互状态规格

#### 纵向布局锚点交互状态

| 状态 | 一级锚点 | 二级锚点 |
|------|---------|---------|
| **默认（未选中）** | 字号: 14px (Regular)<br>色值: --o-color-info2 | 二级字号: 12px (Regular)<br>二级色值: --o-color-info2 |
| **未选中·悬浮** | 悬浮: --o-color-primary2<br>字号: 14px (Regular) | — |
| **已选中·悬浮** | 一级字号: 14px (SemiBold)<br>一级色值: --o-color-primary1<br>二级锚点(选中): 二级字号: 12px (SemiBold)<br>二级色值: --o-color-primary1 | — |
| **未选中·按下** | 点击: --o-color-primary3<br>字号: 14px (Regular) | — |
| **已选中·按下** | 一级字号: 14px (SemiBold)<br>一级色值: --o-color-primary3<br>二级锚点(选中): 二级字号: 12px (SemiBold)<br>二级色值: --o-color-primary3 | — |
| **选中** | 选中: --o-color-primary1<br>字号: 14px (SemiBold) | — |
| **禁用** | 一级字号: 14px (Regular)<br>一级色值: --o-color-info4<br>二级锚点(选中): 二级字号: 12px (Regular)<br>二级色值: --o-color-info4 | — |

#### 横向布局锚点交互状态

| 状态 | 锚点选项 | 锚点选中 |
|------|---------|---------|
| **默认（未选中）** | 字号: 16px (Regular)<br>色值: --o-color-info3 | — |
| **未选中·悬浮** | 色值: --o-color-primary2<br>字号: 16px (Regular) | — |
| **已选中·悬浮** | — | 字号: 16px (SemiBold)<br>色值: --o-color-primary1 |
| **未选中·按下** | 字号: 16px (SemiBold)<br>色值: --o-color-primary3 | — |
| **已选中·按下** | — | 字号: 16px (SemiBold)<br>色值: --o-color-primary1 |
| **选中** | — | 字号: 16px (SemiBold)<br>色值: --o-color-primary1 |
| **禁用** | 字号: 16px (Regular)<br>色值: color-info4 | — |

---

### 交互状态说明

- **默认**：组件初始状态，未选中时使用辅助信息色（info系列）
- **悬浮（Hover）**：鼠标悬停时的反馈状态
  - 未选中项悬浮：使用 `--o-color-primary2`
  - 已选中项悬浮：保持选中态样式不变
- **按下（Active/Pressed）**：鼠标点击按下时的反馈状态
  - 未选中项按下：使用 `--o-color-primary3`，字重可能加粗
  - 已选中项按下：保持选中态样式或加深至 `--o-color-primary3`
- **选中（Selected）**：当前激活的锚点项，使用品牌主色 `--o-color-primary1` + SemiBold 字重
- **禁用（Disabled）**：不可交互状态，使用 `--o-color-info4` 禁用色，字重 Regular

#### 颜色Token速查

| Token名称 | 用途 | 出现状态 |
|----------|------|---------|
| `--o-color-info2` | 默认未选中文字色 | 默认态（竖向） |
| `--o-color-info3` | 默认未选中文字色 | 默认态（横向） |
| `--o-color-info4` | 禁用态文字色 | 禁用态 |
| `--o-color-primary1` | 选中态/已选中悬浮/已选中按下 | 选中、已选中悬浮、已选中按下 |
| `--o-color-primary2` | 未选中悬浮态 | 未选中·悬浮 |
| `--o-color-primary3` | 按下态（未选中/已选中） | 未选中·按下、已选中·按下 |
---

### 组件层级结构

**横向布局（Group3）**
```
OAnchor（HORIZONTAL，自适应宽，固定高 48px）
│  itemSpacing: 32px
│
├── [一级锚点项 INSTANCE]（level1, Enabled）
│   └── [文字 PARAGRAPH]（16px Regular，--o-color-info3）
│
├── [一级锚点项 INSTANCE]（level1, Actived）
│   └── [文字 PARAGRAPH]（16px SemiBold，--o-color-primary1）
│
└── ...
```

**纵向布局（Group2）**
```
OAnchor（VERTICAL，固定宽，自适应高）
│  itemSpacing: 0px | padding: 0px
│
├── [容器行 FRAME]（HORIZONTAL，固定高 40px）
│   ├── [分隔线区域 FRAME]（固定宽 4.5px）
│   │   └── [矩形分隔线 RECTANGLE]（stroke 分隔线色）
│   └── [一级锚点项 INSTANCE]（level1, Enabled/Actived）
│       └── [文字 PARAGRAPH]（左内边距 12px）
│
├── [容器行 FRAME]（HORIZONTAL，固定高 32px）
│   ├── [分隔线区域 FRAME]（固定宽 4.5px）
│   │   └── [矩形分隔线 RECTANGLE]（stroke 分隔线色）
│   └── [二级锚点项 INSTANCE]（level2, Enabled/Actived）
│       └── [文字 PARAGRAPH]（左内边距 24px）
│
└── ...
```

---

### 变体 Node ID 速查

| 变体组合 | Node ID | 尺寸 | 布局 |
|---------|---------|------|------|
| size=medium, level=level1, state=Enabled, Dark=off, Layout=v | `1042:16477` | 224×40 | VERTICAL |
| size=medium, level=level1, state=Actived, Dark=off, Layout=v | `1042:16495` | 224×40 | VERTICAL |
| size=medium, level=level2, state=Enabled, Dark=off, Layout=v | `1042:16485` | 224×32 | VERTICAL |
| size=medium, level=level2, state=Actived, Dark=off, Layout=v | `1042:16501` | 224×32 | VERTICAL |
| size=small, level=level1, state=Enabled, Dark=off, Layout=v | `1042:16481` | 160×32 | VERTICAL |
| size=small, level=level1, state=Actived, Dark=off, Layout=v | `1042:16497` | 160×32 | VERTICAL |
| size=medium, level=Group, state=Enabled, Dark=off, Layout=v | `1042:16653` | 232×200 | VERTICAL |
| size=medium, level=Group2, state=Enabled, Dark=off, Layout=v | `1042:16715` | 232×256 | VERTICAL |
| size=medium, level=Group3, state=Enabled, Dark=off, Layout=h | `1042:16810` | 600×48 | HORIZONTAL |

---

### Pixso 操作速查

1. **插入组件**：组件面板搜索「OAnchor」或「锚点」，拖入画布
2. **切换尺寸**：右侧属性面板 → size 选择 `medium` 或 `small`
3. **切换层级**：右侧属性面板 → level 选择 `level1` / `level2` / `Group` / `Group2` / `Group3`
4. **切换布局**：右侧属性面板 → Layout 选择 `h`（横向）或 `v`（纵向）
5. **切换主题**：右侧属性面板 → Dark 选择 `on` 或 `off`
6. **修改文字**：双击进入组件实例 → 双击文字图层修改锚点名称
7. **设置选中态**：右侧属性面板 → state 选择 `Actived`

---

### 注意事项

- 锚点组件用于页面内导航定位，不切换内容区显示（与 Tab 组件区分）
- 纵向布局必须配合左侧分隔线区域，分隔线宽度固定为 4.5px
- 二级锚点左内边距比一级多 12px（一级 12px，二级 24px），用于视觉层级区分
- 选中态仅通过文字颜色和字重变化体现，无背景高亮或边框装饰
- 横向布局锚点间距固定 32px，不适合过多锚点项（建议不超过 5–6 个）
- 组件未配置 Pixso Component Properties 变体功能，变体通过命名约定和多个 Symbol 实现，需手动选择对应变体节点