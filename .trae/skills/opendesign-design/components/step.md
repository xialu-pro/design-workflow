> ← [组件索引](../SKILL.md#组件索引) · [README](../README.md)

# OStep 步骤条 · 设计 Skill

> 组件集合节点：`1042:16835` · 组件名：OStep 步骤条 · 变体总数：4

---

## Part A：设计使用卡

### 组件概览

**OStep 步骤条**：引导用户按照流程完成任务的导航组件，展示当前步骤在整个流程中的位置。通过步骤图标、标题和连接线清晰传达流程进度。支持进行中、完成、等待、成功、警告、错误等多种状态。

---

### 适用场景

- ✅ **表单分步填写**：多步骤表单流程，如注册、申请、提单等
- ✅ **购物流程**：电商购物车、下单、支付、完成等流程引导
- ✅ **任务引导**：新手引导、操作流程、配置步骤等
- ✅ **进度展示**：订单状态、审批流程、处理进度等
- ❌ **不适合**：无流程顺序的独立操作、单一页面任务

---

### 变体说明

**Direction（方向）**
- `h` — 水平方向，步骤横向排列，适合页面顶部流程展示
- `v` — 垂直方向，步骤纵向排列，适合侧边栏或内容区流程引导

**Dark（主题）**
- `off` — 浅色模式（默认）
- `on` — 深色模式，品牌色系和背景色自动切换为对应深色值

---

### 布局结构

> 🧩 **布局结构**：步骤条由多个步骤节点组成，每个节点包含图标容器、标题和连接线。水平方向时连接线在节点之间，垂直方向时连接线在节点下方。

**Direction=h 模式**

```
OStep（HORIZONTAL，自适应宽度，固定高度 64px）
├── autoLayoutItemSpacing: 8px
├── autoLayoutPadding: 32px（左/右）
│
├── [连接线容器 FRAME]（自适应宽度×16）
│   └── [直线 LINE]（1px高度）
│         stroke: --o-color-primary1（已完成）/ --o-color-control4（等待）
│
├── [步骤节点 FRAME]（32×64，VERTICAL）
│   ├── autoLayoutItemSpacing: 8px（标题↔图标）
│   ├── [标题容器 FRAME]（96×24）
│   │     └── [标题文字 PARAGRAPH]
│   │           fill: --o-color-primary1（进行中）/ --o-color-info2（完成）/ --o-color-info4（等待）
│   └── [图标容器 FRAME]（32×32，cornerRadius: 100）
│   │     fill: --o-color-primary1（进行中）/ --o-color-success1（完成）/ --o-color-primary4（等待）
│   │     └── [数字 PARAGRAPH]（12×28）
│   │           fill: white
│
├── [连接线容器 FRAME]
├── [步骤节点 FRAME]
├── [连接线容器 FRAME]
└── [步骤节点 FRAME]
```

**Direction=v 模式**

```
OStep（垂直排列，宽度 96px（含标题）/ 32px（仅图标），自适应高度）
│
├── [步骤节点 FRAME]（32×64，VERTICAL）
│   ├── autoLayoutItemSpacing: 8px（标题↔图标）
│   ├── [标题容器 FRAME]（96×24）
│   └── [图标容器 FRAME]（32×32，cornerRadius: 100）
│
├── [连接线 LINE]（40×1）
│   stroke: --o-color-primary1（已完成）/ --o-color-control4（等待）
│
├── [步骤节点 FRAME]
├── [连接线 LINE]
└── [步骤节点 FRAME]
```

---

### 组合搭配

> 🔗 **常见搭配**：
> - **表单分步**：`Direction=v` 模式，配合表单组件，左侧引导右侧内容
> - **顶部流程**：`Direction=h` 模式，置于页面顶部展示整体流程
> - **购物流程**：`Direction=h` 模式，展示下单-支付-完成流程
> - **移动端适配**：`Direction=v` 模式，垂直布局更适合小屏幕

---

### 设计稿识别指南

> 🔍 **识别特征**：
> - 包含圆形图标节点（32×32px，cornerRadius: 100），图标内显示数字（白色）
> - 步骤之间有连接线（LINE元素），颜色区分完成和等待状态
> - 每个步骤有标题文字（96×24px节点），颜色随状态变化
> - 水平布局时：整体高度64px，步骤间距8px，左右padding 32px
> - 垂直布局时：连接线高度40px（实际为LINE 40×1px）

> 🔄 **易混淆组件**：
> - 与 **OPagination 分页**：步骤条引导流程进度，分页用于数据分页导航
> - 与 **OBreadcrumb 面包屑**：步骤条是流程引导，面包屑是层级路径导航

---

### 响应式行为

参照 [栅格规范](../SKILL.md#数据资源) 中的断点定义：
- **PC（1920px）**：推荐使用 `Direction=h` 水平布局，流程展示清晰
- **移动端（360px）**：推荐切换为 `Direction=v` 垂直布局，节省横向空间

---
## Part A+：交互状态规范

### 状态总览

步骤条组件包含 **三种核心交互状态**，分别对应流程中的不同阶段：
[✓ 步骤一] ———— [② 步骤二] ———— [③ 步骤三] ↑ ↑ ↑ 已完成 进行中 未开始

---

### 状态一：已完成步骤（Completed）

**视觉特征**：
- 图标：绿色勾选符号（✓）
- 标题文字颜色较深，表示该步骤已执行完毕

**规格参数**：

| 属性 | 值 | 说明 |
|------|-----|------|
| 完成图标尺寸 | 24×24px | 勾选图标大小 |
| 图标色值 | `--o-color-white` | 白色勾选符号 |
| 背景色值 | `--o-color-success1` | 绿色背景，表示成功完成 |
| 标题字重 | Regular | 常规字重 |
| 标题字号 | 16px | 标准字号 |
| 标题色值 | `--o-color-info2` | 深灰色文字 |

**适用场景**：
- 用户已完成当前步骤，进入下一阶段
- 流程历史回顾，展示已完成的操作节点

---

### 状态二：进行中步骤（In Progress）⭐

**视觉特征**：
- 图标：蓝色实心圆 + 白色数字序号
- 标题文字加粗高亮，表示当前正在执行的步骤

**规格参数**：

| 属性 | 值 | 说明 |
|------|-----|------|
| 图标容器尺寸 | 32×32px | 圆形容器 |
| 数字字号 | 20px | 序号大小 |
| 数字色值 | `--o-color-white` | 白色数字 |
| 背景色值 | `--o-color-primary1` | 品牌蓝色背景 |
| 标题字重 | **SemiBold (600)** | 加粗强调 |
| 标题字号 | 16px | 标准字号 |
| 标题色值 | `--o-color-primary1` | 品牌蓝文字，与图标呼应 |

**设计要点**：
- 这是整个步骤条的**视觉焦点**
- 通过字重（SemiBold）和品牌色（primary1）双重强调
- 引导用户关注当前需要完成的任务

---

### 状态三：未开始步骤（Waiting/Pending）

**视觉特征**：
- 图标：浅灰色圆 + 灰色数字序号
- 标题文字颜色较浅，表示尚未到达的步骤

**规格参数**：

| 属性 | 值 | 说明 |
|------|-----|------|
| 图标容器尺寸 | 32×32px | 圆形容器 |
| 数字字号 | 20px | 序号大小 |
| 数字色值 | `--o-color-white` | 白色数字（或浅灰） |
| 背景色值 | `--o-color-primary4` | 浅蓝色/灰色背景 |
| 标题字重 | Regular | 常规字重 |
| 标题字号 | 16px | 标准字号 |
| 标题色值 | `--o-color-info3 / --o-color-info4` | 浅灰色文字 |

**设计原则**：
- 视觉层级最低，避免干扰用户对当前步骤的关注
- 颜色对比度适中，保证可读性的同时不抢焦点
- 预示未来步骤的存在，但不强调细节

---

### 连接线规范

**作用**：连接各步骤节点，暗示流程的方向性和连续性

| 连接区间 | 颜色 | 说明 |
|---------|------|------|
| 已完成 → 进行中 | `--o-color-primary1` | 品牌蓝，强调已打通的路径 |
| 进行中 → 未开始 | `--o-color-control4` | 浅灰色，表示待完成的路径 |

**样式**：
- 线宽：1px
- 样式：实线（Solid）
- 方向：水平布局时横向连接，垂直布局时纵向连接

---

### 状态转换逻辑
未开始 ──→ 进行中 ──→ 已完成 (等待) (当前) (完成)



**典型流程示例**（以3步流程为例）：

| 阶段 | 步骤1 | 步骤2 | 步骤3 |
|------|-------|-------|-------|
| 初始状态 | **进行中** | 未开始 | 未开始 |
| 完成步骤1 | 已完成 | **进行中** | 未开始 |
| 完成步骤2 | 已完成 | 已完成 | **进行中** |
| 全部完成 | 已完成 | 已完成 | 已完成 |

---

### 设计注意事项

✅ **最佳实践**：
- 同一时刻**只能有一个"进行中"步骤**
- "已完成"步骤出现在"进行中"步骤**左侧/上方**
- "未开始"步骤出现在"进行中"步骤**右侧/下方**
- 保持视觉一致性，所有步骤的间距、对齐方式统一

❌ **避免问题**：
- 不要同时高亮多个步骤（造成困惑）
- 不要使用过于相近的颜色区分状态（考虑无障碍访问）
- 不要省略连接线（破坏流程连贯性感知）

---

### 快速参考卡片

#### 颜色速查表

| 状态 | 背景色 Token | 文字色 Token | 视觉权重 |
|------|-------------|-------------|----------|
| **进行中** | `--o-color-primary1` | `--o-color-primary1` | ⭐⭐⭐ 最高 |
| **已完成** | `--o-color-success1` | `--o-color-info2` | ⭐⭐ 中等 |
| **未开始** | `--o-color-primary4` | `--o-color-info4` | ⭐ 最低 |

#### 字体速查表

| 元素 | 字号 | 字重 | 行高 |
|------|------|------|------|
| 标题（进行中） | 16px | SemiBold (600) | 24px |
| 标题（其他） | 16px | Regular (400) | 24px |
| 图标数字 | 20px | SemiBold (600) | 28px |
| 完成图标 | 24×24px | — | -- |

---


## Part B：规格速查参考

### 变体索引表

| 变体属性 | 可选值 | 默认值 | 视觉差异 |
|---------|--------|--------|---------|
| Direction | `h` / `v` | `h` | 水平排列/垂直排列 |
| Dark | `off` / `on` | `off` | 品牌色切换为深色主题对应值 |

---

### 布局规格

| 规格项 | Direction=h | Direction=v |
|--------|-------------|-------------|
| 整体高度 | 64px | 自适应 |
| 整体宽度 | 自适应 | 96px（含标题）/ 32px（仅图标） |
| 左右 Padding | 32px | — |
| Token（Padding） | `gap-4` | — |
| 步骤节点尺寸 | 32×64px | 32×64px |
| 步骤节点内部间距 | 8px（标题↔图标） | 8px（标题↔图标） |
| Token（内部间距） | `gap-2` | `gap-2` |
| 步骤节点间距（横向） | 8px | — |
| Token（横向间距） | `gap-2` | — |
| 连接线间距（纵向） | — | 40px |
| Token（纵向间距） | — | `gap-3` |
| 图标容器尺寸 | 32×32px | 32×32px |
| Token（图标容器） | `control_size-m` | `control_size-m` |
| 图标容器圆角 | 100px（全圆角） | 100px（全圆角） |
| 标题容器尺寸 | 96×24px | 96×24px |
| 数字节点尺寸 | 12×28px | 12×28px |

---

### 颜色 Token 映射

#### 进行中状态

| 区域 | Light 模式 | Dark 模式 | Token | RGB 值 |
|------|-----------|----------|-------|--------|
| 图标容器背景 | `--o-color-primary1` | `--o-color-primary1` | brand-6 | rgb(0,47,167) → rgb(73,122,248) |
| 图标内数字 | `white` | `white` | — | rgb(255,255,255) |
| 标题文字 | `--o-color-primary1` | `--o-color-primary1` | brand-6 | rgb(0,47,167) → rgb(73,122,248) |
| 连接线 | `--o-color-primary1` | `--o-color-primary1` | brand-6 | rgb(0,47,167) → rgb(73,122,248) |

#### 完成状态

| 区域 | Light 模式 | Dark 模式 | Token | RGB 值 |
|------|-----------|----------|-------|--------|
| 图标容器背景 | `--o-color-success1` | `--o-color-success1` | green-6 | rgb(11,177,81) → rgb(51,193,104) |
| 图标内数字 | `white` | `white` | — | rgb(255,255,255) |
| 标题文字 | `--o-color-info2` | `--o-color-info2` | grey-14 @ 0.8 | rgba(0,0,0,0.8) → rgba(255,255,255,0.8) |
| 连接线 | `--o-color-primary1` | `--o-color-primary1` | brand-6 | rgb(0,47,167) → rgb(73,122,248) |

#### 等待状态

| 区域 | Light 模式 | Dark 模式 | Token | RGB 值 |
|------|-----------|----------|-------|--------|
| 图标容器背景 | `--o-color-primary4` | `--o-color-primary4` | brand-3 | rgb(132,161,220) → rgb(29,51,120) |
| 图标内数字 | `white` | `white` | — | rgb(255,255,255) |
| 标题文字 | `--o-color-info4` | `--o-color-info4` | grey-14 @ 0.4 | rgba(0,0,0,0.4) → rgba(255,255,255,0.4) |
| 连接线 | `--o-color-control4` | `--o-color-control4` | grey-14 @ 0.1/0.15 | rgba(0,0,0,0.1) → rgba(255,255,255,0.15) |

#### 成功状态（success）

| 区域 | Light 模式 | Dark 模式 | Token | RGB 值 |
|------|-----------|----------|-------|--------|
| 图标容器背景 | `--o-color-success1` | `--o-color-success1` | green-6 | rgb(11,177,81) → rgb(51,193,104) |
| 图标内数字/勾号 | `white` | `white` | — | rgb(255,255,255) |
| 标题文字 | `--o-color-success1` | `--o-color-success1` | green-6 | rgb(11,177,81) → rgb(51,193,104) |
| 连接线 | `--o-color-success1` | `--o-color-success1` | green-6 | rgb(11,177,81) → rgb(51,193,104) |

#### 警告状态（warning）

| 区域 | Light 模式 | Dark 模式 | Token | RGB 值 |
|------|-----------|----------|-------|--------|
| 图标容器背景 | `--o-color-warning1` | `--o-color-warning1` | orange-6 | rgb(250,115,5) → rgb(251,143,43) |
| 图标内数字 | `white` | `white` | — | rgb(255,255,255) |
| 标题文字 | `--o-color-warning1` | `--o-color-warning1` | orange-6 | rgb(250,115,5) → rgb(251,143,43) |
| 连接线 | `--o-color-warning1` | `--o-color-warning1` | orange-6 | rgb(250,115,5) → rgb(251,143,43) |

#### 错误状态（danger）

| 区域 | Light 模式 | Dark 模式 | Token | RGB 值 |
|------|-----------|----------|-------|--------|
| 图标容器背景 | `--o-color-danger1` | `--o-color-danger1` | red-6 | rgb(230,0,18) → rgb(235,35,45) |
| 图标内数字 | `white` | `white` | — | rgb(255,255,255) |
| 标题文字 | `--o-color-danger1` | `--o-color-danger1` | red-6 | rgb(230,0,18) → rgb(235,35,45) |
| 连接线 | `--o-color-danger1` | `--o-color-danger1` | red-6 | rgb(230,0,18) → rgb(235,35,45) |

---

### 字体样式映射

| 使用场景 | 字号 Token | 行高 Token | 字重 | 字号值 | 行高值 |
|---------|-----------|-----------|------|-------|-------|
| 步骤标题（完成/等待） | `font_size-text1` | `line_height-text1` | regular | 16px | 24px |
| 步骤标题（进行中） | `font_size-text1` | `line_height-text1` | semibold | 16px | 24px |
| 图标内数字 | `font_size-h4` | `line_height-h4` | semibold | 20px | 28px |

> **说明**：数据来源于 Pixso 设计稿核实。tokens.json 中仅有 regular (400) 和 bold (600)，semibold 通常为 600。

字体族：`font_family`（HarmonyOS / HarmonyHeiTi Regular）

---

### 组件层级结构（精确）

```
OStep（Direction=h）
│  GUID: 1042:16836
│  Width: 自适应 | Height: 64px
│  Auto Layout: HORIZONTAL
│  autoLayoutItemSpacing: 8px
│  autoLayoutPaddingLeft: 32px
│  autoLayoutPaddingRight: 32px
│
├── [连接线容器 FRAME]
│   GUID: 组合 15981（等待状态）/ 组合 15980（完成状态）
│   Width: 自适应 | Height: 16px
│   └── [直线 LINE]
│       Width: 自适应 | Height: 1px
│       stroke: rgba(0,0,0,0.1)（Light等待）/ rgba(255,255,255,0.15)（Dark等待）
│               rgba(0,47,167,1)（Light完成）/ rgba(73,122,248,1)（Dark完成）
│
├── [步骤节点 FRAME]
│   GUID: 容器 95（进行中）/ 容器 91（完成）/ 容器 96（等待）
│   Width: 32px | Height: 64px
│   Auto Layout: VERTICAL
│   autoLayoutItemSpacing: 8px（标题↔图标间距）
│   │
│   ├── [标题容器 FRAME]
│   │   Width: 96px | Height: 24px
│   │   └── [标题文字 PARAGRAPH]
│   │       Width: 96px | Height: 24px
│   │       fill: rgba(0,47,167,1)（Light进行中）/ rgba(73,122,248,1)（Dark进行中）
│   │             rgba(0,0,0,0.8)（Light完成）/ rgba(255,255,255,0.8)（Dark完成）
│   │             rgba(0,0,0,0.4)（Light等待）/ rgba(255,255,255,0.4)（Dark等待）
│   │
│   └── [图标容器 FRAME]
│       GUID: 组合 15976（进行中）/ 组合 15974（完成）/ 组合 15978（等待）
│       Width: 32px | Height: 32px
│       cornerRadius: 100px（全圆角）
│       fill: rgba(0,47,167,1)（Light进行中）/ rgba(73,122,248,1)（Dark进行中）
│             rgba(11,177,81,1)（Light完成）/ rgba(51,193,104,1)（Dark完成）
│             rgba(132,161,220,1)（Light等待）/ rgba(29,51,120,1)（Dark等待）
│       └── [数字 PARAGRAPH]
│           Width: 12px | Height: 28px
│           fill: rgba(255,255,255,1)（白色）
│
├── [连接线容器 FRAME]（重复结构）
├── [步骤节点 FRAME]（重复结构）
├── [连接线容器 FRAME]（重复结构）
└── [步骤节点 FRAME]（重复结构）
```

---

### 变体 componentKey 速查

| 变体组合 | node_id | 尺寸 | 说明 |
|---------|---------|------|------|
| Direction=h, Dark=off | `1042:16836` | 591×64px | 水平布局，浅色模式 |
| Direction=h, Dark=on | `1042:16856` | 591×64px | 水平布局，深色模式 |
| Direction=v, Dark=off | `1065:8846` | 96×304px | 垂直布局，浅色模式 |
| Direction=v, Dark=on | `1065:8899` | 96×304px | 垂直布局，深色模式 |

> **注意**：componentKey 需从 Pixso 组件面板获取。

---

### Pixso 操作速查

1. **插入组件**：组件面板搜索「OStep」，拖入画布
2. **切换方向**：右侧面板 → Direction 属性 → 选择 `h` / `v`
3. **切换主题**：右侧面板 → Dark 属性 → 选择 `off` / `on`
4. **修改步骤数量**：进入组件 → 复制/删除步骤节点实例
5. **修改步骤状态**：进入步骤节点 → 修改图标容器背景色、标题文字色、连接线描边色
6. **修改标题内容**：进入步骤节点 → 双击标题文字修改内容

---

### 注意事项

- **进行中状态**：使用品牌色（brand-6）背景 + primary1 标题，视觉权重最高，仅一个步骤可处于此状态
- **完成状态**：使用成功色（green-6）背景 + info2 标题，传达已完成信息，进行中之前的步骤
- **等待状态**：使用禁用品牌色（brand-3）背景 + info4 标题，视觉降级，进行中之后的步骤
- **成功状态**：使用成功色（green-6）背景 + success1 标题，强调成功结果，标题和图标颜色一致
- **警告状态**：使用警告色（orange-6）背景 + warning1 标题，需要用户关注的步骤
- **错误状态**：使用错误色（red-6）背景 + danger1 标题，需要用户处理问题的步骤
- **连接线规则**：连接线颜色跟随前一步骤状态或当前步骤警示色
- **图标容器**：全圆角（cornerRadius: 100px），形成圆形步骤节点，内含白色数字
- **标题宽度**：固定 96px，可容纳较长标题文字（约 6-7 个汉字）
- **数字节点**：宽度 12px（数字"1"、"2"等实际宽度），高度 28px
- **水平布局**：左右各有 32px padding，确保边缘步骤不贴边
- **垂直布局**：连接线为独立 LINE 元素（40×1px），而非包含在容器中
- **深色模式**：品牌色自动切换为 rgb(73,122,248)，成功色切换为 rgb(51,193,104)，禁用品牌色切换为 rgb(29,51,120)

---

## Part C：交互与状态

### 状态定义

| 状态 | 图标容器背景 | 标题文字颜色 | 连接线颜色 | 说明 |
|------|-------------|-------------|-----------|------|
| 进行中 | `--o-color-primary1`（brand-6） | `--o-color-primary1` | `--o-color-primary1` | 当前步骤，仅一个 |
| 完成 | `--o-color-success1`（green-6） | `--o-color-info2` | `--o-color-primary1` | 已完成步骤，进行中之前 |
| 等待 | `--o-color-primary4`（brand-3） | `--o-color-info4` | `--o-color-control4` | 等待步骤，进行中之后 |
| 成功 | `--o-color-success1`（green-6） | `--o-color-success1` | `--o-color-success1` | 成功完成，标题强调成功色 |
| 警告 | `--o-color-warning1`（orange-6） | `--o-color-warning1` | `--o-color-warning1` | 警告状态，需要关注 |
| 错误 | `--o-color-danger1`（red-6） | `--o-color-danger1` | `--o-color-danger1` | 错误状态，需要处理 |

---

### 状态切换逻辑

- **进行中状态**：仅一个步骤处于进行中，是用户当前所在的步骤
- **完成状态**：进行中之前的所有步骤均为完成状态
- **等待状态**：进行中之后的所有步骤均为等待状态
- **成功状态**：可替代完成状态，用于强调成功结果（如支付成功、审批通过）
- **警告状态**：步骤存在问题需要关注，但不阻断流程（如库存不足、需补充材料）
- **错误状态**：步骤失败需要处理，阻断流程继续（如支付失败、审批驳回）
- **连接线规则**：连接线颜色跟随前一步骤状态或当前步骤警示色

---

### 示例：三步骤流程状态

```
步骤1（完成） ───primary1──→ 步骤2（进行中） ───control4──→ 步骤3（等待）
   绿色背景              品牌色背景                禁用品牌色背景
   info2标题            primary1标题              info4标题
```

---

## Part D：设计变量绑定

### 推荐绑定变量

| 元素 | 属性 | 推荐变量 Token |
|------|------|---------------|
| 进行中图标容器背景 | fill | `--o-color-primary1` |
| 进行中图标容器背景 | fill | `--o-color-primary1` |
| 完成图标容器背景 | fill | `--o-color-success1` |
| 成功图标容器背景 | fill | `--o-color-success1` |
| 警告图标容器背景 | fill | `--o-color-warning1` |
| 错误图标容器背景 | fill | `--o-color-danger1` |
| 等待图标容器背景 | fill | `--o-color-primary4` |
| 图标内数字 | fill | `white` |
| 进行中标题文字 | fill | `--o-color-primary1` |
| 完成标题文字 | fill | `--o-color-info2` |
| 成功标题文字 | fill | `--o-color-success1` |
| 警告标题文字 | fill | `--o-color-warning1` |
| 错误标题文字 | fill | `--o-color-danger1` |
| 等待标题文字 | fill | `--o-color-info4` |
| 完成/进行中连接线 | stroke | `--o-color-primary1` |
| 成功连接线 | stroke | `--o-color-success1` |
| 警告连接线 | stroke | `--o-color-warning1` |
| 错误连接线 | stroke | `--o-color-danger1` |
| 等待连接线 | stroke | `--o-color-control4` |
| 图标容器圆角 | cornerRadius | 100px（全圆角） |
| 图标容器尺寸 | width/height | `control_size-m`（32px） |
| 步骤间距 | autoLayoutItemSpacing | `gap-2`（8px） |
| 标题↔图标间距 | autoLayoutItemSpacing | `gap-2`（8px） |
| 连接线间距（垂直） | height | `gap-3`（40px） |
| 标题字号 | fontSize | `font_size-text1`（16px） |
| 数字字号 | fontSize | `font_size-h4`（20px） |

---

## Part E：最佳实践

### 使用建议

1. **步骤数量控制**：推荐 3-5 个步骤，过多步骤会降低用户完成率
2. **标题简洁明确**：每个步骤标题控制在 2-4 个字，如"下单"、"支付"、"完成"
3. **状态清晰区分**：通过颜色（primary1/success1/primary4）明确区分三种状态
4. **进度反馈及时**：用户完成当前步骤后立即更新步骤条状态

### 设计提示

- 步骤条通常置于流程页面顶部或侧边，与内容区域保持适当间距（16-24px）
- 水平布局适合流程步骤较少的场景（3-4 步），垂直布局适合步骤较多或移动端
- 标题宽度固定 96px，如需更长标题，考虑缩小字号或使用多行布局
- 连接线宽度自适应，确保在不同容器宽度下视觉效果一致
- 深色模式下注意所有元素色值同步切换，避免遗漏

---

## 技术备注

- 组件节点 ID：`1042:16835`（组件集）
- 设计稿 URL：https://pixso.cn/app/design/JZkjW0mhmT61Mtd98dCfBw?item-id=1042:16835
- 生成日期：2026-04-16
- 变体总数：4（Direction=h/v × Dark=off/on）
- 字号说明：基于 tokens.json 推断，与同尺寸控件保持一致
- 数据来源：Pixso MCP get_node_dsl（节点 ID: 1042:16836 等）