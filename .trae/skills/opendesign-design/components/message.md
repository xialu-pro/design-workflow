> ← [组件索引](../SKILL.md#组件索引) · [README](../README.md)

# OMessage 消息提示 · 设计 Skill

> 组件集合节点：`1042:21437` · 组件名：OMessage 消息提示 · 变体总数：26

---

## Part A：设计使用卡

### 组件概览

**OMessage 消息提示**：用于向用户传递操作结果、系统状态或重要提示信息的轻量级反馈组件。分为全局提示和内联提示两大类，支持成功、信息、危险、警告、加载五种状态。适用于操作结果反馈、系统公告、表单校验提示等场景。

---

### 适用场景

- ✅ **全局提示**：操作成功/失败反馈（如提交成功、删除确认）、页面顶部居中浮现后自动消失
- ✅ **内联提示1**：表单下方单行提示、页面区块内状态说明、简短无需操作的提示文案
- ✅ **内联提示2**：需要标题+正文两段内容的说明区块、可关闭的通知横幅、较复杂的提示信息
- ❌ **不适合**：需要用户决策的弹框（用 ODialog/OModal）、底部通知（用 Toast）、需要持久跟踪的任务进度（用 OProgress）

---

### 变体说明

**type（消息类型）**
- `全局提示` — 居中浮层，带阴影，出现于页面顶部，自动消失。支持 success / info / danger / warning / loading 五种状态
- `内联提示1` — 嵌入内容区的单行提示条，带图标 + 文案，背景为状态色浅色。支持 success / info / danger / warning 四种状态
- `内联提示2` — 嵌入内容区的双行提示块，带图标 + 标题 + 描述，背景为状态色浅色，支持关闭按钮。支持 success / info / danger / warning 四种状态

**status（状态类型）**
- `success` — 成功，绿色系，操作完成
- `info` — 信息，蓝色系（使用 primary 色），一般提示
- `danger` — 危险/错误，红色系，操作失败或危险警告
- `warning` — 警告，黄/橙色系，提醒注意
- `loading` — 加载中，仅全局提示使用，旋转加载图标

**Dark（主题）**
- `off` — 浅色模式（默认）
- `on` — 深色模式

---

### 布局结构

> 🧩 **布局结构**：全局提示为独立浮层容器，内联提示为嵌入式横幅容器，内部均采用水平自动布局。

**type=全局提示 布局**

```
OMessage-全局提示（FRAME，HORIZONTAL，自适应宽度）
│  cornerRadius: 4px
│  fill: color-fill2（带阴影）
│  effects: shadow（浮层阴影）
│  padding: 上下 8px · 左右 16px
│  itemSpacing: 8px
│
├── [状态图标 Icon INSTANCE]（24×24px）
│     fill: 对应 status color（如 color-success1）
│
└── [提示文字 PARAGRAPH]
      font: Body-16/Regular
      fill: color-info1
```

**type=内联提示1 布局**

```
OMessage-内联提示1（FRAME，HORIZONTAL，自适应宽高）
│  cornerRadius: 4px
│  fill: 无（由子层承载背景）
│  overflow: hidden
│
├── [左侧色条 RECT]（width: 4px，height: 100%）
│     fill: 对应 status color（如 color-success1）
│
└── [内容区 FRAME，HORIZONTAL]
    │  fill: 对应 status color + 5% 透明度
    │  padding: 上下 4px · 左右 12px   ← 从色条右边缘算起
    │  itemSpacing: 8px
    │  flex: 1
    │
    ├── [状态图标 Icon INSTANCE]（24×24px，assets/message/）
    ├── [提示文字 PARAGRAPH]
    │     font: Body-14/Regular
    │     fill: color-info1（rgba(0,0,0,1)）
    │     flex: 1
    └── [关闭图标 INSTANCE]（24×24px，assets/public icons/关闭.svg）
          fill: color-info2（rgba(0,0,0,0.8)）
```

**type=内联提示2 布局**

```
OMessage-内联提示2（FRAME，HORIZONTAL，自适应宽高）
│  cornerRadius: 4px
│  fill: 无（由子层承载背景）
│  overflow: hidden
│
├── [左侧色条 RECT]（width: 4px，height: 100%）
│     fill: 对应 status color（如 color-success1）
│
└── [内容区 FRAME，VERTICAL]
    │  fill: 对应 status color + 5% 透明度
    │  padding: 上 4px · 下 8px · 左右 12px   ← 从色条右边缘算起
    │  itemSpacing: 4px   ← 顶部行与描述文字之间
    │  flex: 1
    │
    ├── [顶部行 FRAME，HORIZONTAL]
    │   │  itemSpacing: 8px
    │   ├── [状态图标 Icon INSTANCE]（24×24px，assets/message/）
    │   └── [标题文字 PARAGRAPH]
    │         font: Body-14/SemiBold
    │         fill: color-info1（rgba(0,0,0,1)）
    │
    └── [描述文字 PARAGRAPH]
          font: Body-14/Regular
          fill: color-info2（rgba(0,0,0,0.8)）
          paddingLeft: 32px（图标24px + 间距8px，与标题对齐）
```

---

### 状态色映射

| status | 图标颜色 Token | 背景色（内联提示） | 背景色（全局提示） | 含义 |
|--------|--------------|-----------------|-----------------|------|
| success | `--o-color-success1` | `--o-color-success1` opacity 5% | `--o-color-fill2` | 操作成功 |
| info | `--o-color-primary1` | `--o-color-primary1` opacity 5% | `--o-color-fill2` | 一般信息 |
| danger | `--o-color-danger1` | `--o-color-danger1` opacity 5% | `--o-color-fill2` | 错误/危险 |
| warning | `--o-color-warning1` | `--o-color-warning1` opacity 5% | `--o-color-fill2` | 警告提醒 |
| loading | `--o-color-info2` | — | `--o-color-fill2` | 加载中（仅全局提示） |

---

### 组合搭配

> 🔗 **常见搭配**：
> - **表单提交**：全局提示（success/danger）在顶部弹出，持续 3 秒后消失
> - **权限提示**：内联提示1（warning）内嵌于功能区上方，常驻展示
> - **系统公告**：内联提示2（info）横跨页面顶部，带关闭按钮
> - **表单校验**：内联提示1（danger）内嵌于表单字段下方

---

### 设计稿识别指南

> 🔍 **识别特征**：
> - 全局提示：带阴影的圆角容器，居中于页面顶部，左侧有彩色状态图标
> - 内联提示1：无阴影，背景为浅状态色（淡绿/淡蓝/淡红/淡黄），单行水平排列
> - 内联提示2：无阴影，背景为浅状态色，含标题行和描述文字两行内容

> 🔄 **易混淆组件**：
> - 与 **OTag 标签** 的区别：Tag 是状态标注标签，Message 是完整提示条，宽度和语义不同
> - 与 **OToast** 的区别：OMessage 全局提示位于顶部居中，OToast 通常位于底部或角落
> - 与 **ODialog 对话框** 的区别：ODialog 是模态对话框需用户处理后才能继续，Message 为非模态轻量提示不阻断操作

---

### 响应式行为

全局提示宽度由内容自适应，有最大宽度限制，在移动端自动跟随断点缩放。内联提示宽度跟随容器 100%，无独立响应式行为。参照 [栅格规范](../SKILL.md#数据资源) 中的断点定义。

---

## Part B：规格速查参考

### 变体索引表

| 变体属性 | 可选值 | 默认值 | 视觉差异 |
|---------|--------|--------|---------|
| type | `全局提示` / `内联提示1` / `内联提示2` | `全局提示` | 浮层 / 内嵌单行 / 内嵌双行 |
| status | `success` / `info` / `danger` / `warning` / `loading` | `info` | 图标+颜色变化（loading 仅全局提示） |
| Dark | `off` / `on` | `off` | 浅色模式 / 深色模式 |

---

### 布局规格

| 规格项 | 全局提示 | 内联提示1 | 内联提示2 |
|--------|---------|---------|---------|
| 圆角 | 4px | 4px | 4px |
| Token（圆角） | `radius_control-m` | `radius_control-m` | `radius_control-m` |
| 背景 | `--o-color-fill2` | 状态色 opacity 5% | 状态色 opacity 5% |
| 左侧竖线 | 无 | 4px · 状态色 | 4px · 状态色 |
| 阴影 | 有（shadow） | 无 | 无 |
| 内边距（上） | 12px | 4px | 4px |
| 内边距（下） | 12px | 4px | 8px |
| 内边距（左右） | 16px | 12px | 12px |
| 图标尺寸 | 24px | 24px | 24px |
| 图标与文字间距 | 8px | 8px | 8px |

---

### 颜色 Token 映射

**浅色模式（Dark=off）**

| 区域/状态 | Token | 实际值（浅色） | 说明 |
|---------|-------|-------------|------|
| 全局提示背景 | `--o-color-fill2` | `#ffffff` | 白色背景 |
| success 图标 | `assets/message/成功.svg` | SVG 原始色 | 内联 SVG，**禁止覆盖颜色** |
| success 背景（内联）/ 左边框 | `--o-color-success1` opacity 5% / solid | `rgba(11, 177, 81, 0.05)` / `rgb(11, 177, 81)` | CSS 属性，使用 Token |
| info 图标 | `assets/message/提示.svg` | SVG 原始色 | 内联 SVG，**禁止覆盖颜色** |
| info 背景（内联）/ 左边框 | `--o-color-primary1` opacity 5% / solid | `rgba(0, 47, 167, 0.05)` / `rgb(0, 47, 167)` | CSS 属性，使用 Token |
| danger 图标 | `assets/message/错误.svg` | SVG 原始色 | 内联 SVG，**禁止覆盖颜色** |
| danger 背景（内联）/ 左边框 | `--o-color-danger1` opacity 5% / solid | `rgba(230, 0, 18, 0.05)` / `rgb(230, 0, 18)` | CSS 属性，使用 Token |
| warning 图标 | `assets/message/警示.svg` | SVG 原始色 | 内联 SVG，**禁止覆盖颜色** |
| warning 背景（内联）/ 左边框 | `--o-color-warning1` opacity 5% / solid | `rgba(250, 115, 5, 0.05)` / `rgb(250, 115, 5)` | CSS 属性，使用 Token |
| loading 图标 | `assets/public icons/icon-loading 加载.svg` | SVG 原始色 | 内联 SVG，**禁止覆盖颜色** |
| 提示文字（主） | `--o-color-info1` | `rgba(0, 0, 0, 1)` | 纯黑，一级文字 |
| 提示文字（辅） | `--o-color-info2` | `rgba(0, 0, 0, 0.8)` | 80% 黑，二级文字 |
| 关闭图标 | `--o-color-info2` | `rgba(0, 0, 0, 0.8)` | 80% 黑，辅助图标 |

**深色模式（Dark=on）**

| 区域/状态 | Token | 说明 |
|---------|-------|------|
| 全局提示背景 | `--o-color-fill2`（深色值） | 深色背景 |
| success 图标色 | `--o-color-success1`（深色值） | 深色模式绿色 |
| info 图标色 | `--o-color-primary1`（深色值） | 深色模式蓝色 |
| danger 图标色 | `--o-color-danger1`（深色值） | 深色模式红色 |
| warning 图标色 | `--o-color-warning1`（深色值） | 深色模式橙色 |

---

### 字体样式映射

| 使用场景 | 字号 Token | 行高 Token | 字重 Token | 字号值 | 行高值 |
|---------|-----------|-----------|-----------|-------|-------|
| 全局提示文字 | `font_size-text1` | `line_height-text1` | `font_weight-regular` | 16px | 24px |
| 内联提示1 文字 | `font_size-tip1` | `line_height-tip1` | `font_weight-regular` | 14px | 22px |
| 内联提示2 标题 | `font_size-tip1` | `line_height-tip1` | `font_weight-semibold` | 14px | 22px |
| 内联提示2 描述 | `font_size-tip1` | `line_height-tip1` | `font_weight-regular` | 14px | 22px |

字体族：`font_family`（HarmonyOS / HarmonyHeiTi）

---

### 组件层级结构

**全局提示**

```
OMessage-全局提示（FRAME，HORIZONTAL，自适应宽度）
│  cornerRadius: 4px（radius_control-m）
│  fill: color-fill2
│  effects: shadow（阴影样式）
│  autoLayoutPadding: 12px 16px
│  autoLayoutItemSpacing: 8px
│
├── [Icon 状态图标 INSTANCE]
│     width/height: 24px
│     fill: color-{status}1
│
└── [文字 PARAGRAPH]
      fill: color-info1
      font: Body-16/Regular（16px · 24px）
```

**内联提示1**

```
OMessage-内联提示1（FRAME，HORIZONTAL，overflow:hidden）
│  cornerRadius: 4px（radius_control-m）
│  fill: 无
│
├── [左侧色条 RECT]（width: 4px）
│     fill: color-{status}1
│
└── [内容区 FRAME，HORIZONTAL]
    │  fill: color-{status}1 opacity 5%
    │  autoLayoutPadding: 上下 4px · 左右 12px
    │  autoLayoutItemSpacing: 8px
    │
    ├── [Icon 状态图标 INSTANCE]（assets/message/，24×24px）
    ├── [文字 PARAGRAPH]（flex:1）
    │     fill: color-info1 = rgba(0,0,0,1)
    │     font: Body-14/Regular（14px · 22px）
    └── [关闭图标 INSTANCE]（assets/public icons/关闭.svg，24×24px）
          fill: color-info2 = rgba(0,0,0,0.8)
```

**内联提示2**

```
OMessage-内联提示2（FRAME，HORIZONTAL，overflow:hidden）
│  cornerRadius: 4px（radius_control-m）
│  fill: 无
│
├── [左侧色条 RECT]（width: 4px）
│     fill: color-{status}1
│
└── [内容区 FRAME，VERTICAL]
    │  fill: color-{status}1 opacity 5%
    │  autoLayoutPadding: 上 4px · 下 8px · 左右 12px
    │  autoLayoutItemSpacing: 4px
    │
    ├── [顶部行 FRAME，HORIZONTAL]
    │   │  autoLayoutItemSpacing: 8px
    │   ├── [Icon 状态图标 INSTANCE]（assets/message/，24×24px）
    │   └── [标题 PARAGRAPH]
    │         fill: color-info1 = rgba(0,0,0,1)
    │         font: Body-14/SemiBold（14px · 22px）
    │
    └── [描述 PARAGRAPH]
          fill: color-info2 = rgba(0,0,0,0.8)
          font: Body-14/Regular（14px · 22px）
          paddingLeft: 32px（图标24px + 间距8px）
```

---

### 变体 componentKey 速查

**type=全局提示**

| status | Dark | node_id | component_key |
|--------|------|---------|---------------|
| success | off | `1042:21438` | `49db453b4501aa21ac4e68c298fee643920ff030` |
| success | on | `1042:21442` | `9bd5c0c11b97cf591a74dde3a6e84bfdca20c447` |
| info | off | `1042:21446` | `6e429a3b640ab167b3c553df5b67467c8e05151b` |
| info | on | `1042:21450` | `d2dd44212181e896185b414c9d2eee4e6d1f4796` |
| danger | off | `1042:21454` | `1f253a6bea23d24a2fe582c670e76ab51d23e641` |
| danger | on | `1042:21458` | `0eb7ec269c0d4b6e30f50e384436e537e992e740` |
| warning | off | `1042:21462` | `2a31dabd2b7c9a3e5415f85c527c1c00f5b02bf2` |
| warning | on | `1042:21466` | `a61a7237bc8fe8ab15211b5ae55fd724d2d409ee` |
| loading | off | `1042:21470` | `7c306253af5c5e5b358c09701f3c18c7d73e486f` |
| loading | on | `1042:21474` | `609555dcf4325e57fc6a7fcd7212fda1bd7893c5` |

**type=内联提示1**

| status | Dark | node_id | component_key |
|--------|------|---------|---------------|
| info | off | `1042:21590` | `fdecb892dca83a544987a48c6ad9d99b60907202` |
| success | off | `1042:21599` | `2dcb1b7fbd912bd63c07b6cc06a053ee4783d228` |
| danger | off | `1042:21608` | `1ebf0eb234aa2ce351dc7d1c13cbea3224caa822` |
| warning | off | `1042:21617` | `d760405d78fa62a8d0ebe3b0e2778beb074c521c` |
| warning | on | `1042:21762` | `4dc3cfccb5e43ad075cb8ba5e4bbe0cd16b6cb92` |
| danger | on | `1042:21771` | `29b46ccbeab9d7ae9add0118d97838e3c50a10e2` |
| success | on | `1042:21780` | `fc99de3efb635c1b1ef91bcf878677fc081d3f19` |
| info | on | `1042:21789` | `de0b7159314e51ea2f24d89d22f3f055a263b987` |

**type=内联提示2**

| status | Dark | node_id | component_key |
|--------|------|---------|---------------|
| success | off | `1042:21626` | `1aff96a4a58d66aa31609deee5c1de2f43cab0e1` |
| info | off | `1042:21633` | `a47dd5f384547e234c6747b6ce9c7ee5537d1db8` |
| danger | off | `1042:21640` | `666d9d675202553d1079ce671fdc9c7929d88608` |
| warning | off | `1042:21647` | `f6cf2063a66fbab66275e062a6f3926ca22c5e1b` |
| warning | on | `1042:21734` | `7e22d420b2ee4fcde4e82f3fbae5cf8478088905` |
| danger | on | `1042:21741` | `1473db4874d19f4cf4201788edc3f2c33c52d0a4` |
| info | on | `1042:21748` | `8ca76f818b5ae33757090feb3dc47e3385f9c88c` |
| success | on | `1042:21755` | `e61040074ed0eae8e3701e50ff80b1496398b0f5` |

---

### Pixso 操作速查

1. **插入全局提示**：组件面板搜索「OMessage」，选 type=全局提示，拖入画布
2. **切换状态**：右侧面板 → status 属性 → 选择 `success` / `info` / `danger` / `warning` / `loading`
3. **切换类型**：右侧面板 → type 属性 → 选择 `全局提示` / `内联提示1` / `内联提示2`
4. **切换主题**：右侧面板 → Dark 属性 → 选择 `off` / `on`
5. **修改文字**：双击组件进入内部 → 双击文字层修改内容
6. **内联提示拉伸宽度**：选中组件 → 拖拽右边缘，组件宽度自适应
7. **添加关闭按钮**（内联提示2）：双击进入顶部行 → 显示关闭图标图层

---

### 注意事项

- **loading 状态仅限全局提示**：内联提示1 和内联提示2 不包含 loading 变体
- **全局提示宽度自适应**：宽度由内容文字决定，勿强制拉伸
- **内联提示铺满容器**：内联提示1 和内联提示2 通常设置宽度约束为「填充容器」
- **深色模式颜色**：深色模式下状态色使用同名 Token 的深色版本，不需要手动修改图标颜色
- **内联提示2 描述缩进**：描述文字的 paddingLeft 与图标+文字间距对齐，保持视觉整洁
- **全局提示不嵌入布局**：全局提示应在独立图层，不影响页面布局流

---

## Part C：交互与状态

### 状态定义

| 状态 | 说明 | 适用类型 |
|------|------|---------|
| 默认 | 消息正常显示 | 全部 |
| 消失中 | 全局提示淡出动效 | 全局提示 |
| 关闭 | 内联提示点击关闭按钮后移除 | 内联提示2 |

### 交互行为

- **全局提示**：出现于页面顶部居中，持续 3 秒后自动消失，支持点击手动关闭
- **内联提示1**：常驻显示，无关闭操作
- **内联提示2**：支持点击右上角关闭图标关闭，关闭后组件从布局中移除

---

## Part D：设计变量绑定

### 推荐绑定变量

| 元素 | 属性 | 推荐变量 Token |
|------|------|---------------|
| 全局提示背景 | fill | `--o-color-fill2` |
| 全局提示阴影 | effects | shadow 样式 |
| 内联提示背景（success） | fill | `--o-color-success1-light` |
| 内联提示背景（info） | fill | `--o-color-primary1-light` |
| 内联提示背景（danger） | fill | `--o-color-danger1-light` |
| 内联提示背景（warning） | fill | `--o-color-warning1-light` |
| 状态图标（success） | fill | `--o-color-success1` |
| 状态图标（info） | fill | `--o-color-primary1` |
| 状态图标（danger） | fill | `--o-color-danger1` |
| 状态图标（warning） | fill | `--o-color-warning1` |
| 圆角 | cornerRadius | `radius_control-m`（4px） |
| 主文字 | fill | `--o-color-info1` |
| 描述文字 | fill | `--o-color-info2` |
| 关闭图标 | fill | `--o-color-info2` |
| 字号 | fontSize | `font_size-tip1`（14px） |
| 行高 | lineHeight | `line_height-tip1`（22px） |

---

## Part E：最佳实践

### 使用建议

1. **按场景选类型**：操作反馈用全局提示；页面区块说明用内联提示1；需要标题+描述的通知用内联提示2
2. **状态语义准确**：success=操作成功、info=一般提醒、danger=错误/危险、warning=需注意
3. **文案简洁**：全局提示文字不超过 20 字；内联提示1 不超过 40 字；内联提示2 标题不超过 15 字
4. **避免叠加**：同一时刻不超过 3 条全局提示；内联提示不在同一区域叠放

### 设计提示

- 全局提示出现位置建议距顶部 16px，居中对齐
- 多条全局提示依次排列，间距 8px，新提示从上方插入
- 内联提示2 的关闭按钮仅在需要用户手动关闭时使用，常驻说明不建议添加
- loading 状态图标应有旋转动效（在原型中体现）
- 深色模式下 info 状态使用 primary1 深色值，与浅色模式保持语义一致

---

## Part F：Assets 图标资源

> 路径：`references/assets/message/`

| 文件名 | 对应 status | 使用位置 |
|--------|-------------|---------|
| `成功.svg` | `success` | 全局提示 / 内联提示1 / 内联提示2 左侧状态图标 |
| `提示.svg` | `info` | 全局提示 / 内联提示1 / 内联提示2 左侧状态图标 |
| `警示.svg` | `warning` | 全局提示 / 内联提示1 / 内联提示2 左侧状态图标 |
| `错误.svg` | `danger` | 全局提示 / 内联提示1 / 内联提示2 左侧状态图标 |
| `icon-loading 加载.svg` | `loading` | 全局提示 loading 状态图标（路径：`references/assets/public icons/`） |

**使用规则**
- 图标尺寸固定为 24×24px
- 所有图标**颜色、路径均来自 SVG 文件本身，禁止用 CSS/fill 属性覆盖**，保持 assets 原始配色
- 生成 HTML 时直接内联 SVG 原始内容，路径：`references/assets/message/<文件名>` 或 `references/assets/public icons/<文件名>`

---

## 技术备注

- 组件节点 ID：`1042:21437`（组件集）
- 设计稿 URL：https://pixso.cn/app/design/JZkjW0mhmT61Mtd98dCfBw?item-id=1042:21437
- 生成日期：2026-04-28
- 变体总数：26（type × status × Dark，loading 仅全局提示）
- 消息类型：全局提示（浮层）、内联提示1（单行）、内联提示2（双行）
- 状态：success / info / danger / warning / loading（loading 仅全局提示）
- 圆角：4px（radius_control-m）
- 全局提示阴影：shadow 样式
- 状态色系：success / primary（info） / danger / warning