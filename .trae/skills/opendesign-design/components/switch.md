> ← [组件索引](../SKILL.md#组件索引) · [README](../README.md)

# OSwitch 开关 · 设计 Skill

> 组件集合节点：`1042:17522` · 组件名：OSwitch 开关 · 变体总数：12

---

## Part A：设计使用卡

### 组件概览

**OSwitch 开关**：用于二元状态（开启/关闭）快速切换的控件。通过滑动轨道和圆形滑块传达当前状态，选中时轨道填充品牌色，滑块位于右侧；未选中时轨道填充灰色，滑块位于左侧。支持三种变体：纯开关、带文字（开/关）、带图标（太阳/月亮，用于主题切换）。

---

### 适用场景

- ✅ **设置面板**：快速切换开关设置，如通知开关、隐私设置
- ✅ **权限配置**：单个权限项启用/禁用切换
- ✅ **功能开关**：功能模块启用/禁用，如夜间模式、自动保存
- ✅ **主题切换**：使用 icon 变体（太阳/月亮图标）切换浅色/深色主题
- ❌ **不适合**：多选场景（用 Checkbox）、单选场景（用 Radio）、表单必填项

---

### 变体说明

**check（开关状态）**
- `Selected` — 开启状态，轨道使用品牌色填充，滑块位于右侧
- `Unselected` — 关闭状态，轨道使用灰色填充，滑块位于左侧
- `Light` — icon 变体专属，显示太阳图标（浅色模式开启）
- `Dark` — icon 变体专属，显示月亮图标（深色模式开启）

**Dark（主题）**
- `off` — 浅色模式（默认）
- `on` — 深色模式，品牌色系和轨道色自动切换为对应深色值

**Variant（变体类型）**
- `regular` — 纯开关，无图标无文字，尺寸 40×24px
- `icon` — 带图标开关，显示太阳/月亮图标，用于主题切换，尺寸 48×24px
- `text` — 带文字开关，显示「开」/「关」文字，尺寸 44×24px

> **说明**：OSwitch 没有 Disabled（禁用）状态变体，所有变体均为 Enabled 状态。

---

### 布局结构

> 🧩 **布局结构**：开关由轨道（SYMBOL 本体）和滑块（ELLIPSE）组成。regular 变体仅包含滑块；icon 变体包含滑块和图标；text 变体包含滑块和文字。滑块尺寸固定 16×16px。

**Variant=regular 模式**

```
OSwitch（SYMBOL，40×24px，cornerRadius: 100px）
├── fill: 轨道背景色（Selected=brand-6 / Unselected=grey-6）
├── cornerRadius: 100px（全圆角）
│
└── [椭圆形 滑块 ELLIPSE]（16×16px）
      fill: white（Light）/ grey-4（Dark）
      位置：Selected=右侧 / Unselected=左侧
```

**Variant=text 模式**

```
OSwitch（SYMBOL，44×24px，cornerRadius: 100px）
├── fill: 轨道背景色
│
└── [容器 FRAME]（32×18px）
    ├── autoLayoutSpacing: 自适应
    ├── [文字 PARAGRAPH]（12×18px）
    │     fontSize: 12px
    │     text: "开"（Selected）/ "关"（Unselected）
    │     fill: Selected Light=white / Selected Dark=black
    │           Unselected Light=info4 / Unselected Dark=info4
    └── [椭圆形 滑块 ELLIPSE]（16×16px）
          fill: white（Light）/ grey-4（Dark）
```

**Variant=icon 模式**

```
OSwitch（SYMBOL，48×24px，cornerRadius: 100px）
├── fill: 轨道背景色
│
└── [容器 FRAME]（32×18px）
    ├── [图标 GROUP]（16×16px）
    │     Light: 太阳图标（用于切换到浅色模式）
    │     Dark: 月亮图标（用于切换到深色模式）
    └── [椭圆形 滑块 ELLIPSE]（16×16px）
          fill: brand-6（选中图标时）
```

---

### 组合搭配

> 🔗 **常见搭配**：
> - **设置列表**：与 ODataTable（单列模式）配合，每项开关控制一个设置
> - **表单开关项**：与 OForm 配合，开关项作为表单一部分
> - **主题切换器**：使用 icon 变体，配合全局主题切换逻辑
> - **权限配置**：与权限列表配合，开关控制单个权限

---

### 设计稿识别指南

> 🔍 **识别特征**：
> - 轨道节点（SYMBOL，cornerRadius: 100px 全圆角），高度固定 24px
> - 滑块节点（ELLIPSE，16×16px 圆形），位于轨道内部
> - Selected 状态：轨道品牌色填充，滑块位于右侧
> - Unselected 状态：轨道灰色填充，滑块位于左侧
> - text 变体：内部包含文字「开」或「关」，字号 12px
> - icon 变体：内部包含太阳或月亮图标（16×16px）

> 🔄 **易混淆组件**：
> - 与 **OCheckbox 复选框**：Switch 是滑动开关，用于二元状态切换；Checkbox 是勾选控件，支持多选
> - 与 **ORadio 单选框**：Switch 是独立开关；Radio 是单选组内互斥选项

---

### 响应式行为

参照 [栅格规范](../SKILL.md#数据资源) 中的断点定义，OSwitch 无自动断点响应式行为。状态和主题通过变体手动切换。

---

## Part B：规格速查参考

### 变体索引表

| 变体属性 | 可选值 | 默认值 | 视觉差异 |
|---------|--------|--------|---------|
| check | `Selected` / `Unselected` / `Light` / `Dark` | `Unselected` | 轨道颜色 + 滑块位置 + 图标类型 |
| Dark | `off` / `on` | `off` | 轨道色/滑块色切换为深色主题对应值 |
| Variant | `regular` / `icon` / `text` | `regular` | 尺寸差异 + 内容类型（无/图标/文字） |

---

### 布局规格

| 规格项 | regular | text | icon |
|--------|---------|------|------|
| 整体宽度 | 40px | 44px | 48px |
| 整体高度 | 24px | 24px | 24px |
| 轨道圆角 | 100px（全圆角） | 100px | 100px |
| 滑块尺寸 | 16×16px | 16×16px | 16×16px |
| 文字尺寸 | — | 12×18px | — |
| 文字字号 | — | 12px | — |
| 文字内容 | — | 「开」/「关」 | — |
| 图标尺寸 | — | — | 16×16px |

---

### 颜色 Token 映射

#### Selected 状态（开启）

| 区域 | Light 模式 | Dark 模式 | Token | RGB 值 |
|------|-----------|----------|-------|--------|
| 轨道背景 | `brand-6` | `brand-6` | `--o-color-primary1` | rgb(0,47,167) → rgb(73,122,248) |
| 滑块 | `white` | `grey-4` | `--o-color-fill2` | rgb(255,255,255) → rgb(36,36,39) |
| 文字（text 变体） | `white` | `black` | `--o-color-white` / `--o-color-black` | rgb(255,255,255) → rgb(0,0,0) |
| 图标（icon 变体） | `white` | `white` | `--o-color-white` | rgb(255,255,255) |

#### Unselected 状态（关闭）

| 区域 | Light 模式 | Dark 模式 | Token | RGB 值 |
|------|-----------|----------|-------|--------|
| 轨道背景 | `grey-6` | `grey-7` | `--o-color-control1-light` | rgb(222,222,227) → rgb(63,63,67) |
| 滑块 | `white` | `grey-4` | `--o-color-fill2` | rgb(255,255,255) → rgb(36,36,39) |
| 文字（text 变体） | `info4` | `info4` | `--o-color-info4` | rgb(85,85,92) → rgb(156,156,159) |
| 图标（icon 变体） | — | — | — | — |

#### Light 状态（icon 变体，浅色模式开启）

| 区域 | Light 模式 | Dark 模式 | Token | RGB 值 |
|------|-----------|----------|-------|--------|
| 轨道背景 | `grey-6` | `grey-7` | `--o-color-control1-light` | rgb(222,222,227) → rgb(63,63,67) |
| 滑块 | `brand-6` | `brand-6` | `--o-color-primary1` | rgb(0,47,167) → rgb(73,122,248) |
| 太阳图标 | `white` | `white` | `--o-color-white` | rgb(255,255,255) |

#### Dark 状态（icon 变体，深色模式开启）

| 区域 | Light 模式 | Dark 模式 | Token | RGB 值 |
|------|-----------|----------|-------|--------|
| 轨道背景 | `grey-6` | `grey-7` | `--o-color-control1-light` | rgb(222,222,227) → rgb(63,63,67) |
| 滑块 | `brand-6` | `brand-6` | `--o-color-primary1` | rgb(0,47,167) → rgb(73,122,248) |
| 月亮图标 | `white` | `white` | `--o-color-white` | rgb(255,255,255) |

---

### 字体样式映射

| 使用场景 | 字号 | 行高 | 字重 | 字号值 |
|---------|------|------|------|-------|
| text 变体文字 | 12px | 18px | Regular | 12px |

字体族：`font_family`（HarmonyOS / HarmonyHeiTi Regular）

---

### 组件层级结构

**Variant=regular**

```
OSwitch（SYMBOL，40×24px）
│  GUID: 1042:17523（Selected Light）/ 1042:17525（Selected Dark）
│        1042:17527（Unselected Light）/ 1042:17529（Unselected Dark）
│  Width: 40px | Height: 24px
│  cornerRadius: 100px（全圆角）
│  fill: rgb(0,47,167) → rgb(73,122,248) → Token: `--o-color-primary1`（Selected）
│        rgb(222,222,227) → rgb(63,63,67) → Token: `--o-color-control1-light`（Unselected）
│
└── [椭圆形 滑块 ELLIPSE]
    GUID: 1042:17524 等
    Width: 16px | Height: 16px
    fill: rgb(255,255,255) → rgb(36,36,39) → Token: `--o-color-fill2`
    位置：Selected 时靠右（left≈22），Unselected 时靠左（left≈2）
```

**Variant=text**

```
OSwitch（SYMBOL，44×24px）
│  GUID: 1042:17532（Selected Light）/ 1042:17536（Selected Dark）
│        1042:17540（Unselected Light）/ 1042:17544（Unselected Dark）
│  Width: 44px | Height: 24px
│  cornerRadius: 100px
│  fill: 同 regular 变体
│
└── [容器 FRAME]
    GUID: 1042:17533 等
    Width: 32px | Height: 18px
    │
    ├── [文字 PARAGRAPH]
    │   Width: 12px | Height: 18px
    │   fontSize: 12px | lineHeight: 18px
    │   nodeText: "开"（Selected）/ "关"（Unselected）
    │   fill: Selected Light=white → Token: `--o-color-white`
    │         Selected Dark=black → Token: `--o-color-black`
    │         Unselected Light=info4 → Token: `--o-color-info4`
    │         Unselected Dark=info4 → Token: `--o-color-info4`
    │
    └── [椭圆形 滑块 ELLIPSE]
        Width: 16px | Height: 16px
        fill: 同 regular 变体
```

**Variant=icon**

```
OSwitch（SYMBOL，48×24px）
│  GUID: 1042:17549（Light Light）/ 1042:17567（Light Dark）
│        1042:17587（Dark Light）/ 1042:17606（Dark Dark）
│  Width: 48px | Height: 24px
│  cornerRadius: 100px
│  fill: rgb(222,222,227) → rgb(63,63,67) → Token: `--o-color-control1-light`
│
└── [容器 FRAME]
    Width: 32px | Height: 18px
    │
    ├── [图标 GROUP]（太阳/月亮）
    │   Width: 16px | Height: 16px
    │   Light: 太阳图标 → 表示切换到浅色模式
    │   Dark: 月亮图标 → 表示切换到深色模式
    │   fill: white → Token: `--o-color-white`
    │
    └── [椭圆形 滑块 ELLIPSE]
        Width: 16px | Height: 16px
        fill: rgb(0,47,167) → rgb(73,122,248) → Token: `--o-color-primary1`
```

---

### 变体 componentKey 速查

| 变体组合 | node_id | 尺寸 | 说明 |
|---------|---------|------|------|
| check=Selected, Dark=off, Variant=regular | `1042:17523` | 40×24px | 开启，浅色，纯开关 |
| check=Selected, Dark=on, Variant=regular | `1042:17525` | 40×24px | 开启，深色，纯开关 |
| check=Unselected, Dark=off, Variant=regular | `1042:17527` | 40×24px | 关闭，浅色，纯开关 |
| check=Unselected, Dark=on, Variant=regular | `1042:17529` | 40×24px | 关闭，深色，纯开关 |
| check=Selected, Dark=off, Variant=text | `1042:17532` | 44×24px | 开启，浅色，带文字"开" |
| check=Selected, Dark=on, Variant=text | `1042:17536` | 44×24px | 开启，深色，带文字"开" |
| check=Unselected, Dark=off, Variant=text | `1042:17540` | 44×24px | 关闭，浅色，带文字"关" |
| check=Unselected, Dark=on, Variant=text | `1042:17544` | 44×24px | 关闭，深色，带文字"关" |
| check=Light, Dark=off, Variant=icon | `1042:17549` | 48×24px | 太阳图标，浅色模式开启 |
| check=Light, Dark=on, Variant=icon | `1042:17567` | 48×24px | 太阳图标，深色模式开启 |
| check=Dark, Dark=off, Variant=icon | `1042:17587` | 48×24px | 月亮图标，浅色模式开启 |
| check=Dark, Dark=on, Variant=icon | `1042:17606` | 48×24px | 月亮图标，深色模式开启 |

---

### Pixso 操作速查

1. **插入组件**：组件面板搜索「OSwitch」，拖入画布
2. **切换开关状态**：右侧面板 → check 属性 → 选择 `Selected` / `Unselected` / `Light` / `Dark`
3. **切换主题**：右侧面板 → Dark 属性 → 选择 `off` / `on`
4. **切换变体类型**：右侧面板 → Variant 属性 → 选择 `regular` / `icon` / `text`
5. **修改文字内容**：text 变体 → 双击进入组件 → 双击文字图层修改（默认「开」/「关」）

---

### 注意事项

- **无 Disabled 状态**：OSwitch 没有 Disabled（禁用）变体，所有状态均为 Enabled，可交互
- **轨道圆角**：全圆角（cornerRadius: 100px），视觉上是胶囊形轨道
- **滑块位置**：Selected 时滑块位于右侧，Unselected 时位于左侧，通过 Auto Layout 或绝对定位实现
- **滑块颜色**：Selected 状态滑块使用 `--o-color-fill2`（Light=white，Dark=grey-4）；Unselected 同样
- **轨道颜色差异**：Selected 使用 `--o-color-primary1`（brand-6），Unselected 使用 `--o-color-control1-light`（grey-6/grey-7）
- **text 变体文字**：字号 12px，内容固定为「开」或「关」，颜色随状态和主题变化
- **icon 变体用途**：专用于主题切换场景，Light 显示太阳图标，Dark 显示月亮图标
- **icon 变体滑块**：滑块颜色为 `--o-color-primary1`（brand-6），与选中图标对应
- **Dark 模式轨道**：Unselected Dark 使用 grey-7（rgb(63,63,67)）而非 grey-6，确保深色背景下可识别

---

## Part C：交互与状态

### 交互状态总览

#### 未选中状态（Unselected）

| 交互状态 | 滑块规格 | 文字规格 | 轨道色值 |
|---------|----------|----------|----------|
| **默认（Default）** | 圆圈：16×16px，`color-fill2`<br>Token: `--o-color-fill2` | 字体：12px，`color-info4`<br>Token: `--o-color-info4` | `color-control1-light`<br>Token: `--o-color-control1-light` |
| **悬浮（Hover）** | 圆圈：16×16px，`color-fill2`<br>Token: `--o-color-fill2` | 字体：12px，`color-info4`<br>Token: `--o-color-info4` | `color-control2-light`<br>Token: `--o-color-control2-light` |
| **按下（Pressed/Active）** | 圆圈：16×16px，`color-fill2`<br>Token: `--o-color-fill2` | 字体：12px，`color-info4`<br>Token: `--o-color-info4` | `color-control3-light`<br>Token: `--o-color-control3-light` |
| **选中（Focused）** | 圆圈：16×16px，`color-fill2`<br>Token: `--o-color-fill2` | 字体：12px，`color-info4`<br>Token: `--o-color-info4` | `color-primary1`<br>Token: `--o-color-primary1` |
| **禁用（Disabled）** | 圆圈：16×16px，`color-fill2`（60%不透明度）<br>Token: `--o-color-fill2` | 字体：12px，`color-white`<br>Token: `--o-color-white` | `color-control4-light`<br>Token: `--o-color-control4-light` |

#### 已选中状态（Selected）

| 交互状态 | 滑块规格 | 文字规格 | 轨道色值 |
|---------|----------|----------|----------|
| **默认（Default）** | 圆圈：16×16px，`color-fill2`<br>Token: `--o-color-fill2` | 字体：12px，`color-white`<br>Token: `--o-color-white` | `color-primary1`<br>Token: `--o-color-primary1` |
| **悬浮（Hover）** | 圆圈：16×16px，`color-fill2`<br>Token: `--o-color-fill2` | 字体：12px，`color-white`<br>Token: `--o-color-white` | `color-primary2`<br>Token: `--o-color-primary2` |
| **按下（Pressed/Active）** | 圆圈：16×16px，`color-fill2`<br>Token: `--o-color-fill2` | 字体：12px，`color-white`<br>Token: `--o-color-white` | `color-primary3`<br>Token: `--o-color-primary3` |
| **选中（Focused）** | 圆圈：16×16px，`color-fill2`<br>Token: `--o-color-fill2` | 字体：12px，`color-white`<br>Token: `--o-color-white` | `color-primary1`<br>Token: `--o-color-primary1` |
| **禁用（Disabled）** | 圆圈：16×16px，`color-fill2`（40%不透明度）<br>Token: `--o-color-fill2` | 字体：12px，`color-white`<br>Token: `--o-color-white` | `color-primary4`<br>Token: `--o-color-primary4` |

---

### 交互状态详解

#### 1. 默认状态（Default）
- **未选中**：轨道使用 `control1-light`（浅灰色），滑块位于左侧
- **已选中**：轨道使用 `primary1`（品牌主色），滑块位于右侧
- 文字颜色根据选中状态显示（未选中=info4，已选中=white）

#### 2. 悬浮状态（Hover）
- 鼠标悬停时触发
- **未选中**：轨道色从 `control1-light` → `control2-light`（略深灰色）
- **已选中**：轨道色从 `primary1` → `primary2`（品牌色悬浮态）
- 滑块和文字颜色保持不变

#### 3. 按下状态（Pressed/Active）
- 鼠标按下或触摸时触发
- **未选中**：轨道色从 `control2-light` → `control3-light`（更深的灰色）
- **已选中**：轨道色从 `primary2` → `primary3`（品牌色按下态）
- 可选：滑块略微缩小或增加阴影效果

#### 4. 选中/聚焦状态（Focused）
- 键盘Tab聚焦或点击后保持的视觉反馈
- **未选中**：轨道色切换为 `primary1`（与已选中默认态相同），提供明确的聚焦指示
- **已选中**：轨道色保持 `primary1`
- 建议配合外边框或阴影增强聚焦可见性

#### 5. 禁用状态（Disabled）
- 组件不可交互时的视觉表现
- **未选中**：
  - 轨道色：`control4-light`（最浅的灰色，表示不可用）
  - 滑块：`fill2` 降低至 60% 不透明度
  - 文字：`white`（降低对比度）
- **已选中**：
  - 轨道色：`primary4`（品牌色的禁用版本，通常更浅或更低饱和度）
  - 滑块：`fill2` 降低至 40% 不透明度（比未选中禁用更明显）
  - 文字：`white`

---

### 交互状态映射表

| 元素 | 状态 | Token 变量 | 说明 |
|------|------|-----------|------|
| **轨道背景** | 未选中-默认 | `--o-color-control1-light` | 关闭状态的默认背景 |
| **轨道背景** | 未选中-悬浮 | `--o-color-control2-light` | 关闭状态的悬浮背景 |
| **轨道背景** | 未选中-按下 | `--o-color-control3-light` | 关闭状态的按下背景 |
| **轨道背景** | 未选中-聚焦 | `--o-color-primary1` | 关闭状态的聚焦背景（使用品牌色强调） |
| **轨道背景** | 未选中-禁用 | `--o-color-control4-light` | 关闭状态的禁用背景 |
| **轨道背景** | 已选中-默认 | `--o-color-primary1` | 开启状态的默认背景 |
| **轨道背景** | 已选中-悬浮 | `--o-color-primary2` | 开启状态的悬浮背景 |
| **轨道背景** | 已选中-按下 | `--o-color-primary3` | 开启状态的按下背景 |
| **轨道背景** | 已选中-聚焦 | `--o-color-primary1` | 开启状态的聚焦背景 |
| **轨道背景** | 已选中-禁用 | `--o-color-primary4` | 开启状态的禁用背景 |
| **滑块填充** | 所有状态 | `--o-color-fill2` | 滑块基础颜色（禁用时调整不透明度） |
| **文字颜色** | 未选中 | `--o-color-info4` | 关闭状态文字（默认/悬浮/按下/聚焦） |
| **文字颜色** | 已选中 | `--o-color-white` | 开启状态文字 |
| **文字颜色** | 禁用 | `--o-color-white` | 禁用状态文字（降低可读性提示） |

---

### 状态切换逻辑

- **Unselected → Selected**：轨道背景切换为 `--o-color-primary1`，滑块滑动到右侧，文字切换为「开」
- **Selected → Unselected**：轨道背景切换为 `--o-color-control1-light`，滑块滑动到左侧，文字切换为「关」
- **Light → Dark（icon 变体）**：太阳图标切换为月亮图标，表示切换到深色模式
- **Dark → Light（icon 变体）**：月亮图标切换为太阳图标，表示切换到浅色模式
- **Light → Dark（主题）**：轨道色、滑块色、文字色同步切换为深色主题对应值

---

### Hover 状态（悬浮态）

> ⚠️ **重要更新**：根据最新交互规范，Hover 状态已完善为：

| 元素 | 未选中 Hover | 已选中 Hover |
|------|-------------|-------------|
| **轨道背景** | `--o-color-control1-light` → `--o-color-control2-light`<br>（浅灰 → 略深灰） | `--o-color-primary1` → `--o-color-primary2`<br>（品牌主色 → 品牌色悬浮态） |
| **滑块** | 保持 `--o-color-fill2`，尺寸/颜色不变 | 保持 `--o-color-fill2`，尺寸/颜色不变 |
| **文字** | 保持 `--o-color-info4` | 保持 `--o-color-white` |

**交互说明**：
- 鼠标悬停时仅轨道背景颜色变化
- 滑块和文字保持默认态样式不变
- 可选增强：滑块轻微放大或添加阴影效果

---

## Part D：设计变量绑定

### 推荐绑定变量

| 元素 | 属性 | 推荐变量 Token |
|------|------|---------------|
| 轨道背景（Selected） | fill | `--o-color-primary1` |
| 轨道背景（Unselected） | fill | `--o-color-control1-light` |
| 滑块（regular/text） | fill | `--o-color-fill2` |
| 滑块（icon 变体） | fill | `--o-color-primary1` |
| 文字（Selected Light） | fill | `--o-color-white` |
| 文字（Selected Dark） | fill | `--o-color-black` |
| 文字（Unselected） | fill | `--o-color-info4` |
| 图标（icon 变体） | fill | `--o-color-white` |
| 轨道圆角 | cornerRadius | 100px（全圆角） |
| 滑块尺寸 | width/height | 16px |
| 文字字号 | fontSize | 12px |

---

## Part E：最佳实践

### 使用建议

1. **二元状态**：Switch 仅用于二元状态（开启/关闭）切换，不适合多选项场景
2. **默认状态**：根据功能默认开启或关闭，避免用户困惑
3. **主题切换**：使用 icon 变体（太阳/月亮），配合全局主题切换逻辑
4. **文字开关**：text 变体适合需要明确状态提示的场景（如「开」/「关」）
5. **纯开关**：regular 变体适合简洁界面或已通过其他方式提示状态的场景

### 设计提示

- Switch 轨道高度固定 24px，确保与其他控件高度一致
- 滑块尺寸固定 16×16px，位于轨道内边缘位置
- icon 变体专用于主题切换，不建议用于其他场景
- text 变体文字固定「开」/「关」，不建议自定义文字
- 同一页面内 Switch 数量不宜过多，避免用户疲劳
- 开关切换应有动效反馈（滑块滑动动画）

---

## 技术备注

- 组件节点 ID：`1042:17522`（组件集）
- 设计稿 URL：https://pixso.cn/app/design/JZkjW0mhmT61Mtd98dCfBw?item-id=1042:17522
- 生成日期：2026-04-17
- 变体总数：12（check=Selected/Unselected × Dark=off/on × Variant=regular/text + check=Light/Dark × Dark=off/on × Variant=icon）
- 无 Disabled 状态：所有变体均为 Enabled
- 尺寸数据：regular=40×24px，text=44×24px，icon=48×24px
- 滑块尺寸：16×16px（ELLIPSE）
- 轨道圆角：100px（全圆角）
- 文字字号：12px（text 变体）
- icon 变体：Light=太阳图标，Dark=月亮图标，用于主题切换