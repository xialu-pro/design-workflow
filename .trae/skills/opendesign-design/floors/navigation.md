← [楼层模板索引](../SKILL.md#楼层模板索引) · [组件规范](../components/navigation.md)

# 导航楼层模板

> 本模板为自包含生成单元。AI 读取本文件后即可精确生成导航楼层，无需再拼凑多个组件规范。

---

## 涉及组件

| 组件 | 规范文档 | 本楼层使用的变体 |
|------|---------|----------------|
| ONavigation | [components/navigation.md](../components/navigation.md) | type=PC, Dark=off（顶导）/ type=PC, 属性2=openEuler（页脚） |

### 生成流程（强制门禁，三步缺一不可）

> 🛑 本楼层的生成流程固定为以下三步，**禁止跳步、禁止合并、禁止把裸顶导当交付**：
>
> 1. **先生成顶导骨架**（Logo / 菜单项 / 右侧操作区，含下方「菜单项（触发器）完整样式」）
> 2. **停下询问用户**——下拉面板要哪种样式：**平铺式**还是**分类平铺式**（见下表）？**未经用户明确选择，禁止自行决定样式、禁止跳过本步**
> 3. **确认后一并生成全套**——`navigation.md`（顶导）+ 用户选定的下拉面板模板 = **一套完整的导航模式**，作为本楼层唯一交付物，再执行第四节「楼层自检清单」
>
> ⛔ 禁止只生成顶导不生成面板；⛔ 禁止顶导与面板分多轮生成；⛔ 禁止未经用户确认自选面板样式。

**两种面板样式**（第 2 步由用户二选一，确认后读对应模板）：

| 样式 | 模板文件 | 特征 |
|------|---------|------|
| **平铺式** | [floors/navigation-dropdown.md](navigation-dropdown.md) | 垂直列表、选项独占一行、无分类标题 |
| **分类平铺式** | [floors/navigation-dropdown-categorized.md](navigation-dropdown-categorized.md) | 多列平铺：分类列头 + 列内条目（标题 + 说明）+ 快捷链接栏 |

菜单项展开的下拉面板规格（面板几何、选项、自适应规则、交互 JS）以对应模板为准；**触发器状态（默认/Hover/展开/选中/禁用）以本文件为唯一真源**——两份下拉模板不定义、不重定义任何顶导与触发器样式。本文件的「踩坑记录」「自检清单」同样适用于面板生成。同一顶导内两种样式**可混挂**（如「开发」挂分类平铺式、「下载」挂平铺式）：两份模板的面板共享 `.o-nav__dropdown` 基类，单开互斥规则同时覆盖两者。

### 生成前必读（缺一不可；第 2 项在用户确认面板样式后读取）

| 顺序 | 文件 | 必须从中取用的内容 |
|---|---|---|
| 1 | **本文件** `floors/navigation.md` | 顶导结构 / Flexbox 三层对齐策略 / **菜单项（触发器）完整样式**（height 48px、padding-bottom 24px、border-bottom 2px、字号字重颜色、禁止 border-radius）/ 精确数值速查 / 踩坑记录 |
| 2 | 下拉面板模板（按样式二选一）：`floors/navigation-dropdown.md`（平铺式）或 `floors/navigation-dropdown-categorized.md`（分类平铺式） | **仅下拉面板**全部规格（面板几何、选项容器、三态配色、单开互斥 CSS、交互 JS；分类平铺式另有列头/条目说明/快捷链接栏/六档自适应）——⚠️ 两份模板**不定义顶导与触发器样式**，那部分只以本文件为准 |
| 3 | `components/navigation.md` | **Part C 触发器交互状态**（默认/Hover/展开/选中/禁用五态，全部背景透明）——本文件的触发器状态即由它派生 |

> ⚠️ **只读本文件会漏掉面板规格；只读下拉面板模板会漏掉顶导与触发器的全部样式**（唯一定义源在本文件，下拉面板模板明确不重定义）。全套导航统一使用 `.o-nav__*` class 命名，无需映射。

---

## 一、PC 顶导（Header）

### 布局结构

```
ONavigation Header PC（1920px × 72px）
│  display: flex | justify: space-between | align: center
│  padding: 0px 216px
│  background: var(--o-color-fill2)（Light；Dark 由 data-o-theme 切换语义 Token 自动适配）
│  boxShadow: var(--o-shadow-1)（一级阴影·浮层）
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
| L1 外层 | `.o-nav` | `center` | 整体垂直居中平衡，**禁止改为 flex-end** |
| L2 左侧 | `.o-nav__left` | `flex-end` | 让子元素默认贴底（Tab 需要） |
| L2 右侧 | `.o-nav__right` | `center` | 操作图标垂直居中 |

**Logo 与 Tab 的非对称对齐**（核心难点）：

Logo 和 Tab 同在 `.o-nav__left` 内，但需要对齐方式不同：
- **Logo**：需要 `align-self: center` 覆盖父级的 `flex-end`，实现垂直居中
- **Tab 菜单**：继承父级的 `flex-end`，实现贴底对齐

```css
/* ✅ 正确写法 */
.o-nav       { display: flex; align-items: center; justify-content: space-between; padding: 0 216px; }  /* ⚠️ 导航栏用padding: 0 216px是正确的，因为导航栏width:100%且无max-width限制 */
.o-nav__left { display: flex; align-items: flex-end; gap: 40px; }
.o-nav__logo { height: 32px; padding: 20px 0; align-self: center; }  /* ⭐ align-self 覆盖 */
.o-nav__menu { align-self: flex-end; }                                /* ⭐ 贴底 */
.o-nav__right{ display: flex; align-items: center; gap: 20px; }
```

**禁止的做法**：

```css
/* ❌ 绝对禁止！会导致右侧操作区也底部对齐 */
.o-nav { align-items: flex-end; }

/* ❌ 禁止！Logo 也会被拉到底部 */
.o-nav__left { align-items: flex-end; }
/* 但不给 Logo 设置 align-self: center */
```

### 菜单项（触发器）完整样式

> ⚠️ **本段是顶导与触发器（菜单项）的唯一样式来源**。无论下拉面板是平铺式还是分类平铺式，顶导与触发器的样式完全一致——两份下拉模板（[navigation-dropdown.md](navigation-dropdown.md) / [navigation-dropdown-categorized.md](navigation-dropdown-categorized.md)）**只定义下拉面板本身，不定义、不重定义、不改写本段任何样式**。若此处缺失，生成方只能自行编写，极易自创 `border-radius`、字重、背景色等错误属性。**生成顶导（含带下拉面板的顶导）时必须完整使用本段样式。**

**命名约定**：全套导航（顶导 + 下拉面板）统一使用 `.o-nav__*` 命名，本段 CSS 即交付代码的命名，**无需映射、禁止改写法**。下拉面板的 class（`.o-nav__dropdown` 等）见两份面板模板，面板挂在 `.o-nav__item` 节点内。

```css
/* ===== 顶导容器（含阴影与层叠层级）===== */
.o-nav {
    width: 100%;
    height: 72px;
    display: flex;
    justify-content: space-between;
    align-items: center;
    padding: 0 216px;
    background: var(--o-color-fill2);
    box-shadow: var(--o-shadow-1);
    backdrop-filter: blur(4.53px);
    position: sticky;            /* 或 fixed；非定位元素时需显式 position: relative，否则下拉面板 top 无参照 */
    top: 0;
    z-index: 100;                /* 整体浮于页面内容之上 */
    /* ⛔ 顶导容器禁止 overflow: hidden——会裁掉下拉面板（z-index 再高也救不回被裁剪的面板） */
}
/* 阴影补偿层：面板是 .o-nav 的后代，永远画在 .o-nav 的 box-shadow 之上，
   必须用伪元素在 .o-nav 内部以更高 z-index 重画同一道阴影，叠在面板之上。
   ⚠️ 几何必须与 .o-nav 本体等高（top: 0 + height: 100%）——阴影锐利度由源盒子高度决定：
   height: 0 完全不渲染 box-shadow（死代码）、height: 1px 被 9px 模糊稀释成 254 几乎不可见。 */
.o-nav::after {
    content: '';
    position: absolute;
    left: 0; right: 0;
    top: 0;                      /* ⭐ 与 .o-nav 同高，不是贴底的一条线 */
    height: 100%;                /* ⭐ 撑满顶导高度，复刻顶导自身 box-shadow 的强度 */
    box-shadow: var(--o-shadow-1);
    z-index: 102;                /* ⭐ 必须 > 面板的 100 */
    pointer-events: none;
}

/* ===== 左侧区域：Logo + 菜单（Flexbox 三层对齐，见上节）===== */
.o-nav__left  { display: flex; align-items: flex-end; gap: 40px; }
.o-nav__logo  { height: 32px; padding: 20px 0; align-self: center; display: flex; }  /* ⭐ align-self 覆盖父级 flex-end */
.o-nav__logo svg { display: block; }
.o-nav__menu  { display: flex; gap: 32px; list-style: none; align-self: flex-end; }  /* ⭐ 贴底 */

/* ===== 菜单项（触发器）===== */
.o-nav__item {
    position: relative;          /* 下拉面板定位锚点（搭配面板模板时必需） */
    align-self: flex-end;        /* 贴底对齐，继承 .o-nav__left 的 flex-end */
}
.o-nav__link {
    display: flex;
    align-items: center;
    height: 48px;                /* 菜单项高 48px（宽由文字撑开，auto） */
    padding: 0 4px 24px;         /* 左右各 4px；底部 24px 留给选中指示线 */
    font-size: 16px;
    line-height: 24px;
    font-weight: 400;            /* ⛔ 禁止改 SemiBold——规范未定义字重变化 */
    color: var(--o-color-info1);
    text-decoration: none;
    white-space: nowrap;
    /* ⛔ 绝对禁止 border-radius：菜单项所有状态背景一律透明（Part C），
       圆角无任何视觉效果，却会把 border-bottom 指示线两端裁成圆弧缺口。
       border-radius: 4px 只属于 .o-nav__search，禁止搬到菜单项上 */
    border-bottom: 2px solid transparent;   /* 默认透明指示线 */
    transition: color .2s ease, border-color .2s ease;
}

/* Hover / 展开态：仅文字变 primary2，背景透明（Part C，2026-10-15 用户统一裁决两种面板样式通用） */
.o-nav__link:hover,
.o-nav__item:focus-within > .o-nav__link,
.o-nav__item[data-open="true"] > .o-nav__link {
    color: var(--o-color-primary2);
}

/* 选中态（当前页）：底部 2px 品牌蓝指示线 + 文字加深（Part C）；
   选中优先于 hover——is-current 项被 hover / 展开时仍保持 primary1 + 指示线 */
.o-nav__item.is-current > .o-nav__link,
.o-nav__item.is-current:hover > .o-nav__link,
.o-nav__item.is-current:focus-within > .o-nav__link,
.o-nav__item.is-current[data-open="true"] > .o-nav__link {
    color: var(--o-color-primary1);
    border-bottom-color: var(--o-color-primary1);   /* 完整一根水平直线，无圆弧 */
}

/* ===== 右侧操作区 ===== */
.o-nav__right { display: flex; align-items: center; gap: 20px; }
.o-nav__search {
    position: relative; width: 160px; height: 32px;
    border: 1px solid var(--o-color-control1); border-radius: 4px;
    background: var(--o-color-fill2); overflow: hidden; cursor: text;
}
.o-nav__search span {           /* 占位文字：在搜索图标【右侧】left = 16px 左内边距 + 16px 图标 + 8px gap = 40px */
    position: absolute; left: 40px; top: 50%; transform: translateY(-50%);
    font-size: 14px; line-height: 22px; color: var(--o-color-info4); pointer-events: none;
}
.o-nav__search svg {            /* 搜索图标在【左侧】；⛔ 严禁 right */
    position: absolute; left: 16px; top: 50%; transform: translateY(-50%);
    width: 16px; height: 16px; pointer-events: none;
}
.o-nav__source {                /* 源码下拉 = ODropdown text 变体 */
    display: flex; align-items: center; gap: 8px; min-width: 48px; height: 24px;
    font-size: 14px; line-height: 22px; color: var(--o-color-info1);
    white-space: nowrap; cursor: pointer;
}
.o-nav__source svg { width: 16px; height: 16px; flex: none; }
.o-nav__icon { width: 24px; height: 24px; display: flex; align-items: center; justify-content: center; cursor: pointer; }
.o-nav__icon svg { width: 24px; height: 24px; display: block; }
```

> ⚠️ **选中态必须可切换**：`is-current` 表示「当前页」，初始写在某一菜单项上，但**点击其他菜单项时必须迁移过去**（先移除其他项的 `is-current`，再添加到点击项）。单选、**不做 toggle**——无选中项会让顶导丢失「当前位置」信息。JS 最小实现见 [navigation-dropdown.md](navigation-dropdown.md) §八。

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
| **菜单项 border-radius** | **禁止（0 / 不设置）** | ⚠️ 本文档（见下方说明） |
| 菜单项间距 | 32px | navigation.md |
| Logo 与菜单间距 | 40px | navigation.md |
| 右侧操作区间距 | 20px | navigation.md |
| 搜索框尺寸 | 160px × 32px | navigation.md |
| 操作图标尺寸 | 24px × 24px | navigation.md |
| 菜单文字字号 | 16px / 24px | 硬约束 §2（常规正文） |
| 源码/搜索占位字号 | 14px / 22px | 硬约束 §2（提示文本1） |
| **顶导阴影** | **`var(--o-shadow-1)`（`0 3px 9px 0 rgba(0,18,85,0.078)`）** | components/navigation.md |
| **顶导 z-index** | **100**（整体浮于页面内容之上） | 本文档 |
| **阴影补偿层（`.o-nav::after`）z-index** | **102**（必须 > 面板的 100，否则阴影被面板整条盖住） | ⚠️ 本文档（2026-10-15 用户验收两次打回后补充） |

> ⛔ **菜单项禁止 `border-radius`**：顶导菜单项所有状态背景一律透明（见 Part C），圆角没有任何视觉效果；但 `border-radius` 会把选中态的 `border-bottom` 指示线**两端裁成圆弧缺口**，导致指示线不是一根完整直线。规范中 `border-radius: 4px` 只属于**搜索框**（`.o-nav__search`），禁止搬到菜单项上。

### 资源引用

所有 SVG（Logo 与图标）统一按 SKILL.md「图标处理规范」处理：**Read 资源文件后逐属性原样内联 `<svg>`**（保留 `fill-rule` / `clip-rule` / `opacity` / `transform` 等全部属性），颜色用 `currentColor` 或 `var(--o-color-*)`。禁止 `<img src>`、CSS mask 或色块占位。

| 用途 | 路径（Read 后逐属性内联） |
|------|--------------------------|
| PC Light Logo | `references/assets/navigation/header-pc-light logo.svg` |
| PC Dark Logo | `references/assets/navigation/header-pc-dark logo.svg` |
| 搜索图标 | `references/assets/navigation/header-pc-search icon.svg` |
| 国际化图标 | `references/assets/navigation/header-pc-语言切换 icon.svg` |
| 主题切换图标 | `references/assets/navigation/header-pc-深浅模式.svg` |
| 用户图标 | `references/assets/navigation/header-pc-头像.svg` |
| 源码下拉箭头 | `references/assets/public icons/icon-下箭头.svg`（24×24，`fill="currentColor"` 或 `var(--o-color-info1)`，**保留 `transform` 属性**） |

> assets 中无对应资源时，才允许手写线性风格 SVG 兜底并注释「assets 中无资源」（见 SKILL.md「SVG 来源优先级」）；禁止凭记忆手写 path 模拟 assets 已有图标的外观。

### 颜色（全量语义 Token，禁止硬编码 rgba / hex）

导航栏颜色一律使用语义 Token，深浅模式由 `data-o-theme` 切换自动适配（Token 同名翻转），**禁止为 Dark 另写一套色值**：

| 区域 | Token |
|------|-------|
| 背景 | `var(--o-color-fill2)`（纯白容器背景，dark 自动翻转） |
| 菜单文字 | `var(--o-color-info1)` |
| 搜索占位文字 | `var(--o-color-info4)` |
| 搜索框边框 | `var(--o-color-control1)`（默认边框） |
| 搜索框背景 | `var(--o-color-fill2)` |
| 源码下拉文字 | `var(--o-color-info1)`（ODropdown text 变体，dropdown.md Part B：文字与图标均为 info1） |
| **搜索图标** | `var(--o-color-info4)`（`rgba(0,0,0,.4)`，见下方说明） |
| **国际化 / 主题切换 / 用户图标** | `var(--o-color-info2)`（`#333333`，见下方说明） |
| 阴影 | `var(--o-shadow-1)`（一级阴影·浮层） |

> 🚨 **顶导阴影必须可见——但不能靠调 z-index 实现**（2026-10-15 用户验收两次打回后确认）：下拉面板 DOM 上位于顶导**内部**，是 `.o-nav` 的**后代**。CSS 绘制规则里**子元素永远画在父元素的 box-shadow 之上**，且 `z-index` **只在同一父级的定位兄弟之间比较**——父子之间比 z-index 毫无意义（给顶导设 101、面板设 100 完全无效）。所以只要面板挂在顶导内部，顶导的下阴影必然被面板整条盖住，表现为交界处"平贴"、没有悬浮感。
>
> **唯一可靠解法 = 用伪元素在 `.o-nav` 内部以更高 z-index 重画同一道阴影**（伪元素与面板同为 `.o-nav` 的后代，z-index 可比）：
>
> ```css
> .o-nav { position: sticky; z-index: 100; box-shadow: var(--o-shadow-1); }
> /* 阴影补偿层：与 .o-nav 自身 box-shadow 同值，叠在面板之上 */
> .o-nav::after {
>     content: '';
>     position: absolute;
>     left: 0; right: 0;
>     top: 0;                    /* ⭐ 与 .o-nav 同高，不是 top: 100% 的一条线 */
>     height: 100%;              /* ⭐ 必须与 .o-nav 等高，见下方「几何陷阱」 */
>     box-shadow: var(--o-shadow-1);
>     z-index: 102;              /* ⭐ 必须 > 面板的 100，才能把阴影画在面板之上 */
>     pointer-events: none;
> }
> ```
>
> ⚠️ **两个 z-index 数字都不能省**：`.o-nav` 的 `z-index: 100` 让它整体浮在页面内容之上（无面板时下阴影正常显示）；`::after` 的 `z-index: 102` 让补偿阴影盖过面板。补偿层自身无背景、无边框，只有 border-box 之外的阴影会画出来（落在 y > 72px 的面板上），不会遮挡顶导内的 Logo / 菜单 / 图标。
>
> 🚨 **补偿层的几何陷阱（2026-10-15 实测，此前规范写错并已导致两次返工）**——阴影的**锐利度由源盒子高度决定**，不能图省事写成一条贴底的细线：
>
> | 写法 | 接缝处实测像素 | 结论 |
> |---|---|---|
> | `top: 100%; height: 0` | `255,255,255` | ❌ **零高度元素的 box-shadow 完全不渲染**，整条补偿规则是死代码。原规范的"零高度线让阴影范围逐像素重合"是错的推理 |
> | `top: 100%; height: 1px` | `254,254,254` | ❌ 源盒子太矮，9px 模糊把阴影摊成弥散斑，几乎看不见 |
> | `top: 0; height: 100%` | `241,241,245` | ✅ 与顶导自身阴影的有效透明度一致（0.055 vs 0.057），正确 |
>
> ⚠️ **阴影被面板盖住 ≠ 没写 box-shadow**：排查"顶导没有阴影"时，先确认 `box-shadow: var(--o-shadow-1)` 是否写了（规范要求写），再判断是"样式缺失"还是"被后代盖住"——前者补样式，后者加补偿层，二者修法不同。
>
> ⚠️ **两个反直觉特性，改完必须实测**：① 父级 `z-index` 提得再高也盖不住自己的后代；② `height: 0` 的元素不渲染 `box-shadow`。这两条都违反直觉，**任何"调 z-index / 加细线"的修法都不要凭信心下结论，用无头浏览器截图 + 单列像素采样验证**。

> ⚠️ **四个图标的填充色互不相同，禁止统一用 info1**（以 assets 资源文件实际填充值为准，见下方对照）：
>
> | 图标 | Light 色 | Dark 色 |
> |---|---|---|
> | 搜索图标（16×16） | `rgba(0,0,0,.4)` = **info4** | `rgba(255,255,255,.4)` = info4-dark |
> | 国际化（24×24） | `rgb(0,0,0)` @80% = **info2**（`#333333`） | `rgba(255,255,255,.85)` = info1-dark |
> | 主题切换（24×24） | `rgb(0,0,0)` @80% = **info2**（`#333333`） | `rgba(255,255,255,.85)` = info1-dark |
> | 用户头像（24×24） | `rgb(0,0,0)` @80% = **info2**（`#333333`） | `rgba(255,255,255,.85)` = info1-dark |
>
> ⚠️ **图标填充色以 `references/assets/navigation/` 资源文件实际填充值为准**，components/navigation.md Part F 的「顶导 Icon SVG」表为辅。实际资源填充为 `rgb(0,0,0)` + `fill-opacity="0.8"`（80% 黑 = `#333333` = info2），文档表格与资源文件冲突时以资源文件为准。
>
> ⚠️ 资源 SVG 内的 `fill-opacity="0.8"` 是切图残留，内联时**必须改为对应语义 Token 并去掉 fill-opacity**（Token 本身已含透明度），否则深浅色模式无法自适应。

#### 顶导所需语义 Token 完整清单（可直接抄入 `:root`）

> ⛔ **Token 必须先定义再使用**。引用**未定义**的 `var(--o-color-xxx)` 时，该 `fill` / `color` / `background` 声明**整体无效**——SVG `<path>` 会回退继承 `<svg fill="none">` 导致**图标完全不可见**（元素还在 HTML 里，但看不见）。
>
> ⚠️ **每次修改色值后，必须回头核对 `:root` 是否定义了该 Token**——这是最隐蔽的"元素凭空消失"故障。

```css
:root {
    --o-color-fill1: #F5F5F5;             /* 页面灰底 */
    --o-color-fill2: #FFFFFF;             /* 顶导 / 面板 / 输入框背景 */
    --o-color-info1: #000000;             /* 主文字、菜单默认文字 */
    --o-color-info2: #333333;             /* 次要文字；国际化/主题切换/用户图标 fill */
    --o-color-info3: #666666;             /* 页脚次要文字（顶导不使用） */
    --o-color-info4: #999999;             /* 占位文字、搜索图标 */
    --o-color-control1: rgba(0,0,0,0.25); /* 搜索框默认描边 */
    --o-color-control2-light: #F0F5FF;    /* 菜单项 Hover 背景（极浅蓝） */
    --o-color-control3-light: #E6EDFF;    /* 菜单项 Active/Press 背景（浅蓝） */
    --o-color-primary1: #002FA7;          /* 品牌蓝：选中指示线、选中态文字 */
    --o-color-primary2: #5177CA;          /* 品牌蓝次级：菜单项 hover 文字色（两种面板样式一致） */
    --o-color-white: #FFFFFF;
    --o-shadow-1: 0 3px 9px 0 rgba(0,18,85,0.078);  /* 顶导 boxShadow */
}
```

> Dark 模式：同名 Token 追加 `-dark` 后缀（如 `--o-color-info1-dark: #FFFFFF`），或由 `data-o-theme` 统一翻转。完整 Light/Dark 值见 [components/navigation.md](../components/navigation.md) Appendix A。

> 组件特例：ONavigation 的 dark 主题特例色、Dark 底部 1px 白色描边（rgba(255,255,255,0.149)）、MB 顶导半透明背景 rgba(255,255,255,0.95) + `backdrop-filter: blur(4.53px)`——上游语义 Token 无对应变量，仅当拉取的目标主题 JSON / components/navigation.md 给出对应 Token 时才可使用；否则保留规格值并加 HTML 注释「组件特例值，来源 components/navigation.md」，禁止扩散到其他区域。

### 搜索框 / 源码下拉实现要点

> CSS 已整段并入上方「菜单项（触发器）完整样式」（`.o-nav__search` / `.o-nav__source`），此处只列极易出错的实现要点：

- **搜索框必须有边框 + 白色背景**（`border: 1px solid var(--o-color-control1)` + `background: var(--o-color-fill2)`），否则不可见
- **搜索框内部顺序固定为「图标 → 文字」**（左到右）：图标 `left: 16px`、文字 `left: 40px`（= 16px 左内边距 + 16px 图标 + 8px gap），HTML 里图标 `<svg>` 也须写在文字 `<span>` 前面；⛔ 严禁给图标设 `right`——会把它推到占位文字右边
- **搜索框内部用绝对定位**（子元素 `position: absolute`），不用 flex 布局
- **搜索图标填充色 = `var(--o-color-info4)`**（`rgba(0,0,0,.4)`）。禁止沿用其他图标的 `info1` + `fill-opacity: 0.8`——搜索图标比源码箭头/国际化/主题/用户图标更浅，是设计规范要求
- **源码下拉宽度 = `auto`**（不是固定 48px，中文「源码」需要约 52px），`min-width: 48px` + `white-space: nowrap` 禁止换行
- **源码下拉字号/行高 = 14px/22px**（成对锁定，hard-constraints §2，禁止配 24px）；文字与箭头均为 `var(--o-color-info1)`（ODropdown text 变体，dropdown.md Part B）

---

## 二、MB 顶导（Header）

### 布局结构

```
ONavigation Header Mb（360px × 76px）
│  position: relative
│  background: rgba(255,255,255,0.95)（组件特例值，见「颜色」节说明）
│  boxShadow: var(--o-shadow-1)（一级阴影·浮层）
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
| 1 | Tab菜单垂直居中，下划线不贴底 | 外层 `.o-nav` 误设 `align-items: flex-end`，或左侧区域未设 `align-items: flex-end` | 外层 `center` + 左侧 `flex-end` + Logo `align-self: center` + 菜单 `align-self: flex-end` |
| 2 | Logo 被拉到底部，不居中 | `.o-nav__left` 设了 `flex-end` 但 Logo 没有覆盖 | Logo 必须加 `align-self: center` |
| 3 | 右侧操作区图标底部对齐 | 外层 `.o-nav` 误设 `align-items: flex-end` | 外层必须 `center`，右侧 `align-items: center` |
| 4 | 搜索框不可见（无框无背景） | 缺少 border 和 background-color | 必须设 `border: 1px solid var(--o-color-control1)` + `background-color: var(--o-color-fill2)` |
| 5 | 搜索框内文字/图标位置错乱 | 使用 flex 布局而非绝对定位 | Light 模式搜索框必须用 `position: relative` + 子元素 `position: absolute` |
| 6 | "源码"文字与箭头换行 | 固定 width: 48px 不够容纳中文 | `width: auto` + `white-space: nowrap` |
| 7 | 右侧图标显示为凭记忆手绘的 SVG | 生成时凭想象写 path 模拟图标 | 必须从 `references/assets/navigation/` Read 资源后逐属性原样内联 `<svg>`（禁止 `<img>` 引用、禁止凭记忆造 path） |
| 8 | CSS 样式被覆盖不生效 | `<style>` 中样式优先级不足 | 提高选择器优先级；确属宿主样式覆盖时允许**带注释的 !important**（硬约束「布局强制要求」豁免场景），禁止无注释滥用、禁止内联 style |
| 9 | **选中态指示线两端被裁成圆弧/截断，或选中态只有蓝字没有指示线** | ① 给 `.o-nav__link` 加了 `border-radius: 4px`（搜索框的圆角被误搬到菜单项上）；② 选中态只写了 `color: primary1`，漏写 `border-bottom-color` | ⛔ 菜单项**禁止** `border-radius`——触发器背景一律透明，圆角无视觉效果，却会把 `border-bottom` 指示线两端裁出弧形缺口；选中态必须**同时**写 `color` + `border-bottom-color: var(--o-color-primary1)`，指示线必须是一根完整水平直线 |
| 10 | **选中态点不动，始终停留在初始项** | `is-current` 只写死在 HTML 上，JS 未处理 | 点击菜单项时把 `is-current` 迁移到该项（先移除其他项）；单选、**不做 toggle**——无选中项会让顶导丢失「当前位置」信息 |
| 11 | **搜索图标跑到占位文字右边** | 凭直觉给 `.o-nav__search svg` 设了 `right: 12px` | ⛔ 搜索图标必须在**左侧**（OSearch small 规范「图标 位置：左侧；占位文字 位置：图标右侧」）。图标 `left: 16px`、文字 `left: 40px`（16+16+8），HTML 里图标也须写在文字前面 |
| 12 | **四个图标填充色统一用了 info1** | 未查图标色表，直接沿用信息级主色 | 四个图标色互不相同：搜索=info4、国际化/主题/用户=**info2**、源码箭头=info1。**图标颜色以 assets 资源文件实际填充值为准**；资源 SVG 的 `fill-opacity="0.8"` 是切图残留，内联时须换成语义 Token 并去掉 fill-opacity |
| 13 | **右侧图标全部消失（HTML 还在但看不见）** | 把图标 fill 从 info1 改成 info3，但 `:root` 里**没有定义 `--o-color-info3`** → `var()` 无效 → path 回退继承 `<svg fill="none">` → 完全不可见 | ⛔ **Token 必须先定义再使用**；每次改色值后必须回头核对 `:root`（见「顶导所需语义 Token 完整清单」）。这是"元素凭空消失"类故障的最高频根因 |
| 14 | **三个右侧图标填成 info3（偏浅）** | 照抄组件文档的图标色表，未核对资源文件实际填充值 | 实际资源为 `rgb(0,0,0)` + `fill-opacity="0.8"` = 80% 黑 = **info2**（`#333333`）。文档表格与资源文件冲突时，**以资源文件为准** |
| 15 | **顶导看起来没有阴影（与面板交界处"平贴"）** | `box-shadow` 写了，但面板是 `.o-nav` 的后代，**子元素永远画在父级 box-shadow 之上**，整条盖住下阴影。⚠️ 曾经的错误修法：给顶导设 `z-index: 101`、面板 100——**完全无效**，z-index 只在同一父级的定位兄弟之间比较，父子之间毫无意义（2026-10-15 实测：顶导 101 时接缝处仍是纯白 255） | 用 `.o-nav::after` 补偿层在顶导内部重画同一道阴影（`z-index: 102` > 面板 100）。⚠️ **补偿层必须 `top: 0` + `height: 100%` 与顶导等高**——阴影锐利度由源盒子高度决定：`height: 0` 不渲染阴影（死代码）、`height: 1px` 被 9px 模糊稀释成 254 几乎不可见、等高才复刻顶导阴影强度（实测 0.055 vs 基准 0.057）。⚠️ 这是"遮挡"不是"样式缺失"——先确认 `box-shadow` 在不在，再查是被谁盖住 |
| 16 | **下拉面板被顶导裁掉一截（怎么调 z-index 都不显示）** | 顶导容器设了 `overflow: hidden`（常见于想"防止内容溢出"） | ⛔ 顶导容器**禁止 `overflow: hidden`**——裁剪发生在 z-index 层叠之前，层级再高也救不回被裁掉的面板 |
| 17 | **hover 菜单项文字变粗；无下拉面板的普通菜单项连 hover 态都没有** | ① 给触发器 hover / 选中态写了 `font-weight: 600`（2026-10-14 设计稿标注 Semibold，2026-10-15 用户否决）；② 状态规则用 `:has(> .o-nav__dropdown--categorized)` 收窄，普通菜单项丢失 hover 变色与 `is-current` 指示线 | ① ⛔ 触发器**全状态 Regular(400)**（本文件明令禁止改 SemiBold）——字重随交互切换会产生肉眼可见的文字跳动，hover 只改 `color`；② ⛔ hover / `is-current` 基础态**禁止 `:has()` 收窄**，必须覆盖全部菜单项；两份下拉模板只写面板，**不再追加任何触发器差异项**（hover 色 primary2 已统一为骨架默认值，见「菜单项（触发器）完整样式」） |
| 18 | **触发器展开态 / 选中态出现背景色块**（展开态自创 `control3-light` 浅蓝底 / `primary1` 实心蓝底白字） | 误以为"展开"需要强调背景；components/navigation.md Part C 的默认/Hover/Active 三态**全部背景透明**，展开态规范未单独定义 | ⛔ 触发器**任何状态**（默认/hover/展开/选中/禁用）都不允许背景色块，禁止引用 control2/control3/primary1 等任何背景 Token；展开态视觉同 Hover（仅文字 `primary2`） |
| 19 | **hover 给选中项加了第二条下划线 / 把指示线冲掉** | hover 规则里写了 `border-bottom`，与选中态规则互相覆盖 | hover / 展开态**只写 `color`**，不碰 `border-bottom`、不碰 `font-weight`；选中优先——`is-current` 项被 hover / 展开时保持 `primary1` + 2px 指示线（CSS 见「菜单项（触发器）完整样式」的 is-current 优先级选择器） |
| 20 | **顶导整体歪斜：Logo 偏低、菜单文字偏高、选中下划线悬在导航栏中间（不贴底）** | 给 `.o-nav__logo` 写了 `height: 32px`，而页面有全局 `* { box-sizing: border-box }` → 32px 变成**含 padding 的总高**（本意是内容高）→ Logo 实际仅 32px，`.o-nav__left` 跟着只剩 48px 高。连锁后果：① 菜单 `align-self: flex-end` 贴到 y=60 而非导航底边 y=72，指示线落在导航栏正中；② Logo `align-self: center` 在 48px 内居中 → 偏到 y=22~54，视觉「沉底」；③ 与外层 72px 导航叠加后整组左区块视觉错位 | ⛔ **Logo 禁止写 `height`**——不写高度，由 SVG 自身 32px + `padding: 20px 0` = **72px 总高**（与导航等高），`.o-nav__left` 才是 72px，菜单才能贴到导航栏底边、Logo 才能在 72px 内居中。⚠️ 根因是**盒模型冲突**：验收页/宿主页常带全局 `box-sizing: border-box`，凡依赖「height + padding 相加得总高」的节点（Logo 32+20+20、菜单项 48+24）都会被压扁。生成时先查页面是否有全局 `box-sizing`，有则该类节点一律**不写显式 height**，让内容或 padding 自然撑出目标总高 |

---

## 四、楼层自检清单

```
□ 【流程门禁】下拉面板样式是否经用户明确选择后生成——全套 = 本文件顶导 + 用户选定的 navigation-dropdown / navigation-dropdown-categorized（⛔ 禁止裸顶导交付、⛔ 禁止未经确认自选样式、⛔ 禁止顶导与面板分多轮生成）？
□ 顶导高度是否为 PC 72px / MB 76px？
□ 左右内边距是否为 PC 216px / MB 24px？
□ Logo 是否从 assets Read 后逐属性内联 <svg>（禁止 <img> / 凭记忆手绘）？
□ Logo 是否设置 align-self: center（在 flex-end 父容器中居中）？
□ Logo 是否【未】写 height（禁写显式高度：须由 SVG 32px + padding 20px 0 撑出 72px 总高与导航等高；若页面有全局 box-sizing: border-box，写 height:32px 会把 Logo 压成 32px、连带左区块只剩 48px，导致菜单不贴底、指示线悬在导航栏正中——见踩坑 #20）？
□ 生成前是否确认页面是否存在全局 box-sizing: border-box，并对所有「height + padding 相加得总高」的节点改为不写显式 height（踩坑 #20）？
□ 菜单项间距是否为 PC 32px？
□ 菜单项是否设置 padding-bottom: 24px + border-bottom？
□ 菜单选中下划线是否贴导航栏底部？
□ 选中态指示线是否为一根完整水平直线（菜单项未设置 border-radius，两端无圆弧缺口）？
□ 点击菜单项时选中态是否随之切换（is-current 迁移，单选不做 toggle）？
□ 搜索图标是否在【左侧】（图标 left:16px、占位文字 left:40px，HTML 中图标在前）？
□ 搜索图标填充色是否为 info4（非 info1、无 fill-opacity 0.8）？
□ 国际化/主题切换/用户图标填充色是否为 info2（80% 黑 = #333333，以 assets 资源文件实际填充值为准）？
□ 是否引用了未定义的语义 Token（改色值后须回头核对 :root 清单，否则元素会完全不可见）？
□ 右侧操作区间距是否为 PC 20px？
□ 右侧操作区是否设置 align-items: center？
□ 操作图标尺寸是否为 24px × 24px？
□ 操作图标是否全部从 assets Read 后逐属性内联 <svg>？
□ Dark 模式底部是否有 1px 白色描边？
□ 搜索框是否有 border + background-color（否则不可见）？
□ 搜索框内部是否用绝对定位（而非 flex 布局）？
□ 搜索框尺寸是否为 160px × 32px？
□ 主题切换图标是否未遗漏？
□ 源码按钮是否设置 white-space: nowrap（禁止换行）？
□ 源码按钮宽度是否为 auto（而非固定48px）？
□ 源码按钮是否为 ODropdown text 变体（无背景无描边，颜色 var(--o-color-info1)，dropdown.md Part B：text 变体文字与图标均 info1）？
□ ⚠️ 外层 .o-nav 是否保持 align-items: center（禁止改为 flex-end！）？
□ ⚠️ 左侧 .o-nav__left 是否设置 align-items: flex-end？
□ 顶导是否设置 box-shadow: var(--o-shadow-1)（一级阴影·浮层，顶导悬浮感的唯一来源）？
□ 顶导下阴影是否【可见】——有下拉面板时必须写 .o-nav::after 补偿层（top: 0 + height: 100% 与顶导等高 + z-index: 102 > 面板 100）？⛔ 不许靠"顶导 z-index 101 > 面板 100"（父子之间比 z-index 无效，实测接缝处仍纯白）· ⛔ 补偿层不许写 height: 0（零高度不渲染 box-shadow）或 height: 1px（被模糊稀释，几乎不可见）？
□ 触发器全状态字重是否统一 Regular(400)——hover / 展开 / 选中均不碰 font-weight（⛔ 禁止切 Semibold：字重随交互切换会产生肉眼可见的文字跳动）？
□ 触发器 hover / 展开态是否仅文字变 var(--o-color-primary2)（无下划线、无背景、不变粗）？
□ 触发器任何状态（默认/hover/展开/选中/禁用）是否都无背景色块（⛔ 禁止 control2/control3/primary1 等任何背景 Token）？
□ 全部菜单项（含无下拉面板的普通项）是否都有 hover 变色与 is-current 指示线（⛔ 禁止用 :has() 把 hover / is-current 基础态收窄成只有挂面板的项才有）？
□ 选中态被 hover 时是否保持 primary1 + 指示线（选中优先，hover 不抢回 primary2）？
□ 顶导与触发器样式是否整段取自本文件「菜单项（触发器）完整样式」（.o-nav__* 命名，未改写法、未让下拉面板模板重定义）？
□ 顶导容器是否【没有】overflow: hidden（会裁掉下拉面板，z-index 再高也救不回）？
□ ⚠️ 被宿主覆盖的关键对齐属性是否仅用带注释的 !important（无内联 style）？
```