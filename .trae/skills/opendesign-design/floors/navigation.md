← [楼层模板索引](../SKILL.md#楼层模板索引) · [组件规范](../components/navigation.md)

# 导航楼层模板

> 本模板为自包含生成单元。AI 读取本文件后即可精确生成导航楼层，无需再拼凑多个组件规范。

---

## 涉及组件

| 组件 | 规范文档 | 本楼层使用的变体 |
|------|---------|----------------|
| ONavigation | [components/navigation.md](../components/navigation.md) | type=PC, Dark=off（顶导）/ type=PC, 属性2=openEuler（页脚） |

---

## 一、PC 顶导（Header）

### 布局结构

```
ONavigation Header PC（1920px × 72px）
│  display: flex | justify: space-between | align: center
│  padding: 0px 216px
│  background: rgba(255,255,255,1)（Light）/ rgba(36,36,39,1)（Dark）
│  boxShadow: 0px 3px 9px 0px rgba(0,18,85,0.078)
│  backdropFilter: blur(4.53px)
│
├── [左侧区域] gap: 40px, align: flex-end
│   ├── [Logo] 136px × 32px（padding: 20px 0, align-self: center）⚠️
│   └── [导航菜单] gap: 32px, align-self: flex-end ⚠️
│       └── [菜单项 × 7] 32px × 48px, padding-bottom: 24px
│             字号: 16px / 行高: 24px / 字重: 400
│             border-bottom: 2px solid transparent（默认）/ brand-color（选中）
│
└── [右侧操作区] gap: 20px, align: center
    ├── [搜索框] 160px × 32px
    ├── [源码下拉] 48px × 24px
    ├── [国际化图标] 24px × 24px
    ├── [主题切换图标] 24px × 24px
    └── [用户图标] 24px × 24px
```

### ⚠️ Flexbox 对齐规则（极易出错，必须严格遵守）

> **设计意图**：Logo 在导航栏内**垂直居中**，Tab 菜单**底部对齐**（选中下划线贴导航底部），右侧操作区**垂直居中**。

**三层对齐策略**：

| 层级 | CSS 选择器 | align-items | 说明 |
|-----|-----------|-------------|------|
| L1 外层 | `.header` | `center` | 整体垂直居中平衡，**禁止改为 flex-end** |
| L2 左侧 | `.header-left` | `flex-end` | 让子元素默认贴底（Tab 需要） |
| L2 右侧 | `.header-right` | `center` | 操作图标垂直居中 |

**Logo 与 Tab 的非对称对齐**（核心难点）：

Logo 和 Tab 同在 `.header-left` 内，但需要对齐方式不同：
- **Logo**：需要 `align-self: center` 覆盖父级的 `flex-end`，实现垂直居中
- **Tab 菜单**：继承父级的 `flex-end`，实现贴底对齐

```css
/* ✅ 正确写法 */
.header      { display: flex; align-items: center; justify-content: space-between; padding: 0 216px; }  /* ⚠️ 导航栏用padding: 0 216px是正确的，因为导航栏width:100%且无max-width限制 */
.header-left { display: flex; align-items: flex-end; gap: 40px; }
.logo        { height: 32px; padding: 20px 0; align-self: center; }  /* ⭐ align-self 覆盖 */
.nav-menu    { align-self: flex-end; }                                /* ⭐ 贴底 */
.header-right{ display: flex; align-items: center; gap: 20px; }
```

**禁止的做法**：

```css
/* ❌ 绝对禁止！会导致右侧操作区也底部对齐 */
.header { align-items: flex-end; }

/* ❌ 禁止！Logo 也会被拉到底部 */
.header-left { align-items: flex-end; }
/* 但不给 Logo 设置 align-self: center */
```

### 精确数值速查

| 规格项 | 值 | 来源 |
|--------|---|------|
| 画布宽度 | 1920px | 硬约束 §4.1 |
| 顶导高度 | 72px | navigation.md |
| 左右内边距 | 216px | 硬约束 §4.1 |
| Logo 尺寸 | 136px × 32px | navigation.md |
| Logo padding | 20px 0（上下各20px，实现居中：(72-32)/2=20） | navigation.md |
| Logo align-self | center（覆盖父级 flex-end） | ⚠️ 本文档 |
| 菜单项尺寸 | 32px × 48px | navigation.md |
| 菜单项 padding-bottom | 24px（下划线距文字底部24px） | navigation.md |
| 菜单项间距 | 32px | navigation.md |
| Logo 与菜单间距 | 40px | navigation.md |
| 右侧操作区间距 | 20px | navigation.md |
| 搜索框尺寸 | 160px × 32px | navigation.md |
| 操作图标尺寸 | 24px × 24px | navigation.md |
| 菜单文字字号 | 16px / 24px | 硬约束 §2（常规正文） |
| 源码/搜索占位字号 | 14px / 22px | 硬约束 §2（提示文本1） |

### 资源引用

| 用途 | 路径 | 用法 |
|------|------|------|
| PC Light Logo | `references/assets/navigation/header-pc-light logo.svg` | `<img>` |
| PC Dark Logo | `references/assets/navigation/header-pc-dark logo.svg` | `<img>` |
| 搜索图标 | `references/assets/navigation/header-pc-search icon.svg` | `<img>` 或 CSS mask |
| 国际化图标 | `references/assets/navigation/header-pc-语言切换 icon.svg` | `<img>` 或 CSS mask |
| 主题切换图标 | `references/assets/navigation/header-pc-深浅模式.svg` | `<img>` 或 CSS mask |
| 用户图标 | `references/assets/navigation/header-pc-头像.svg` | `<img>` 或 CSS mask |

> ⚠️ **禁止自绘 SVG**：所有图标必须引用 `references/assets/navigation/` 下的资源文件，禁止用内联 SVG path 模拟图标外观。

### 颜色

| 区域 | Light | Dark |
|------|-------|------|
| 背景 | rgba(255,255,255,1) | rgba(36,36,39,1) |
| 菜单文字 | rgba(0,0,0,1) | rgba(255,255,255,1) |
| 搜索占位文字 | rgba(0,0,0,0.4) | rgba(255,255,255,0.4) |
| Dark 底部描边 | — | 1px rgba(255,255,255,0.149) |

### 搜索框实现（Light 模式）

```css
.search-box {
    width: 160px;
    height: 32px;
    position: relative;
    border: 1px solid rgba(0,0,0,0.25);  /* ⚠️ 必须有边框，否则不可见 */
    border-radius: 4px;
    background-color: #FFFFFF;            /* ⚠️ 必须有白色背景 */
    cursor: pointer;
    overflow: hidden;
}
.search-box span {    /* 占位文字 */
    position: absolute;
    left: 16px;
    top: 50%;
    transform: translateY(-50%);
    font-size: 14px;
    color: rgba(0,0,0,0.4);
    pointer-events: none;
}
.search-box img {     /* 搜索图标 */
    position: absolute;
    right: 12px;
    top: 50%;
    transform: translateY(-50%);
    width: 16px;
    height: 16px;
    pointer-events: none;
}
```

### 源码下拉实现

```css
.source-dropdown {
    width: auto;           /* ⚠️ auto，不是固定48px（中文"源码"需要约52px） */
    min-width: 48px;
    height: 24px;
    display: flex;
    align-items: center;
    gap: 8px;
    white-space: nowrap;   /* ⚠️ 必须禁止换行 */
    cursor: pointer;
    font-size: 14px;
    line-height: 24px;
    color: rgba(0,0,0,0.85);  /* ODropdown text 变体颜色 */
}
```

---

## 二、MB 顶导（Header）

### 布局结构

```
ONavigation Header Mb（360px × 76px）
│  position: relative
│  background: rgba(255,255,255,0.95)（Light）
│  boxShadow: 0px 3px 8px 0px rgba(0,0,0,0.078)
│
├── [状态栏] 348.5px × 18px, overflow: hidden
├── [汉堡菜单] 24px × 24px, left: 24px, top: 36px
├── [Logo] 85px × 20px, left: 138px, top: 38px
└── [右侧操作] 64px × 24px, left: 272px, top: 36px
    ├── [搜索图标] 24px × 24px
    └── [用户头像] 24px × 24px
```

### 精确数值速查

| 规格项 | 值 | 来源 |
|--------|---|------|
| 画布宽度 | 360px | 硬约束 §4.1 |
| 顶导高度 | 76px（含 18px 状态栏） | navigation.md |
| 状态栏高度 | 18px | navigation.md |
| 汉堡图标 | 24px × 24px, left: 24px | 硬约束 §4.1 + §5 |
| Logo 尺寸 | 85px × 20px | navigation.md |

---

## 三、踩坑记录（已验证，必须遵守）

> 以下问题均在实际生成中踩过，每条都导致视觉错误。生成时必须逐条对照，确保不再复现。

| # | 问题现象 | 根因 | 正确做法 |
|---|---------|------|---------|
| 1 | Tab菜单垂直居中，下划线不贴底 | 外层 `.header` 误设 `align-items: flex-end`，或左侧区域未设 `align-items: flex-end` | 外层 `center` + 左侧 `flex-end` + Logo `align-self: center` + 菜单 `align-self: flex-end` |
| 2 | Logo 被拉到底部，不居中 | `.header-left` 设了 `flex-end` 但 Logo 没有覆盖 | Logo 必须加 `align-self: center` |
| 3 | 右侧操作区图标底部对齐 | 外层 `.header` 误设 `align-items: flex-end` | 外层必须 `center`，右侧 `align-items: center` |
| 4 | 搜索框不可见（无框无背景） | 缺少 border 和 background-color | 必须设 `border: 1px solid rgba(0,0,0,0.25)` + `background-color: #FFFFFF` |
| 5 | 搜索框内文字/图标位置错乱 | 使用 flex 布局而非绝对定位 | Light 模式搜索框必须用 `position: relative` + 子元素 `position: absolute` |
| 6 | "源码"文字与箭头换行 | 固定 width: 48px 不够容纳中文 | `width: auto` + `white-space: nowrap` |
| 7 | 右侧图标显示为自绘SVG | 生成时用内联 SVG path 模拟图标 | 必须用 `<img>` 引用 `references/assets/navigation/` 下的资源文件 |
| 8 | CSS 样式被覆盖不生效 | `<style>` 中样式优先级不足 | 关键对齐属性使用**内联 style + !important** 确保生效 |

---

## 四、楼层自检清单

```
□ 顶导高度是否为 PC 72px / MB 76px？
□ 左右内边距是否为 PC 216px / MB 24px？
□ Logo 是否用 <img> 引用 SVG 资源（禁止自绘）？
□ Logo 是否设置 align-self: center（在 flex-end 父容器中居中）？
□ 菜单项间距是否为 PC 32px？
□ 菜单项是否设置 padding-bottom: 24px + border-bottom？
□ 菜单选中下划线是否贴导航栏底部？
□ 右侧操作区间距是否为 PC 20px？
□ 右侧操作区是否设置 align-items: center？
□ 操作图标尺寸是否为 24px × 24px？
□ 操作图标是否全部引用 assets 资源文件（禁止自绘SVG）？
□ Dark 模式底部是否有 1px 白色描边？
□ 搜索框是否有 border + background-color（否则不可见）？
□ 搜索框内部是否用绝对定位（而非 flex 布局）？
□ 搜索框尺寸是否为 160px × 32px？
□ 主题切换图标是否未遗漏？
□ 源码按钮是否设置 white-space: nowrap（禁止换行）？
□ 源码按钮宽度是否为 auto（而非固定48px）？
□ 源码按钮是否为 ODropdown text 变体（无背景无描边，颜色 rgba(0,0,0,.85)）？
□ ⚠️ 外层 .header 是否保持 align-items: center（禁止改为 flex-end！）？
□ ⚠️ 左侧 .header-left 是否设置 align-items: flex-end？
□ ⚠️ 关键对齐属性是否使用内联 style + !important 确保优先级？
```