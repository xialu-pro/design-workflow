> ← [组件索引](../SKILL.md#组件索引) · [README](../README.md)

# OToggle 切换按钮 · 设计 Skill

> 组件集合节点：`1042:17416` · 组件名：OToggle 切换按钮 · 变体总数：8

---

## Part A：设计使用卡

### 组件概览

**OToggle 切换按钮**：用于筛选、过滤场景的单选/多选切换控件。通过背景填充和描边样式区分选中与未选中状态。多个 Toggle 按钮组合使用时，可实现单选（类似 Radio）或多选（类似 Checkbox）效果。支持两种尺寸（medium/small）和 Light/Dark 主题。

---

### 适用场景

- ✅ **筛选条件**：表格筛选、搜索筛选、商品筛选等场景
- ✅ **过滤标签**：内容过滤、类型过滤、状态过滤
- ✅ **视图切换**：列表/卡片视图切换、排列方式切换
- ✅ **工具栏按钮组**：编辑工具栏、操作工具栏的按钮组
- ❌ **不适合**：页面主导航（用 OTab）、表单提交按钮（用 OButton）、二元状态切换（用 OSwitch）

---

### 变体说明

**size（尺寸）**
- `medium` — 宽度自适应（最小约 124px），高度 32px，字号 16px，适合大多数筛选场景
- `small` — 宽度自适应（最小约 68px），高度 28px，字号 12px，适合紧凑布局或标签筛选

**checked（选中状态）**
- `Enabled` — 未选中/默认状态，背景填充 grey-2/grey-3，无描边，文字使用 grey-14
- `Actived` — 选中状态，背景透明，描边 brand-6，文字使用 brand-6

**Dark（主题）**
- `off` — 浅色模式（默认）
- `on` — 深色模式，背景色和品牌色自动切换为对应深色值

> **说明**：`checked=Group` 变体是组合示例，展示多个 Toggle 按钮的排列效果，不是单独的组件状态。

---

### 布局结构

> 🧩 **布局结构**：Toggle 按钮是单个矩形按钮，包含文字标签。未选中状态使用背景填充，选中状态使用描边样式。按钮宽度随文字自适应。

**Enabled（未选中）状态**

```
OToggle（SYMBOL，自适应宽度，固定高度）
├── Width: 自适应（文字宽度 + padding）
├── Height: 32px（medium）/ 28px（small）
├── cornerRadius: 4px
├── fill: grey-2（Light）/ grey-3（Dark）
├── stroke: 无
│
└── [文字 PARAGRAPH]（自适应宽度）
      fontSize: 16px（medium）/ 12px（small）
      fill: grey-14（Light）/ grey-14（Dark）
      text: 筛选条件文字
```

**Actived（选中）状态**

```
OToggle（SYMBOL，自适应宽度，固定高度）
├── Width: 自适应（文字宽度 + padding）
├── Height: 32px（medium）/ 28px（small）
├── cornerRadius: 4px
├── fill: transparent（无背景填充）
├── stroke: brand-6（Light）/ brand-6（Dark）
├── strokeWeight: 1px INSIDE
│
└── [文字 PARAGRAPH]（自适应宽度）
      fontSize: 16px（medium）/ 12px（small）
      fill: brand-6（Light）/ brand-6（Dark）
      text: 筛选条件文字
```

---

### 组合搭配

> 🔗 **常见搭配**：
> - **筛选栏**：多个 OToggle 水平排列，配合筛选结果区域
> - **工具栏**：与 OButton、OIcon 配合，形成操作工具栏
> - **标签组**：多个 Toggle 组合，实现单选或多选筛选
> - **视图切换**：列表/卡片视图切换，配合内容区域
---

### 组合使用场景详解（重要）

> ⚠️ **核心原则**：OToggle 组件通常以**按钮组形式组合使用**，而非单独使用。

#### 布局规范：标签与按钮组对齐
筛选父类型 [全部] [✓单选中] [筛选条件] [筛选条件] [...] 筛选子类型 [全部] [✓单选中] [筛选条件] [筛选条件] [...]

- ✅ **水平排列**：标签与按钮组在同一行，左对齐
- ✅ **垂直居中**：标签文字与按钮高度中心对齐（`align-items: center`）
- ✅ **间距统一**：标签与按钮间距 16px
- ✅ **标签颜色**：`--o-color-info1`（黑色 rgb(0,0,0)）
- ✅ **不换行**：标签文字 `white-space: nowrap`

#### 多层级筛选布局规范（重要）

当存在**筛选父类型**和**筛选子类型**的多层级筛选场景时：

```
┌─────────────────────────────────────────────────────┐
│ 筛选父类型  [全部] [✓单选中] [筛选条件] [筛选条件]    │
│ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ │  ← 1px 分隔线
│ 筛选子类型  [全部] [✓单选中] [筛选条件] [筛选条件]    │
└─────────────────────────────────────────────────────┘
```

- ✅ **分隔线样式**：筛选父类型与筛选子类型之间使用 **1px 实线分隔**
- ✅ **分隔线颜色**：`--o-color-control4`（控制色-4）
- ✅ **上下间距**：分隔线距离上下内容各 **16px**（`margin-top/bottom: 16px`）
- ✅ **视觉层次**：通过分隔线清晰区分不同的筛选维度
- ✅ **适用场景**：多维度筛选、父子级联筛选、分类+子类筛选

> **CSS 实现示例**：
> ```css
> .filter-divider {
>   height: 1px;
>   background: var(--o-color-control4);
>   margin: 16px 0;
> }
> ```

#### 使用模式

**模式一：单选模式（Radio）⭐ 最常用**
- 适用场景：类型筛选、状态筛选、互斥选择
- 规则：同一组仅一个选中，点击其他自动取消当前项

**模式二：多选模式（Checkbox）**
- 适用场景：标签筛选、多维度过滤、可同时选择多项
- 规则：可多选，再次点击已选项可取消选中

#### 多行筛选组合（真实业务场景）

当需要多维度筛选时，采用**多行独立控制**布局：
- 每行代表一个筛选维度（类型、状态、时间范围等）
- 每行独立管理自己的选择状态
- 行间用分隔线或间距区分（建议 20-24px）

#### 包含禁用状态的组合

- 禁用态透明度降低至 50%
- 不响应任何鼠标事件（hover、click、active 均无效）
- 光标显示为 `not-allowed`
- 适用于权限限制或数据不可用的场景

---

### 设计稿识别指南

> 🔍 **识别特征**：
> - 矩形按钮节点（SYMBOL），高度固定 28/32px
> - 圆角 4px，宽度随文字自适应
> - Enabled 状态：背景填充（grey-2/grey-3），无描边
> - Actived 状态：无背景填充，描边 1px INSIDE（brand-6）
> - 文字字号：medium=16px，small=12px
> - 文字颜色：Enabled=grey-14，Actived=brand-6

> 🔄 **易混淆组件**：
> - 与 **OTab 标签页**：Tab 用于页面主导航切换，多个 Tab 联动选中；Toggle 用于筛选/过滤，可单选或多选
> - 与 **OButton 按钮**：Button 用于触发操作；Toggle 用于状态切换，有明确的选中/未选中视觉区分
> - 与 **OCheckbox 复选框**：Checkbox 是勾选控件，方形+勾号；Toggle 是按钮形态，背景/描边区分状态

---

### 响应式行为

参照 [栅格规范](../SKILL.md#数据资源) 中的断点定义，OToggle 按钮宽度随文字自适应，无固定断点响应式行为。

---

## Part B：规格速查参考

### 变体索引表

| 变体属性 | 可选值 | 默认值 | 视觉差异 |
|---------|--------|--------|---------|
| size | `medium` / `small` | `medium` | 高度 32/28px，字号 16/12px |
| checked | `Enabled` / `Actived` | `Enabled` | 背景/描边样式区分 |
| Dark | `off` / `on` | `off` | 背景色/品牌色切换为深色主题对应值 |

---

### 布局规格

| 规格项 | medium | small |
|--------|--------|-------|
| 整体高度 | 32px | 28px |
| 整体宽度 | 自适应（示例 124px） | 自适应（示例 68px） |
| 圆角 | 4px | 4px |
| Token（圆角） | `radius_control-xs` | `radius_control-xs` |
| Token（高度） | `control_size-m` | — |
| 文字字号 | 16px | 12px |
| Token（字号） | `font_size-text1` | `font_size-tip2` |
| 文字高度 | 24px | 16px |
| Token（行高） | `line_height-text1` | — |
| 描边宽度 | 1px INSIDE（Actived） | 1px INSIDE（Actived） |

---

### 颜色 Token 映射

#### Enabled（未选中）状态

| 区域 | Light 模式 | Dark 模式 | Token | RGB 值 |
|------|-----------|----------|-------|--------|
| 按钮背景 | `grey-2` | `grey-3` | `--o-color-fill1` | rgb(243,243,245) → rgb(26,26,28) |
| 文字 | `grey-14` | `grey-14` | `--o-color-info1` | rgb(0,0,0) → rgb(255,255,255) |
| 描边 | 无 | 无 | — | — |

#### Actived（选中）状态

| 区域 | Light 模式 | Dark 模式 | Token | RGB 值 |
|------|-----------|----------|-------|--------|
| 按钮背景 | transparent | transparent | — | 无填充 |
| 描边 | `brand-6` | `brand-6` | `--o-color-primary1` | rgb(0,47,167) → rgb(73,122,248) |
| 文字 | `brand-6` | `brand-6` | `--o-color-primary1` | rgb(0,47,167) → rgb(73,122,248) |

> **说明**：Actived 状态按钮背景透明，仅通过描边和文字颜色区分选中状态。描边宽度 1px INSIDE。

---

### 字体样式映射

| 使用场景 | 字号 Token | 行高 Token | 字重 Token | 字号值 | 行高值 |
|---------|-----------|-----------|-----------|-------|-------|
| medium 文字 | `font_size-text1` | `line_height-text1` | `font_weight-regular` | 16px | 24px |
| small 文字 | `font_size-tip2` | — | `font_weight-regular` | 12px | 16px |

字体族：`font_family`（HarmonyOS / HarmonyHeiTi Regular）

---

### 组件层级结构

```
OToggle（SYMBOL）
│  GUID: 1042:17417（medium Enabled Light）/ 1042:17423（medium Actived Light）
│        1042:17516（small Enabled Light）/ 1042:17510（small Actived Light）
│  Width: 自适应（示例 124px medium / 68px small）| Height: 32px（medium）/ 28px（small）
│  cornerRadius: 4px
│  fill: rgb(243,243,245) → Token: `--o-color-fill1`（Enabled Light）
│        rgb(26,26,28) → Token: `--o-color-fill1`（Enabled Dark）
│        transparent（Actived）
│  stroke: 无（Enabled）/ rgba(0,47,167,1) → Token: `--o-color-primary1`（Actived Light）
│           rgba(73,122,248,1) → Token: `--o-color-primary1`（Actived Dark）
│  strokeWeight: 1px INSIDE（仅 Actived）
│  strokeAlign: INSIDE
│
└── [文字 PARAGRAPH]
    GUID: 1042:17419（medium）/ 1042:17518（small）
    Width: 自适应 | Height: 24px（medium）/ 16px（small）
    fontSize: 16px（medium）/ 12px（small）
    fontFamily: HarmonyHeiTi | fontStyle: Regular
    fill: rgb(0,0,0) → Token: `--o-color-info1`（Enabled Light）
          rgb(255,255,255) → Token: `--o-color-info1`（Enabled Dark）
          rgb(0,47,167) → Token: `--o-color-primary1`（Actived Light）
          rgb(73,122,248) → Token: `--o-color-primary1`（Actived Dark）
    nodeText: "筛选条件"（示例）
```

---

### 变体 componentKey 速查

| 变体组合 | node_id | 尺寸（示例） | 说明 |
|---------|---------|------------|------|
| size=medium, checked=Enabled, Dark=off | `1042:17417` | 124×32px | 中尺寸，未选中，浅色 |
| size=medium, checked=Actived, Dark=off | `1042:17423` | 124×32px | 中尺寸，选中，浅色 |
| size=medium, checked=Enabled, Dark=on | `1042:17420` | 124×32px | 中尺寸，未选中，深色 |
| size=medium, checked=Actived, Dark=on | `1042:17426` | 124×32px | 中尺寸，选中，深色 |
| size=small, checked=Enabled, Dark=off | `1042:17516` | 68×28px | 小尺寸，未选中，浅色 |
| size=small, checked=Actived, Dark=off | `1042:17510` | 68×28px | 小尺寸，选中，浅色 |
| size=small, checked=Enabled, Dark=on | `1042:17519` | 68×28px | 小尺寸，未选中，深色 |
| size=small, checked=Actived, Dark=on | `1042:17513` | 68×28px | 小尺寸，选中，深色 |

> **注意**：示例尺寸（124px/68px）仅为演示，实际宽度随文字自适应。

---

### Pixso 操作速查

1. **插入组件**：组件面板搜索「OToggle」，拖入画布
2. **切换尺寸**：右侧面板 → size 属性 → 选择 `medium` / `small`
3. **切换状态**：右侧面板 → checked 属性 → 选择 `Enabled` / `Actived`
4. **切换主题**：右侧面板 → Dark 属性 → 选择 `off` / `on`
5. **修改文字内容**：双击进入组件 → 双击文字图层修改内容
6. **组合使用**：多个 Toggle 实例放入同一容器，形成筛选按钮组

---

### 注意事项

- **状态识别**：Enabled 有背景填充（grey-2/grey-3），Actived 有描边无背景（brand-6）
- **描边样式**：Actived 状态描边宽度 1px INSIDE，不影响按钮尺寸
- **背景颜色**：Enabled Light 使用 grey-2，Enabled Dark 使用 grey-3
- **文字颜色**：Enabled 使用 grey-14，Actived 使用 brand-6，与描边颜色一致
- **圆角统一**：所有尺寸和状态的圆角均为 4px
- **宽度自适应**：按钮宽度随文字内容自适应，无固定宽度
- **字号差异**：medium=16px，small=12px，确保文字在按钮内清晰可读
- **组合使用**：多个 Toggle 组合时，间距建议 8-12px（gap-2 ~ gap-3）
- **选中逻辑**：组合使用时可实现单选（仅一个 Actived）或多选（多个 Actived）

---

## Part C：交互与状态

### 交互状态

| 元素 | 状态 | 视觉表现 |
|------|------|---------|
| 按钮 | Enabled | 背景：`--o-color-fill1`，无描边，文字：`--o-color-info1` |
| 按钮 | Actived | 背景：transparent，描边：`--o-color-primary1`（1px INSIDE），文字：`--o-color-primary1` |
| 文字 | Enabled | fill：`--o-color-info1`（grey-14） |
| 文字 | Actived | fill：`--o-color-primary1`（brand-6） |

---

### 交互规则

#### 交互状态详情表

|  | **默认** | **悬浮** | **按下** | **选中** | **禁用** |
|--|---------|---------|---------|---------|---------|
| **未选中** | <br>📋 **筛选条件**<br><br>• 文字：16px (Regular)<br>  颜色：`--o-color-info1`<br>• 图标：16×16<br>  颜色：`--o-color-info1`<br>• 默认底块：`--o-color-fill1` | <br>📋 **筛选条件**<br><br>• 文字：16px (Regular)<br>  颜色：`--o-color-info1`<br>• 图标：16×16<br>  颜色：`--o-color-info1`<br>• 悬浮底块：`--o-color-control2-light` | <br>📋 **筛选条件**<br><br>• 文字：16px (Regular)<br>  颜色：`--o-color-info1`<br>• 图标：16×16<br>  颜色：`--o-color-info1`<br>• 按下底块：`--o-color-control3-light` | <br>📋 **筛选选中**<br><br>• 文字：16px (Regular)<br>  颜色：`--o-color-primary1`<br>• 图标：16×16<br>  颜色：`--o-color-primary1`<br>• 底块描边：`--o-color-primary1` | <br>📋 **筛选条件**<br><br>• 文字：16px (Regular)<br>  颜色：`--o-color-info4`<br>• 图标：16×16<br>  颜色：`--o-color-info4` |
| **已选中** | <br>📋 **筛选选中**<br><br>• 文字：16px (Regular)<br>  颜色：`--o-color-primary1`<br>• 图标：16×16<br>  颜色：`--o-color-primary1`<br>• 默认填充：无<br>• 默认描边：`--o-color-primary1` | <br>📋 **筛选选中**<br><br>• 文字：16px (Regular)<br>  颜色：`--o-color-primary1`<br>• 图标：16×16<br>  颜色：`--o-color-primary1`<br>• 默认填充：无<br>• 默认描边：`--o-color-primary1` | <br>📋 **筛选选中**<br><br>• 文字：16px (Regular)<br>  颜色：`--o-color-primary1`<br>• 图标：16×16<br>  颜色：`--o-color-primary1`<br>• 默认填充：无<br>• 默认描边：`--o-color-primary1` | <br>📋 **筛选选中**<br><br>• 文字：16px (Regular)<br>  颜色：`--o-color-info1`<br>• 图标：16×16<br>  颜色：`--o-color-info1`<br>• 默认底块：`--o-color-fill1` | <br>📋 **筛选选中**<br><br>• 文字：16px (Regular)<br>  颜色：`--o-color-primary4`<br>• 图标：16×16<br>  颜色：`--o-color-primary4`<br>• 禁用填充：无<br>• 禁用描边：`--o-color-primary4` |

---

#### Token 映射说明

**颜色 Token 对照**

| 视觉元素 | Token 名称 | 说明 |
|---------|-----------|------|
| `--o-color-info1` | 默认文字/图标颜色（未选中态） | grey-14 |
| `--o-color-primary1` | 选中态主色 | brand-6 |
| `--o-color-info4` | 禁用态文字/图标颜色（未选中） | 灰色禁用色 |
| `--o-color-primary4` | 禁用态文字/图标颜色（已选中） | 品牌禁用色 |
| `--o-color-fill1` | 默认背景填充 | grey-2/grey-3 |
| `--o-color-control2-light` | 悬浮态背景色 | 浅品牌色 |
| `--o-color-control3-light` | 按下态背景色 | 中品牌色 |

**尺寸规范**

| 元素 | 尺寸值 |
|------|-------|
| 文字字号 | 16px (Regular) |
| 图标尺寸 | 16×16 px |

---

### 状态切换逻辑

- **Enabled → Actived**：背景填充切换为透明，描边显示（brand-6），文字颜色切换为 brand-6
- **Actived → Enabled**：描边隐藏，背景填充显示（grey-2/grey-3），文字颜色切换为 grey-14
- **Light → Dark（Enabled）**：背景色切换 grey-2 → grey-3，文字颜色不变（grey-14）
- **Light → Dark（Actived）**：描边和文字颜色切换 brand-6 Light → brand-6 Dark

**交互流程**

```
未选中状态交互链：
默认 ──hover──→ 悬浮 ──mousedown──→ 按下 ──mouseup+click──→ 选中（变为已选中状态）
  │                                                                              │
  └──────────────────── mouseleave ←──────────────────────────────────────────────┘

已选中状态交互链：
默认 ──hover──→ 悬浮 ──mousedown──→ 按下 ──mouseup+click──→ 选中（变为未选中状态）
  │                                                                              │
  └──────────────────── mouseleave ←──────────────────────────────────────────────┘
```

---

### 各状态详细说明

#### 1. 未选中 × 默认
- **视觉特征**：灰色背景填充，无描边
- **适用场景**：初始状态或取消选中后的默认展示
- **可操作性**：✅ 可点击、可悬浮

#### 2. 未选中 × 悬浮
- **视觉特征**：背景色变浅（control2-light），提示可交互
- **触发条件**：鼠标 hover 进入按钮区域
- **过渡动画**：背景色过渡 150ms ease

#### 3. 未选中 × 按下
- **视觉特征**：背景色加深（control3-light），反馈按压操作
- **触发条件**：鼠标 mousedown 且未释放
- **恢复条件**：mouseup 或 mouseleave 后恢复到前一状态

#### 4. 未选中 × 选中
- **视觉特征**：透明背景 + 主色描边，文字和图标变为主色
- **触发条件**：click 操作完成
- **状态变化**：从未选中切换到已选中状态

#### 5. 未选中 × 禁用
- **视觉特征**：整体降低透明度（info4），不可交互
- **触发条件**：组件设置 disabled 属性
- **可操作性**：❌ 不可点击、不响应 hover

#### 6. 已选中 × 默认
- **视觉特征**：透明背景 + 主色描边，文字图标为主色
- **适用场景**：选中后的常态展示
- **可操作性**：✅ 可点击取消选中

#### 7. 已选中 × 悬浮 / 按下
- **视觉特征**：与默认态一致（保持描边样式不变）
- **设计考量**：避免选中态的 hover/active 干扰视觉识别

#### 8. 已选中 × 选中（取消选中）
- **视觉特征**：恢复为未选中默认态（灰色背景填充）
- **触发条件**：再次 click 操作
- **状态变化**：从已选中切换回未选中状态

#### 9. 已选中 × 禁用
- **视觉特征**：保持描边但使用禁用色（primary4），降低对比度
- **触发条件**：组件在选中状态下设置 disabled
- **可操作性**：❌ 不可点击取消选中

---

### 注意事项

- **交互一致性**：所有交互状态应保持 150ms 过渡动画，提供流畅的视觉反馈
- **焦点状态**：键盘 Tab 聚焦时显示 focus ring（outline: 2px solid primary1, offset: 2px）
- **禁用层级**：禁用态优先级最高，禁用后不响应任何鼠标事件
- **组合场景**：多个 Toggle 组合使用时，各按钮独立管理自己的交互状态
- **触摸设备**：移动端简化为 default 和 selected 两态，去除 hover 和 active 态

---

## Part D：设计变量绑定

### 推荐绑定变量

| 元素 | 属性 | 推荐变量 Token |
|------|------|---------------|
| 按钮背景（Enabled） | fill | `--o-color-fill1` |
| 按钮描边（Actived） | stroke | `--o-color-primary1` |
| 文字（Enabled） | fill | `--o-color-info1` |
| 文字（Actived） | fill | `--o-color-primary1` |
| 按钮圆角 | cornerRadius | `radius_control-xs`（4px） |
| 按钮高度（medium） | height | `control_size-m`（32px） |
| 文字字号（medium） | fontSize | `font_size-text1`（16px） |
| 文字字号（small） | fontSize | `font_size-tip2`（12px） |
| 描边宽度 | strokeWeight | 1px INSIDE |

---

## Part E：最佳实践

### 使用建议

1. **筛选场景**：Toggle 主要用于筛选、过滤场景，不适合主导航
2. **组合排列**：多个 Toggle 水平排列，间距 8-12px
3. **单选/多选**：根据业务需求实现单选（仅一个 Actived）或多选（多个 Actived）
4. **文字简洁**：筛选文字建议 2-6 个字，如"全部"、"已发布"、"草稿"

### 设计提示

- Toggle 宽度随文字自适应，确保文字清晰可读
- medium 尺寸适合筛选栏，small 尺寸适合标签组或紧凑布局
- Actived 状态通过描边和文字颜色区分，背景透明避免视觉干扰
- 组合使用时，同一组 Toggle 应保持一致的尺寸和间距
- 深色模式下注意背景色和品牌色同步切换

---

## Part F：Assets 图标资源

> 路径：`references/assets/public icons/`

| 文件名 | 使用位置 | 结构性质 |
|--------|---------|---------|
| `icon-占位.svg` | 组件内图标槽位 | **占位符**，可替换为任意业务图标 |

**图标颜色与尺寸规则**

颜色和尺寸完全跟随 OToggle 组件内部定义，**不单独设置**：

| checked 状态 | 图标颜色 Token |
|-------------|--------------|
| Enabled | `--o-color-info1`（grey-14） |
| Actived | `--o-color-primary1`（brand-6） |

> 图标颜色始终与文字颜色一致，深色模式下同名 Token 自动切换为深色值。替换图标时进入实例修改图标内容，颜色无需手动调整。

---

## 技术备注

- 组件节点 ID：`1042:17416`（组件集）
- 设计稿 URL：https://pixso.cn/app/design/JZkjW0mhmT61Mtd98dCfBw?item-id=1042:17416
- 生成日期：2026-04-17
- 变体总数：8（size=medium/small × checked=Enabled/Actived × Dark=off/on）
- Group 变体：组合示例，非单独组件状态
- 尺寸数据：medium=124×32px（示例），small=68×28px（示例）
- 圆角：4px（radius_control-xs）
- 描边：Actived 状态 1px INSIDE
- 字号：medium=16px（font_size-text1），small=12px（font_size-tip2）
- 背景：Enabled=grey-2/grey-3（color-fill1），Actived=transparent
- 描边/文字：Actived=brand-6（color-primary1）