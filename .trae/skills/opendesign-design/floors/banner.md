← [楼层模板索引](../SKILL.md#楼层模板索引) · [组件规范](../components/banner.md)

# Banner 楼层模板

> 本模板为自包含生成单元。AI 读取本文件后即可精确生成 Banner 楼层，无需再拼凑多个组件规范。

---

## 涉及组件

| 组件 | 规范文档 | 本楼层使用的变体 |
|------|---------|----------------|
| OBanner | [components/banner.md](../components/banner.md) | XL / L / M / 无 |
| OButton | [components/button.md](../components/button.md) | size=large（Desktop）/ medium（Laptop/Pad） |

---

## 一、PC Desktop（> 1680px）

### 布局结构

```
Banner Desktop（1920px × H）
│  内容区宽度: 1488px, 距两边各 216px
│  文字内容最大宽度: 占 12 columns
│  水平对齐: 左对齐   │  垂直对齐: banner 高度内垂直居中
│  内容从上到下: 标题 → 正文 → 按钮
│
├── [标题]（必填）
├── [正文]（可选）
└── [按钮 OButton]（可选，large, solid）
```

> ⚠️ **垂直对齐规则（Desktop / Laptop / Pad）**：内容整体在 banner 高度内**垂直居中**。
> 「标题距导航」为**顶部对齐场景**专属值，与垂直居中**不可同时成立**——设计稿未明确要求顶部对齐时，一律按垂直居中实现。
> 垂直居中实现要点：外层全宽容器 `display:flex; align-items:center;`，内层内容区 `padding:0`（禁止用 padding-top 做偏移）。

### ⚠️ 楼层容器 CSS 实现（必须严格遵守）

> 🔴 Banner楼层为全宽背景+内部内容区模式。生成HTML时，外层全宽背景，内层内容区**必须**使用以下CSS。**禁止使用 `margin: 0 auto` + `padding: 0 216px` 组合**（border-box下padding吃进max-width，内容区变窄）。

**PC端**：
```css
.banner-section {
    /* 全宽背景 */
    width: 100%;
    padding: [高度相关值] 0;   /* 上下padding按等级，左右为0 */
}
/* 内层内容区 */
.banner-content {
    width: 1488px;                    /* 内容区固定宽度，撑满 */
    max-width: 1488px;                /* 禁止 1200px / 1140px / 960px */
    margin-left: 216px !important;    /* 固定左边距 */
    margin-right: 216px !important;   /* 固定右边距 */
    padding: 0 !important;            /* 禁止用padding控制楼层边距 */
}
```

**MB端**：
```css
.banner-section-mb {
    width: 100%;
    padding: [高度相关值] 0;
}
.banner-content-mb {
    width: 312px;
    max-width: 312px;                 /* 禁止 360px */
    margin-left: 24px !important;
    margin-right: 24px !important;
    padding: 0 !important;
}
```

### 各等级精确数值

| 等级 | 高度 | 标题距导航 | 标题字号/行高 | 正文字号/行高 | 按钮尺寸 |
|------|------|-----------|-------------|-------------|---------|
| **XL** | 460px | 136px | 56px / 80px | 20px / 28px | large（40px 高） |
| **L** | 360px | 88px | 48px / 64px | 18px / 26px | large（40px 高） |
| **M** | 280px | 64px | 48px / 64px | 18px / 26px | large（40px 高） |
| **无** | — | 40px | 48px / 64px | — | — |

### 字重与颜色

颜色一律使用语义 Token，深浅模式由 `data-o-theme` 切换自动适配（Token 同名翻转），禁止硬编码 rgba / hex：

| 元素 | 字重 | 颜色 Token |
|------|------|-----------|
| 标题 | SemiBold | `var(--o-color-info1)` |
| 正文 | Regular | `var(--o-color-info2)` |

---

## 二、PC Laptop（1201px ~ 1680px）

> 垂直对齐同 Desktop：内容在 banner 高度内**垂直居中**（除非设计稿明确要求顶部对齐，此时用「标题距导航」）。

| 等级 | 高度 | 标题距导航 | 标题字号/行高 | 正文字号/行高 | 按钮尺寸 |
|------|------|-----------|-------------|-------------|---------|
| **XL** | 400px | 120px | 48px / 64px | 18px / 26px | medium（32px 高） |
| **L** | 280px | 64px | 40px / 56px | 16px / 24px | medium（32px 高） |
| **M** | 220px | 40px | 40px / 56px | 16px / 24px | medium（32px 高） |

---

## 三、Pad（841px ~ 1200px）

> 垂直对齐同 Desktop：内容在 banner 高度内**垂直居中**（除非设计稿明确要求顶部对齐，此时用「标题距导航」）。

| 等级 | 高度 | 标题距导航 | 标题字号/行高 | 正文字号/行高 | 按钮尺寸 |
|------|------|-----------|-------------|-------------|---------|
| **XL** | 320px | 88px | 40px / 56px | 16px / 24px | medium（32px 高） |
| **L** | 220px | 48px | 32px / 44px | 14px / 22px | medium（32px 高） |
| **M** | 180px | 32px | 32px / 44px | 14px / 22px | medium（32px 高） |

---

## 四、MB（360px ~ 840px）

### 特殊约束

- 高度**固定**，横向拉伸，**不做等比缩放**
- Banner 为**带圆角的卡片样式**
- Banner **不支持按钮组件**

| 等级 | 高度 | 标题字号/行高 | 正文字号/行高 | 标题对齐 |
|------|------|-------------|-------------|---------|
| **XL** | 184px | 22px / 30px | 12px / 18px | 垂直居中 |
| **L** | 120px | 22px / 30px | 不支持正文 | 垂直居中 |
| **M** | 无 banner | — | — | — |

---

## 五、等级选择决策树

```
你的页面是什么类型？
├─ 社区首页 → XL
├─ 一级导航栏目主页（聚合主页）→ L
├─ 一级导航栏目内子页面 → M
└─ 新闻/技术专栏（资讯类）→ 无（不渲染 banner，标题直接出现在导航下方）
```

---

## 六、楼层自检清单

```
□ Banner 高度是否与等级+断点匹配？
□ 标题字号/行高是否成对且来自硬约束白名单？
□ 正文字号/行高是否成对且来自硬约束白名单？
□ 标题距导航距离是否精确？
□ 按钮尺寸是否与断点匹配（Desktop=large, 其余=medium）？
□ MB 端是否未渲染按钮？
□ MB 端 Banner 是否为带圆角卡片样式？
□ 内容是否左对齐 + 在 banner 高度内垂直居中（PC）/ 垂直居中（MB）？
□ 垂直居中实现是否为外层 `display:flex; align-items:center`（而非内层 `padding-top` 偏移）？
□ 「标题距导航」是否仅用于设计稿明确要求顶部对齐的场景（未与垂直居中同时使用）？
□ 文字最大宽度是否受栅格列数限制？
```