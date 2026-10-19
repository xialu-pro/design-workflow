> ← [组件索引](../SKILL.md#组件索引) · [README](../README.md)

# OCard 卡片 · 设计 Skill

> 组件集合节点：`1042:21386` · 组件名：OCard 卡片 · 变体总数：8（2 type × 2 layout × 2 Dark）
> 另含 Web 实现侧扩展 type：`full-cover`（全幅封面，详见 Part B「full-cover 规格」）

---

## Part A：设计使用卡

### 组件概览

**OCard 卡片**：用于承载和展示内容的容器组件，通过卡片边界划分内容区域，提升页面视觉层次感。支持封面卡片（`cover` / `full-cover`）和图标卡片（`icon`）三种类型，水平/垂直两种布局，可选头部、内容区、底部三段式结构。适用于信息展示、列表项、内容分组等场景。

---

### 适用场景

- ✅ **封面卡片（cover）**：产品展示、图文卡片、新闻列表项，封面图突出视觉吸引力
- ✅ **全幅封面卡片（full-cover）**：产品能力卡、运营入口卡，封面铺满整卡、文字叠于封面上层，视觉冲击更强
- ✅ **图标卡片（icon）**：功能入口、快捷操作、统计卡片，图标辅助语义传达
- ✅ **信息展示**：用户信息卡、订单信息卡、商品信息卡等
- ✅ **列表项**：卡片式列表布局，每个列表项为独立卡片
- ✅ **内容分组**：页面中不同内容模块的边界标识
- ❌ **不适合**：表单分组（用分割线）、导航切换（用 Tab）、大量文字描述（用详情页）

---

### 变体说明

**type（卡片类型）**
- `cover` — 封面卡片，头部包含封面图片区域，视觉吸引力强，适合产品展示、图文卡片
- `icon` — 图标卡片，头部包含图标区域，语义传达清晰，适合功能入口、统计卡片
- `full-cover` — 全幅封面卡片，封面图作为卡片自身背景**铺满整卡**，文字内容直接叠在封面上层，适合产品能力卡、运营入口卡

**layout（布局方式）**
- `horizontal` — 水平布局，卡片内容横向排列，适合侧边栏卡片、列表项横向展示
- `vertical` — 垂直布局，卡片内容纵向排列，**默认布局**，适合大多数卡片场景

**Dark（主题）**
- `off` — 浅色模式（默认）
- `on` — 深色模式，背景色自动切换为对应深色值

> 🔑 **`cover` 与 `full-cover` 的核心区别**：
> - `cover`：封面图是卡片**内部的一个区域**（位于 Header），与文字分区并列，封面自带内边距与内层圆角
> - `full-cover`：封面图**就是卡片背景本身**，铺满整卡、不占独立 Header 区、不设内层圆角（外圆角即封面圆角），文字层整块叠在封面上层

---

### 布局结构

> 🧩 **布局结构**：卡片整体为容器节点（FRAME），内部由 Header（头部）、Body（内容区）、Footer（底部）三部分组成，布局方式由 `layout` 变体控制。

**layout=vertical 布局**

```
OCard（VERTICAL，自适应宽高）
│  cornerRadius: 4px
│  fill: color-fill2（卡片背景）
│  shadow: shadow-2（悬浮时显示）
│
├── [Header 头部 FRAME]（自适应宽，固定高度）
│   ├── [封面图片 IMAGE]（cover 类型）/ [图标 Icon INSTANCE]（icon 类型）
│   └── [标题文字 PARAGRAPH]
│
├── [Body 内容区 FRAME]（自适应宽高）
│   └── [内容节点...]
│
└── [Footer 底部 FRAME]（自适应宽，固定高度）
    └── [操作按钮 INSTANCE...]
```

**layout=horizontal 布局**

```
OCard（HORIZONTAL，自适应宽高）
│  cornerRadius: 4px
│  fill: color-fill2
│  shadow: shadow-2（悬浮时显示）
│
├── [Header 头部 FRAME]（固定宽度，自适应高度）
│   ├── [封面图片 IMAGE]（cover）/ [图标 Icon INSTANCE]（icon）
│   └── [标题文字 PARAGRAPH]
│
├── [Body 内容区 FRAME]（自适应宽高）
│   └── [内容节点...]
│
└── [Footer 底部 FRAME]（固定宽度，自适应高度）
    └── [操作按钮 INSTANCE...]
```

---

### 组合搭配

> 🔗 **常见搭配**：
> - **产品列表**：封面卡片（cover + vertical）纵向排列，用于电商产品列表、新闻列表
> - **功能入口**：图标卡片（icon + horizontal）横向排列，用于快捷操作入口
> - **信息卡片**：图标卡片（icon + vertical）独立展示，用于用户信息卡、订单信息卡
> - **统计卡片**：图标卡片（icon + vertical + large padding），用于数据统计展示

---

### 设计稿识别指南

> 🔍 **识别特征**：
> - 容器节点（FRAME），圆角 4px，无边框描边
> - 背景为 `--o-color-fill2`（浅灰填充色）
> - 悬浮时显示阴影 `shadow-2`
> - 内部结构分为 Header、Body、Footer 三部分（可选）
> - Header 包含封面图片（cover）或图标（icon）+ 标题

> 🔄 **易混淆组件**：
> - 与 **ORow/OCol 容器** 的区别：Card 是独立卡片单元，有封面图/标题/正文的固定语义分区和圆角阴影；ORow/OCol 是通用栅格容器，无固定内部结构和卡片样式
> - 与 **ODataTable 单列行** 的区别：Card 是独立组件，可脱离列表使用，有封面图和圆角阴影；ODataTable 单列模式的行是列表子单元，无独立卡片样式

---

### 响应式行为

参照 [栅格规范](../SKILL.md#数据资源) 中的断点定义，OCard 无自动断点响应式行为。宽度由容器或栅格系统控制，高度由内容自适应。布局方式通过 `layout` 变体手动切换。

---

## Part B：规格速查参考

### 变体索引表

| 变体属性 | 可选值 | 默认值 | 视觉差异 |
|---------|--------|--------|---------|
| type | `cover` / `full-cover` / `icon` | `cover` | 分区封面图 / 全幅封面图（铺满整卡）/ 图标 |
| layout | `horizontal` / `vertical` | `vertical` | 水平排列 / 垂直排列 |
| Dark | `off` / `on` | `off` | 浅色模式 / 深色模式 |

---

### full-cover 规格（全幅封面卡片）

> 🖼 **定义**：封面图作为卡片自身背景铺满整卡，文字内容层整块叠在封面上层。
> 封面**不占独立 Header 区、不设内层圆角**（外圆角即封面圆角）。

**布局规格**

| 规格项 | 档位 | Token | 默认基准 | 当前 openEuler 页面实现 |
|--------|------|-------|---------|------------------------|
| 卡片圆角 | 外档 | `radius_control-l` | 4px | 16px |
| 封面填充 | — | 铺满整卡（`background-size:cover`） | — | — |
| 封面内层圆角 | — | 无（继承卡片外圆角） | — | — |
| 卡内元素圆角 | 内档 | `radius_control-s` | 4px | 8px |
| 内容层内边距 | — | 上 28px · 左右 24px · 下 24px | — | — |
| 悬浮阴影 | — | `shadow-2` | `shadow-2` | `shadow-2` |

> 🔲 **圆角取值遵循上文「圆角基准说明」**：卡片取**外档**、卡内元素取**内档**，两者保持「外大一档 / 内小一档」的分层关系；
> 具体 px 以社区 token 为准（默认基准 4px）。表中「当前 openEuler 页面实现」为 `opendesign-home.html`
> 「最新上线」楼层的实际渲染值，该页圆角为页面级指定值（外 16px / 内 8px），非组件默认值。

**文字规格**（沿用全局字体样式映射）

| 层级 | 字号 | 行高 | 字重 | 颜色 |
|------|------|------|------|------|
| 卡片标题 | `font_size-h3`（22px） | `line_height-h3`（30px） | bold | `--o-color-info1` |
| 卡片正文 | `font_size-text1`（16px） | `line_height-text1`（24px） | regular | `--o-color-info3` |
| 徽标（NEW/HOT） | `font_size-tip2`（12px） | 16px | regular | `--o-color-white` |

**结构示意**

```
OCard（full-cover，VERTICAL）
│  cornerRadius: 外档（radius_control-l，默认基准 4px）
│  background: 封面图铺满整卡（无独立内圆角）
│
└── [内容层 FRAME]（叠在封面上层，padding 28/24/24）
    ├── [Head 行]
    │   ├── [图标槽] 32px / 圆角内档（radius_control-s）/ fill2 底 + primary1 图标
    │   ├── [标题文字] font_size-h3 / Bold / info1
    │   └── [徽标] 高 20px / 圆角 100px / primary1 底 + 白字
    ├── [正文文字] font_size-text1 / Regular / info3
    └── [内容区]（可选，如界面示意）
          fill2 底 / 圆角内档（radius_control-s）/ shadow-1
```

**代码基准**（来自 opendesign-home.html「最新上线」楼层，圆角为该页指定值）

```css
.oc-card--full-cover{
  display:flex;flex-direction:column;
  padding:28px 24px 24px;border-radius:16px;      /* 外档：radius_control-l（该页指定 16px） */
  background-image:var(--o-cover);background-size:cover;  /* 封面铺满整卡 */
}
.oc-card--full-cover__icon{width:32px;height:32px;border-radius:8px;}   /* 内档：radius_control-s（该页指定 8px） */
.oc-card--full-cover__title{font-size:var(--o-r-font_size-h3);font-weight:600;color:var(--o-color-info1);}
.oc-card--full-cover__desc{font-size:var(--o-r-font_size-text1);font-weight:400;color:var(--o-color-info3);}
.oc-card--full-cover__visual{border-radius:8px;background-color:var(--o-color-fill2);box-shadow:var(--o-shadow-1);}
```

> ⚠️ **封面明度硬约束**：文字直接叠在封面上，封面必须保证与文字色的对比度达标（WCAG AA）。
> 浅色/柔和彩底封面 → 文字用 `info1` / `info3`；深色封面 → 文字必须切换到
> `--o-color-info1-inverse` / `--o-color-info3-inverse`（白色系）。禁止黑字压深底。

---

### 圆角基准说明

> 🔲 **默认基准 4px**：card.md 内所有圆角一律按 **4px** 描述（对应 `radius_control-m` 档），这是 OpenDesign 的默认基准规范。
> **实际生效值由各社区 token 决定**，不同社区对 `radius_control-*` 各档的取值不同：
>
> | 社区 | `xs` | `s` | `m` | `l` |
> |------|------|-----|-----|-----|
> | openEuler / Kunpeng | 4px | 4px | 4px | 4px |
> | openUBMC / MindSpore / Ascend / openGauss | 4px | 8px | 12px | 16px |
>
> 落地时**以社区 token 为准**，card.md 中的 4px 仅表示「默认基准档位」，不代表所有社区都必须渲染为 4px。
> 凡涉及具体社区页面，圆角取值需回查该社区的 `opendesign-tokens` 定义。

---

### 布局规格

| 规格项 | cover | icon |
|--------|-------|------|
| 圆角 | 4px | 4px |
| Token（圆角） | `radius_control-m` | `radius_control-m` |
| 背景 | `--o-color-fill2` | `--o-color-fill2` |
| 边框 | 无 | 无 |
| 阴影（悬浮） | `shadow-2` | `shadow-2` |
| Header 内边距 | 见下表 | 见下表 |
| Body 内边距 | 见下表 | 见下表 |
| Footer 内边距 | 见下表 | 见下表 |

> **说明**：卡片宽度由容器或栅格系统控制，高度由内容自适应。内边距根据布局和内容密度有不同规格。

---

### 内边距规格

> **cover 类型**：卡片容器统一 padding 8px，各 section 无独立 padding，Body（文字区域）额外增加 bottom 16px、left/right 16px。
> **icon 类型**：卡片容器统一 padding top/bottom 24px、left/right 32px，无独立 section padding。

**cover 类型内边距**

| 区域 | vertical（上/下） | horizontal（左/右） |
|------|-----------------|-------------------|
| Header 内边距 | 8px | 8px |
| Body 内边距 | 上 0px · 下 16px | 16px |
| Footer 内边距 | 8px | 8px |
| 区域间距（Header↔Body） | 24px | — |
| 区域间距（Body↔Footer） | 24px | — |

**icon 类型内边距**

| 区域 | vertical（垂直布局） | horizontal（水平布局） |
|------|-----------------|-------------------|
| 卡片容器 padding（上/下） | 24px | 24px |
| 卡片容器 padding（左/右） | 32px | 32px |
| 内容项间距（gap） | 16px | 24px |

---

### 封面图片规格（cover 类型）

| 规格项 | vertical | horizontal |
|--------|----------|------------|
| 图片宽度 | 自适应（跟随卡片宽度） | 固定（如 120px） |
| 图片高度 | 固定比例（如 16:9）或固定高度 | 自适应（跟随卡片高度） |
| 圆角 | 0px（无圆角）或顶部圆角 4px | 0px 或左侧圆角 4px |

---

### 图标规格（icon 类型）

| 规格项 | vertical | horizontal |
|--------|----------|------------|
| 图标尺寸 | 32px | 32px |
| Token（图标尺寸） | `icon_size-l` | `icon_size-l` |
| 图标颜色 | `--o-color-primary1` | `--o-color-primary1` |
| 图标背景 | `--o-color-primary1-light` | `--o-color-primary1-light` |
| 图标圆角 | 4px | 4px |

---

### 颜色 Token 映射

| 区域 | Light 模式 Token | Dark 模式 Token | RGB 值 |
|------|-----------------|----------------|--------|
| 卡片背景 | `--o-color-fill2` | `--o-color-fill2` | rgb(255,255,255) → rgb(36,36,39) |
| 悬浮阴影 | `shadow-2` | `shadow-2` | 0 2px 24px rgba(0,0,0,0.15) → 0 2px 24px rgba(255,255,255,0.15) |

---

## Part C：交互状态规格

> 📐 **字号基准（唯一权威值）**：卡片标题 **22px（`font_size-h3`）/ Bold**、正文 **16px（`font_size-text1`）/ Regular**，
> 与 Part D「字体样式映射」一致。下文各交互表中的「22px / 16px」即这两个 Token 的 Desktop 基准值，
> 三处（Part C / Part D 字体映射 / Part D 变量绑定）不得再出现其他数字。

### 交互状态总览

> 🎯 **交互状态说明**：OCard 支持两种主要交互状态——**默认状态**和**悬浮/按下状态**。不同卡片类型在交互状态下呈现不同的视觉反馈，主要通过颜色变化体现交互响应。

**交互状态类型**
- `default` — 默认状态，卡片的正常展示状态
- `hover/pressed` — 悬浮/按下状态，鼠标悬浮或点击时的交互反馈状态

---

### 单体卡片交互规格

**默认状态**

| 元素 | 规格项 | Token 值 | 说明 |
|------|--------|----------|------|
| 标题文字 | 字号/字重 | 22px (Semibold) | 一级标题样式 |
| 标题文字 | 颜色 | `--o-color-info1` | 默认标题颜色 |
| 内容文字 | 字号/字重 | 16px (Regular) | 正文样式 |
| 内容文字 | 颜色 | `--o-color-info3` | 默认内容颜色 |

**悬浮/按下状态**

| 元素 | 规格项 | Token 值 | 说明 |
|------|--------|----------|------|
| 标题文字 | 字号/字重 | 22px (Semibold) | 保持不变 |
| 标题文字 | 颜色 | `--o-color-primary1` | **变化**：主题色高亮 |
| 内容文字 | 字号/字重 | 16px (Regular) | 保持不变 |
| 内容文字 | 颜色 | `--o-color-info3` | 保持不变 |

> 💡 **交互反馈**：单体卡片在悬浮/按下时，仅**标题文字颜色**从 `info1` 变为 `primary1`，提供视觉焦点引导。

---

### 复合卡片交互规格

**默认状态**

| 元素 | 规格项 | Token 值 | 说明 |
|------|--------|----------|------|
| 标题文字 | 字号/字重 | 22px (Semibold) | 一级标题样式 |
| 标题文字 | 颜色 | `--o-color-info1` | 默认标题颜色 |
| 内容文字 | 字号/字重 | 16px (Regular) | 正文样式 |
| 内容文字 | 颜色 | `--o-color-info3` | 默认内容颜色 |
| 操作按钮 | 描述文字颜色 | `--o-color-primary1` | 按钮链接颜色 |

**悬浮/按下状态**

| 元素 | 规格项 | Token 值 | 说明 |
|------|--------|----------|------|
| 标题文字 | 字号/字重 | 22px (Semibold) | 保持不变 |
| 标题文字 | 颜色 | `--o-color-info1` | 保持不变 |
| 内容文字 | 字号/字重 | 16px (Regular) | 保持不变 |
| 内容文字 | 颜色 | `--o-color-info3` | 保持不变 |
| 主要操作按钮 | 填充背景色 | `--o-color-primary1` | **变化**：主按钮实心填充 |
| 主要操作按钮 | 文字颜色 | `--o-color-white` | **变化**：白色文字对比 |

> 💡 **交互反馈**：复合卡片在悬浮/按下时，**主操作按钮**从描边样式变为 `primary1` 实心填充，强化可点击感知。

---

### 图文卡片交互规格

**默认状态**

| 元素 | 规格项 | Token 值 | 说明 |
|------|--------|----------|------|
| 封面图片 | 缩放比例 | 100% | 默认尺寸 |
| 标题文字 | 字号/字重 | 22px (Semibold) | 一级标题样式 |
| 标题文字 | 颜色 | `--o-color-info1` | 默认标题颜色 |
| 内容文字 | 字号/字重 | 16px (Regular) | 正文样式 |
| 内容文字 | 颜色 | `--o-color-info3` | 默认内容颜色 |

**悬浮/按下状态**

| 元素 | 规格项 | Token 值 | 说明 |
|------|--------|----------|------|
| 封面图片 | 缩放比例 | 105% | **变化**：微放大效果 |
| 标题文字 | 字号/字重 | 22px (Semibold) | 保持不变 |
| 标题文字 | 颜色 | `--o-color-primary1` | **变化**：主题色高亮 |
| 内容文字 | 字号/字重 | 16px (Regular) | 保持不变 |
| 内容文字 | 颜色 | `--o-color-info3` | 保持不变 |

> 💡 **交互反馈**：图文卡片在悬浮/按下时，**封面图片放大至 105%** + **标题颜色变为 primary1`，双重视觉反馈增强交互体验。

---

### 全幅封面卡片交互规格（full-cover）

**默认状态**

| 元素 | 规格项 | Token 值 | 说明 |
|------|--------|----------|------|
| 封面（卡片背景） | 缩放比例 | 100% | 铺满整卡 |
| 标题文字 | 字号/字重 | 22px (Semibold) | 一级标题样式 |
| 标题文字 | 颜色 | `--o-color-info1` | 默认标题颜色 |
| 内容文字 | 字号/字重 | 16px (Regular) | 正文样式 |
| 内容文字 | 颜色 | `--o-color-info3` | 默认内容颜色 |

**悬浮/按下状态**

| 元素 | 规格项 | Token 值 | 说明 |
|------|--------|----------|------|
| 封面（卡片背景） | 缩放比例 | 105% | **变化**：微放大效果 |
| 标题文字 | 字号/字重 | 22px (Semibold) | 保持不变 |
| 标题文字 | 颜色 | `--o-color-info1` | 保持不变 |
| 内容文字 | 字号/字重 | 16px (Regular) | 保持不变 |
| 内容文字 | 颜色 | `--o-color-info3` | 保持不变 |
| 卡片阴影 | effects | `shadow-2` | **变化**：悬浮时显示阴影 |

> 💡 **交互反馈**：full-cover 的封面即卡片背景，放大必须通过内层封面元素 `transform:scale(1.05)` 实现
> （容器 `overflow:hidden` 固定尺寸），**禁止直接 scale 卡片容器**。
> 若卡片有且只有一种交互行为（如整卡跳转），**不得内嵌操作按钮**——整卡作为 `<a>` 链接，
> 悬浮反馈只通过阴影 `shadow-2` 体现，避免与封面放大叠加造成视觉过载。

---

### 交互状态 Token 汇总表

| 卡片类型 | 状态 | 标题颜色 | 内容颜色 | 特殊元素变化 |
|---------|------|----------|----------|-------------|
| **单体卡片** | default | `--o-color-info1` | `--o-color-info3` | — |
| **单体卡片** | hover/pressed | `--o-color-primary1` | `--o-color-info3` | 标题变色 |
| **复合卡片** | default | `--o-color-info1` | `--o-color-info3` | 按钮：描边 + `primary1` 文字 |
| **复合卡片** | hover/pressed | `--o-color-info1` | `--o-color-info3` | 主按钮：`primary1` 填充 + 白字 |
| **图文卡片** | default | `--o-color-info1` | `--o-color-info3` | 图片：100% |
| **图文卡片** | hover/pressed | `--o-color-primary1` | `--o-color-info3` | 图片：105% + 标题变色 |
| **全幅封面卡片** | default | `--o-color-info1` | `--o-color-info3` | 封面：100% |
| **全幅封面卡片** | hover/pressed | `--o-color-info1` | `--o-color-info3` | 封面：105% + 阴影 shadow-2 |

---

### 硬约束规则（Hard Constraints）

> ⚠️ **以下规则为全局通用硬性约束，所有 OCard 实例必须严格遵守**

#### 规则 1：封面图片 Hover 放大效果

**问题描述**：封面卡片的 hover 效果不应改变图片容器区域尺寸，仅放大内部填充的图片内容。

**正确实现方式**：
```
┌─────────────────────────────┐
│  cover-img (固定尺寸容器)    │  ← 容器尺寸不变
│  ┌───────────────────────┐  │
│  │                       │  │
│  │   图片内容 (scale:1)  │  │  ← 默认状态
│  │                       │  │
│  └───────────────────────┘  │
└─────────────────────────────┘

Hover 时：
┌─────────────────────────────┐
│  cover-img (固定尺寸容器)    │  ← 容器尺寸保持不变
│  ┌───────────────────────┐  │
│  │  ╔═════════════════╗  │  │
│  │  ║                 ║  │  │
│  │  ║ 图片内容(105%)  ║  │  │  ← 仅内部图片放大
│  │  ║                 ║  │  │
│  │  ╚═══════════════╝  │  │
│  └───────────────────────┘  │
└─────────────────────────────┘
```

**技术实现要求**：
- 封面图容器（`.cover-img`）设置 `overflow: hidden` 且**不应用 transform**
- 内部图片元素或占位符应用 `transition: transform .2s` 和 hover 时 `transform: scale(1.05)`
- 容器宽高保持固定值（如 width:100%, height:160px），不受内部缩放影响

**错误实现** ❌：
```css
/* 错误：直接对容器应用 scale */
.card:hover .cover-img { transform: scale(1.05); }
/* 这会导致整个容器区域放大，影响布局 */
```

**正确实现** ✅：
```css
/* 正确：对容器内的图片内容应用 scale */
.cover-img {
  overflow: hidden; /* 裁剪超出部分 */
}
.cover-img > img,
.cover-img > .cover-placeholder,
.cover-img > [class*="cover-content"] {
  transition: transform .2s;
}
.card:hover .cover-img > img,
.card:hover .cover-img > .cover-placeholder,
.card:hover .cover-img > [class*="cover-content"] {
  transform: scale(1.05);
}
```

---

#### 规则 2：Footer 操作区按钮布局与顺序

**问题描述**：Footer 底部操作区的按钮必须遵循固定的位置和排列顺序。

**布局位置规则**：
- 所有按钮必须**左对齐**（`justify-content: flex-start`）
- **禁止使用** `space-between`、`center`、`flex-end` 或 `right` 对齐
- 按钮组从左侧开始排列，右侧留空

**按钮排列顺序规则**（优先级从高到低）：
```
[Solid 按钮] → [Outline 按钮] → [Text 按钮]
```

**示例**：

✅ **正确示例**：
```html
<div class="card-footer">
  <!-- 第一个：Solid 按钮（主要操作）-->
  <button class="o-btn-solid">立即购买</button>
  
  <!-- 第二个：Outline 按钮（次要操作）-->
  <button class="o-btn-outline">加入购物车</button>
  
  <!-- 第三个：Text 按钮（辅助操作）-->
  <button class="o-btn-text">查看详情</button>
</div>
```

❌ **错误示例**：
```html
<!-- 错误1：使用了 space-between -->
<div class="card-footer" style="justify-content: space-between;">
  <button class="o-btn-text">收藏</button>
  <button class="o-btn-solid">立即购买</button>
</div>

<!-- 错误2：按钮顺序错误 -->
<div class="card-footer">
  <button class="o-btn-text">查看详情</button>  <!-- Text 不应在最前 -->
  <button class="o-btn-solid">立即购买</button>
</div>

<!-- 错误3：按钮右对齐 -->
<div class="card-footer" style="justify-content: flex-end;">
  <button class="o-btn-solid">立即购买</button>
</div>
```

**CSS 强制约束**：
```css
.card-footer {
  display: flex;
  align-items: center;
  justify-content: flex-start; /* ⚠️ 必须左对齐 */
  gap: 12px;
  padding: 0 16px 8px;
}

/* icon 类型卡片同样适用 */
.card-icon .card-footer {
  justify-content: flex-start; /* ⚠️ 同样左对齐 */
  padding: 0;
}
```

**适用范围**：
- 此规则适用于**所有类型**的 OCard（cover / icon）
- 此规则适用于**所有布局**方式（horizontal / vertical）
- 此规则为**全局通用**，不仅限于 Card 组件

---

#### 规则 3：禁止使用自定义图标（Emoji/Unicode 符号）

**问题描述**：OCard 组件内禁止使用任何形式的自定义图标、Emoji 表情符号或 Unicode 字符作为图标替代品。

**硬性约束**：
- ❌ **禁止使用** Emoji 表情符号（如 🎨 📊 ❤️ ⚡ 👥 💰 等）
- ❌ **禁止使用** Unicode 特殊符号（如 → ← ↑ ↓ ★ ● ■ 等）
- ❌ **禁止使用** 文字字符模拟图标（如 [图标] <icon> 等）
- ✅ **必须使用** OpenDesign 图标系统中的标准 Icon 实例
- ✅ **必须遵循** 对应组件的 icon 规范（如 card.md 中的图标规格）

**正确实现方式**：
```html
<!-- ✅ 正确：使用 SVG 图标实例 -->
<div class="icon-wrapper">
  <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
    <path d="M13 2L3 14h9l-1 8 10-12h-9l1-8z"/>
  </svg>
</div>

<!-- ✅ 正确：调用 OpenDesign 图标资源 -->
<img src="[icon-path]/icon-light.svg" alt="功能图标" />
```

**错误示例**：
```html
<!-- ❌ 错误：使用 Emoji -->
<span>🎨 产品封面图</span>
<button>❤️ 点赞</button>

<!-- ❌ 错误：使用 Unicode 箭头 -->
<a href="#">查看详情 →</a>

<!-- ❌ 错误：使用特殊符号模拟图标 -->
<div class="card-title">⚡ 快捷操作</div>
```

**违规后果**：
- 破坏设计系统的视觉一致性
- 不同操作系统/浏览器显示效果不一致
- 无法保证无障碍访问（屏幕阅读器兼容性）
- 违反 OpenDesign 设计规范的核心原则

---

#### 规则 4：标签（Tag/Badge）必须调用 OTag 组件 Skill + 位置约束

**问题描述**：如果需要在卡片上添加状态标签、分类标记等 Tag 元素，必须严格调用 OTag 标签组件，且位置有固定约束。

**硬性约束 A - 组件调用**：
- ❌ **禁止自创** Badge/Tag/Lable 的 CSS 样式
- ❌ **禁止使用** 自定义的 `.badge`、`.tag`、`.label` 类名
- ✅ **必须调用** `OTag` 组件（参见 [tag.md](./tag.md)）
- ✅ **必须遵循** OTag 的变体规范（fill/stroke × size × color）

**硬性约束 B - 位置规则**：

> ⚠️ **Tag 默认位置约束**：状态标记类 Tag（如 HOT、NEW、推荐等）**必须放置在卡片的右上角**

**位置优先级表**：

| Tag 类型 | 推荐位置 | 说明 |
|---------|---------|------|
| **状态标记**（HOT/NEW/推荐/限量） | 右上角（absolute定位） | 使用 `position:absolute;top:12px;right:12px;z-index:1;` |
| **分类标签**（技术/数码/热销） | 内容区内标题下方 | 嵌入在 `.card-body` 中，作为辅助信息 |
| **版本号**（v2.0/v1.5） | 元数据行或内容区底部 | 与发布时间、作者等信息并列 |

**标准实现方式**：

```html
<div class="card card-cover w-320">
  <!-- ✅ 正确：状态标记放在右上角 -->
  <div class="otag otag-fill otag-medium otag-danger"
       style="position:absolute;top:12px;right:12px;z-index:1;">
    HOT
  </div>

  <div class="cover-img">...</div>
  
  <div class="card-body">
    <div class="card-title">产品标题</div>
    
    <!-- ✅ 正确：分类标签放在内容区 -->
    <div style="display:flex;gap:6px;margin-top:8px;">
      <div class="otag otag-stroke otag-small otag-primary">技术</div>
      <div class="otag otag-stroke otag-small otag-info">热销</div>
    </div>
  </div>
</div>
```

**多标签组合场景**：

当卡片需要同时展示多种类型标签时：
```html
<div class="card card-cover w-320">
  <!-- 右上角：可放1-2个状态标记（横向排列）-->
  <div style="position:absolute;top:12px;right:12px;z-index:1;display:flex;gap:6px;">
    <div class="otag otag-fill otag-small otag-danger">限量</div>   <!-- 主状态 -->
    <div class="otag otag-stroke otag-small otag-primary">新品</div> <!-- 次状态 -->
  </div>
  
  <div class="cover-img">...</div>
  <div class="card-body">
    <div class="card-title">商品名称</div>
    
    <!-- 内容区：分类标签组 -->
    <div style="display:flex;gap:6px;margin-top:8px;">
      <div class="otag otag-stroke otag-small otag-success">数码</div>
      <div class="otag otag-stroke otag-small otag-info">促销</div>
    </div>
  </div>
</div>
```

**错误与正确对比示例**：

```html
<!-- ════════════════════════════════════════ -->
<!-- 错误1：自创 Badge 样式（禁止）-->
<!-- ════════════════════════════════════════ -->
<!-- ❌ 错误：使用自定义 .badge 类名和内联样式 -->
<div class="badge" style="position:absolute;top:12px;right:12px;padding:4px 10px;
    background:#ff4d4f;color:#fff;border-radius:12px;font-size:12px;">NEW</div>

<!-- ✅ 正确：调用 OTag 组件 + 右上角位置 -->
<div class="otag otag-fill otag-medium otag-success"
     style="position:absolute;top:12px;right:12px;z-index:1;">
  NEW
</div>


<!-- ════════════════════════════════════════ -->
<!-- 错误2：Tag 位置错误（禁止）-->
<!-- ════════════════════════════════════════ -->
<!-- ❌ 错误：状态标记放在左下角（位置错误）-->
<div class="otag otag-fill otag-medium otag-danger"
     style="position:absolute;bottom:12px;left:12px;">
  HOT
</div>

<!-- ✅ 正确：状态标记必须放在右上角 -->
<div class="otag otag-fill otag-medium otag-danger"
     style="position:absolute;top:12px;right:12px;z-index:1;">
  HOT
</div>


<!-- ════════════════════════════════════════ -->
<!-- 错误3：自定义 Tag 类名（禁止）-->
<!-- ════════════════════════════════════════ -->
<!-- ❌ 错误：使用 .custom-tag 自定义类名 -->
<span class="custom-tag" style="padding:2px 8px;background:#52c41a;color:#fff;
      border-radius:4px;font-size:12px;">促销</span>

<!-- ✅ 正确：使用 OTag 组件标准类名 -->
<div class="otag otag-stroke otag-small otag-warning">促销</div>
```

**OTag 变体选择指南**：
| 场景 | Variant | Size | Color | 位置 |
|------|---------|------|-------|------|
| 状态标记（HOT/NEW） | `fill` | `medium`/`small` | `danger`/`success` | **右上角** |
| 分类标签 | `stroke` | `small` | `primary`/`info` | 内容区 |
| 版本号 | `stroke` | `small` | `warning` | 元数据行 |

**引用规范**：
- 所有 Tag 相关实现必须参考 [OTag 标签 · 设计 Skill](./tag.md)
- 必须遵循 tag.md 中的变体说明、布局结构、尺寸规格
- 如需扩展功能，应更新 tag.md 而非在 Card 中自创实现

---

#### 规则 5：封面图片固定比例

**问题描述**：cover 类型卡片的封面图片区域必须使用固定的宽高比，禁止随意设置高度或使用竖向比例。

**硬性约束**：
- ✅ **支持比例**：`16:9`（标准横向）或 `3:1`（超宽横向）
- ❌ **禁止使用** 自定义高度（如 `height:120px`, `height:160px` 等）
- ❌ **禁止使用** 竖向比例（如 2:3, 3:4, 9:16 等）
- ❌ **禁止使用** 其他横向比例（如 4:3, 21:9, 1:1 等）

**技术实现**：
```css
/* 16:9 标准横向比例（默认） */
.cover-img {
  width: 100%;
  aspect-ratio: 16 / 9; /* 或 padding-bottom: 56.25% */
}

/* 3:1 超宽横向比例（适用于水平布局卡片） */
.cover-img--wide {
  aspect-ratio: 3 / 1; /* 或 padding-bottom: 33.33% */
}
```

**应用场景**：

| 布局类型 | 推荐比例 | 适用场景 |
|---------|---------|---------|
| **垂直布局** (card-cover) | `16:9` | 标准产品展示、新闻列表、图文内容、文章预览 |
| **水平布局** (card-horizontal) | `3:1` | 列表项、文档卡片、横幅式展示 |

**重要说明**：
- **所有 cover 类型卡片**（无论垂直还是水平布局）都必须使用**横向比例**
- **禁止竖向图片**：封面图必须是横向排列，宽度大于高度
- **垂直布局卡片**统一使用 `16:9` 标准比例
- **水平布局卡片**推荐使用 `16:9` 或 `3:1` 比例

**水平布局特殊规则**：

> ⚠️ **关键约束**：水平布局带图片的卡片**不会截断内容**

1. **图片尺寸**：图片保持固定比例（16:9 或 3:1），宽度可自定义（如 200px、220px）
2. **自适应宽度**：卡片整体宽度由内容决定，不设置 max-width 限制
3. **内容完整显示**：所有文字内容完整展示，不截断、不省略
4. **flex 布局**：
   - 图片区域：`flex-shrink: 0; width: [固定值];`
   - 内容区域：`width: auto; min-width: 0;`

**正确实现方式**：
```css
/* 水平布局卡片 */
.card-horizontal {
  display: flex;
  flex-direction: row;
  width: auto;
  max-width: none; /* 不限制最大宽度 */
}

/* 封面图 - 固定宽度 + 固定比例 */
.card-horizontal .cover-img {
  width: 200px; /* 可调整 */
  aspect-ratio: 16 / 9; /* 或 3/1 */
  flex-shrink: 0; /* 防止压缩 */
}

/* 内容区 - 自适应宽度 */
.card-horizontal .content-wrapper {
  width: auto; /* 根据内容自适应 */
  min-width: 0; /* 允许收缩但不截断 */
}
```

**错误示例**：
```css
/* ❌ 错误：使用竖向比例 */
.cover-img { aspect-ratio: 3 / 4; } /* 竖图 */

/* ❌ 错误：固定高度 */
.cover-img { width:200px; height:180px; } /* 不符合比例规范 */

/* ❌ 错误：水平布局使用竖向占位符 */
<div class="cover-placeholder" style="writing-mode:vertical-lr;">文档</div>
```

**正确示例**：
```css
/* ✅ 正确：垂直布局使用 16:9 */
.card-cover .cover-img { aspect-ratio: 16 / 9; }

/* ✅ 正确：水平布局使用 3:1 */
.card-horizontal .cover-img { aspect-ratio: 3 / 1; }
```

**兼容性说明**：
- 现代浏览器支持 `aspect-ratio` CSS 属性
- 旧版浏览器可使用 `padding-bottom` 百分比方案实现

---

#### 规则 6：按钮显示条件与交互逻辑

**问题描述**：并非所有卡片都显示 Footer 操作按钮，按钮显示取决于跳转方式的数量。

**硬性约束**：

**场景 A：单一跳转方式（无 Footer 按钮）**
- 卡片整体可点击，直接触发跳转/操作
- **不显示** Footer 区域或任何按钮
- 点击事件绑定在 `.card` 容器上
- 适用场景：详情页链接、单操作入口

```html
<!-- ✅ 正确：无按钮，整卡可点 -->
<div class="card card-cover w-320" onclick="handleCardClick(this)">
  <div class="cover-img">...</div>
  <div class="card-body">
    <div class="card-title">产品标题</div>
    <div class="card-desc">描述文字</div>
  </div>
</div>
```

**场景 B：多跳转方式（显示全部按钮）**
- 显示 Footer 区域，包含**所有可用操作按钮**
- 按钮顺序遵循**规则 2**（Solid → Outline → Text）
- 按钮左对齐
- 卡片容器本身**不可点击**（避免冲突）
- 适用场景：多操作入口、复合功能卡片

```html
<!-- ✅ 正确：多按钮，卡片不可点 -->
<div class="card card-cover w-320">
  <div class="cover-img">...</div>
  <div class="card-body">...</div>
  <div class="card-footer">
    <button class="o-btn-solid">主要操作</button>
    <button class="o-btn-outline">次要操作</button>
    <button class="o-btn-text">辅助操作</button>
  </div>
</div>
```

**决策流程图**：
```
判断跳转方式数量
├── = 1 个
│   ├── 不显示 Footer
│   ├── 整个卡片可点击
│   └── cursor: pointer on .card
│
└── ≥ 2 个
    ├── 显示 Footer
    ├── 列出所有按钮（按优先级排序）
    ├── .card 无 onclick
    └── 按钮独立绑定事件
```

---

#### 规则 7：数据统计区域位置规则

**问题描述**：卡片底部的点赞、收藏、浏览等统计数据区域的位置取决于是否包含评分元素。

**硬性约束**：

**默认情况（无评分）- 数据居左**
- 所有统计项（点赞数、浏览量、评论数等）**左对齐**排列
- 使用 `justify-content: flex-start` 或默认流式布局

```html
<div class="stats-bar" style="justify-content: flex-start;">
  <span>点赞: 128</span>
  <span>浏览: 2.3k</span>
  <span>评论: 45</span>
</div>
```

**特殊情况（含评分）- 评分居左，其余居右**
- **评分元素**（星级评分、数字评分等）**强制左对齐**
- **其他统计项**（点赞、浏览、时间等）**右对齐**
- 使用 Flexbox 的 `space-between` 或分组布局

```html
<!-- 方案1：space-between -->
<div class="stats-bar" style="justify-content: space-between;">
  <!-- 左侧：评分 -->
  <span>★ 4.8 分</span>
  
  <!-- 右侧：其他统计 -->
  <div style="display:flex;gap:12px;">
    <span>点赞: 128</span>
    <span>浏览: 2.3k</span>
  </div>
</div>

<!-- 方案2：明确分组 -->
<div class="stats-bar">
  <div class="stats-left">★ 4.8 分</div>
  <div class="stats-right">
    <span>点赞: 128</span>
    <span>浏览: 2.3k</span>
  </div>
</div>
```

**CSS 强制实现**：
```css
.stats-bar {
  display: flex;
  align-items: center;
  gap: 8px; /* 图标与文字间距 */
  padding: 12px 16px;
  font-size: 14px; /* 固定字号 */
  line-height: 22px;
  font-weight: 400;
  color: var(--o-color-info3, rgba(0,0,0,.6)); /* 使用 info3 颜色 Token */
  background: transparent; /* 无背景 */
  border: none; /* 无边框 */
}

/* 默认：无评分时左对齐 */
.stats-bar {
  justify-content: flex-start;
}

/* 特殊：有评分时左右分布 */
.stats-bar.has-rating {
  justify-content: space-between;
}
.stats-bar.has-rating > :not(.rating-element) {
  /* 非评分元素归入右侧组 */
}
```

**数据统计区域字体规格**：

| 规格项 | 值 | 说明 |
|--------|-----|------|
| 字号 | `font_size-tip1`（14px） | 固定值，不随卡片尺寸变化，始终小于正文 text1 |
| 行高 | `22px` | 固定值，保持与 tip1 行高一致 |
| 字重 | `400` (Regular) | 正常字重，非粗体 |
| 颜色 | `--o-color-info3` | 次要信息颜色 (rgba(0,0,0,.6)) |
| 图标间距 | `8px` | 统计项内图标与文字的间距 |
| 统计项间距 | `16px` | 不同统计项之间的间距 |

**视觉示例**：

```
┌─────────────────────────────────────┐
│ [无评分情况]                         │
│ ┌─────────────────────────────────┐ │
│ │ 👁 2.3k   💬 128               │ │ ← 14px / o-color-info3 / 间距8px
│ └─────────────────────────────────┘ │
└─────────────────────────────────────┘

┌─────────────────────────────────────┐
│ [有评分情况]                         │
│ ┌─────────────────────────────────┐ │
│ │ ★ 4.8分    👁 2.3k   💬 128     │ │ ← 评分左 / 其余右 / 同样14px+info3
│ └─────────────────────────────────┘ │
└─────────────────────────────────────┘
```

**注意事项**：
- 数据统计文字**禁止使用**其他字号（如 12px、13px、18px、20px），且不得放大到与正文 `text1`（16px）同级
- 数据统计区域为**辅助信息区**，其 `tip1`（14px）规格不受"正文 16px"约束，是有意降级的一档
- **必须使用** `--o-color-info3` 颜色 Token，确保视觉层级正确
- 当统计项包含图标时，图标与文字间距为 **8px**
- 多个独立统计项之间保持 **16px** 间距

**视觉示例**：

```
┌─────────────────────────────────────┐
│ [无评分情况]                         │
│ ┌─────────────────────────────────┐ │
│ │ ♥ 128  👁 2.3k  💬 45          │ │ ← 全部左对齐
│ └─────────────────────────────────┘ │
└─────────────────────────────────────┘

┌─────────────────────────────────────┐
│ [有评分情况]                         │
│ ┌─────────────────────────────────┐ │
│ │ ★ 4.8分     ♥ 128  👁 2.3k  💬 45│ │ ← 评分左，其余右
│ └─────────────────────────────────┘ │
└─────────────────────────────────────┘
```

**适用范围**：
- 此规则适用于 cover 类型的 stats-bar 区域
- icon 类型卡片如需显示统计信息同样适用
- 评分元素包括：星级图标（★）、数字评分（4.8/5.0）、百分制评分等

---

### 交互设计原则

> ✨ **设计要点**：
> 1. **渐进式反馈**：从单体→复合→图文，交互反馈强度递进（颜色→按钮→图片+颜色）
> 2. **一致性基准**：所有卡片类型的**内容文字颜色保持不变**（`--o-color-info3`），确保可读性
> 3. **主题色引导**：悬浮/按下状态统一使用 `--o-color-primary1` 作为高亮色，建立品牌一致性
> 4. **适度动效**：图文卡片图片缩放限制在 105%，避免过度动画影响性能
> 5. **无障碍考量**：颜色对比度符合 WCAG AA 标准，确保视觉障碍用户可识别状态变化
> 6. **容器稳定性**：hover 动效不得破坏原有布局结构，图片放大需通过 overflow:hidden + 内部元素 scale 实现
> 7. **操作区规范性**：Footer 按钮严格遵循左对齐 + Solid→Outline→Text 的顺序规范
| 标题文字 | `--o-color-info1` | `--o-color-info1` | rgb(0,0,0) → rgb(255,255,255) |
| 内容文字 | `--o-color-info3` | `--o-color-info3` | rgba(0,0,0,0.6) → rgba(255,255,255,0.6) |
| 图标颜色 | `--o-color-info1` | `--o-color-info1` | rgb(0,0,0) → rgb(255,255,255) |

---

### 字体样式映射

| 使用场景 | 字号 Token | 行高 Token | 字重 Token | 字号值 | 行高值 |
|---------|-----------|-----------|-----------|-------|-------|
| Header 标题 | `font_size-h3` | `line_height-h3` | `font_weight-bold` | 22px | 30px |
| Body 正文 | `font_size-text1` | `line_height-text1` | `font_weight-regular` | 16px | 24px |
| Footer 按钮 | `font_size-text1` | `line_height-text1` | `font_weight-regular` | 16px | 24px |

字体族：`font_family`（HarmonyOS / HarmonyHeiTi）

> **基准说明**：上表为 Desktop（≥1440px）基准值。`h3` / `text1` 均为响应式 Token，
> 在 Laptop / Pad 断点下会按各自的响应式梯度降档（见 `styles/tokens.md`），
> 卡片内文字只需绑定 Token，不写死 px。

---

### 组件层级结构

```
OCard（FRAME，自适应宽高）
│  cornerRadius: 4px（radius_control-m）
│  fill: color-fill2
│  effects: shadow-2（悬浮时）
│
├── [Header 头部 FRAME]（自适应宽/固定宽）
│   autoLayoutPadding: 见内边距规格
│   autoLayoutDirection: HORIZONTAL / VERTICAL
│   │
│   ├── [封面图片 IMAGE]（cover 类型）
│   │     width: 自适应（vertical）/ 固定（horizontal）
│   │     height: 固定比例或自适应
│   │   或 [图标容器 FRAME]（icon 类型）
│   │     width/height: 32px（icon_size-l）
│   │     cornerRadius: 4px
│   │     fill: color-primary1-light
│   │     └── [图标 Icon INSTANCE]
│   │           fill: color-primary1
│   │
│  └── [标题文字 PARAGRAPH]
│         font: font_family Bold | font_size-h3
│         fill: color-info1
│
├── [Body 内容区 FRAME]（自适应宽高）
│   autoLayoutPadding: 见内边距规格
│   └── [内容节点...]
│         font: font_family Regular | font_size-text1
│         fill: color-info3
│
└── [Footer 底部 FRAME]（自适应宽/固定宽）
    autoLayoutPadding: 见内边距规格
    autoLayoutDirection: HORIZONTAL
    └── [操作按钮 OButton INSTANCE...]
```

**type=full-cover 层级结构**（封面即卡片背景）

```
OCard（full-cover，FRAME，自适应宽高）
│  cornerRadius: 外档（radius_control-l，默认基准 4px）
│  background: 封面图铺满整卡（无独立内层圆角）
│  overflow: hidden（供封面 hover 缩放裁剪）
│
└── [内容层 FRAME]（叠于封面上层）
    autoLayoutPadding: 28px 24px 24px
    autoLayoutDirection: VERTICAL
    │
    ├── [Head 行 FRAME]（HORIZONTAL）
    │   ├── [图标槽 FRAME] 32px / cornerRadius 内档（radius_control-s）/ fill: color-fill2
    │   │     └── [图标 Icon] fill: color-primary1
    │   ├── [标题文字 PARAGRAPH]
    │   │     font: font_family Bold | font_size-h3
    │   │     fill: color-info1
    │   └── [徽标 FRAME] 高 20px / cornerRadius 100px / fill: color-primary1
    │         └── [徽标文字] font_size-tip2 / fill: color-white
    │
    ├── [正文文字 PARAGRAPH]
    │     font: font_family Regular | font_size-text1
    │     fill: color-info3
    │
    └── [内容区 FRAME]（可选，如界面示意）
          cornerRadius: 内档（radius_control-s）
          fill: color-fill2
          effects: shadow-1
```

> 📌 **结构要点**：full-cover 没有独立 Header 图片节点——封面挂在卡片根节点的 background 上，
> 所有内容（图标/标题/徽标/正文/内容区）都排在同一个「内容层」里，靠 padding 与封面边缘保持距离。
> 圆角取「外档 / 内档」两档（默认基准均为 4px，实际按社区 token），形成外大一档、内小一档的层次。

---

### 变体 componentKey 速查

| 变体组合 | componentKey | node_id |
|---------|-------------|---------|
| type=cover, layout=vertical, Dark=off | `a22044aaa494bba137bd3ebba74319449b998694` | `1042:21387` |
| type=cover, layout=vertical, Dark=on | `b442f4ab38ddf235f7b2f1ca16e5d011d777fa6a` | `1042:21393` |
| type=cover, layout=horizontal, Dark=off | `0fceafcdae43547862ffb167bdcce89f510c3c35` | `1080:9979` |
| type=cover, layout=horizontal, Dark=on | `1d440d9b3a482404e267607061409a25607dfa5b` | `1081:8783` |
| type=icon, layout=vertical, Dark=off | `6103b3265e752087a546bf1b77a0f1b5bfef888b` | `1042:21417` |
| type=icon, layout=vertical, Dark=on | `730ae95ca65d8dbe6c3c72776d98d3cca249ef39` | `1042:21423` |
| type=icon, layout=horizontal, Dark=off | `e2a10a804dc8cb9ab507d237c712f65c7cf12f5e` | `1042:21399` |
| type=icon, layout=horizontal, Dark=on | `e9916f1e7742eeeba207639563416798817546ad` | `1042:21408` |
| type=full-cover, layout=vertical, Dark=off | 待补充（Pixso 未建节点） | 待补充 |
| type=full-cover, layout=vertical, Dark=on | 待补充（Pixso 未建节点） | 待补充 |

> **说明**：`full-cover` 为 Web 实现侧新增的 type 取值（Pixso 尚无对应组件节点），
> 落地时以 `cover` 节点为基底改造：把封面图从 Header 区移到卡片根节点 background，
> 去掉 Header 独立 padding 与封面内层圆角即可。补齐节点后回填本表。

---

### Pixso 操作速查

1. **插入组件**：组件面板搜索「OCard」，拖入画布
2. **切换类型**：右侧面板 → type 属性 → 选择 `cover` / `icon`（`full-cover` 需按上文说明手工改造）
3. **切换布局**：右侧面板 → layout 属性 → 选择 `horizontal` / `vertical`
4. **切换主题**：右侧面板 → Dark 属性 → 选择 `off` / `on`
5. **设置宽度**：拖拽卡片边缘或设置容器宽度约束
6. **替换封面**：双击进入 Header → 替换封面图片资源
7. **替换图标**：双击进入 Header → 双击图标层 → 选择新图标
8. **修改标题**：双击进入 Header → 双击标题文字修改内容
9. **添加内容**：双击进入 Body → 添加内容节点
10. **添加按钮**：双击进入 Footer → 添加 OButton 组件实例

---

### 注意事项

- **宽度控制**：卡片宽度由容器或栅格系统控制，无固定宽度约束
- **高度自适应**：卡片高度由内容自适应，避免固定高度导致内容溢出
- **悬浮阴影**：默认无阴影，悬浮时显示 `shadow-2`，提升交互反馈
- **封面图片**：需确保图片比例与卡片布局匹配，避免图片变形
- **图标语义**：图标应与卡片内容语义一致，增强信息传达
- **深色模式**：背景色自动切换为深色值，确保在深色背景上可见
- **三段式结构**：Header、Body、Footer 可根据场景选择性使用，不必全部包含
- ⚠️ **Footer 操作按钮排列顺序（硬约束）**：Footer 内多个元素并排时，**操作按钮（OButton / 链接）必须排在左侧，辅助信息（版本号、日期、meta 等）排在右侧**。禁止将操作按钮放在右侧，无论 justify-content 如何设置。
- ⚠️ **Footer 左右边距须与 Body 对齐（硬约束）**：Footer 的左右内边距必须与 Body 保持一致——在"容器 8px + Body 额外 8px = 总计 16px"的模型下，Footer 也需要额外 `padding: 0 8px`，使操作按钮与正文文字左右对齐。禁止 Footer 左右无内边距导致内容顶到容器边缘。
- ⚠️ **Footer 操作按钮使用 OButton medium text 变体（硬约束）**：卡片 Footer 内的操作按钮必须使用 **OButton size=medium, Variant=text**。关键规格：高度 32px、padding=0（无水平内边距）、文字色 `grey-14`（Light: `rgb(0,0,0)` **黑色**）、无背景无边框。**禁止使用品牌蓝 `#002FA7` / `--o-color-primary1` 作为 text 变体文字色**——蓝色是 outline/solid 变体的颜色，text 变体始终为黑色。
- ⚠️ **full-cover 封面即背景，不设内层圆角（硬约束）**：full-cover 的封面铺满整卡并直接作为卡片 background，圆角只由卡片根节点的**外档**（`radius_control-l`）统一裁剪。禁止给封面再包一层带**内档**（`radius_control-s`）圆角的容器——那会退化成普通 `cover` 的"分区封面"语义。
- ⚠️ **full-cover 圆角分层（硬约束）**：full-cover 卡内元素（图标槽、界面示意区、内容区）统一取**内档**（`radius_control-s`），比卡片外圆角低一档，形成「外大一档 / 内小一档」的层次。两档默认基准均为 4px，实际取值按社区 token（见「圆角基准说明」）。
- ⚠️ **full-cover 单交互禁内嵌按钮（硬约束）**：当卡片有且只有一种交互行为（如整卡跳转）时，整卡即为 `<a>` 链接，**不得再内嵌操作按钮**；悬浮反馈只通过 `shadow-2` 体现。
- ⚠️ **full-cover 文字对比度（硬约束）**：文字叠在封面上，封面明度必须与文字色匹配——浅色/柔和彩底用 `info1`/`info3`，深色封面必须切换 `info1-inverse`/`info3-inverse`。

---

## Part C：交互与状态

### 状态定义

| 状态 | 说明 | 视觉差异 |
|------|------|---------|
| 默认 | 卡片正常显示，无悬浮 | 无阴影，无边框 |
| 悬浮 | 鼠标悬浮在卡片上 | 显示阴影 shadow-2 |
| 选中 | 卡片被选中（可选功能） | 边框切换为 color-primary1 或其他强调色 |
| 禁用 | 卡片禁用，不可交互 | 整体透明度降低或使用禁用色 |

### 交互行为

- **悬浮阴影**：鼠标悬浮时显示阴影，提升交互反馈
- **点击跳转**：卡片整体可点击，跳转到详情页或执行操作
- **按钮交互**：Footer 底部按钮可独立点击，执行对应操作

---

## Part D：设计变量绑定

### 推荐绑定变量

| 元素 | 属性 | 推荐变量 Token |
|------|------|---------------|
| 卡片背景 | fill | `--o-color-fill2` |
| 悬浮阴影 | effects | `shadow-2` |
| 圆角 | cornerRadius | `radius_control-m`（4px） |
| 标题文字 | fill | `--o-color-info1` |
| 内容文字 | fill | `--o-color-info3` |
| 图标颜色 | fill | `--o-color-primary1` |
| 图标背景 | fill | `--o-color-primary1-light` |
| 图标尺寸 | width/height | `icon_size-l`（32px） |
| Header 内边距 | autoLayoutPadding | 待确认 |
| Body 内边距 | autoLayoutPadding | 待确认 |
| Footer 内边距 | autoLayoutPadding | 待确认 |
| 区域间距 | autoLayoutItemSpacing | 待确认 |
| 标题字号 | fontSize | `font_size-h3`（22px） |
| 标题字重 | fontWeight | `font_weight-bold` |
| 正文字号 | fontSize | `font_size-text1`（16px） |

---

## Part E：最佳实践

### 使用建议

1. **类型匹配内容**：图文内容使用 cover 类型，功能/统计使用 icon 类型
2. **布局匹配场景**：独立展示使用 vertical，侧边栏或横向排列使用 horizontal
3. **宽度控制**：卡片宽度由栅格系统控制，遵循栅格规范
4. **内容密度**：卡片内容不宜过多，避免信息过载

### 设计提示

- 卡片悬浮阴影提升交互反馈，但同一区域不宜过多使用悬浮效果
- 封面图片比例应统一，如 16:9 或 1:1，避免不同卡片视觉不一致
- 图标尺寸和颜色应保持统一，增强视觉一致性
- Footer 底部按钮不超过 3 个，避免操作过多造成用户困惑
- 深色模式下背景色和边框色均需切换，确保可见性
- 卡片间距推荐 16px（gap-4）或 24px（gap-5），根据场景调整

---

## 技术备注

- 组件节点 ID：`1042:21386`（组件集）
- 设计稿 URL：https://pixso.cn/app/design/JZkjW0mhmT61Mtd98dCfBw?item-id=1042:21386
- 生成日期：2026-04-17
- 变体总数：8（2 type × 2 layout × 2 Dark）
- 卡片类型：cover（封面卡片）、icon（图标卡片）
- 布局方式：horizontal（水平）、vertical（垂直）
- 结构组成：Header（头部）、Body（内容区）、Footer（底部）
- 圆角：4px（radius_control-m）
- 悬浮阴影：shadow-2
- 数据来源：用户提供的设计规范信息，部分规格需从设计稿确认