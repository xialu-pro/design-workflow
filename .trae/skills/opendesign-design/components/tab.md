> ← [组件索引](../SKILL.md#组件索引) · [README](../README.md)

# OTab 标签页 · 设计 Skill

> 组件集合节点：`1042:13041` · 组件名：OTab 标签页 · 变体总数：26

---

## Part A：设计使用卡

### 组件概览

**OTab 标签页**：在同一页面区域内切换显示不同内容分组，是多内容区块切换的核心导航控件。通过文字/图标排列和选中态指示器，帮助用户感知当前位置并在分类间自由切换。

---

### 适用场景

- ✅ **内容分类导航**：同一页面内的内容模块切换，如"概览 / 详情 / 日志"，使用 `text` 变体搭配顶部或底部分区
- ✅ **功能区块切换**：设置页、工具栏中的选项组切换，使用 `button` 变体呈现视觉分组感
- ✅ **底部导航（移动端）**：图标在上、文字在下的 `small` 尺寸 `text` 变体，适合移动端底部 Tab Bar
- ✅ **紧凑工具栏**：空间有限时使用 `medium` 或 `small` 尺寸，减少占用高度
- ❌ **不适合**：不同页面之间的跳转（用导航链接或路由按钮）；单个操作触发（用 OButton）；层级面包屑（用 Breadcrumb）

---

### 变体说明

**Variant（外观类型）**
- `text` — 下划线风格，仅用文字/图标区分选中与未选中，选中时底部出现 2px 品牌色指示线；视觉轻量，适合页面内容区顶部导航
- `button` — 胶囊/分段控件风格，所有标签项排布在灰色容器内，选中项提升为白色圆角卡片；视觉权重较高，适合工具栏或功能区选择

**size（尺寸）**
- `large` — 高 48px（text 变体）/ 容器高 48px（button 变体），字号 20px，图标 32×32（text）/ 24×24（button）；适合页面主标题区标签
- `medium` — 高 44px（text 变体）/ 容器高 32px（button 变体），字号 18px（text）/ 14px（button），图标 24×24（text）/ 16×16（button）；**默认尺寸**，适合大多数内容区标签
- `small` — 高 56px（text 变体，竖向排列），字号 14px，图标 24×24，图标在上、文字在下；适合移动端底部导航栏

**state（状态）**
- `Enabled` — 未选中状态，文字使用 80% 透明度黑/白色，无指示器
- `Actived` — 已选中状态，文字切换为品牌主色，显示底部指示器（text 变体）或白色卡片背景（button 变体），字重加粗
- `Group` — 完整标签组，包含多个标签（一个激活、其余未激活），直接插入可复用的完整 Tab 栏
- `Group 反色` — 反色完整标签组，用于深色背景区域（如 Hero Banner 内的 Tab）

**Dark（主题）**
- `off` — 浅色模式（默认）
- `on` — 深色模式，品牌色和文字色随 Token 自动切换

---

### 布局结构

> 🧩 **布局结构（text 变体，large / medium）**：整体水平排列（HORIZONTAL），内容区由图标和文字横向组合，图标与文字间距 8px；底部留有 16px 的"底边区"用于容纳选中指示器（2px 绝对定位矩形条，圆角 100px）。

```
OTab text（HORIZONTAL，自适应宽度，固定高度）
│  paddingTop: 0（large）/ 2px（medium）
│  paddingBottom: 16px（为底部指示器预留空间）
│  paddingLeft/Right: 0
│
├── [图标 Icon/占位符号 INSTANCE]（固定 32×32 large / 24×24 medium，可选）
│
├── [文字 PARAGRAPH]（自适应宽，固定行高）
│     font: HarmonyHeiTi Regular（Enabled）/ Medium or SemiBold（Actived）
│     fontSize: 20px（large）/ 18px（medium）
│
└── [选中指示器 RECTANGLE]（绝对定位，宽度 = 组件宽，高 2px，底部贴边，圆角 100px）
      仅 Actived 状态显示，fill: --o-color-primary1
```

> 🧩 **布局结构（text 变体，small）**：垂直排列（VERTICAL），图标居上，文字居下，两者居中对齐；整体宽高均为 56×56px。选中指示器为 16px 宽的短线（非全宽），绝对定位在文字下方。

```
OTab text small（VERTICAL，56×56px）
├── [图标 Icon/占位符号 INSTANCE]（24×24，居中）
└── [下方容器 FRAME]（VERTICAL）
    ├── [文字 PARAGRAPH]（14px，居中对齐）
    └── [选中指示器 RECTANGLE]（16px宽×2px，圆角 9px，仅 Actived 显示）
```

> 🧩 **布局结构（button 变体）**：整体水平排列的分段控件，外层容器为灰色圆角背景（grey-3，cr=4px），内部各标签项等宽横向排列，选中项浮起为白色卡片（cr=4px）。

```
OTab button Group（HORIZONTAL，固定高度，内边距 4px）
│  fill: grey-3（浅色模式）
│  cornerRadius: 4px（radius_control-s）
│
├── [选中标签项 FRAME]（白色背景，strokeColor=--o-color-control4，cr=4px）
│   ├── [图标 INSTANCE]（24×24 large / 16×16 medium）
│   └── [text PARAGRAPH]（SemiBold，--o-color-primary1）
│
└── [未选中标签项 FRAME] × N（透明背景，cr=2px）
    ├── [图标 INSTANCE]
    └── [text PARAGRAPH]（Regular，grey-14 @ 80%）
```

---

### 组合搭配

> 🔗 **常见搭配**：
> - **内容区顶部导航**：`text` 变体横排，下方紧接对应内容区（分隔线可选）；多个 Tab 等宽或自适应排列
> - **工具栏选项切换**：`button` 变体搭配右侧的操作按钮（OButton），整体放置在顶部工具栏区域
> - **移动端底部导航**：4–5 个 `small` 尺寸 `text` 变体并排，等宽分布于底部安全区上方
> - **卡片内标签**：`medium` 尺寸 `text` 变体放置在卡片顶部，控制卡片内部内容切换

---

### 设计稿识别指南

> 🔍 **识别特征（text 变体）**：
> - 水平排列的多个同高度文字节点，部分文字颜色为品牌主色（`--o-color-primary1`），其余为 80% 透明度黑色
> - 选中项下方有 2px 矩形条（全圆角，品牌色），Enabled 项无任何指示线
> - 整体无背景填充，视觉轻量
> - `small` 尺寸时图标在文字上方（竖向结构），外层为正方形容器（56×56）

> 🔍 **识别特征（button 变体）**：
> - 外层为灰色（grey-3）圆角矩形容器，内含等宽子块
> - 选中子块有白色背景 + 细描边（rgba 10% 黑），未选中子块无背景
> - 整体看起来像"分段控件"或"Segmented Control"

> 🔄 **易混淆组件**：
> - 与 **OButton（text 变体）** 的区别：Tab 是导航切换控件，多个并排且有选中态联动，同一时刻只有一个处于 Actived；OButton 是独立触发操作，没有选中态概念
> - 与 **OButton（outline/solid 多个并排）** 的区别：OButton 每个独立触发，Tab 有状态关联；Tab 的 `button` 变体视觉上像 Segmented Control，但本质是导航而非动作
> - 与 **底部导航栏** 的区别：底部导航跨页面切换；Tab 在同一页面内切换内容区块

---

### 响应式行为

参照 [栅格规范](../SKILL.md#数据资源) 中的断点定义，OTab 无自动断点响应式行为。尺寸通过 `size` 变体手动控制，深色模式通过 `Dark` 变体手动切换。小屏两档（601~840px / 340~600px）推荐使用 `small` 尺寸 `text` 变体。

---

## Part B：规格速查参考

### 变体索引表

| 变体属性 | 可选值 | 默认值 | 视觉差异 |
|---------|--------|--------|---------|
| Variant | `text` / `button` | `text` | 下划线指示器 / 分段控件胶囊背景 |
| size | `large` / `medium` / `small` | `medium` | 字号和图标尺寸变化；small 为竖向布局 |
| state | `Enabled` / `Actived` / `Group` / `Group 反色` | `Enabled` | 文字颜色和指示器显隐；Group 为完整多标签组合 |
| Dark | `off` / `on` | `off` | 品牌色和中性色随主题切换 |

---

### 布局规格（text 变体）

| 规格项 | large | medium | small |
|--------|-------|--------|-------|
| 整体高度 | 48px | 44px | 56px |
| 整体宽度 | 随内容自适应 | 随内容自适应 | 56px（固定） |
| 内边距 上 | 0px | 2px | 0px |
| 内边距 下 | 16px | 16px | 0px |
| 内边距 左/右 | 0px | 0px | 0px |
| 内容间距（图标↔文字） | 8px | 8px | — |
| 布局方向 | HORIZONTAL | HORIZONTAL | VERTICAL（图标上，文字下） |
| 图标尺寸 | 32×32px | 24×24px | 24×24px |
| 字号 | 20px | 18px | 14px |
| Token（字号） | `font_size-h4` | `font_size-text2` | `font_size-tip1` |
| 行高 | 28px | 26px | 22px |
| Token（行高） | `line_height-h4` | `line_height-text2` | `line_height-tip1` |
| 选中指示器高度 | 2px | 2px | 2px |
| 选中指示器宽度 | 全宽（等于组件宽） | 全宽 | 16px（居中短线） |
| 选中指示器圆角 | 100px | 100px | 9px |

---

### 布局规格（button 变体）

| 规格项 | large | medium |
|--------|-------|--------|
| 外容器高度 | 48px | 32px |
| 外容器内边距 | 4px（全周） | 2px（上下）/ 2px（左右） |
| 外容器圆角 | 4px (`radius_control-s`) | 4px (`radius_control-s`) |
| 标签项高度 | 40px | 28px |
| 标签项内边距（左/右） | 16px | 16px |
| 标签项内边距（上/下） | 4px（Enabled）/ 3px | 4px |
| 标签项间距（图标↔文字） | 4px | 4px |
| 选中项圆角 | 4px | 4px |
| 未选中项圆角 | 2px | 2px |
| 图标尺寸 | 24×24px | 16×16px |
| 字号 | 18px (`font_size-text2`) | 14px (`font_size-tip1`) |
| 行高 | 26px (`line_height-text2`) | 22px (`line_height-tip1`) |

---

### 颜色 Token 映射

#### text 变体

| 区域 | 状态 | Light 模式 Token | Dark 模式 Token | 说明 |
|------|------|-----------------|----------------|------|
| 文字 / 图标 | Enabled | `grey-14 @ 80%` | `grey-14 @ 80%` | Light: rgba(0,0,0,0.8)；Dark: rgba(255,255,255,0.8) |
| 文字 / 图标 | Actived | `--o-color-primary1` | `--o-color-primary1` | brand-6：rgb(0,47,167) → rgb(73,122,248) |
| 选中指示器 | Actived | `--o-color-primary1` | `--o-color-primary1` | 同文字色 |
| 背景 | — | 无 | 无 | 无背景填充 |

#### button 变体

| 区域 | 状态 | Light 模式 Token | Dark 模式 Token | 说明 |
|------|------|-----------------|----------------|------|
| 外容器背景 | — | `grey-3` | `grey-3` | Light: rgb(237,237,240)；Dark: rgb(26,26,28) |
| 选中项背景 | Actived | `white` | — | rgb(255,255,255)；Dark 模式下可能为 grey-4 |
| 选中项边框 | Actived | `--o-color-control4` | `--o-color-control4` | rgba(grey-14, 0.1)，strokeWeight=1px INSIDE |
| 选中项文字 | Actived | `--o-color-primary1` | `--o-color-primary1` | brand-6：rgb(0,47,167) → rgb(73,122,248) |
| 未选中项文字 | Enabled | `grey-14 @ 80%` | `grey-14 @ 80%` | Light: rgba(0,0,0,0.8)；Dark: rgba(255,255,255,0.8) |
| 未选中项背景 | Enabled | 透明 | 透明 | 无填充 |

---

### 字体样式映射

| 使用场景 | 字号 Token | 行高 Token | 字重 | 字号值 | 行高值 |
|---------|-----------|-----------|------|-------|-------|
| `large` 标签文字（Enabled） | `font_size-h4` | `line_height-h4` | Regular | 20px | 28px |
| `large` 标签文字（Actived） | `font_size-h4` | `line_height-h4` | **Medium** | 20px | 28px |
| `medium` 标签文字（Enabled） | `font_size-text2` | `line_height-text2` | Regular | 18px | 26px |
| `medium` 标签文字（Actived） | `font_size-text2` | `line_height-text2` | **SemiBold** | 18px | 26px |
| `small` 标签文字（Enabled） | `font_size-tip1` | `line_height-tip1` | Regular | 14px | 22px |
| `small` 标签文字（Actived） | `font_size-tip1` | `line_height-tip1` | **Medium** | 14px | 22px |
| `button` large 标签文字（Enabled） | `font_size-text2` | `line_height-text2` | Regular | 18px | 26px |
| `button` large 标签文字（Actived） | `font_size-text2` | `line_height-text2` | **SemiBold** | 18px | 26px |
| `button` medium 标签文字（Enabled） | `font_size-tip1` | `line_height-tip1` | Regular | 14px | 22px |
| `button` medium 标签文字（Actived） | `font_size-tip1` | `line_height-tip1` | **SemiBold** | 14px | 22px |

字体族：`font_family`（HarmonyOS / HarmonyHeiTi）

> **注意**：选中（Actived）态字重会加粗，但 `tokens.json` 中仅定义了 `font_weight-regular` 和 `font_weight-bold`。SemiBold（600）与 Medium（500）为字体文件中的具体字重，通过 fontStyle 属性直接指定，不依赖 Token 映射。

---

### 组件层级结构

#### text 变体（large / medium）

```
OTab text（HORIZONTAL，自适应宽，固定高 48/44px）
│  paddingTop: 0/2px | paddingBottom: 16px | paddingLeft/Right: 0
│  fill: 无
│
├── [图标 Icon/占位符号 INSTANCE]（32×32 large / 24×24 medium，可选）
│     fill: --o-color-primary1（Actived）/ grey-14 @ 80%（Enabled）
│
├── [文字 PARAGRAPH]（自适应宽，行高固定）
│     font: HarmonyHeiTi Regular（Enabled）/ Medium or SemiBold（Actived）
│     fill: --o-color-primary1（Actived）/ grey-14 @ 80%（Enabled）
│
└── [选中指示器 RECTANGLE]（绝对定位，全宽×2px，底部，圆角 100px）
      fill: --o-color-primary1（仅 Actived 显示）
```

#### text 变体（small，竖向）

```
OTab text small（VERTICAL，56×56px）
├── [图标 Icon/占位符号 INSTANCE]（24×24，居上居中）
│     fill: --o-color-primary1（Actived）/ grey-14 @ 80%（Enabled）
└── [下方容器 FRAME]（VERTICAL，居中对齐，paddingTop 4px，paddingBottom 2/6px）
    ├── [文字 PARAGRAPH]（14px，居中，固定行高 22px）
    │     fill: --o-color-primary1（Actived）/ grey-14 @ 80%（Enabled）
    └── [选中指示器 RECTANGLE]（16px×2px，圆角 9px，仅 Actived 显示）
          fill: --o-color-primary1
```

#### button 变体

```
OTab button Group（HORIZONTAL，自适应宽，固定高）
│  fill: grey-3 | cornerRadius: 4px（radius_control-s）
│  padding: 4px（large）/ 2px（medium）
│
├── [选中标签项 FRAME]（固定高 40/28px，HORIZONTAL，gap 4px）
│   │  fill: white | strokeColor: --o-color-control4 | strokeWeight: 1px INSIDE
│   │  cornerRadius: 4px | paddingLeft/Right: 16px
│   ├── [图标 INSTANCE]（24×24 large / 16×16 medium，可选）
│   └── [text PARAGRAPH]（SemiBold，fill: --o-color-primary1）
│
└── [未选中标签项 FRAME] × N（固定高，HORIZONTAL，gap 4px）
    │  fill: 透明 | cornerRadius: 2px | paddingLeft/Right: 16px
    ├── [图标 INSTANCE]（可选）
    └── [text PARAGRAPH]（Regular，fill: grey-14 @ 80%）
```

---

### 变体 componentKey 速查

| 变体组合 | node_id | componentKey |
|---------|---------|-------------|
| size=large, state=Enabled, Dark=off, Variant=text | `1042:13042` | `5fe83eaf96af4c8c0f62579de188c3c7520940cb` |
| size=large, state=Actived, Dark=off, Variant=text | `1042:13048` | `e231068b6fbd9b451ceee01710db2fd0bec0b7cd` |
| size=medium, state=Enabled, Dark=off, Variant=text | `1042:13087` | `d0ae6a081cfb3ed7ed98fd3bb171540732c9bd71` |
| size=medium, state=Actived, Dark=off, Variant=text | `1042:13077` | `b6c73373ce811445dc1bef1264a9b417c81e1935` |
| size=small, state=Enabled, Dark=off, Variant=text | `1042:13093` | `ebf2a796a667ccc60eb87416e7aad4d3e84e8704` |
| size=small, state=Actived, Dark=off, Variant=text | `1042:13111` | `214a279c79cb82da622bd86070a35a6348f53e76` |
| size=large, state=Group, Dark=off, Variant=text | `1042:13059` | `f9eed856e94614765ff0a8613aba015512c47291` |
| size=medium, state=Group, Dark=off, Variant=text | `1042:13121` | `2532b2f1ac7113bbf42667916b341ed87ebb0595` |
| size=small, state=Group, Dark=off, Variant=text | `1042:13127` | `4126f08210577e31eca7526d86d8595d31311271` |
| size=large, state=Group, Dark=off, Variant=button | `1042:16877` | `1e1ec8f775c24db7df775c6127a25550639d50d5` |
| size=medium, state=Group, Dark=off, Variant=button | `1042:16887` | `107f3a263a5fd0261719a5f4790bc2574bba09a0` |
| size=large, state=Group 反色, Dark=off, Variant=button | `1042:16917` | `b188fa4461e715c0861ddd63932ad7fd66b2d60c` |

---

### Pixso 操作速查

1. **插入单个标签**：组件面板搜索「OTab」，选取对应 size + Variant 的 Enabled 或 Actived 变体拖入；或 AI 调用 `create_instance("d0ae6a081cfb3ed7ed98fd3bb171540732c9bd71")`（medium/text/Enabled 为例）
2. **插入完整标签组**：直接选用 `state=Group` 变体（如 `f9eed856e94614765ff0a8613aba015512c47291`），内含完整标签排列
3. **切换外观**：右侧面板 → Variant 属性 → 选择 `text` / `button`
4. **切换选中状态**：Variant 属性 → state 选择 `Enabled` / `Actived`
5. **切换主题**：Variant 属性 → Dark 选择 `on` / `off`
6. **修改标签文字**：双击进入组件实例 → 双击文字层修改内容
7. **隐藏图标**：双击进入实例 → 选中图标层 → 设为不可见

---

### 注意事项

- `text` 变体的 `paddingBottom=16px` 是为底部选中指示器（2px）预留的空间，不要将其理解为普通底边距——实际内容区与组件底边的可见间距约为 14px
- `small` 尺寸为**竖向布局**（图标在上，文字在下），与 `large`/`medium` 的横向布局完全不同；使用前确认场景（移动端底栏或紧凑图标导航）
- `button` 变体展示的是完整标签**组**（Group），不是单个标签——插入时直接使用 Group 状态变体，内部已包含选中/未选中标签项的组合
- `Group 反色` 变体用于深色背景（如 Banner 图上），颜色方案与普通 Group 相反，使用前确认背景色是否匹配
- Actived 态的字重加粗（Medium/SemiBold）会导致文字宽度略微变化，若标签宽度为自适应，切换选中态时可能出现轻微布局偏移——对于固定宽度布局建议手动设置标签项最小宽度
- `small` 尺寸的选中指示器为 16px 宽短线（非全宽），视觉上类似"点状"指示，与 large/medium 全宽指示线风格不同
---

## Part C：交互状态

### text 变体交互状态

> 🔴 **hover / press 一律不改字重**（字重只由「是否选中」决定：非选中恒 Regular、选中恒 SemiBold）。原因见下方「交互行为说明 · Hover」。下表为 2026-10-20 用户裁决后的修订版。

| 交互状态 | 标签类型 | 字号 | 字重 | 色值 | 下划线 |
|---------|---------|------|------|------|--------|
| Default | 非选中 | 18px | Regular | `--o-color-info2` | 无 |
| Default | 选中 | 18px | **SemiBold** | `--o-color-primary1` | 2px 高, `--o-color-primary1` |
| Hover | 非选中 | 18px | Regular（不变） | `--o-color-primary2` | 无 |
| Hover | 选中 | 18px | **SemiBold** | `--o-color-primary1` | 2px 高, `--o-color-primary1` |
| Press | 非选中 | 18px | Regular（不变） | `--o-color-primary3` | 2px 高, `--o-color-primary3` |
| Press | 选中 | 18px | **SemiBold** | `--o-color-primary3` | 2px 高, `--o-color-primary3` |
| Actived | 非选中 | 18px | **SemiBold** | `--o-color-primary1` | 2px 高, `--o-color-primary1` |
| Actived | 选中 | 18px | **SemiBold** | `--o-color-primary1` | 2px 高, `--o-color-primary1` |
| Disabled | 非选中 | 18px | Regular | `--o-color-info4` | 无 |
| Disabled | 选中 | 18px | Regular | `--o-color-info4` | 无 |

### button 变体交互状态

| 交互状态 | 字号 | 字重 | 色值 | 图标尺寸及颜色 | 底色/背景色 |
|---------|------|------|------|-------------|------------|
| Default | 16px | Regular | `--o-color-info2` | 16×16, `--o-color-info2` | — |
| Hover | 16px | Regular | `--o-color-primary2` | 16×16, `--o-color-primary2` | — |
| Press | 16px | Regular | `--o-color-primary3` | 16×16, `--o-color-primary3` | `--o-color-fill2` |
| Actived | 16px | Regular | `--o-color-primary1` | 16×16, `--o-color-primary1` | `--o-color-fill2` |
| Disabled | 16px | Regular | `--o-color-info4` | 16×16, `--o-color-info4` | — |

### 交互行为说明

- **Default（默认）**：标签的基础显示态，非选中项使用常规字重和 info 色；选中项使用 SemiBold 字重、品牌主色和底部指示线
- **Hover（悬浮）**：鼠标悬停时，文字颜色提升至 primary2（较浅的品牌色），**字重保持不变**；选中项保持 primary1 不变
- **Press（按下）**：鼠标按下瞬间，颜色加深至 primary3（深品牌色）；text 变体此时显示同色下划线；button 变体增加 fill2 背景色；**字重保持不变**
- **Actived（选中）**：当前激活标签的稳定态，使用最强的视觉强调（primary1 + 指示线/背景）；同一时刻仅一个标签处于此状态
- **Disabled（禁用）**：不可交互状态，退化为 info4 弱化色，字重恢复 Regular，移除所有指示器的可见性和特殊背景。**⚠️ 禁用是终态：hover / press 对禁用项一律不得生效**（含不变色、不出现指示器）——详见下方 🔴 段
  - **「移除指示器」指视觉不可见，不是移出布局**：small 档为竖向 56×56（`::after` 为 16px 短线 + `margin-top: 4px`，在文档流内占位），禁用项若写 `display: none` 会连占位一起删掉，导致该页签在 `justify-content: center` 下只剩文字、垂直居中，**文字比同排其他页签低一截**（实测错位约 10px）。正确写法：`.otab__item:disabled::after { visibility: hidden; }`——保留 16px + 4px 占位，文字与其他页签顶对齐/同位；对 large / medium（指示器绝对定位、不占文档流）无副作用。⛔ 禁用 `.otab__item:disabled::after { display: none; }`。

> 🔴 **为什么 hover / press 禁止改字重（2026-10-20 用户裁决，已升级为 [hard-constraints.md](../global/hard-constraints.md) §2.1 全局字重铁律）**：
> 原 Part C 曾规定「Hover 非选中 → SemiBold」，该条已作废。理由三条：
> 1. **设计语义错误**：字重在本组件中只承担「是否选中」一个语义（非选中 Regular / 选中 SemiBold）。hover 是瞬态提示，让它也加粗 = 把「选中」的视觉标识稀释成「鼠标随便划过一下也长这样」，用户无法从字重判断当前在哪一档。
> 2. **实测会引起文字跳动**：页签宽度由文字撑开（auto）、选中指示器为全宽，字重一切换文字 advance width 就变——实测拉丁/数字混排标签 "Docs v1.2" 400→600 宽 +1.91px、"Blog" +1.03px，下划线随之变宽、后续页签被推挤位移（纯中文等宽全角字形差值 0px，所以只在英文标签上暴露，更容易被漏掉）。
> 3. **已多次复现**：navigation.md 踩坑 #17（顶导触发器）、navigation-dropdown-categorized.md 踩坑 #30（分类条目）各自独立立过同一条禁令，但本组件 Part C 一直没改，导致每处引用 tab.md 的生成都重新踩一次。用户最终裁决：**这是全局规范，不是 Tab 的局部约定**——字重只随「是否选中」变化，hover / press / focus / 展开态一律不碰。
>
> **正确写法**：hover / press 只改 `color`（+ press 的同色下划线），一条 `font-weight` 都不写；且 Press 规则特异性必须 ≥ Hover 规则（按下时鼠标必然同时处于 hover，否则 Press 色会被 Hover 色盖掉）——例如 `.otab__item:active:not(.is-active), .otab__item.is-active:active { color: var(--o-color-primary3); }`。

> 🔴 **为什么 hover / press 必须带 `:not(:disabled)` 限定（2026-10-20 用户裁决，已升级为 [hard-constraints.md](../global/hard-constraints.md) §2.2 禁用态铁律）**：
> 原 Part C 列了 Disabled 状态，却**从未规定 hover / press 要排除禁用项**，导致禁用页签被鼠标划过时依然变色。实测（无头 Chrome）：禁用项 hover → `primary2`、press → `primary3` 且指示器背景变色——**与「禁用」的语义完全相反**。
>
> **根因是特异性**：`.otab__item:hover:not(.is-active)` = **(0,3,0)**，而 `.otab__item:disabled` 仅 **(0,2,0)**——禁用规则被 hover 规则直接盖掉。`:not()` 会继承其参数的特异性，所以每加一个 `:not(.xxx)` 就抬高一级，很容易在不知不觉中越过禁用规则。
>
> **正确写法**：hover / press 规则一律补 `:not(:disabled)`，让交互态**从选择器层面就不匹配禁用元素**：
> ```css
> .otab__item:hover:not(.is-active):not(:disabled) { color: var(--o-color-primary2); }
> .otab__item:active:not(.is-active):not(:disabled),
> .otab__item.is-active:active:not(:disabled) { color: var(--o-color-primary3); }
> .otab__item:active:not(:disabled)::after { background: var(--o-color-primary3); }
> .otab__item:disabled { font-weight: 400; color: var(--o-color-info4); cursor: not-allowed; }
> .otab__item:disabled::after { visibility: hidden; }  /* 视觉移除但保留占位：small 档竖排时禁用项文字与其他页签对齐；⛔ 禁写 display: none（会删掉 16px+4px 占位，文字低一截） */
> ```
> 另一条路是把禁用规则写成 `.item:disabled:hover, .item:disabled:active`（0,3,0）硬压，但**首选前者**——语义更直白（"禁用项没有交互态"），且不必为每种交互态补一条禁用规则。参考实现见 `references/examples/dropdown-demo.html`（`:hover:not(.disabled)`）。

> **注意**：text 变体的下划线仅在 Actived 和 Press 状态下的非选中标签上出现（表示正在操作的目标）；button 变体的背景色仅在 Press 和 Actived 状态下生效