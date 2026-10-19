> ← [组件索引](../SKILL.md#组件索引) · [README](../README.md)

# OLink 链接 · 设计 Skill

> 组件集合节点：`1042:17184` · 组件名：OLink 链接 · 变体总数：12

---

## Part A：设计使用卡

### 组件概览

**OLink 链接**：用于导航跳转的文字链接组件，可带后缀图标（跳出图标）指示链接行为。通过尺寸和状态传达链接的视觉层级和可用性。

---

### 适用场景

- ✅ **文字导航跳转**：页面内的文字链接，如"查看详情"、"了解更多"
- ✅ **外链提示**：带跳出图标的外部链接，明确告知用户将跳转至外部页面
- ✅ **表格行内操作**：列表中的链接操作，如"编辑"、"删除"
- ✅ **辅助导航**：面包屑、页码等辅助导航中的链接元素
- ❌ **不适合**：主要操作按钮（用 OButton）、标签切换（用 OTab）

---

### 变体说明

**size（尺寸）**
- `large` — 高 40px，字号 16px，适合页面主标题区或突出的链接
- `medium` — 高 34px，字号 14px，**默认尺寸**，适合大多数内容区链接
- `small` — 高 30px，字号 14px，适合紧凑布局或辅助信息链接

**state（状态）**
- `Enabled` — 正常可点击状态，使用品牌色 link1
- `Disabled` — 禁用状态，使用禁用色 link4，不可点击

**Dark（主题）**
- `off` — 浅色模式（默认）
- `on` — 深色模式，品牌色系自动切换为对应深色值

**后缀图标（可选）**
- `true` — 显示跳出图标，指示外链或新窗口打开
- `false` — 不显示图标，纯文字链接

### 交互状态规范

| 状态 | 视觉表现 | 图标 | 文本 |
|------|---------|------|------|
| **默认** | 🔗 文字链接 | 尺寸：20×20px<br>颜色：--o-color-link1 | 字体：鸿蒙黑体-常规<br>字号：16px<br>颜色：--o-color-link1 |
| **悬浮（Hover）** | 🔗 <u>文字链接</u> | 尺寸：20×20px<br>颜色：--o-color-link2 | 字体：鸿蒙黑体-常规<br>字号：16px<br>颜色：--o-color-link2<br>✅ 显示下划线 |
| **按下（Active）** | 🔗 <u>文字链接</u> | 尺寸：20×20px<br>颜色：--o-color-link3 | 字体：鸿蒙黑体-常规<br>字号：16px<br>颜色：--o-color-link3<br>✅ 显示下划线 |
| **选中（已访问）** | 🔗 <u>文字链接</u> | 尺寸：20×20px<br>颜色：--o-color-link1 | 字体：鸿蒙黑体-常规<br>字号：16px<br>颜色：--o-color-link1<br>✅ 显示下划线 |
| **禁用（Disabled）** | 🚫 文字链接（灰色） | 尺寸：20×20px<br>颜色：--o-color-link4 | 字体：鸿蒙黑体-常规<br>字号：16px<br>颜色：--o-color-link4 |
默认 → 悬浮（鼠标移入） 悬浮 → 默认（鼠标移出） 悬浮 → 按下（鼠标点击） 按下 → 选中/已访问（点击完成后） 选中 → 悬浮（再次鼠标移入） 任何状态 → 禁用（通过属性控制）

#### 关键设计要点

- ✅ **下划线反馈**：悬浮、按下、选中状态下显示下划线，提供明确的视觉反馈
- ✅ **颜色层级**：使用 --o-color-link1~4 的语义化颜色系统，保持一致性
- ✅ **图标同步**：图标颜色始终与文本颜色保持一致
- ✅ **尺寸统一**：所有状态下图标固定为 20×20px，文本固定为 16px

#### 状态转换逻辑

---

### 布局结构

> 🧩 **布局结构**：链接组件水平排列（HORIZONTAL Auto Layout），由文字和可选后缀图标组成。Auto Layout Padding 控制垂直位置，图标与文字间距 4px。

```
OLink（HORIZONTAL，自适应宽度，固定高度）
├── autoLayoutItemSpacing: 4px（图标↔文字间距）
├── autoLayoutPaddingTop: 8px（large）/ 5px（medium）/ 3px（small）
├── autoLayoutPaddingBottom: 8px（large）/ 5px（medium）/ 3px（small）
├── autoLayoutPaddingLeft: 0px
├── autoLayoutPaddingRight: 0px
├── autoLayoutCounterAlign: center（垂直居中）
│
├── [text PARAGRAPH]（自适应宽度，固定行高）
│     fill: --o-color-link1（Enabled）/ --o-color-link4（Disabled）
│     font: large=16px/24px, medium/small=14px/22px
│
└── [图标 Icon/跳出 INSTANCE]（24×24，可选）
      fill: --o-color-link1（Enabled）/ --o-color-link4（Disabled）
      与文字颜色一致
```

---

### 组合搭配

> 🔗 **常见搭配**：
> - **面包屑导航**：OLink 作为面包屑层级项，配合 OBreadcrumb 使用
> - **表格操作**：OLink 作为表格行内的链接操作，配合 ODataTable 使用
> - **外链提示**：带后缀图标的 OLink，指示将跳转至外部页面
> - **帮助文档**：OLink 作为帮助文档链接入口

---

### 设计稿识别指南

> 🔍 **识别特征**：
> - 水平自动布局节点，高度固定为 30/34/40px
> - 文字使用品牌色（brand-6）或禁用品牌色（brand-3）
> - 可选后缀跳出图标（24×24），颜色与文字一致
> - 无背景、无边框，纯文字+图标组合

> 🔄 **易混淆组件**：
> - 与 **OButton（text 变体）**：Button 用于触发操作，Link 用于导航跳转；Button 有点击区域 padding，Link 无左右 padding
> - 与 **OBreadcrumb**：OLink 是单个链接组件，Breadcrumb 是层级路径导航组件

---

### 响应式行为

参照 [栅格规范](../SKILL.md#数据资源) 中的断点定义，OLink 无自动断点响应式行为。尺寸通过 `size` 变体手动控制，深色模式通过 `Dark` 变体手动切换。

---

## Part B：规格速查参考

### 变体索引表

| 变体属性 | 可选值 | 默认值 | 视觉差异 |
|---------|--------|--------|---------|
| size | `large` / `medium` / `small` | `medium` | 高度 40/34/30px，字号和 Padding 有差异 |
| state | `Enabled` / `Disabled` | `Enabled` | 颜色切换为 link1/link4 |
| Dark | `off` / `on` | `off` | 品牌色切换为深色主题对应值 |

---

### 布局规格

| 规格项 | large | medium | small |
|--------|-------|--------|-------|
| 整体高度 | 40px | 34px | 30px |
| Token（高度） | `control_size-l` | — | — |
| Auto Layout Padding（上） | 8px | 5px | 3px |
| Auto Layout Padding（下） | 8px | 5px | 3px |
| Auto Layout Padding（左） | 0px | 0px | 0px |
| Auto Layout Padding（右） | 0px | 0px | 0px |
| 图标↔文字间距 | 4px | 4px | 4px |
| Token（间距） | `gap-1` | `gap-1` | `gap-1` |
| 图标尺寸 | 24×24px | 24×24px | 24×24px |
| Token（图标尺寸） | `icon_size_control-m` | `icon_size_control-m` | `icon_size_control-m` |

---

### 颜色 Token 映射

#### Enabled 状态

| 区域 | Light 模式 Token | Dark 模式 Token | RGB 值 |
|------|-----------------|----------------|--------|
| 文字 | `--o-color-link1` | `--o-color-link1` | rgb(0,47,167) → rgb(73,122,248) |
| 后缀图标 | `--o-color-link1` | `--o-color-link1` | 与文字颜色一致 |

#### Disabled 状态

| 区域 | Light 模式 Token | Dark 模式 Token | RGB 值 |
|------|-----------------|----------------|--------|
| 文字 | `--o-color-link4` | `--o-color-link4` | rgb(132,161,220) → rgb(29,51,120) |
| 后缀图标 | `--o-color-link4` | `--o-color-link4` | 与文字颜色一致 |

> **说明**：`--o-color-link1` = brand-6，`--o-color-link4` = brand-3。链接色与品牌色系一致。

---

### 字体样式映射

| 使用场景 | 字号 Token | 行高 Token | 字重 Token | 字号值 | 行高值 |
|---------|-----------|-----------|-----------|-------|-------|
| large 链接文字 | `font_size-text1` | `line_height-text1` | `font_weight-regular` | 16px | 24px |
| medium 链接文字 | `font_size-tip1` | `line_height-tip1` | `font_weight-regular` | 14px | 22px |
| small 链接文字 | `font_size-tip1` | `line_height-tip1` | `font_weight-regular` | 14px | 22px |

字体族：`font_family`（HarmonyOS / HarmonyHeiTi Regular）

---

### 组件层级结构

```
OLink（HORIZONTAL，自适应宽度，固定高度 30/34/40px）
│  GUID: 1042:17185（large/Enabled/off）等
│  autoLayoutMode: HORIZONTAL
│  autoLayoutItemSpacing: 4px
│  autoLayoutPaddingTop: 8px（large）/ 6px（medium）/ 4px（small）
│  autoLayoutPaddingBottom: 8px（large）/ 6px（medium）/ 4px（small）
│  autoLayoutPaddingLeft: 0px
│  autoLayoutPaddingRight: 0px
│  autoLayoutPrimaryAlign: flex-start
│  autoLayoutCounterAlign: center
│
├── [text PARAGRAPH]
│   Width: 自适应（large=64px示例, medium/small=56px示例）
│   Height: 24px（large）/ 22px（medium/small）
│   fill: rgba(0,47,167,1)（Light Enabled）/ rgba(73,122,248,1)（Dark Enabled）
│         rgba(132,161,220,1)（Light Disabled）/ rgba(29,51,120,1)（Dark Disabled）
│   font: font_size-text1（large）/ font_size-tip1（medium/small）
│
└── [图标 Icon/跳出 INSTANCE]（可选）
    Width: 24px | Height: 24px
    fill: 与文字颜色一致
```

---

### 变体 componentKey 速查

| 变体组合 | node_id | 尺寸 | 说明 |
|---------|---------|------|------|
| size=large, state=Enabled, Dark=off | `1042:17185` | 92×40px | 大尺寸，浅色，正常状态 |
| size=large, state=Disabled, Dark=off | `1042:17188` | 92×40px | 大尺寸，浅色，禁用状态 |
| size=medium, state=Enabled, Dark=off | `1042:17191` | 84×34px | 中尺寸，浅色，正常状态 |
| size=medium, state=Disabled, Dark=off | `1042:17194` | 84×34px | 中尺寸，浅色，禁用状态 |
| size=small, state=Enabled, Dark=off | `1042:17197` | 84×30px | 小尺寸，浅色，正常状态 |
| size=small, state=Disabled, Dark=off | `1042:17200` | 84×30px | 小尺寸，浅色，禁用状态 |
| size=large, state=Enabled, Dark=on | `1042:17203` | 92×40px | 大尺寸，深色，正常状态 |
| size=large, state=Disabled, Dark=on | `1042:17206` | 92×40px | 大尺寸，深色，禁用状态 |
| size=medium, state=Enabled, Dark=on | `1042:17209` | 84×34px | 中尺寸，深色，正常状态 |
| size=medium, state=Disabled, Dark=on | `1042:17212` | 84×34px | 中尺寸，深色，禁用状态 |
| size=small, state=Enabled, Dark=on | `1042:17215` | 84×30px | 小尺寸，深色，正常状态 |
| size=small, state=Disabled, Dark=on | `1042:17218` | 84×30px | 小尺寸，深色，禁用状态 |

> **注意**：componentKey 需从 Pixso 组件面板获取。

---

### Pixso 操作速查

1. **插入组件**：组件面板搜索「OLink」，拖入画布
2. **切换尺寸**：右侧面板 → size 属性 → 选择 `large` / `medium` / `small`
3. **切换状态**：右侧面板 → state 属性 → 选择 `Enabled` / `Disabled`
4. **切换主题**：右侧面板 → Dark 属性 → 选择 `off` / `on`
5. **显示/隐藏图标**：右侧面板 → 后缀图标属性 → 切换开关
6. **修改文字内容**：双击进入组件 → 双击 text 图层修改内容

---

### 注意事项

- **链接颜色**：使用链接色 Token（`--o-color-link1`/`--o-color-link4`），与品牌色系（brand-6/brand-3）一致
- **Auto Layout Padding**：上下 Padding 控制文字/图标在容器内的垂直位置（large=8px, medium=5px, small=3px），左右 Padding 为 0
- **图标颜色一致**：后缀图标颜色与文字颜色相同，不使用独立颜色
- **图标尺寸固定**：所有尺寸的图标均为 24×24px，不随 size 变化
- **禁用状态**：禁用时颜色切换为 `--o-color-link4`（brand-3），无其他视觉变化
- **后缀图标语义**：跳出图标指示链接将打开新窗口或跳转至外部页面
- **深色模式**：品牌色自动切换为 rgb(73,122,248)，禁用色切换为 rgb(29,51,120)

---

## Part C：交互与状态

### 交互状态

| 元素 | 状态 | 视觉表现 |
|------|------|---------|
| 文字 | Enabled | 颜色：`--o-color-link1`（brand-6），可点击 |
| 文字 | Disabled | 颜色：`--o-color-link4`（brand-3），不可点击 |
| 文字 | Hover | 颜色：`--o-color-link2`（brand-4），悬浮提示 |
| 文字 | Active | 颜色：`--o-color-link3`（brand-7），点击反馈 |
| 后缀图标 | Enabled | 颜色：与文字一致（`--o-color-link1`） |
| 后缀图标 | Disabled | 颜色：与文字一致（`--o-color-link4`） |

---

### 状态切换逻辑

- **Enabled 状态**：默认状态，链接可点击，颜色为 link1
- **Disabled 状态**：禁用状态，链接不可点击，颜色降级为 link4
- **Hover 状态**：鼠标悬浮时颜色切换为 link2（交互提示）
- **Active 状态**：点击时颜色切换为 link3（点击反馈）

---

## Part D：设计变量绑定

### 推荐绑定变量

| 元素 | 属性 | 推荐变量 Token |
|------|------|---------------|
| 文字颜色（Enabled） | fill | `--o-color-link1` |
| 文字颜色（Disabled） | fill | `--o-color-link4` |
| 图标颜色 | fill | 与文字一致 |
| 文字字号（large） | fontSize | `font_size-text1`（16px） |
| 文字字号（medium/small） | fontSize | `font_size-tip1`（14px） |
| 文字行高（large） | lineHeight | `line_height-text1`（24px） |
| 文字行高（medium/small） | lineHeight | `line_height-tip1`（22px） |
| 图标尺寸 | width/height | `icon_size_control-m`（24px） |
| 图标↔文字间距 | autoLayoutItemSpacing | `gap-1`（4px） |
| Auto Layout Padding（large） | paddingTop/Bottom | 8px |
| Auto Layout Padding（medium） | paddingTop/Bottom | 5px |
| Auto Layout Padding（small） | paddingTop/Bottom | 3px |
| Auto Layout Padding（左/右） | paddingLeft/Right | 0px |

---

## Part E：最佳实践

### 使用建议

1. **外链明确提示**：外部链接必须显示后缀跳出图标，告知用户将跳转至外部页面
2. **尺寸选择**：内容区默认使用 `medium`，突出链接使用 `large`，辅助信息使用 `small`
3. **禁用合理**：仅在没有目标页面或权限不足时禁用链接，避免过度禁用
4. **避免滥用**：链接用于导航跳转，主要操作应使用 OButton

### 设计提示

- 链接文字建议简短明确，如"查看详情"、"了解更多"、"帮助"
- 后缀图标仅在需要提示跳转行为时显示，内部页面跳转可不显示
- 链接在文本流中可自然嵌入，无需固定容器宽度
- 深色模式下注意链接颜色自动切换，确保可识别性

---

## Part F：Assets 图标资源

> 路径：`references/assets/public icons/`

| 文件名 | 使用位置 | 结构性质 |
|--------|---------|---------|
| `icon-外链.svg` | 组件内后缀图标（右侧） | **固定结构**，不可替换 |

**图标颜色与尺寸规则**

颜色和尺寸完全跟随 OLink 组件内部定义，**不单独设置**：

| 尺寸（所有 size） | 图标尺寸 | Token |
|-----------------|---------|-------|
| large / medium / small | 24×24px | `icon_size_control-m` |

| state | 图标颜色 Token |
|-------|--------------|
| Enabled | `--o-color-link1` |
| Disabled | `--o-color-link4` |

> 图标颜色始终与文字颜色一致，深色模式下同名 Token 自动切换为深色值。

---

## 技术备注

- 组件节点 ID：`1042:17184`（组件集）
- 设计稿 URL：https://pixso.cn/app/design/JZkjW0mhmT61Mtd98dCfBw?item-id=1042:17184
- 生成日期：2026-04-16
- 变体总数：12（size=large/medium/small × state=Enabled/Disabled × Dark=off/on）
- 字号数据：来源于 Pixso 设计稿确认
- 图标尺寸：所有尺寸统一 24×24px