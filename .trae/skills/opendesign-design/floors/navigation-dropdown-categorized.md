← [楼层模板索引](../SKILL.md#楼层模板索引) · [组件规范](../components/navigation.md)

# 导航下拉面板楼层模板（分类平铺式）

> 本模板为自包含生成单元。AI 读取本文件后即可精确生成「顶导菜单项点击/hover 展开的**分类平铺式**下拉面板（mega menu）」，无需再拼凑多个组件规范。
>
> 🔗 **搭配关系**：本模板**不单独构成页面楼层**，必须与 [floors/navigation.md](navigation.md)（顶导 Header）配合生成——顶导由 navigation 楼层产出，下拉面板由本模板产出，二者挂在同一个菜单项节点上。组件规格以 [components/navigation.md](../components/navigation.md) 为准，本模板只补充「顶导菜单项 → 分类平铺面板」这一层规范。
>
> ⚠️ **两种下拉面板样式分工**（同一菜单结构，不同菜单项可挂不同样式，禁止混用 class）：
>
> | 样式 | 模板文件 | 特征 |
> |------|---------|------|
> | **平铺式** | [floors/navigation-dropdown.md](navigation-dropdown.md) | 垂直列表、选项独占一行、无分类标题 |
> | **分类平铺式** | **本文件** | 多列平铺：分类列头 + 列内条目（标题 + 说明）+ 快捷链接栏 |
>
> 🔴 **顶导与触发器（菜单项）的全部样式唯一定义源是 [floors/navigation.md](navigation.md)**（`.o-nav__*` 命名，含容器 / Logo / 菜单 / 触发器五态 / 右侧操作区，可整段取用）：本模板**不定义、不重定义、不改写任何顶导与触发器样式**——顶导样式与下拉面板类型无关，两种面板样式下完全一致。禁止自创 `border-radius`（会把选中指示线两端裁成圆弧）、字重变化、背景色块，相关禁令与踩坑见 navigation.md 踩坑 #9/#17/#18/#19。
>
> ⚠️ **全套导航统一使用 `.o-nav__*` 命名，无需映射**：顶导骨架（`.o-nav` / `.o-nav__left` / `.o-nav__logo` / `.o-nav__menu` / `.o-nav__item` / `.o-nav__link` / `.o-nav__right` / …）整段取自 navigation.md「菜单项（触发器）完整样式」；面板 class 中，`.o-nav__dropdown` 为两种样式共用的基类（定位/显隐/互斥/JS 由 navigation-dropdown.md 定义），`.o-nav__dropdown--categorized` 为**仅本模板**的分类平铺式修饰类。

---

## 生成前必读（按顺序读取，缺一不可）

| 顺序 | 文件 | 必须从中取用的内容 |
|---|---|---|
| 1 | **本文件** | 分类平铺式全部规格（多列结构、列头/条目/快捷链接栏、六档自适应、MB 双列、截断规则） |
| 2 | `floors/navigation.md` | 顶导结构 / Flexbox 三层对齐 / **菜单项（触发器）完整样式**（`.o-nav__*` 命名，基础几何 + 五态 + 禁止 border-radius，可整段取用） |
| 3 | `floors/navigation-dropdown.md` | **`.o-nav__dropdown` 基类的全部共享规则**：面板定位（top calc(100% + 4px)、居中触发器）、显隐三规则（hover / focus-within / data-open）、单开互斥 `:has()`、4px 热区桥接、MB 左对齐、交互 JS（⚠️ 本模板的 top 改为 100% 零间距、热区桥接关闭，见 §一差异表） |
| 4 | `components/navigation.md` | Part C 触发器交互状态（默认/Hover/Active）——顶导与触发器样式以其派生结果 `floors/navigation.md` 为唯一真源，**本文件不派生、不重定义** |

> ⚠️ **只读本文件会漏掉基类共享规则（定位/显隐/互斥/JS）；只读 navigation-dropdown.md 会漏掉多列布局规格；两份都不含顶导与触发器样式**（唯一定义源是 floors/navigation.md，全套统一 `.o-nav__*` 命名，无需映射）。

> 🚨 **生成后必须实测，不许"写完就交"**（2026-10-15 多次返工换来的教训）：本楼层历史上所有翻车点——面板溢出视口、阴影看不见、图标消失、菜单掉到 Logo 下方、层级互相遮挡、hover 字重跳动、普通菜单项丢 hover 态——**没有一个是"读规范能读出来的"，全部只有渲染出来才看得见**。规范里已沉淀的 44 条踩坑，每一条都来自"生成了但没验证"。
>
> **交付前最低限度的自检（一条都不能省）**：
>
> 1. **用无头浏览器截图**（`chrome --headless --screenshot` / Playwright），窗口宽度至少覆盖 **1920 / 1440 / 1200 / 840 / 600** 五档，确认每档都不溢出、不塌陷
> 2. **交界处逐像素采样**：顶导与面板接缝（y ≈ 72px）取一列无文字区的像素，确认阴影渐变存在（应从约 241 渐到 255）；**纯白 255 = 阴影没画出来**
> 3. **hover / 点击态截图**：仅截图默认态会漏掉全部交互样式问题
> 4. **浏览器控制台无报错**：`:root` 里 Token 未定义会导致 `var()` 整体无效（元素凭空消失），控制台通常会提示
>
> ⚠️ **两条反直觉特性，靠"看代码"永远发现不了，只有实测能抓到**：① 父级 `z-index` 提得再高也盖不住自己的后代（`box-shadow` 随父元素自身绘制，先于一切后代）；② `height: 0` 的元素**不渲染** `box-shadow`。**改完这两类问题必须重新截图验证，不许凭信心下结论。**

---

## 〇、模板定位：分类平铺式是什么

**适用场景**：有层级分类、但条目数量较多时使用（每个顶导菜单项下挂多个分类，每个分类下多个条目）。

**桌面端结构**（≥1681px，以 OpenEuler 为例）：

- 浮层面板贴于顶栏下方，主体为**多列平铺**：**列头（分类）** + **列内条目（标题 + 说明）**
- 典型列数：开发 Tab 为 **5 列**（贡献社区 / 构建 / 发布 / 分析 / 项目）；社区 Tab 为 **4 列 + 右侧快捷链接栏**
- 条目内元素：标题右侧可带**外链图标（↗）**；标题后可挂**红色小标签（BETA / 新）**
- 列底部可有 **「更多项目 >」** 入口（= OButton `text` + `large`，右箭头 24×24 取自 assets）；**快捷链接栏**为**可选模块**——看需求挂载，**默认不出现**
- 字号分档：**>1440px** 分类列头 16px / 选项标题 16px / 选项说明 12px；**≤1440px** 分类列头 14px / 选项标题 14px / 选项说明仍 12px；列头与选项标题颜色两档统一 info4 / info1

```
ONavigation 顶导（见 floors/navigation.md，高 72px）
│
├── [Logo] [导航菜单 FRAME]
│              │
│              └── [菜单项 × N]← 触发器（完整样式见 floors/navigation.md）；挂分类平铺面板时 position: static（包含块上移到顶导，见本节差异表）
│                   │  data-open="true" 时展开面板
│                   │
│                   └── [分类平铺式下拉面板]（position: absolute, top: 100%，相对顶导本体）
│                       │  ⭐ 与顶导【零间距】——像抽屉一样紧贴顶导底边展开（2026-10-15 用户确认，覆盖平铺式的 4px 间距）
│                       │  display: flex | flex-direction: row | flex-wrap: wrap
│                       │  left: 0 | right: 0 | width: auto（⭐ 通栏撑满页面宽度）
│                       │  padding: 24px 216px（上下 24px / 左右 216px = PC 栅格边距）
│                       │  background: var(--o-color-fill2) | border-radius: 0 0 4px 4px（上圆角压平，零间距贴合）
│                       │
│                       ├── [分类列 × N]（.o-nav__cat，flex: none，列间 gap 24px）
│                       │     ├── [列头]（.o-nav__cat-title）>1440px 16px/24px · ≤1440px 14px/22px / Regular / info4，无交互
│                       │     └── [条目列表]（.o-nav__cat-list）flex column，条目间上下 gap 24px
│                       │            └── [条目 × N]（.o-nav__cat-item）
│                       │                  ├── [标题] >1440px 16px/24px · ≤1440px 14px/22px / Semibold（全状态统一）/ info1 + [↗外链图标 24px，与文本间距 8px] + [红标签 BETA，与文本间距 8px]
│                       │                  └── [说明] 12px/18px Regular / info2
│                       │            └── [更多项目]（.o-nav__cat-more，列底部入口 = OButton text large，右箭头 24×24 取自 assets）
│                       │
│                       └── [快捷链接栏]（.o-nav__quick，宽 342px，栏内左对齐 + 整栏 margin-left:auto 靠右）⚠️ 可选模块，默认不挂载
│                             ├── [栏标题]（.o-nav__quick-title，规格同分类列头）
│                             ├── [纯文本 link × N]（.o-nav__quick-link 灰底块 宽342/高40/圆角4/fill3/距左24 → 内层 .o-nav__quick-link-text = OLink medium 高34/上下padding5/左右0/14-22、link1→link2+下划线→link3+下划线、后缀图标 24×24）
│                             └── [带图活动卡片]（.o-nav__quick-card，左图 16:9 + 右文 标题/说明，无背后色块）
│
└── [右侧操作区]（搜索 / 源码下拉 / 图标）
```

> ⚠️ **列头无交互**：分类列头只是分组标题，不是链接、不是按钮（PC 端），禁止给它加 hover / 点击态。MB 端列头才成为左侧一级导航列的可点击项（见 §七）。

---

## 涉及组件

| 组件 | 规范文档 | 本楼层使用的变体 |
|------|---------|----------------|
| ONavigation | [components/navigation.md](../components/navigation.md) | type=PC 顶导（面板挂载于顶导菜单项） |
| OLink | [components/link.md](../components/link.md) | 条目标题链接（外链带后缀跳出图标） |
| OTag | [components/tag.md](../components/tag.md) | fill + danger + small（BETA / 新 红色小标签） |
| ODropdown | [components/dropdown.md](../components/dropdown.md) | 仅参照其「展开面板」通用规格（间距 4px / 圆角 4px / 阴影） |
| OPopover | opendesign-components skill `references/popover.*.md` | 文字截断后的悬浮 tooltip（复用既有组件，见 §八） |

---

## 一、面板定位与触发规则（继承平铺式，本文件只列差异）

**以下规则全部继承 [floors/navigation-dropdown.md](navigation-dropdown.md)，禁止在本文件重复定义、禁止改动**：

| 项 | 规则 |
|---|---|
| 触发器 | 顶导菜单项本身 |
| 展开方式 | hover = 临时展开（移走即消失）；click = 锁定展开（`data-open`，需再次点击关闭）；两种途径并存 |
| 与顶导间距 | 4px（`top: calc(100% + 4px)`）+ `::before` 透明热区桥接；**⚠️ 分类平铺式改为零间距（`top: 100%`），见下方差异表** |
| 水平定位 | 水平居中于触发器（`left: 50%` + `translate(-50%, …)`）；**分类平铺式改为通栏撑满页面宽度 + 内部 padding 对齐栅格（见下方差异表）**；MB 端同为通栏 |
| 显隐三规则 | `:hover` / `:focus-within` / `[data-open="true"]` 三条；**分类平铺式的 transform 改为 `translateY`（无 translateX），见下方警告** |
| 单开互斥 | click JS 先关其他 + CSS `:has()` 压制 hover 与锁定态重叠 |
| 同时展开 | 同一时刻只允许一个面板展开 |
| z-index | 100；顶导容器禁止 `overflow: hidden` |
| 面板背景 / 圆角 / 阴影 | `var(--o-color-fill2)` / 4px / `0 6px 24px rgba(18,20,23,0.08)`（组件特例值） |
| 面板高度 | `height: auto`，随内容增长，禁止写死、禁止内部滚动条 |
| 关闭兜底 | 关闭时 `link.blur()`，否则 `:focus-within` 使面板重新显示 |

**本文件只覆盖以下差异项**：

| 项 | 分类平铺式取值 |
|---|---|
| 布局方向 | `flex-direction: row` + `flex-wrap: wrap`（多列横向排布） |
| 面板宽度 | **通栏 `width: auto`**（`left: 0` + `right: 0`，相对顶导本体撑满页面宽度） |
| **水平定位** | **通栏撑满页面宽度**（⚠️ 不用基类的「居中触发器」，也不用 `max-content` 窄面板，见下方警告） |
| 面板内边距 | 上下 24px / 左右 **216px**（PC 栅格边距）；≤840px 上下 12px / 左右 **24px**（MB 栅格边距）→ 见 §六 六档自适应 |
| **与顶导间距** | **零间距**（`top: 100%`，紧贴顶导底边，像抽屉一样展开）——⚠️ 覆盖基类的 4px 间距 |
| 面板圆角 | **`0 0 4px 4px`**（上圆角压平为 0）——零间距后上圆角会在左上/右上露出月牙形缺口，视觉上又"裂开"；只保留下圆角。⚠️ 覆盖基类的 `4px` |
| 面板 z-index | **100**；顶导下阴影靠 `.o-nav::after` 补偿层（`z-index: 102`）叠在面板之上——⚠️ **不是**"顶导 101 > 面板 100"（父子之间比 z-index 无效，详见 floors/navigation.md §顶导阴影） |
| 列间距（条目间左右间距） | 24px（≥1681px）/ 16px（1200~841px） |
| 条目间距（条目间上下间距） | 24px（PC 各档一致） |

> 🚨 **宽面板禁止沿用「居中于触发器」定位**：5 列 + 快捷链接栏的面板宽约 1400px，若用基类的 `left: 50%` + `translateX(-50%)`，会以触发器（顶导菜单第二项）为中心向两侧展开，**左侧整体溢出视口、第一列被挤出屏幕**（已在 check HTML 复现并修正）。正确做法 = **通栏撑满页面宽度 + 内部 padding 把条目内容对齐到栅格内容区**：
>
> ```css
> /* 0) 零间距：紧贴顶导底边展开（像抽屉），覆盖基类的 4px 间距；无需 ::before 热区桥接 */
> .o-nav__dropdown.o-nav__dropdown--categorized {
>     top: 100%;                         /* ⭐ 与顶导零间距（2026-10-15 用户确认，覆盖基类 calc(100% + 4px)） */
>     border-radius: 0 0 4px 4px;        /* ⭐ 上圆角压平：否则左上/右上露出月牙缺口，视觉上又"裂开" */
> }
> .o-nav__dropdown.o-nav__dropdown--categorized::before { display: none; }  /* 无间隙，不需要桥接 */
> /* 1) 把包含块从 .o-nav__item 上移到顶导本体（sticky 即定位元素） */
> .o-nav__item:has(> .o-nav__dropdown--categorized) { position: static; }
> /* 2) 面板通栏，内部 padding 对齐栅格内容区 */
> .o-nav__dropdown.o-nav__dropdown--categorized {
>     left: 0;
>     right: 0;
>     width: auto;                    /* 通栏：左右贴屏幕边缘 */
>     max-width: none;                /* ⭐ 必须覆盖基类的 max-width: 1488px，否则通栏被限宽（见下方警告） */
>     padding: 24px 216px;            /* PC：上下 24px / 左右 216px 栅格边距；MB 改 24px */
>     transform: translateY(-8px);    /* 覆盖基类的 translate(-50%, -8px) */
> }
> /* 3) 显隐三规则与互斥规则都要覆盖 transform（否则基类的 translate(-50%, 0) 优先级更高） */
> .o-nav__item:hover > .o-nav__dropdown.o-nav__dropdown--categorized,
> .o-nav__item:focus-within > .o-nav__dropdown.o-nav__dropdown--categorized,
> .o-nav__item[data-open="true"] > .o-nav__dropdown.o-nav__dropdown--categorized {
>     opacity: 1; visibility: visible; transform: translateY(0);
> }
> .o-nav__menu:has(> .o-nav__item:hover > .o-nav__dropdown) > .o-nav__item:not(:hover) > .o-nav__dropdown.o-nav__dropdown--categorized,
> .o-nav__menu:has(> .o-nav__item:focus-within > .o-nav__dropdown) > .o-nav__item:not(:focus-within) > .o-nav__dropdown.o-nav__dropdown--categorized {
>     opacity: 0; visibility: hidden; transform: translateY(-8px);
> }
> ```
>
> ⚠️ **优先级陷阱**：`.o-nav__item:hover > .o-nav__dropdown` 的特异度（0,3,0）高于修饰类 `.o-nav__dropdown.o-nav__dropdown--categorized`（0,2,0），因此**只写修饰类的 `transform` 不够**，必须为显隐三规则和互斥规则各补一条带 `.o-nav__dropdown--categorized` 的版本（0,4,0）。
>
> 🚨 **通栏必须显式覆盖基类 `max-width`**：基类 `.o-nav__dropdown` 为「居中触发器」定位写了 `max-width: 1488px`（防宽面板溢出视口）。分类平铺式改成通栏定位后，`max-width` **不会**因为写了 `width: auto` 而自动失效——不显式 `max-width: none` 覆盖，面板宽被锁死 1488px，`left: 0; right: 0` 实际撑不到屏幕边缘，`margin-left: auto` 的快捷链接栏只能停在 1488px 内的右端，**距视口右缘不是 216px、而是悬在页面中间**（1920/1680 档已复现并修正）。通用规则：凡基类有而修饰类不适应的约束（`max-width` / `transform` / `top` / `border-radius`）都必须逐条显式覆盖，不能只写自己新增的属性。
>
> ⚠️ **面板左右边距必须跟栅格走**（hard-constraints §4.1）：PC 内容区宽 1488px、距画布左右各 **216px**（216 + 1488 + 216 = 1920）；MB 内容区宽 312px、距画布左右各 **24px**（24 + 312 + 24 = 360）。面板 `padding-left/right` 就取这两个值，**禁止**再用 64px / 40px / 12px 等自创边距——否则条目内容左缘会和页面内容区、顶导内容区三处错位。
>
> ⚠️ **包含块上移后 `top` 仍成立**：顶导本体高 = 72px，`top: 100%` = 顶导顶 + 72px = 顶导底 + 0px（零间距）；若顶导未用 sticky/fixed（非定位元素），需给顶导显式 `position: relative` 或改用固定 `top: 72px`。
>
> ⚠️ **零间距后不要再写 `::before` 热区桥接**：桥接的作用是补上鼠标穿过面板与顶导之间空隙的路径；`top: 100%` 时面板顶边与顶导底边重合，不存在可穿过的空隙，桥接只会白占顶导底部 4px。分类平铺式必须显式 `display: none` 掉基类的 `::before`。
>
> ⚠️ **零间距必须同时压平上圆角**：`border-radius: 0 0 4px 4px`。若保留基类的 `4px` 全圆角，面板左上 / 右上会各露出一道月牙形缺口，视觉上面板与顶导又"裂开"了，抽屉式贴合就白做了。下圆角保留（面板底边是自由边）。
>
> ⚠️ **动画位移不算间距**：隐藏态 `translateY(-8px)` 只是入场位移，展开终态是 `translateY(0)`，**不要**因为看到 -8px 就把 `top` 改成 `calc(100% + 8px)`——那会在展开态留下 8px 真实间隙。

> ⚠️ **混用两种样式时互斥自动生效**：单开互斥的 `:has()` 选择器指向基类 `.o-nav__dropdown`，平铺式与分类平铺式面板共享该基类，因此同一菜单内两种样式混挂时，互斥规则**同时覆盖两者**，无需另写规则。前提是分类平铺式面板必须**同时挂基类 + 修饰类**（`class="o-nav__dropdown o-nav__dropdown--categorized"`），只挂修饰类会导致定位/显隐/互斥全部失效。

---

## 二、面板内容排布（PC 多列结构）

### 2.1 结构示意（开发 Tab · 5 列）

```
┌──────────────────────────────────────────────────────────────────────────────┐
│ 贡献社区        构建           发布           分析            项目            │
│ ─────────      ─────────      ─────────      ─────────      ─────────        │
│ 提交 Issue ↗   本地构建       发布镜像       代码扫描        全部项目          │
│ 在 Gitee 提…   OBS 编译       发布 RPM       静态检查…       孵化项目          │
│ 提交 PR ↗      交叉编译       版本路线图     安全漏洞[BETA]  归档项目          │
│ SIG 组         构建文档                       性能分析        更多项目 >       │
│ 开发者论坛                                                                    │
└──────────────────────────────────────────────────────────────────────────────┘
```

### 2.2 元素构成规则

| 元素 | 规则 |
|---|---|
| 分类列（`.o-nav__cat`） | `flex: none` 禁止被压缩；一列一个分类；列内 = 列头 + 条目列表 |
| 列头（`.o-nav__cat-title`） | **>1440px 16px/24px · ≤1440px 14px/22px** / Regular(400) / `info4`；与条目列表间距 12px；**无交互态** |
| 条目（`.o-nav__cat-item`） | 透明容器（**默认无背景、无 hover 背景**，本样式 hover 只改文字）；内部 = 标题 + 说明，间距 4px |
| 条目标题（`.o-nav__cat-item-title`） | **>1440px 16px/24px · ≤1440px 14px/22px** / `info1`；**字重全状态统一 Semibold(600)**（默认态即 Semibold，hover / 按下不切换；禁用态是唯一例外，回落 Regular）；最多 2 行，超出 `...`（见 §八） |
| 条目说明（`.o-nav__cat-item-desc`） | 12px/18px Regular / `info2`；**`min-height: 36px` 预留两行高度**（2 × 18px 行高，文案只有 1 行时也占两行空间，后续延展成 2 行不挤压其他条目，2026-10-15 用户指定）；最多 6 行，超出 `...`（见 §八） |
| 外链图标（↗） | 仅外链条目挂载；**24×24**；`fill="currentColor"` 跟随标题态；**与文本间距 8px**；**必须 Read `references/assets/public icons/icon-外链.svg` 后逐属性内联**，禁止手写 SVG |
| 红色小标签（BETA / 新） | OTag `fill + danger + small`：高 20px / 水平 padding 6px / 圆角 4px / 12px-18px / 白字 / 底 `--o-color-danger1`；**与文本间距 8px**（与外链图标一致，2026-10-15 用户指定） |
| 「更多项目 >」 | 列底部入口 = **OButton `text` + `large`**（components/button.md）：高 40px / 水平内边距 0 / 16px-24px Regular / 默认 `info1` → hover `primary2` → 按下 `primary3`；**右箭头图标 24×24 在右侧**（取自 `icon-右箭头.svg`，`fill="currentColor"`），图文间距 8px；⛔ 必须是 `<button>`，禁止字符 `>` 代替 |
| 快捷链接栏（`.o-nav__quick`） | **可选模块，默认不挂载**；面板最右侧，`margin-left: auto` 让**整栏靠右**；**栏内整体左对齐**（`align-items: flex-start`，标题 / 色块 / 卡片同一左缘）；宽 **342px**；`flex: none` 禁止被压缩 |

### 2.3 快捷链接栏（社区 Tab · 4 列 + 右侧栏）

```
┌──────────────────────────────────────────────────────────────────┐
│ 社区动态    参与贡献    资源      支持        │ 快捷链接            │
│  条目…      条目…      条目…     条目…       │ ┌────────────────┐ │
│                                            │ │企业签署CLA流程 │ │
│                                            │ ├────────────────┤ │
│                                            │ │CLA-FAQ ↗      │ │
│                                            │ ├────────────────┤ │
│                                            │ │开发者日历      │ │
│                                            │ └────────────────┘ │
│                                            │ [图] openEuler…    │
│                                            │ [图] 操作系统大会… │
└──────────────────────────────────────────────────────────────────┘
         ↑ 栏内整体左对齐（标题/色块/卡片同一左缘）        ↑ 整栏 margin-left:auto 靠右
```

> 🚨 **快捷链接栏是可选模块，看需求挂载，默认不出现**（2026-10-15 用户确认）。开发 Tab 这类 5 列场景通常不挂；只有社区 Tab 这类"4 列 + 右侧栏"的场景才挂。HTML 骨架中该段默认注释掉，需要时取消注释。

**挂载时的四条硬规则（2026-10-15 用户确认）**：

1. **栏首必须有「快捷链接」标题**（`.o-nav__quick-title`）——与左侧分类列头视觉对齐，规格完全一致：**>1440px 16px/24px · ≤1440px 14px/22px / Regular / `info4`**，无交互态
2. **纯文本项 = 灰底按钮块（外层容器）+ 内部 OLink medium（components/link.md）**——**两层叠加，缺一不可**：
   - **灰底按钮块（外层）**：宽 **342px 固定**、高 **40px**、圆角 **4px**、背景 **`--o-color-fill3`**（⛔ 不得去掉）、块内 link **距左 24px** + 上下居中
   - **OLink medium（内层文字）**：整体高 **34px**、Auto Layout Padding **上 5px / 下 5px / 左 0px / 右 0px**、文字 **14px/22px** Regular（`font_family`）、文字色 **`--o-color-link1`**，hover 变 **`--o-color-link2`** 并**加下划线**，按下变 **`--o-color-link3`** 并**加下划线**；**后缀图标 24×24**（`icon_size_control-m`，颜色随文字，取 `icon-外链.svg`）、图标↔文字间距 **4px**（gap-1）
   - ⚠️ **「块」与「OLink」是两层，不是二选一**：把 OLink 的 link 色直接刷在无背景的裸文字上（丢块）、或保留块却把 40px/24px 左右 padding 写在 link 本体上（丢 link.md 的 34px 高 + 5px 上下 + 0px 左右），都是错的
   - ⚠️ HTML 结构：`<a class="o-nav__quick-link">` 是**灰底块**，内部再包一层 `<span class="o-nav__quick-link-text">` 承载 OLink medium 规格（含后缀图标）
3. **栏内整体左对齐 + 整栏靠右**：
   - `.o-nav__quick { align-items: flex-start; margin-left: auto; width: 342px; }`
   - **栏内左对齐**：标题 / 色块 / 卡片共用同一条左缘，⛔ 不是每个选项各自右对齐
   - **整栏靠右**：`margin-left: auto` 让整栏贴栅格内容区右缘，**PC 端快捷链接内容距视口右边固定 216px**，与页面内容区右缘、顶导内容区右缘三处对齐
   - MB 端（≤840px）跟随 MB 栅格：`justify-content: flex-start` 栏内左对齐，距视口左右 24px
4. **图+文本卡片没有背后的色块**：`.o-nav__quick-card` **无背景、无圆角、无内边距**（与纯文本项的灰底块是两种形态，禁止给卡片也刷 `fill3`）；左图右文、**图文垂直居中**（`align-items: center`）、图文间距 8px；图片自身圆角 4px 可保留

**带图 + 文本快捷链接（活动卡片）**：可选形态——
- **左图右文**（`flex-direction: row`） + **图文垂直居中**（`align-items: center`），宽 342px（与色块同宽，保证栏内左缘一致）
- **图与文本内容间距 8px**（间距 2）
- 图尺寸：**高 68px、宽 121px**（16:9），`object-fit: cover`
- 标题：**16px/24px** Semibold / `info1`；描述：**12px/18px** Regular / **`info3`**（用户 2026-10-15 指定，原 info2 作废）
- **hover / 按下态与左侧分类条目保持完全一致**（用户 2026-10-15 指定）：hover 标题色 → `--o-color-primary2`、按下 → `--o-color-primary3`；**无背景色块、无字重变化，图片与描述行各态均不变**
- 图按 SKILL.md「位图素材」规则生成或占位（禁止编造 URL），`<img>` 必须带 alt 与「示意配图，上线前替换真实素材」注释

---

## 三、精确数值速查

| 规格项 | 值 | 来源 |
|--------|---|------|
| **面板与顶导间距** | **0px（`top: 100%`，紧贴顶导底边，像抽屉一样展开）** | 用户规格（2026-10-15，覆盖继承平铺式的 4px） |
| 面板圆角 / 背景 / 阴影 | **`0 0 4px 4px`**（上圆角压平，零间距贴合必需）/ `var(--o-color-fill2)` / `0 6px 24px rgba(18,20,23,0.08)` | 圆角为用户规格（2026-10-15，覆盖继承平铺式的 4px 全圆角）；背景/阴影继承 navigation-dropdown.md |
| 面板布局 | `flex-direction: row` + `flex-wrap: wrap` + `align-items: flex-start` | 用户规格（多列平铺） |
| 面板宽度 | **通栏 `auto`**（`left: 0` + `right: 0`，相对顶导本体），条目内容由内部 padding 对齐栅格 | hard-constraints §4.1 + 用户规格（2026-10-15） |
| **面板内边距（≥1681px）** | **上下 24px / 左右 216px** | 左右 = PC 栅格边距（hard-constraints §4.1：1488+216×2=1920）；上下 24px ⚠️ 推断值（待确认 #2） |
| 面板内边距（1680~1441px） | 上下 24px / **左右 216px** | 跟随栅格（该档未规定面板边距，延续基准档） |
| 面板内边距（1440~1201px） | 上下 24px / **左右 216px** | 跟随栅格（同上） |
| 面板内边距（1200~841px） | 上下 24px / **左右 216px**（延续上一档） | 跟随栅格（该档仅规定条目间距与字号，边距须显式写出） |
| **列间距（条目间左右间距）** | **24px（≥1681px）/ 16px（1200~841px）** | 用户规格 |
| **条目间距（条目间上下间距）** | **24px（PC 全档）** | 用户规格 |
| 列头字号 / 行高 | **16px / 24px（>1440px）· 14px / 22px（≤1440px）** | 用户规格（2026-10-15） |
| 列头颜色 | `var(--o-color-info4)` | 用户规格（2026-10-15，原 info3 作废） |
| 列头与条目列表间距 | 12px（间距 3） | 推断值，待确认 #11 |
| 条目标题字号 / 行高 | **16px / 24px（>1440px）· 14px / 22px（≤1440px）** | 用户规格（2026-10-15）+ 硬约束 §2 |
| 条目标题默认色 | `var(--o-color-info1)` | 用户规格 |
| **条目标题字重** | **Semibold(600) 全状态统一**（默认即 Semibold；hover / 按下不切换；仅禁用态回落 Regular 400） | 用户规格（2026-10-15） |
| 条目说明字号 / 行高 | 12px / 18px（提示文本 2，两档断点均不变） | 用户规格 |
| 条目说明颜色 | `var(--o-color-info2)` | 用户规格 |
| **条目说明预留高度** | **`min-height: 36px`**（2 × 18px 行高；文案只有 1 行时也占两行空间，后续延展成 2 行不挤压其他条目） | 用户规格（2026-10-15） |
| 标题↔说明间距 | 4px（间距 1） | 推断值，待确认 #11 |
| **条目状态集合** | **只有默认 / hover / 按下 / 禁用四种**（🚨 **无选中态**：条目点击即跳走，不记录位置；禁止 `.is-selected` / `primary1`） | 用户规格（2026-10-15） |
| 条目 hover 背景 | **无**（只改文字色，字重不变） | 用户规格（仅规定文字） |
| 条目圆角 / 默认背景 | **无圆角 / 透明**（本样式无色块，不需要圆角） | 用户规格 |
| 外链图标尺寸 | **24×24**（用户指定，2026-10-15） | 用户规格（覆盖硬约束 §5 的 16×16 默认档） |
| 外链图标↔文本间距 | **8px**（间距 2）；红标签↔文本间距同为 **8px** | 用户规格（2026-10-15） |
| 外链图标来源 | `references/assets/public icons/icon-外链.svg`，Read 后逐属性内联，`fill="currentColor"` | SKILL.md 图标处理规范 |
| 右箭头（更多项目）来源 | `references/assets/public icons/icon-右箭头.svg`，Read 后逐属性内联，**24×24**（text L 图标）+ `fill="currentColor"` | components/button.md |
| 上/下箭头（MB 切换）来源 | `references/assets/public icons/icon-上箭头.svg` / `icon-下箭头.svg`，16×16 + `fill="currentColor"` | SKILL.md 图标处理规范 |
| 红标签（OTag small） | 高 20px / 水平 padding 6px / 圆角 4px / 12px-18px / 白字 / 底 `--o-color-danger1` | components/tag.md |
| 红标签与标题间距 | **8px**（间距 2，与外链图标一致） | 用户规格（2026-10-15，覆盖 OTag gap 规格） |
| 「更多项目」按钮 | **OButton text + large**：高 40px / 水平内边距 0 / 16px-24px Regular / `info1`（hover `primary2`、按下 `primary3`）/ 图标 24×24 在右侧 + 图文间距 8px | components/button.md（2026-10-15 用户确认，原 12px-18px info3 作废） |
| **快捷链接栏（整体）** | **可选模块，默认不挂载**；`margin-left: auto` 让**整栏靠右**（贴栅格内容区右缘，PC 距视口右 216px）；**栏内整体左对齐**（`align-items: flex-start`，标题 / 色块 / 卡片同一左缘）；宽 **342px**；`flex: none` | 用户规格（2026-10-15） |
| **快捷链接栏标题** | `.o-nav__quick-title`，规格同分类列头（>1440px 16px/24px · ≤1440px 14px/22px / Regular / `info4`） | 用户规格（2026-10-15） |
| **快捷链接纯文本项（两层）** | **① 灰底按钮块（外层）**：宽 **342px 固定** / 高 **40px** / 圆角 4px / **`--o-color-fill3` 底**（⛔ 不得去掉）/ 块内 link **距左 24px** + 上下居中<br>**② OLink medium（内层文字，components/link.md）**：整体高 **34px**、Auto Layout Padding **上 5px / 下 5px / 左 0px / 右 0px**、文字 **14px/22px** Regular、色 **`--o-color-link1`**，hover **`link2` + 下划线**，按下 **`link3` + 下划线**；**后缀图标 24×24**（`icon_size_control-m`，取 `icon-外链.svg`，颜色随文字）、图文间距 **4px**（gap-1）<br>⚠️ 两层缺一不可；`<a>` 是灰底块，内部包 `<span class="o-nav__quick-link-text">` 承载 OLink 规格 | 用户规格（块）+ components/link.md（内层，2026-10-15 二次修正） |
| 快捷链接按钮块间距 | 8px（间距 2） | ⚠️ 推断值，待确认 #5 |
| **快捷链接卡片（带图+文本）** | **无背后的色块**（无背景 / 无圆角 / 无内边距，⛔ 禁止刷 `fill3`）；**左图右文** + **图文垂直居中**（`align-items: center`）、图文间距 **8px**、宽 **342px**（与色块同宽）；图 **121×68**（16:9）；标题 **16px/24px** Semibold `info1`；描述 **12px/18px** Regular **`info3`**；**hover 标题 → `primary2`、按下 → `primary3`**（与左侧分类条目完全一致，无背景、无字重变化，图与描述不变） | 用户规格（2026-10-15） |
| 触发器全部样式 | 见 [floors/navigation.md](navigation.md)「菜单项（触发器）完整样式」——**本模板不定义** | — |
| MB 左列最大宽度 | 144px（超长自动换行） | 用户规格 |
| MB 左列折叠态最小宽度 | 32px | 用户规格 |
| MB 面板距左右边距（≤840px） | **24px**（MB 栅格边距） | hard-constraints §4.1：312+24×2=360（2026-10-15 用户确认，覆盖原 12px 设计稿值） |
| 840~601px 右内容区最大宽度 | 整屏 40%（黄金分割）；**≤600px 不按 40% 收窄**（用户确认） | 用户规格 |

---

## 四、状态矩阵（触发器 + 列头 + 条目）

> 🔴 **触发器（顶导菜单项）的状态不在本模板定义**：五态（默认 / Hover / 展开 / 选中 / 禁用）的完整样式——字号字重、颜色、指示线、全部禁令——**唯一定义源是 [floors/navigation.md](navigation.md)「菜单项（触发器）完整样式」**（由 components/navigation.md Part C 派生），生成顶导时必须从该处整段取用，本模板不重定义、不改写、不收窄。列头与条目（面板内部元素）的状态由本模板定义，见 §4.2 / §4.3。二者必须分开实现，禁止把触发器状态搬到条目上。

### 4.1 触发器（顶导菜单项）——样式归 navigation.md，本模板只列与面板相关的行为

与面板相关的两条行为规则（属交互模型，非样式定义）：

1. **面板展开时，触发器视觉同 Hover**（仅文字 `primary2`、背景透明、无指示线、字重不变）——完整五态与禁令（任何状态无背景色块 / 全状态 Regular(400) / 禁止 `border-radius` / 选中优先 / 基础态禁止 `:has()` 收窄）见 [floors/navigation.md](navigation.md) 踩坑 #9/#17/#18/#19
2. **`is-current` 随点击迁移**（单选、不做 toggle），JS 由 navigation-dropdown.md §八 负责

### 4.2 分类列头（无交互）

| 状态 | 字号 / 行高 | 颜色 |
|------|-----------|------|
| 唯一态（PC） | **>1440px 16px/24px · ≤1440px 14px/22px** / Regular(400) | `var(--o-color-info4)` |

> ⛔ PC 端列头**不是链接**：无 hover / 按下 / 选中 / 禁用态，无下划线，无指针手势（`cursor: default`）。MB 端列头升级为左侧一级导航列的可点击项，状态见 §七。

### 4.3 面板条目（OLink · 透明容器）

| 状态 | 标题字号 / 行高 | 标题字重 | 标题色 | 说明 | 背景 |
|------|---------------|---------|--------|------|------|
| 默认 | **>1440px 16px/24px · ≤1440px 14px/22px** | **Semibold(600)** | `var(--o-color-info1)` | 12px/18px Regular / `info2` | **透明** |
| Hover | 同上 | **Semibold(600)（不变）** | `var(--o-color-primary2)` | 同默认（不变） | **透明（不加色块）** |
| 按下 Press | 同上 | **Semibold(600)（不变）** | `var(--o-color-primary3)` | 同默认 | 透明 |
| 禁用 Disabled | 同上 | **Regular(400)（唯一例外）** | `var(--o-color-info4)` | 12px/18px / `info4` | 透明 |

> 🚨 **面板条目没有「选中态」**（2026-10-15 用户确认）：面板条目是**直接点击跳走的链接**，不存在"当前停在这一项"的持久状态，因此**只有默认 / hover / 按下 / 禁用四种状态**，禁止给条目加 `.is-selected` 类或 `primary1` 选中色。
>
> ⚠️ **分类平铺式条目无按下背景色块**：与平铺式（`:active` 有 `control3-light` 色块 + `primary1` 文字）不同，本样式的按下态**只改文字颜色**（`primary3`），背景保持透明。禁止照搬平铺式的点击态色块。
>
> ⚠️ **本样式条目 hover 只改文字颜色，不加背景色块、不切字重**——与平铺式（hover 有 `control2-light` 色块撑满整行）是两种不同的交互表达，禁止把平铺式的色块搬到这里。
>
> ⚠️ **标题字重全状态统一 Semibold**（2026-10-15 用户确认）：默认态即为 Semibold，hover / 按下**不再切换字重**（原「默认 Regular → hover Semibold」的方案已作废）。禁用态是唯一允许回落 Regular 的状态——禁用不是交互态。
>
> ⚠️ **hover / 按下两色分级**：hover = `primary2`（#5177CA）、按下 = `primary3`（#002793）。禁止二者混用（最常见错误：hover 也用 `primary1`）。
>
> ⚠️ **说明文字所有状态保持不变**（默认/hover/按下均为 `info2`），只有禁用态变 `info4`。禁止给说明加 hover 变色。

---

## 五、颜色（全量语义 Token，禁止硬编码 rgba / hex）

| 区域 | Token | 值（openEuler Light） |
|------|-------|----------------------|
| 面板背景 | `var(--o-color-fill2)` | #FFFFFF |
| 分类列头 / 快捷链接栏标题 | `var(--o-color-info4)` | #999999（40% 黑，2026-10-15 由 info3 改为 info4） |
| 条目标题（默认） | `var(--o-color-info1)` | #000000 |
| 条目说明 | `var(--o-color-info2)` | #333333（80% 黑） |
| 条目禁用文字 | `var(--o-color-info4)` | #999999（40% 黑） |
| 条目标题 hover | `var(--o-color-primary2)` | #5177CA（见下方消歧） |
| 条目标题按下 | `var(--o-color-primary3)` | #002793 |
| 触发器 hover / 展开态文字 | `var(--o-color-primary2)` | 同上 |
| 触发器选中态文字 + 指示线 | `var(--o-color-primary1)` | #002FA7 |
| 红标签背景（OTag fill danger） | `var(--o-color-danger1)` | rgb(230,0,18) |
| 红标签文字 | `var(--o-color-white)` | #FFFFFF |
| 灰底按钮块背景 | `var(--o-color-fill3)` | rgb(237,237,240) |
| 快捷链接纯文本（OLink Enabled） | `var(--o-color-link1)` | #002FA7（= primary1 = brand-6） |
| 快捷链接纯文本（OLink Hover） | `var(--o-color-link2)` | #5177CA（= primary2 = brand-4） |
| 快捷链接纯文本（OLink Active） | `var(--o-color-link3)` | #002793（= primary3 = brand-7） |

> 📌 **link1/link2/link3 的 Token 来源**（2026-10-15 用户质询后补充核查）：值取自 **[opendesign-tokens/references/tokens-openeuler.md](../../opendesign-tokens/references/tokens-openeuler.md)** 「品牌色 / Primary / 链接色」表——`#002FA7` → `--o-color-primary1` / `--o-color-link1` / `--o-color-control2`（主色、链接常规色、悬浮边框）；`#5177CA` → `--o-color-primary2` / `--o-color-link2`（主色悬浮、链接悬浮）；`#002793` → `--o-color-primary3` / `--o-color-link3` / `--o-color-control3`（主色激活、链接激活）。`components/link.md` Part B 亦注明 `--o-color-link1` = brand-6、`--o-color-link4` = brand-3。**不是为某个具体社区自创的色值**，是 openEuler 设计体系的语义 Token。
>
> ⚠️ **引用前必须先在 `:root` 定义**：HTML 示例的 `:root` 若漏写 `--o-color-link1/2/3`，`var(--o-color-link1)` 会**整条声明失效**，文字回落成继承色（info1 黑），表现为"链接不是蓝色"。check 页曾出现此问题。

### ✅ primary2 色值消歧（已确认 2026-10-14，2026-10-15 补 cross-file 消歧）

设计稿标注 hover 色为 **#5177FA**，而上游 `openeuler-token.json` 的 `--o-color-primary2` = **#5177CA**（rgb 81,119,202），两者仅末两位不同（FA vs CA）。按 SKILL.md「颜色必须用语义 Token、禁止硬编码」原则，**一律写 `var(--o-color-primary2)`，禁止把 #5177FA 写死进 CSS**。用户已确认：**以上游语义 Token 为准**取 `#5177CA`）。

> ⚠️ **2026-10-15 补充：同一 Token 在 dropdown.md / search.md 附录里记作 `#0047D9`**（来源各自的 demo 页，非上游真源）。导航楼层**只认 `#5177CA`**；给「源码」按钮或「更多项目」按钮写 hover 时同样取 `#5177CA`，禁止从这两个附录抄 `#0047D9`，否则同一顶导内会出现两种 hover 蓝。详见 navigation-dropdown.md「primary2 的取值消歧」。

### ⚠️ 本模板不使用 control2-light / control3-light

分类平铺式条目**没有任何背景色块**（hover 只改文字），因此**不引用** `--o-color-control2-light` / `--o-color-control3-light`——这两个 Token 在本仓多文档中有多种取值（#F0F5FF / #EBF1FA / #002FA7 …），最容易取错。本模板从源头规避：无背景即无取值歧义。若后续设计稿补充条目 hover 背景，必须先按 navigation-dropdown.md「五、颜色」的消歧表取值，禁止直接引用。

### 顶导所需语义 Token 完整清单（可直接抄入 `:root`）

```css
:root {
    --o-color-fill2: #FFFFFF;             /* 面板 / 顶导背景 */
    --o-color-fill3: #EDEDF0;             /* 活动卡片背景（嵌套卡片背景） */
    --o-color-info1: #000000;             /* 条目标题默认、触发器默认 */
    --o-color-info2: #333333;             /* 条目说明（80% 黑） */
    --o-color-info3: #666666;             /* MB 切换箭头（60% 黑） */
    --o-color-info4: #999999;             /* 分类列头 / 快捷链接栏标题 / 禁用文字（40% 黑） */
    --o-color-primary1: #002FA7;          /* 选中态标题、触发器选中态 + 指示线 */
    --o-color-primary2: #5177CA;          /* hover 态标题、触发器 hover 态（上游值，见消歧） */
    --o-color-primary3: #002793;          /* 按下态标题 */
    --o-color-danger1: #E60012;           /* 红标签背景 rgb(230,0,18) */
    --o-color-white: #FFFFFF;             /* 红标签文字 */
    --o-color-link1: #002FA7;             /* 快捷链接纯文本默认（OLink Enabled） */
    --o-color-link2: #5177CA;             /* 快捷链接纯文本 hover（OLink Hover） */
    --o-color-link3: #002793;             /* 快捷链接纯文本按下（OLink Active） */
}
```

> ⛔ **Token 必须先定义再使用**：引用未定义的 `var(--o-color-xxx)` 时，该 `color` / `background` 声明整体无效——文字会回退成继承色、SVG 会回退 `fill="none"` 导致图标完全不可见。每次改色值后必须回头核对本清单。

---

## 六、自适应规则（六档断点，来自设计稿）

| 断点区间 | 布局规则 |
|---|---|
| **≥1681px** | 条目间左右间距 **24px**，上下间距 **24px**；**列头 16px/24px、条目标题 16px/24px**（`info4` / `info1`）；面板内边距 上下 24px / 左右 **216px**（PC 栅格边距） |
| **1680 ~ 1441px** | 定制导航区菜单间距 32px（归 navigation.md 管），**面板左右边距沿用 216px**（跟随栅格）；列头 / 标题仍 **16px** |
| **1440 ~ 1201px** | **面板左右边距沿用 216px**（跟随栅格）；**列头与条目标题字号由 16px 降为 14px**（行高 24px→22px），颜色不变（仍 `info4` / `info1`），条目说明仍 12px |
| **1200 ~ 841px** | 条目间左右间距 **16px**，上下间距 24px；列头 / 标题 **14px**（延续上一档，无需另写）；左右边距沿用 **216px** |
| **840 ~ 601px**（折叠屏 / 手机横屏 / Pad 竖屏） | 面板 padding 左右 **24px**（MB 栅格边距）；右侧内容区最大占整屏 **40%**（黄金分割）；布局变为**左一级导航列 + 右内容区**；列头 / 标题 **14px** |
| **600 ~ 360px**（手机竖屏） | 面板 padding 左右 **24px**（MB 栅格边距）；左侧菜单左对齐；菜单最大宽度 **144px**，超长自动换行；**右内容区不按 40% 收窄**（用户确认）；Pad 按比例算最大宽度（屏宽 40%，黄金分割） |

**实现要点**：

1. **≥1681px 为基准档**：列间距 24px、条目间距 24px、**列头与条目标题 16px/24px**、条目说明 12px/18px、面板 padding `24px 216px`
2. **1680~1441px 面板边距不动**：左右仍 **216px**（跟随栅格）；列间距、条目间距、字号全部不动
3. **1440 是字号分水岭**：`@media (max-width: 1440px)` 一条规则同时把 `.o-nav__cat-title` / `.o-nav__quick-title` / `.o-nav__cat-item-title` 降为 **14px/22px**；**只降字号，不降颜色**（列头仍 `info4`、标题仍 `info1`、说明仍 `info2`）
4. **1200~841px 只改列间距**：24px → **16px**（间距 4）；字号已在 ≤1440px 档降为 14px，**不要重复写**；左右内边距仍 **216px**（该档未规定边距，必须显式写出，避免阅读时误以为回退基准档）；条目上下间距保持 24px
5. **≤840px 换布局**：从「多列平铺」切换为「左一级导航列 + 右内容区」双列结构（见 §七）；面板仍通栏撑满，padding 改为上下 12px / 左右 **24px**（MB 栅格边距）
6. **面板高度一律 `auto`**，随条目数量增长，禁止写死、禁止裁切、禁止内部滚动条
7. **列数不写死**：由内容决定（开发 Tab 5 列 / 社区 Tab 4 列 + 快捷栏）；超宽时 `flex-wrap: wrap` 自动折行，禁止压缩列宽导致文字竖排

> ⚠️ **601~840px 为 hard-constraints 规范空档**（§1 只定义 ≤600px MB 与 ≥841px PC），本模板按用户设计稿把 840~601px 实现为独立档（双列结构），与 navigation-dropdown.md 待确认项 #5 同源，已列入本文件待确认项 #8。
>
> ✅ **断点区间已修正**（2026-10-14 用户确认）：原稿「1680~1201px 内容区距左右 40px」为笔误，正确区间为 **1440~1201px**。修正后 `1680~1441px`（左右 64px）与 `1440~1201px`（左右 40px）**互不重叠**。⚠️ **2026-10-15 用户再次确认**：该两档的「面板左右 64px / 40px」**已作废**，面板边距统一跟随栅格 **216px**（见 §十二 已确认 #14），笔误修正记录保留备查。

---

## 七、MB / 折叠屏规格（≤840px：左一级导航列 + 右内容区）

### 7.1 结构

```
┌─────────────────────────────────────┐
│ 贡献社区    │ 提交 Issue ↗           │
│ 构建        │ 在 Gitee 提交缺陷…      │
│ 发布        │ 提交 PR ↗              │
│ 分析        │ 在 Gitee 提交代码…      │
│ 项目    ⌃   │ SIG 组                 │
│             │ 开发者论坛              │
├─────────────────────────────────────┤
│ [快捷链接栏：横铺，占满整宽]           │
└─────────────────────────────────────┘
   ↑ 左一级导航列        ↑ 右内容区（当前分类条目）
```

| 规格项 | 840~601px（折叠屏/Pad 竖屏） | 600~360px（手机竖屏） |
|---|---|---|
| 面板布局 | `display: grid`：`minmax(32px, 144px) 1fr` | 同左 |
| 面板宽度 | 100%（全屏宽） | 100% |
| 面板内边距 | 上下 12px / 左右 **24px**（MB 栅格边距，第三轮确认已作废 12px） | 上下 12px / 左右 **24px** |
| 左列与右内容区间距 | 12px（间距 3） | 12px |
| **右内容区最大宽度** | **整屏 40%**（黄金分割） | **不按 40% 收窄**（`max-width: none`，用户确认） |
| 左列对齐 | 左对齐 | 左对齐 |
| **左列最大宽度** | 144px | **144px**，超长自动换行 |
| 左列折叠态最小宽度 | 32px | 32px |
| 左列项字号 / 行高 | 14px / 22px | 14px / 22px |
| 右内容区条目标题 | 14px / 22px（已在 ≤1440px 档降为 14px） | 14px / 22px |
| 右内容区条目说明 | 12px / 18px | 12px / 18px |
| 快捷链接栏 | 占满整宽（`grid-column: 1 / -1`），按钮块横铺换行，**整体右对齐**（`justify-content: flex-end`） | 同左 |
| 面板水平定位 | 通栏 `left: 0` + `right: 0`（相对顶导） | 同左 |

### 7.2 MB 端状态（左列项 = 一级导航）

| 状态 | 字重字号 | 颜色 |
|------|---------|------|
| 默认态 | 14px Regular | `var(--o-color-info2)` |
| 按下态 | 14px Semibold | `var(--o-color-primary1)` |
| 选中态（当前分类） | 14px Semibold | `var(--o-color-primary1)` |
| 禁用态 | 14px Regular | `var(--o-color-info4)` |

> 右内容区条目沿用 §4.3 桌面端状态矩阵，仅字号降为 14px/22px（标题）+ 12px/18px（说明）。

### 7.3 MB 交互规则

1. **点击左列分类 → 右内容区切换**：同一时刻只有一个分类处于 `.is-active`（其 `.o-nav__cat-list` 显示在右列第一行），其余分类列表 `display: none`
2. **导航项过多 → 左列最后一项显示切换箭头**：点击箭头展开/收起左列（面板加/去 `.is-collapsed`）
3. **折叠态最小宽度 32px**：`.is-collapsed` 时左列收成 32px，仅保留箭头/圆点，分类文字隐藏（`grid-template-columns` 改 `32px 1fr`）
4. 左列文字**允许换行**（`white-space: normal`，与 PC 条目 `nowrap` 相反），最大宽度 144px

---

## 八、溢出 / 截断规则

| # | 规则 | 实现 |
|---|------|------|
| 1 | **下拉标题最多显示 2 行**，超出显示 `...` | `.o-nav__cat-item-title` 用 `-webkit-line-clamp: 2` + `display: -webkit-box` + `overflow: hidden` |
| 2 | **下拉选项最多显示 6 行**，超出 6 行出现 `...` | `.o-nav__cat-item-desc` 用 `-webkit-line-clamp: 6`（同上三件套） |
| 3 | **悬浮 tooltip 显示完整内容**，tips 复用设计系统既有组件 | 截断元素外包一层带 `title` / OPopover 的容器；**design skill 的 components/ 暂无 popover 文档**，引用 opendesign-components skill 的 `references/popover.*.md`（待确认 #6）；其他「显示不全」场景一律参照此交互 |
| 4 | **导航项过多时，最后一项显示切换箭头**；**左侧折叠态最小宽度 32px** | 见 §7.3：左列末项挂 `.o-nav__cat-toggle`（切换箭头），面板 `.is-collapsed` 收至 32px。⚠️ 该箭头**仅 ≤840px 的 MB 双列布局生效**：基础样式必须默认 `display: none`，仅在 `@media (max-width: 840px) and (min-width: 601px)` 里恢复 `display: block`——漏写隐藏规则时，PC 档箭头会裸露在「项目」列末尾（2026-10-16 验收复现） |

> ⚠️ **line-clamp 必须四件套齐全**（`display: -webkit-box` + `-webkit-box-orient: vertical` + `-webkit-line-clamp: N` + `overflow: hidden`），缺 `overflow: hidden` 时省略号不出现、文字直接溢出。
>
> ⚠️ **被截断的元素必须挂 tooltip**（规则 3），否则信息丢失；tooltip 内容取完整原文，不取截断后的文本。
>
> 🚨 **MB 专属元素在 PC 档必须显式隐藏**：凡只服务 MB 双列布局的节点（`.o-nav__cat-toggle` 切换箭头等），基础样式一律默认 `display: none`，仅在对应 MB 媒体查询里恢复显示。HTML 骨架是无条件渲染全量节点的，靠 CSS 分档显隐——只写「MB 显示」不写「PC 隐藏」，PC 档就会多出不该有的元素。

---

## 九、HTML 实现骨架（含 MB 双列 + 快捷链接栏）

```html
<!-- 顶导菜单项（触发器）：.o-nav__item 即 floors/navigation.md 顶导骨架的菜单项节点
     （全套导航统一 .o-nav__* 命名，无需映射；顶导与触发器样式由 navigation.md 整段提供，此处不重定义）
     · is-current = 当前页选中态（底部 2px 指示线），只能有一个，点击时 JS 迁移
     · data-open = 面板锁定展开态，点击切换，单开互斥 -->
<div class="o-nav__item is-current" data-open="false">
    <a class="o-nav__link" href="#">开发</a>

    <!-- 分类平铺式下拉面板：基类 + 修饰类必须同时挂载（漏基类会导致定位/显隐/互斥全部失效） -->
    <div class="o-nav__dropdown o-nav__dropdown--categorized" role="menu">

        <!-- ===== 分类列 × 5（开发 Tab） ===== -->
        <div class="o-nav__cat is-active">        <!-- MB 端 .is-active = 右列当前展示的分类 -->
            <div class="o-nav__cat-title"><span>贡献社区</span></div>
            <div class="o-nav__cat-list">
                <a class="o-nav__cat-item" href="#">
                    <span class="o-nav__cat-item-title">
                        <span class="o-nav__cat-item-text">提交 Issue</span>
                        <!-- 外链图标：references/assets/public icons/icon-外链.svg，
                             Read 后逐属性内联，width/height 24，fill="currentColor" 跟随标题态；
                             ⛔ 禁止手写 SVG / 禁止 <img src> / 禁止字符 ↗ 代替 -->
                        <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none">
                            <path d="M12.7077 5.2806C12.7077 4.95843 12.4465 4.69727 12.1243 4.69727L6.26823 4.69727C5.34775 4.69727 4.60156 5.44346 4.60156 6.36393L4.60156 17.509C4.60156 18.4295 5.34775 19.1756 6.26823 19.1756L17.622 19.1756C18.5425 19.1756 19.2887 18.4295 19.2887 17.509L19.2887 11.7114C19.2887 11.3892 19.0275 11.1281 18.7053 11.1281C18.3832 11.1281 18.122 11.3892 18.122 11.7114L18.122 17.509C18.122 17.7851 17.8982 18.009 17.622 18.009L6.26823 18.009C5.99209 18.009 5.76823 17.7851 5.76823 17.509L5.76823 6.36393C5.76823 6.08779 5.99209 5.86393 6.26823 5.86393L12.1243 5.86393C12.4465 5.86393 12.7077 5.60276 12.7077 5.2806ZM17.4223 4.70081C18.3428 4.70081 19.089 5.44699 19.089 6.36747L19.089 9.65911C19.089 9.98128 18.8278 10.2424 18.5057 10.2424C18.1835 10.2424 17.9223 9.98128 17.9223 9.65911L17.9223 6.7314L13.812 10.8039C13.5831 11.0307 13.2138 11.0289 12.987 10.8001C12.7603 10.5712 12.762 10.2019 12.9908 9.97515L17.1367 5.86747L14.1307 5.86747C13.8085 5.86747 13.5473 5.6063 13.5473 5.28414C13.5473 4.96197 13.8085 4.70081 14.1307 4.70081L17.4223 4.70081Z" fill="currentColor" fill-rule="evenodd" />
                        </svg>
                    </span>
                    <span class="o-nav__cat-item-desc">在 Gitee 提交缺陷与需求</span>
                </a>
                <a class="o-nav__cat-item" href="#">
                    <span class="o-nav__cat-item-title">
                        <span class="o-nav__cat-item-text">安全漏洞</span>
                        <!-- 红色小标签：OTag fill + danger + small（components/tag.md）
                             高 20px / 水平 padding 6px / 圆角 4px / 12px-18px / 白字 -->
                        <span class="o-tag o-tag--fill o-tag--danger o-tag--small">BETA</span>
                    </span>
                    <span class="o-nav__cat-item-desc">查看安全公告与修复方案</span>
                </a>
                <a class="o-nav__cat-item" href="#">
                    <span class="o-nav__cat-item-title">
                        <span class="o-nav__cat-item-text">SIG 组</span>
                    </span>
                    <span class="o-nav__cat-item-desc">加入兴趣小组，参与技术讨论</span>
                </a>
            </div>
        </div>

        <div class="o-nav__cat">
            <div class="o-nav__cat-title"><span>构建</span></div>
            <div class="o-nav__cat-list">
                <a class="o-nav__cat-item" href="#">
                    <span class="o-nav__cat-item-title">
                        <span class="o-nav__cat-item-text">本地构建</span>
                    </span>
                    <span class="o-nav__cat-item-desc">基于源码本地编译镜像</span>
                </a>
                <a class="o-nav__cat-item" href="#">
                    <span class="o-nav__cat-item-title">
                        <span class="o-nav__cat-item-text">OBS 编译</span>
                    </span>
                    <span class="o-nav__cat-item-desc">使用 openEuler OBS 云构建</span>
                </a>
                <a class="o-nav__cat-item" href="#">
                    <span class="o-nav__cat-item-title">
                        <span class="o-nav__cat-item-text">交叉编译</span>
                    </span>
                    <span class="o-nav__cat-item-desc">为多架构生成软件包</span>
                </a>
            </div>
        </div>

        <div class="o-nav__cat">
            <div class="o-nav__cat-title"><span>发布</span></div>
            <div class="o-nav__cat-list">
                <a class="o-nav__cat-item" href="#">
                    <span class="o-nav__cat-item-title">
                        <span class="o-nav__cat-item-text">发布镜像</span>
                    </span>
                    <span class="o-nav__cat-item-desc">上传镜像至社区仓库</span>
                </a>
                <a class="o-nav__cat-item" href="#">
                    <span class="o-nav__cat-item-title">
                        <span class="o-nav__cat-item-text">发布 RPM</span>
                    </span>
                    <span class="o-nav__cat-item-desc">提交软件包至 EPOL / everything</span>
                </a>
                <a class="o-nav__cat-item is-disabled" aria-disabled="true" href="#">
                    <span class="o-nav__cat-item-title">
                        <span class="o-nav__cat-item-text">发布容器</span>
                    </span>
                    <span class="o-nav__cat-item-desc">暂未开放，敬请期待</span>
                </a>
                <!-- 列底部「更多项目」= OButton text large（components/button.md）。
                     右箭头取自 references/assets/public icons/icon-右箭头.svg，
                     Read 后逐属性内联，24×24 + fill="currentColor"（text L 图标 24×24）；
                     ⛔ 必须是 <button>（不是 <a>、不是 div、不是字符 >） -->
                <button class="o-btn o-btn--text o-btn--large o-nav__cat-more" type="button">
                    更多项目
                    <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none">
                        <path d="M0.7 0C1.05438 0 1.34726 0.263344 1.39361 0.605014L1.4 0.7L1.4 5.85451C1.4 5.99653 1.49868 6.1155 1.63121 6.14659L1.7 6.15451L6.90354 6.15451C7.29014 6.15451 7.60354 6.46791 7.60354 6.85451C7.60354 7.2089 7.3402 7.50177 6.99852 7.54812L6.90354 7.55451L1.7 7.55451C0.810531 7.55451 0.0806099 6.87141 0.00624001 6.0012L0 5.85451L0 0.7C0 0.313401 0.313401 0 0.7 0Z" fill="currentColor" fill-rule="evenodd" transform="matrix(-0.707107,0.707107,0.707107,0.707107,10.2466,6.64082)" />
                    </svg>
                </button>
            </div>
        </div>

        <div class="o-nav__cat">
            <div class="o-nav__cat-title"><span>分析</span></div>
            <div class="o-nav__cat-list">
                <!-- ⛔ 面板条目没有选中态：点击即跳走，禁止加 .is-selected / primary1 -->
                <a class="o-nav__cat-item" href="#">
                    <span class="o-nav__cat-item-title">
                        <span class="o-nav__cat-item-text">代码扫描</span>
                    </span>
                    <span class="o-nav__cat-item-desc">静态检查与质量门禁</span>
                </a>
                <a class="o-nav__cat-item" href="#">
                    <span class="o-nav__cat-item-title">
                        <span class="o-nav__cat-item-text">性能分析</span>
                    </span>
                    <span class="o-nav__cat-item-desc">性能数据与火焰图</span>
                </a>
                <a class="o-nav__cat-item" href="#">
                    <span class="o-nav__cat-item-title">
                        <span class="o-nav__cat-item-text">安全漏洞</span>
                        <span class="o-tag o-tag--fill o-tag--danger o-tag--small">新</span>
                    </span>
                    <span class="o-nav__cat-item-desc">CVE 跟踪与修复指引</span>
                </a>
            </div>
        </div>

        <div class="o-nav__cat">
            <div class="o-nav__cat-title"><span>项目</span></div>
            <div class="o-nav__cat-list">
                <a class="o-nav__cat-item" href="#">
                    <span class="o-nav__cat-item-title">
                        <span class="o-nav__cat-item-text">全部项目</span>
                    </span>
                    <span class="o-nav__cat-item-desc">浏览社区全部仓库</span>
                </a>
                <a class="o-nav__cat-item" href="#">
                    <span class="o-nav__cat-item-title">
                        <span class="o-nav__cat-item-text">孵化项目</span>
                    </span>
                    <span class="o-nav__cat-item-desc">查看孵化期项目进展</span>
                </a>
                <a class="o-nav__cat-item" href="#">
                    <span class="o-nav__cat-item-title">
                        <span class="o-nav__cat-item-text">归档项目</span>
                    </span>
                    <span class="o-nav__cat-item-desc">已归档项目历史存档</span>
                </a>
                <!-- 左列最后一项：导航项过多时显示切换箭头（.o-nav__cat-toggle），见 §7.3。
                     箭头图标取自 assets：展开态 icon-上箭头.svg（⌃）／折叠态 icon-下箭头.svg（⌄），
                     Read 后逐属性内联，16×16 + fill="currentColor"；⛔ 禁止用字符 ⌃ 代替 -->
                <button class="o-nav__cat-toggle" type="button" aria-label="展开或收起分类列表">
                    <svg class="icon-collapse" xmlns="http://www.w3.org/2000/svg" width="16" height="16" viewBox="0 0 24 24" fill="none">
                        <path d="M0.250616 0.205025C2.94745e-05 0.455612 -0.0208527 0.848918 0.187969 1.12329L0.250616 1.19497L5.26695 6.21131C5.36737 6.31173 5.38172 6.46563 0.30999 6.58133L5.26695 6.63558L0.205025 11.6975C-0.0683418 11.9709 -0.0683418 12.4141 0.205025 12.6875C0.455612 12.938 0.848918 12.9589 1.12329 12.7501L1.19497 12.6875L6.2569 7.62553C6.88585 6.99658 6.91895 5.99741 6.35621 5.32949L6.2569 5.22136L1.24057 0.205025C0.967198 -0.0683418 0.523983 -0.0683418 0.250616 0.205025Z" fill="currentColor" fill-rule="evenodd" transform="matrix(0,-1,1,0,5.55376,15.3774)" />
                    </svg>
                    <svg class="icon-expand" xmlns="http://www.w3.org/2000/svg" width="16" height="16" viewBox="0 0 24 24" fill="none">
                        <path d="M0.250616 0.205025C2.94745e-05 0.455612 -0.0208527 0.848918 0.187969 1.12329L0.250616 1.19497L5.26695 6.21131C5.36737 6.31173 5.38172 6.46563 0.30999 6.58133L5.26695 6.63558L0.205025 11.6975C-0.0683418 11.9709 -0.0683418 12.4141 0.205025 12.6875C0.455612 12.938 0.848918 12.9589 1.12329 12.7501L1.19497 12.6875L6.2569 7.62553C6.88585 6.99658 6.91895 5.99741 6.35621 5.32949L6.2569 5.22136L1.24057 0.205025C0.967198 -0.0683418 0.523983 -0.0683418 0.250616 0.205025Z" fill="currentColor" fill-rule="evenodd" transform="matrix(0,1,1,0,5.55376,8.62259)" />
                    </svg>
                </button>
            </div>
        </div>

        <!-- ===== 快捷链接栏（社区 Tab 为 4 列 + 本栏）⚠️ 可选模块，默认注释掉 =====
             挂载条件：看需求。开发 Tab 这类 5 列场景通常不挂；社区 Tab 这类
             「4 列 + 右侧栏」场景才挂。需要时取消下面整段注释。
             四条硬规则：① 栏首必须有「快捷链接」标题（规格同分类列头）；
                          ② 纯文本项 = 灰底按钮块（宽 342px 固定、高 40px、圆角 4px、fill3 底、
                             块内 link 距左 24px、上下居中）+ 内部 OLink medium
                             （整体高 34px、上下 padding 5px、左右 0、14/22 Regular、
                              link1 → hover link2+下划线 → active link3+下划线、后缀图标 24×24、图文间距 4px）；
                             ⚠️ <a> 是灰底块，内部包 <span class="o-nav__quick-link-text"> 承载 OLink 规格；
                          ③ 栏内整体左对齐（align-items: flex-start，标题/色块/卡片同一左缘），
                             整栏 margin-left:auto 靠栅格内容区右缘（PC 距右 216px）；
                          ④ 图+文本卡片没有背后的色块（无背景/无圆角/无内边距） -->
        <!--
        <div class="o-nav__quick">
            <div class="o-nav__quick-title"><span>快捷链接</span></div>
            <a class="o-nav__quick-link" href="#">
                <span class="o-nav__quick-link-text">企业签署CLA流程</span>
            </a>
            <a class="o-nav__quick-link" href="#">
                <span class="o-nav__quick-link-text">
                    CLA-FAQ
                    <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none">
                        <path d="…" fill="currentColor" fill-rule="evenodd" />
                    </svg>
                </span>
            </a>
            <a class="o-nav__quick-link" href="#">
                <span class="o-nav__quick-link-text">开发者日历</span>
            </a>
            <a class="o-nav__quick-card" href="#">
                <img src="…" alt="openEuler Developer Day 2025 活动示意配图，上线前替换真实素材">
                <span class="o-nav__quick-card-body">
                    <span class="o-nav__quick-card-title">openEuler Developer Day 2025</span>
                    <span class="o-nav__quick-card-desc">时间: 2024/07/26 | 杭州</span>
                </span>
            </a>
        </div>
        -->
    </div>
</div>
```

> ⚠️ 条目状态类名共三种：默认（无类）/ `.is-disabled` + `aria-disabled`（禁用）；分类列 MB 端激活类 `.is-active`；触发器状态类名：`.is-current`（选中）+ `data-open`（锁定展开）+ `:hover`（临时展开）。
>
> 🚨 **条目没有 `.is-selected`（已选）类**（2026-10-15 用户确认）：面板条目点击即跳走，不存在持久选中态。禁止给条目加 `.is-selected` 或 `primary1` 文字色——那是顶导触发器 `is-current` 的语义，不是条目的。
>
> ⚠️ `.o-nav__cat-item-title` 内**必须再包一层 `.o-nav__cat-item-text`**：因为标题元素本身要参与 line-clamp 截断（`display: -webkit-box`），外链图标 / 红标签若直接放里面会被当作文本行计算行数、且 `flex` 布局与 `-webkit-box` 冲突。用内层 span 包文字、外层负责截断，图标与标签才能正常横排。

```css
/* ⚠️ 本模板不定义 :root 变量（清单见「五、颜色」节）。
   基类 .o-nav__dropdown 的定位 / 显隐 / 互斥 / 热区桥接 / MB 左对齐
   全部继承 floors/navigation-dropdown.md，此处只写修饰类与内部结构 */

/* 🔴🔴🔴 顶导骨架 + 触发器状态【本模板不定义，一处都不许写】 🔴🔴🔴
   .o-nav / .o-nav__left / .o-nav__logo / .o-nav__menu / .o-nav__item /
   .o-nav__link（基础几何 + hover / 展开态 / 选中态全部规则）/
   .o-nav__right / .o-nav__search / .o-nav__source / .o-nav__icon
   ——唯一定义源是 floors/navigation.md「菜单项（触发器）完整样式」（.o-nav__* 命名），
   与平铺式模板共用同一份。生成时必须从该处整段取用，
   ⛔ 禁止自行改写、禁止凭印象重写、禁止增删属性（含 align-self: flex-end 这类易漏项），
   ⛔ 禁止用 :has() 把 hover / is-current 基础态收窄（无下拉面板的普通菜单项同样要有这些态，
      2026-10-15 用户两次打回，详见 navigation.md 踩坑 #17）。
   本模板的 CSS 从「包含块上移 + .o-nav__dropdown--categorized 修饰类」开始，往上一律不碰。 */

/* 🔴 hard-constraints §2 铁律：全局字体一律鸿蒙黑体（HarmonyOS Sans / HarmonyHeiTi），
   禁止 PingFang / 苹方 / 思源 / 微软雅黑 / Inter / Roboto / 系统默认字体或任何其他字族 */
body { font-family: 'HarmonyOS Sans', 'HarmonyHeiTi'; }

/* ===== 包含块上移：触发器退化为 static，面板相对顶导本体（sticky）定位 ===== */
.o-nav__item:has(> .o-nav__dropdown--categorized) { position: static; }

/* ===== 分类平铺式面板（多列 + 快捷链接栏） ===== */
/* ⚠️ 必须写成复合选择器（两类同级），优先级才高于基类 .o-nav__dropdown，
   否则 flex-direction / gap / padding / left / transform 覆盖不生效 */
.o-nav__dropdown.o-nav__dropdown--categorized {
    display: flex;
    flex-direction: row;                    /* ⭐ 多列横向排布 */
    flex-wrap: wrap;                        /* ⭐ 超宽时整列折行，禁止压缩列宽 */
    align-items: flex-start;                /* 列头顶部对齐，禁止 stretch 拉成等高 */
    gap: 24px;                              /* ⭐ 列间距（条目间左右间距）≥1681px */
    top: 100%;                              /* ⭐ 与顶导零间距，紧贴底边像抽屉一样展开（2026-10-15 用户确认，覆盖基类 calc(100% + 4px)） */
    left: 0;                                /* ⭐ 通栏：相对顶导贴屏幕左缘 */
    right: 0;                               /* ⭐ 通栏：相对顶导贴屏幕右缘 */
    width: auto;                            /* ⭐ 由 left/right 撑满页面宽度 */
    height: auto;                           /* ⚠️ 禁止写死高度 */
    padding: 24px 216px;                    /* 上下 24px（推断，待确认 #2）/ 左右 216px PC 栅格边距 */
    border-radius: 0 0 4px 4px;             /* ⭐ 上圆角压平为 0：零间距紧贴顶导，保留上圆角会在左上/右上露月牙缺口（2026-10-15 用户确认） */
    transform: translateY(-8px);            /* ⚠️ 必须覆盖基类的 translate(-50%, -8px) */
    /* background / box-shadow / position / z-index 全部继承基类 */
}
/* 零间距后基类的 ::before 热区桥接已无用（无间隙可穿），显式关掉 */
.o-nav__dropdown.o-nav__dropdown--categorized::before { display: none; }

/* 显隐三规则：同步覆盖 transform（特异性 (0,4,0) 高于基类 (0,3,0)） */
.o-nav__item:hover > .o-nav__dropdown.o-nav__dropdown--categorized,
.o-nav__item:focus-within > .o-nav__dropdown.o-nav__dropdown--categorized,
.o-nav__item[data-open="true"] > .o-nav__dropdown.o-nav__dropdown--categorized {
    opacity: 1; visibility: visible; transform: translateY(0);
}

/* 单开互斥：同步覆盖 transform（无 translateX） */
.o-nav__menu:has(> .o-nav__item:hover > .o-nav__dropdown) > .o-nav__item:not(:hover) > .o-nav__dropdown.o-nav__dropdown--categorized,
.o-nav__menu:has(> .o-nav__item:focus-within > .o-nav__dropdown) > .o-nav__item:not(:focus-within) > .o-nav__dropdown.o-nav__dropdown--categorized {
    opacity: 0; visibility: hidden; transform: translateY(-8px);
}

/* ===== 分类列 ===== */
.o-nav__cat {
    display: flex;
    flex-direction: column;
    flex: none;                             /* ⭐ 禁止被压缩（防文字竖排） */
    min-width: 0;
}
.o-nav__cat-title {
    margin-bottom: 12px;                    /* 列头与条目列表间距（间距 3，推断值） */
    font-size: 16px;                        /* ⭐ >1440px 基准档；≤1440px 见响应式 2 */
    line-height: 24px;
    font-weight: 400;
    color: var(--o-color-info4);            /* ⭐ info4（2026-10-15 由 info3 改为 info4） */
    cursor: default;                        /* ⛔ PC 端列头无交互 */
}
.o-nav__cat-title span { display: block; }

.o-nav__cat-list {
    display: flex;
    flex-direction: column;
    gap: 24px;                              /* ⭐ 条目间上下间距（PC 全档 24px） */
}

/* ===== 条目（透明容器，hover 只改文字颜色、无背景、不切字重） ===== */
.o-nav__cat-item {
    display: flex;
    flex-direction: column;
    gap: 4px;                               /* 标题↔说明间距（间距 1） */
    text-decoration: none;
    /* ⛔ 禁止 background / border-radius：本样式无色块 */
}
.o-nav__cat-item-title {
    display: flex;
    align-items: center;
    gap: 8px;                               /* 标题↔外链图标/红标签间距（间距 2，用户 2026-10-15 指定） */
    font-size: 16px;                        /* ⭐ >1440px 基准档；≤1440px 见响应式 2 */
    line-height: 24px;
    font-weight: 600;                       /* ⭐ Semibold 全状态统一（默认即 Semibold） */
    color: var(--o-color-info1);
}
/* 截断规则 1：标题最多 2 行（截断放在内层文字上，图标/标签不占行） */
.o-nav__cat-item-text {
    display: -webkit-box;
    -webkit-box-orient: vertical;
    -webkit-line-clamp: 2;
    overflow: hidden;
    min-width: 0;
}
.o-nav__cat-item-title svg { flex: none; width: 24px; height: 24px; }   /* 外链图标 24×24，currentColor 跟随标题态 */
.o-nav__cat-item-desc {
    font-size: 12px;
    line-height: 18px;
    font-weight: 400;
    color: var(--o-color-info2);
    /* 截断规则 2：选项最多 6 行 */
    display: -webkit-box;
    -webkit-box-orient: vertical;
    -webkit-line-clamp: 6;
    overflow: hidden;
}

/* 条目两态 + 禁用：只改颜色，字重恒为 Semibold（禁用态是唯一例外） */
/* ⛔ 无 .is-selected 规则：面板条目点击即跳走，不存在持久选中态（2026-10-15 用户确认） */
.o-nav__cat-item:hover .o-nav__cat-item-title { color: var(--o-color-primary2); }
.o-nav__cat-item:active .o-nav__cat-item-title { color: var(--o-color-primary3); }
.o-nav__cat-item.is-disabled .o-nav__cat-item-title { font-weight: 400; color: var(--o-color-info4); }
.o-nav__cat-item.is-disabled .o-nav__cat-item-desc { color: var(--o-color-info4); }
.o-nav__cat-item.is-disabled { cursor: not-allowed; }

/* 红色小标签 = OTag（components/tag.md）fill + danger + small */
.o-tag {
    display: inline-flex;
    align-items: center;
    justify-content: center;
    flex: none;
    height: 20px;                           /* OTag small 高 20px */
    padding: 0 6px;                         /* ⚠️ tag.md 垂直 padding 2px 与 20px 高冲突，取 0（待确认 #10） */
    border-radius: 4px;                     /* radius_control-m */
    font-size: 12px;
    line-height: 18px;
    font-weight: 400;
    white-space: nowrap;
}
.o-tag--fill.o-tag--danger { background: var(--o-color-danger1); color: var(--o-color-white); }

/* 列底部「更多项目」入口 = OButton text large（components/button.md）：高 40px / 水平内边距 0 /
   16px-24px Regular / 默认 info1 → hover primary2 → 按下 primary3；图标 24×24 在右侧，图文间距 8px */
.o-btn {
    display: inline-flex; align-items: center; justify-content: center;
    border: none; background: none; cursor: pointer; font-family: inherit;
}
.o-btn--large { height: 40px; padding: 8px 24px; border-radius: 100px; font-size: 16px; line-height: 24px; font-weight: 400; }
.o-btn--text { padding-left: 0; padding-right: 0; min-width: 0; gap: 8px; color: var(--o-color-info1); }
.o-btn--text:hover { color: var(--o-color-primary2); }
.o-btn--text:active { color: var(--o-color-primary3); }
.o-btn--text:disabled { color: var(--o-color-info4); cursor: not-allowed; }
.o-btn svg { flex: none; width: 24px; height: 24px; }        /* large 图标 24×24（icon_size_control-m） */
.o-nav__cat-more { align-self: flex-start; }                /* 列是 column flex，贴左缘不拉宽 */

/* 左列最后一项的切换箭头（导航项过多时）：图标取自 assets 上/下箭头 */
/* ⚠️ 仅 ≤840px 的 MB 双列布局使用：PC 档必须默认 display: none 隐藏，
   否则箭头会裸露在「项目」列末尾（2026-10-16 验收时复现并修正） */
.o-nav__cat-toggle {
    display: none;                        /* ⭐ PC 默认隐藏，仅在 ≤840px 媒体查询里恢复 */
    align-self: flex-start;
    width: 16px;
    height: 16px;
    padding: 0;
    border: none;
    background: none;
    color: var(--o-color-info3);
    cursor: pointer;
}
.o-nav__cat-toggle svg { display: block; width: 16px; height: 16px; }
.o-nav__cat-toggle .icon-collapse { display: block; }      /* 展开态 → ⌃（点击收起） */
.o-nav__cat-toggle .icon-expand { display: none; }
.o-nav__dropdown--categorized.is-collapsed .o-nav__cat-toggle .icon-collapse { display: none; }
.o-nav__dropdown--categorized.is-collapsed .o-nav__cat-toggle .icon-expand { display: block; }   /* 折叠态 → ⌄ */

/* ===== 快捷链接栏（⚠️ 可选模块：看需求挂载，默认不出现） ===== */
/* ⭐ 栏内整体左对齐（标题 / 色块 / 卡片同一左缘）；整栏 margin-left:auto 靠栅格内容区右缘 */
.o-nav__quick {
    display: flex;
    flex-direction: column;
    align-items: flex-start;                /* ⭐ 栏内左对齐（⛔ 不是每个选项各自右对齐） */
    gap: 8px;                               /* 按钮块间距（间距 2，⚠️ 推断值待确认 #5） */
    flex: none;                             /* ⭐ 禁止被压缩 */
    width: 342px;                           /* ⭐ 栏宽固定（与色块同宽） */
    margin-left: auto;                      /* ⭐ 整栏靠右（PC 距视口右 216px） */
}
/* 栏标题：规格同分类列头（>1440px 16px/24px · ≤1440px 14px/22px / Regular / info4） */
.o-nav__quick-title {
    margin-bottom: 4px;
    font-size: 16px;
    line-height: 24px;
    font-weight: 400;
    color: var(--o-color-info4);
    cursor: default;
}
.o-nav__quick-title span { display: block; }

/* ① 灰底按钮块（外层容器）：宽 342px 固定 / 高 40px / 圆角 4px / fill3 底（⛔ 不得去掉）/
      块内 link 距左 24px + 上下居中 */
.o-nav__quick-link {
    display: flex; align-items: center;
    width: 342px; height: 40px;
    padding-left: 24px;
    border-radius: 4px;
    background: var(--o-color-fill3);
    text-decoration: none;
}
/* ② OLink medium（内层文字，components/link.md）：整体高 34px / 上下 padding 5px / 左右 0 /
      文字 14px-22px Regular；link1 → hover link2 + 下划线 → active link3 + 下划线；
      后缀图标 24×24（颜色随文字）、图文间距 4px（gap-1） */
.o-nav__quick-link-text {
    display: inline-flex; align-items: center; gap: 4px;
    height: 34px; padding: 5px 0;
    font-size: 14px; line-height: 22px; font-weight: 400;
    color: var(--o-color-link1);                            /* OLink Enabled */
}
.o-nav__quick-link:hover .o-nav__quick-link-text { color: var(--o-color-link2); text-decoration: underline; }
.o-nav__quick-link:active .o-nav__quick-link-text { color: var(--o-color-link3); text-decoration: underline; }
.o-nav__quick-link-text svg { flex: none; width: 24px; height: 24px; }   /* OLink 后缀图标 24×24 */

/* 带图 + 文本卡片：左图右文、图文垂直居中，图文间距 8px；
   ⭐ 无背景、无圆角、无内边距（图+文本的快捷链接没有背后的色块） */
.o-nav__quick-card {
    display: flex;
    flex-direction: row;                    /* ⭐ 左图右文 */
    align-items: center;                    /* ⭐ 文字与图水平居中 */
    gap: 8px;                               /* ⭐ 图↔文本内容间距 8px */
    width: 342px;                           /* 与色块同宽，保证栏内左缘一致 */
    text-decoration: none;
}
.o-nav__quick-card img {
    flex: none;
    width: 121px;                           /* 16:9 → 高 68px 时宽 121px */
    height: 68px;
    object-fit: cover;
    border-radius: 4px;                     /* 图片自身圆角可保留（不是卡片色块） */
}
.o-nav__quick-card-body { display: flex; flex-direction: column; gap: 4px; min-width: 0; }
.o-nav__quick-card-title {
    display: block;
    font-size: 16px;
    line-height: 24px;
    font-weight: 600;
    color: var(--o-color-info1);
}
.o-nav__quick-card-desc {
    display: block;
    font-size: 12px;
    line-height: 18px;
    font-weight: 400;
    color: var(--o-color-info3);
}
/* hover / 按下态与左侧分类条目保持完全一致：只改标题颜色，无背景、无字重变化 */
.o-nav__quick-card:hover .o-nav__quick-card-title { color: var(--o-color-primary2); }
.o-nav__quick-card:active .o-nav__quick-card-title { color: var(--o-color-primary3); }

/* ===== 响应式 1：1680~1441px 面板边距不动（仍跟随栅格 216px，字号不动） ===== */
@media (max-width: 1680px) and (min-width: 1441px) {
    .o-nav__dropdown.o-nav__dropdown--categorized { padding-left: 216px; padding-right: 216px; }
}

/* ===== 响应式 2a：≤1440px 列头 / 栏标题 / 条目标题降为 14px/22px（只降字号，颜色不变） ===== */
@media (max-width: 1440px) {
    .o-nav__cat-title,
    .o-nav__quick-title { font-size: 14px; line-height: 22px; }
    .o-nav__cat-item-title { font-size: 14px; line-height: 22px; }
}

/* ===== 响应式 2b：1440~1201px 面板边距不动（仍跟随栅格 216px） ===== */
@media (max-width: 1440px) and (min-width: 1201px) {
    .o-nav__dropdown.o-nav__dropdown--categorized { padding-left: 216px; padding-right: 216px; }
}

/* ===== 响应式 3：1200~841px 列距 16px（字号已在 ≤1440px 档降为 14px，勿重复写） ===== */
@media (max-width: 1200px) and (min-width: 841px) {
    .o-nav__dropdown.o-nav__dropdown--categorized {
        gap: 16px;                          /* 条目间左右间距 24→16px */
        padding-left: 216px;                /* 跟随栅格（该档未规定，须显式写出） */
        padding-right: 216px;
    }
    /* ⚠️ 条目上下间距 24px、列结构、通栏定位、字号全部保持不变 */
}

/* ===== 响应式 4：840~601px 折叠屏 / Pad 竖屏 → 左一级导航列 + 右内容区 ===== */
@media (max-width: 840px) and (min-width: 601px) {
    .o-nav__dropdown.o-nav__dropdown--categorized {
        display: grid;
        grid-template-columns: minmax(32px, 144px) 1fr;   /* 左一级导航列 + 右内容区 */
        gap: 12px;
        left: 0;
        right: 0;                           /* 通栏撑满（MB 端同为通栏） */
        width: auto;
        padding: 12px 24px;                 /* 上下 12px / 左右 24px MB 栅格边距 */
        transform: translateY(-8px);        /* 通栏定位，去掉 translateX(-50%) */
    }
    /* 分类列拆成网格子项：列头进第 1 列，条目列表进第 2 列 */
    .o-nav__dropdown.o-nav__dropdown--categorized .o-nav__cat { display: contents; }
    .o-nav__cat-toggle { display: block; } /* ⭐ MB 档恢复显示切换箭头（PC 档已默认隐藏） */
    .o-nav__cat-title { grid-column: 1; margin-bottom: 8px; cursor: pointer; }
    .o-nav__cat:not(.is-active) .o-nav__cat-list { display: none; }
    .o-nav__cat.is-active .o-nav__cat-list {
        display: flex;
        grid-column: 2;
        grid-row: 1;                        /* ⭐ 右内容区固定顶格，不随分类位置下沉 */
        max-width: 40%;                     /* 右侧内容区最大占整屏 40%（黄金分割） */
    }
    .o-nav__quick {
        grid-column: 1 / -1;
        flex-direction: row;
        flex-wrap: wrap;
        justify-content: flex-start;        /* ⭐ MB 端栏内同样左对齐（贴 MB 栅格左缘） */
        align-items: flex-start;
        width: auto;                        /* 取消 PC 端 342px 固定宽 */
        min-width: 0;
        margin-left: 0;
    }
    .o-nav__quick-link { width: 100%; }     /* MB 端色块撑满栏宽 */
    .o-nav__quick-card { width: 100%; }
    .o-nav__quick-title { flex-basis: 100%; }
}

/* ===== 响应式 5：≤600px 手机竖屏（MB 栅格边距同为 24px） ===== */
@media (max-width: 600px) {
    .o-nav__dropdown.o-nav__dropdown--categorized {
        padding-left: 24px;                 /* MB 栅格边距 */
        padding-right: 24px;
    }
    .o-nav__cat-title { white-space: normal; }   /* 菜单最大宽度 144px，超长自动换行 */
    .o-nav__cat.is-active .o-nav__cat-list { max-width: none; }  /* 手机不按 40% 收窄（用户确认） */
    /* 折叠态：左列最小宽度 32px，仅留切换箭头 */
    .o-nav__dropdown.o-nav__dropdown--categorized.is-collapsed { grid-template-columns: 32px 1fr; }
    .o-nav__dropdown.o-nav__dropdown--categorized.is-collapsed .o-nav__cat-title span { display: none; }
}
```

> ⚠️ **单开互斥的 transform 覆盖**：基类互斥规则写在 navigation-dropdown.md（用 `translate(-50%, -8px)`）；本文件已在互斥规则中同步覆盖为 `translateY(-8px)`（见上方 CSS），因此互斥时面板同样保持通栏隐藏态，不会错位。≤840px 三档选择器优先级更高且同样覆盖 `transform`，无需另写。

**交互 JS 最小实现**（在 navigation-dropdown.md §八 的 JS 基础上，追加分类平铺式专属逻辑）：

```js
// 1) MB 端左列分类切换（同一时刻只有一个分类展示在右内容区）
document.querySelectorAll('.o-nav__dropdown--categorized').forEach((panel) => {
    panel.querySelectorAll('.o-nav__cat-title').forEach((title) => {
        title.addEventListener('click', () => {
            if (window.matchMedia('(min-width: 841px)').matches) return;   // PC 端列头无交互
            panel.querySelectorAll('.o-nav__cat').forEach((c) => { c.classList.remove('is-active'); });
            title.closest('.o-nav__cat').classList.add('is-active');
        });
    });
    // 2) 导航项过多：左列最后一项的切换箭头 → 展开/收起左列（折叠态最小宽度 32px）
    const toggle = panel.querySelector('.o-nav__cat-toggle');
    if (toggle) {
        toggle.addEventListener('click', () => { panel.classList.toggle('is-collapsed'); });
    }
});
```

> ⚠️ click 锁定 / 单开互斥 / `link.blur()` 兜底仍由 navigation-dropdown.md §八 的 JS 负责，两段 JS 同时生效，禁止互相覆盖。

---

## 十、踩坑记录（面板部分；触发器踩坑见 floors/navigation.md）

> 🔴 **触发器（顶导菜单项）相关踩坑已统一收入 [floors/navigation.md](navigation.md) 踩坑记录**：背景色块 #18、指示线形态/漏写 #9、`is-current` 迁移 #10、字重与 `:has()` 收窄 #17、hover 抢选中态 #19。本表只列**分类平铺式面板**踩坑。

| # | 问题现象 | 根因 | 正确做法 |
|---|---------|------|---------|
| 1 | **分类平铺式被做成垂直单列** | 照搬平铺式 `flex-direction: column` | 分类平铺式 = `flex-direction: row` + `flex-wrap: wrap`，一列一个分类 |
| 2 | **修饰类漏挂，面板变成平铺式** | 只写 `class="o-nav__dropdown--categorized"`，漏了基类 | 必须 `class="o-nav__dropdown o-nav__dropdown--categorized"` 两类同挂；漏基类会导致定位/显隐/互斥全部失效 |
| 3 | **修饰类覆盖不生效** | 用单类选择器写修饰类，优先级低于基类 | 必须写复合选择器 `.o-nav__dropdown.o-nav__dropdown--categorized` |
| 4 | **列头被做成可点击链接（PC 端）** | 给列头加了 `<a>` / hover 态 | PC 端列头是纯标题（`cursor: default`）；只有 MB 端列头才是可点击的一级导航项 |
| 5 | **条目 hover 出现背景色块 / 字重跳动** | 照搬平铺式的 `control2-light` 色块；或给 hover 写了 `font-weight: 600` | 本样式 hover **只改文字颜色**（`primary2`），字重恒为 Semibold 不切换，禁止任何背景；也就不存在 control2/control3 取值歧义 |
| 6 | **hover 文字用了 primary1** | 三色分级没分清 | hover = `primary2`、按下 = `primary3`；`primary1` 只属于顶导触发器 `is-current`，条目没有选中态 |
| 7 | **说明文字 hover 也变色** | 给 `.o-nav__cat-item-desc` 加了 hover 规则 | 说明所有状态保持 `info2`，仅禁用变 `info4` |
| 8 | **禁用态还是 Semibold** | 禁用规则漏写字重回落 | 禁用 = Regular(400) + `info4`（**禁用态是唯一允许回落 Regular 的状态**） |
| 9 | **红标签用了 stroke 变体或非 danger 色** | OTag 变体选错 | 必须 fill + danger + small：底 `--o-color-danger1`、白字、12px/18px、圆角 4px、高 20px、水平 padding 6px |
| 10 | **外链图标漏挂 / 手写 SVG / 用字符 ↗ 代替** | 图标没从 assets Read，或图省事手写 path | 外链条目必须挂 `icon-外链.svg`（Read 后逐属性内联），16×16 + `fill="currentColor"` 跟随标题态；禁止 `<img src>`、CSS mask、色块占位、字符代替 |
| 11 | **列宽被压缩、文字竖排** | 列没写 `flex: none` | 每列必须 `flex: none` + `min-width: 0`；靠 `flex-wrap: wrap` 整列折行，禁止压缩列宽 |
| 12 | **列头与条目列表间距不一致** | 用 `margin-top` 加在列表上，MB 档忘了覆盖 | 用列头 `margin-bottom`，MB 档单独覆盖为 8px |
| 13 | **标题 line-clamp 失效 / 图标掉到第二行** | 图标与文字直接混在 `-webkit-box` 容器里 | 标题外层 `display: flex` 负责横排，文字包 `.o-nav__cat-item-text` 且 line-clamp 写在它身上，图标与标签 `flex: none` |
| 14 | **省略号不出现、文字直接溢出** | line-clamp 缺 `overflow: hidden` | 四件套齐全：`display:-webkit-box` + `-webkit-box-orient:vertical` + `-webkit-line-clamp:N` + `overflow:hidden` |
| 15 | **截断后信息丢失** | 没挂 tooltip | 截断元素必须挂 tooltip（复用 OPopover），内容取完整原文 |
| 16 | **MB 右内容区随分类位置下沉** | `grid-row` 未固定 | `.is-active .o-nav__cat-list` 必须写 `grid-row: 1` |
| 17 | **MB 右内容区被压成 40% 导致文字折行** | 把 40% 也应用到了手机竖屏 | 40% 只用于 840~601px；≤600px 覆盖为 `max-width: none`（用户确认） |
| 18 | **折叠态左列仍占 144px** | 漏写 `.is-collapsed` 覆盖 | `.is-collapsed` 时 `grid-template-columns: 32px 1fr` |
| 19 | **PC 端列头出现 hover 变色** | 把 MB 的 `cursor: pointer` / hover 规则漏在 PC 档 | PC 列头 `cursor: default`、无 hover 规则；MB 才加 |
| 20 | **快捷链接栏被 flex 压缩变形** | 漏写 `flex: none` | `.o-nav__quick` 必须 `flex: none` + `width: 342px`（PC 端）；MB 端覆盖为 `width: auto` |
| 21 | **快捷链接栏做成"每个选项各自右对齐"** | 把 `align-items: flex-end` 当成"靠右对齐"，导致标题 / 色块 / 卡片左缘参差不齐 | 对齐要分两层：**栏内 `align-items: flex-start` 左对齐**（标题 / 色块 / 卡片同一左缘）+ **整栏 `margin-left: auto` 靠右**（贴栅格内容区右缘）。⛔ 不是每个选项单独右对齐 |
| 22 | **OLink 规格写在了灰底块本体上**（块高 40px + 左右 padding 24px 当成 OLink 的规格） | 把「块」与「OLink」合并成一个元素 | OLink medium 是**内层独立元素**：整体高 **34px**、Auto Layout Padding **上 5px / 下 5px / 左 0px / 右 0px**。块的 40px 高 / 24px 距左属于外层容器。HTML 必须是 `<a class="o-nav__quick-link"><span class="o-nav__quick-link-text">…</span></a>` |
| 23 | **给图+文本卡片也刷了灰底色块** | 误以为栏内所有项都有 `fill3` 底 | 只有**纯文本项**有灰底块；**图+文本卡片没有背后的色块**（无背景 / 无圆角 / 无内边距） |
| 24 | **快捷链接栏宽度用了 min-width: 200px 或自创值** | 凭感觉估宽 | 栏宽与色块宽统一 **342px 固定**（用户规格），MB 端才覆盖为 `width: auto` + 子项 `width: 100%` |
| 25 | **快捷链接纯文本项只做了一层**：要么把灰底整块删掉只留裸 OLink 文字（丢块），要么保留灰底却把 40px / 24px 左右 padding 写在 link 本体上（丢 link.md 规格） | 把「块」与「OLink」当成二选一——二者是**两层叠加** | 正确 = **灰底按钮块（`fill3` 底 / 宽 342px / 高 40px / 圆角 4px / link 距左 24px / 上下居中）+ 内层 OLink medium**（整体高 **34px**、上下 padding **5px**、左右 padding **0px**、14px/22px Regular、`link1` → hover `link2` + 下划线 → active `link3` + 下划线、后缀图标 24×24、图文间距 4px）。⛔ **两层缺一不可**（2026-10-15 用户验收两次打回过） |
| 26 | **面板居中后左侧溢出屏幕 / 第一列被挤出** | 宽面板（5 列 + 快捷栏约 1400px）沿用基类「居中于触发器」定位，`left: 50%` + `translateX(-50%)` 以触发器为中心向两侧展开，左侧整体溢出视口 | 改为**通栏撑满页面宽度**：`.o-nav__item:has(> .o-nav__dropdown--categorized) { position: static; }` 上移包含块到顶导本体，面板 `left: 0; right: 0; width: auto`，内部 `padding: 24px 216px` 对齐栅格 |
| 27 | **面板条目内容左缘与页面内容区、顶导内容区三处错位** | 面板 padding 用了自创边距（64px / 40px / 12px），未跟随栅格 | padding-left/right 必须取 hard-constraints §4.1 的栅格边距：PC **216px**（1488+216×2=1920）、MB **24px**（312+24×2=360） |
| 28 | **显隐时面板横向跳动/偏移** | 只写了修饰类的 `transform`，基类 `.o-nav__item:hover > .o-nav__dropdown`（0,3,0）优先级更高，`translate(-50%, 0)` 仍然生效 | 显隐三规则 + 互斥规则各补一条带 `.o-nav__dropdown--categorized` 的版本（0,4,0），transform 统一改为 `translateY(0)` / `translateY(-8px)` |
| 29 | **活动卡片图片变形 / 未替换占位图上线** | 未设 `object-fit` / 未加注释 | 图 `object-fit: cover` + `width/height 121/68px`，`<img>` 必须带 alt 与「示意配图，上线前替换真实素材」注释 |
| 30 | **条目标题字重随交互从 Regular 跳到 Semibold** | 把 `font-weight: 600` 写在 hover / active 规则里，默认态留 400 | 默认态即 `font-weight: 600`，hover / 按下**只写 `color`**；唯一例外是禁用态回落 400 |
| 31 | **分类列头用了 info3 / 14px 一档到底** | 沿用旧值 | 列头颜色统一 **info4**；字号按 **>1440px 16px / ≤1440px 14px** 分档，用一条 `@media (max-width: 1440px)` 同时覆盖列头 / 栏标题 / 条目标题 |
| 32 | **快捷链接栏默认就被挂上 / 漏了栏标题 / 没有靠右对齐** | 把它当成必填模块；或挂载时漏写 `.o-nav__quick-title`、漏写 `margin-left: auto` | ① **可选模块，默认不渲染**（HTML 中默认注释掉）；② 挂载时栏首必须有「快捷链接」标题（规格同列头）；③ 整栏 `margin-left: auto` 靠右 + 栏内 `align-items: flex-start` 左对齐 |
| 33 | **MB 切换箭头用字符 ⌃ / 「更多项目」用字符 > 或做成 `<a>`** | 图省事用文本字符，或当成链接 | ① MB 切换箭头必须内联 `icon-上箭头.svg` / `icon-下箭头.svg`，16×16 + `fill="currentColor"`；② 「更多项目」必须是 `<button class="o-btn o-btn--text o-btn--large">`（OButton text + large，button.md），右箭头 24×24 在右侧，禁止字符代替 |
| 34 | **外链图标用了 16×16 / 与文本间距 4px** | 沿用硬约束 §5 的默认图标档，未按用户指定改 | 外链图标 **24×24**，与文本间距 **8px**；红标签↔文本间距同为 **8px**（`.o-nav__cat-item-title { gap: 8px }`） |
| 35 | **快捷链接纯文本不是蓝色（发黑）** | `:root` 漏定义 `--o-color-link1/2/3`，`var()` 整条声明失效，文字回落成继承色 info1 | 引用任何语义 Token 前先在 `:root` 定义；link1 #002FA7 / link2 #5177CA / link3 #002793，来源 `opendesign-tokens/references/tokens-openeuler.md` 品牌色·链接色表 |
| 36 | **图+文本卡片图文顶部对齐 / 图高 36px / 描述用了 info2** | 沿用旧值 | 图文**垂直居中**（`align-items: center`）、图 **121×68**、描述色 **`--o-color-info3`**、标题 **16px/24px** Semibold |
| 37 | **验收页与模板规格不一致**（模板改了验收页没同步；或验收页把可选模块也注释掉导致「验收时看不到」；图例 `fixed` 在右下角挡住快捷链接） | 两份文件靠人工记忆同步，没有约定 | ① 验收页必须**默认渲染所有可选模块**（模板仍默认注释，两处刻意不同）；② 模板每轮修正后必须同步验收页并做双向 diff；③ 验收页图例 / 标注等辅助元素禁止 `fixed` 遮挡验证区，默认 `<details>` 收起或移到不遮挡位置 |
| 38 | **图+文本卡片没有任何 hover / active 态** | 只给左侧分类条目写了状态规则，右侧同类元素漏写 | 同结构同类元素状态集合必须一致：卡片 hover 标题 → `--o-color-primary2`、按下 → `--o-color-primary3`，与左侧分类条目完全相同（无背景、无字重变化，图与描述行不变） |
| 39 | **描述文案延展成两行时挤压其他条目** | 描述只有 1 行时高度塌陷，无预留空间 | `.o-nav__cat-item-desc { min-height: 36px }`（= 2 × 18px 行高），文案 1 行时也占两行空间，后续延展不挤布局；⛔ 不是把 line-clamp 从 6 改成 2 |
| 40 | **给面板条目加了选中态**（`.is-selected` / `primary1` 文字） | 误把顶导触发器 `is-current` 的语义套到条目上；或照搬平铺式的 `:active` 点击态色块 | 面板条目**点击即跳走、无持久选中态**：条目状态只有默认 / hover / 按下 / 禁用四种，禁止 `.is-selected`、禁止 `primary1` 文字色。平铺式的 `control3-light` 色块是它的**点击态**（`:active`，按下瞬间），本样式按下态只改文字色 `primary3`、无背景。触发器才有 `is-current`（当前页 + 2px 指示线） |
| 41 | **面板与顶导之间留了 4px 间隙**（没用抽屉式贴合） | 直接继承平铺式基类的 `top: calc(100% + 4px)`，没看分类平铺式的差异表 | 分类平铺式必须 `top: 100%` **零间距**，紧贴顶导底边像抽屉一样展开（2026-10-15 用户确认）；同时把基类 `::before` 4px 热区桥接 `display: none`（无间隙可穿），并注意别把隐藏态 `translateY(-8px)` 误当成间距 |
| 42 | **顶导的下阴影看不见，交界处"平贴"** | 面板是 `.o-nav` 的后代，**子元素永远画在父级 box-shadow 之上**，零间距通栏面板整条盖住下阴影。⚠️ 曾经的错误修法：顶导 `z-index: 101` > 面板 100——**完全无效**（z-index 只在同一父级的定位兄弟之间比较，父子之间无意义；2026-10-15 实测顶导 101 时接缝处仍纯白 255） | 写 `.o-nav::after` 补偿层（`z-index: 102`）在顶导内部重画同一道阴影。⚠️ **补偿层必须 `top: 0` + `height: 100%` 与顶导等高**——阴影锐利度由源盒子高度决定，实测三种高度：`height: 0` 完全不渲染（死代码）、`height: 1px` 被 9px 模糊稀释成 254（几乎不可见）、等高才复刻顶导阴影强度（0.055 vs 基准 0.057）。详见 floors/navigation.md 踩坑 #15 |
| 43 | **顶导容器加了 overflow: hidden，下拉面板被裁掉** | 想"防止内容溢出"给顶导加了裁剪 | ⛔ 顶导容器禁止 `overflow: hidden`——裁剪发生在 z-index 层叠之前，层级再高也救不回被裁的面板。详见 floors/navigation.md 踩坑 #16 |
| 44 | **改了色值后 hover / 展开态"没反应"**（2026-10-15 实战踩中） | 把 `primary1` 改成 `primary2` 时**只改了 CSS 引用、漏在 `:root` 加定义**——`var()` 指向未定义变量时该属性**直接失效**（不是回落默认值），`color` 不生效，文字仍是默认色，看不出报错只能看到"hover 无变化" | 改完 CSS **必须跑定义/引用比对**（见「CSS 变量定义/引用比对」节的 comm 命令），差集为空才算过；缺的变量补进 `:root`，取值查 Appendix A / opendesign-tokens。同类历史坑：已确认 #17④ 漏定义 `link1/2/3` 导致右侧图标与快捷链接失效、"不是蓝色" |

---

## 十一、楼层自检清单

```
□ 面板是否同时挂载基类 + 修饰类（class="o-nav__dropdown o-nav__dropdown--categorized"）？
□ 修饰类是否写成复合选择器（优先级高于基类）？
□ 面板与顶导是否为【零间距】——top: 100%（不是 calc(100% + 4px)），展开时像抽屉一样紧贴顶导底边？
□ 面板 z-index 是否为 100，且顶导下阴影靠 .o-nav::after 补偿层（top: 0 + height: 100% + z-index: 102）保证可见？⛔ 不许写 height: 0（零高度不渲染 box-shadow）/ height: 1px（被模糊稀释）· ⛔ 不许靠"顶导 101 > 面板 100"（父子比 z-index 无效）？
□ 上圆角是否已压平为 border-radius: 0 0 4px 4px（零间距下保留 4px 上圆角会在左上/右上露月牙缺口）？
□ 基类的 ::before 4px 热区桥接是否已显式 display: none（零间距后无间隙可穿，桥接只剩副作用）？
□ 是否没把隐藏态的 translateY(-8px) 当成间距去改 top（-8px 只是入场位移，展开终态是 translateY(0)）？
□ 面板布局是否为 flex-direction: row + flex-wrap: wrap（多列平铺，非垂直单列）？
□ 面板 align-items 是否为 flex-start（列头顶部对齐，非 stretch 等高）？
□ 面板是否通栏撑满页面宽度（left: 0 + right: 0 + width: auto，包含块上移到顶导本体）？
□ 面板是否 height: auto（禁止写死高度 / 内部滚动条）？
□ 每列是否 flex: none + min-width: 0（禁止压缩列宽导致文字竖排）？
□ 列间距（条目间左右间距）是否为 24px（≥1681px）/ 16px（1200~841px）？
□ 条目间距（条目间上下间距）是否 PC 全档 24px？
□ 列头是否为 >1440px 16px/24px · ≤1440px 14px/22px / Regular / var(--o-color-info4)？
□ PC 端列头是否无交互（cursor: default、无 hover 规则、不是 <a>）？
□ 列头与条目列表间距是否为 12px（MB 档 8px）？
□ 条目标题是否为 >1440px 16px/24px · ≤1440px 14px/22px / var(--o-color-info1)？
□ 条目标题默认态字重是否即为 Semibold(600)，且 hover / 按下不切换字重（只改 color）？
□ 条目禁用态是否为唯一的 Regular(400)（+ info4、说明同步 info4）？
□ 条目说明是否 12px/18px Regular / var(--o-color-info2)（两档断点均不降字号）？
□ 条目说明是否 min-height: 36px 预留两行高度（2 × 18px 行高，文案 1 行时也占两行空间，延展成 2 行不挤压其他条目）？
□ 标题↔说明间距是否为 4px？
□ 条目默认是否透明（无背景、无圆角）？
□ 条目 hover 是否仅标题变色为 var(--o-color-primary2)（无背景色块、无字重变化、说明不变色）？
□ 条目按下是否为 var(--o-color-primary3)？
□ 条目是否【无选中态】——既没有 .is-selected 类、也没有 primary1 文字色规则（条目点击即跳走，不记录位置）？
□ ≤1440px 是否用一条 @media 同时把列头 / 栏标题 / 条目标题降为 14px/22px（只降字号不降颜色）？
□ 外链图标是否 24×24 + currentColor（跟随标题态）+ 从 assets Read 后逐属性内联（非手写 SVG / 非 <img> / 非字符 ↗）？
□ 外链图标↔文本间距、红标签↔文本间距是否都为 **8px**（`.o-nav__cat-item-title { gap: 8px }`）？
□ 「更多项目」右箭头是否取自 icon-右箭头.svg（16×16 + currentColor），未用字符 > 代替？
□ MB 切换箭头是否取自 icon-上箭头.svg / icon-下箭头.svg（16×16 + currentColor），未用字符 ⌃ 代替？
□ 红标签是否 OTag fill + danger + small（高 20px / 水平 padding 6px / 圆角 4px / 12px-18px / 白字 / 底 --o-color-danger1）？
□ 标题是否 line-clamp: 2、说明 line-clamp: 6（四件套齐全含 overflow: hidden）？
□ 截断元素是否挂 tooltip（复用 OPopover，内容取完整原文）？
□ 标题文字是否包在 .o-nav__cat-item-text 内（图标/标签 flex: none 不占行）？
□ 「更多项目」是否为 `<button class="o-btn o-btn--text o-btn--large">`（OButton text + large：高 40px / 水平 padding 0 / 16px-24px / info1 → primary2 → primary3）？
□ 「更多项目」右箭头是否取自 icon-右箭头.svg（24×24 + currentColor，在文字右侧，图文间距 8px），未用字符 `>` 代替？
□ 快捷链接栏是否为可选模块、默认不渲染（HTML 中默认注释掉）？
□ 挂载快捷链接栏时，栏首是否有「快捷链接」标题（.o-nav__quick-title，规格同分类列头）？
□ 快捷链接栏是否**栏内整体左对齐**（align-items: flex-start，标题 / 色块 / 卡片同一左缘），而不是每个选项各自右对齐？
□ 快捷链接栏是否**整栏靠右**（margin-left: auto，贴栅格内容区右缘，PC 距视口右 216px）？
□ 快捷链接栏宽 / 色块宽是否为 **342px 固定**（MB 端覆盖为 width: auto + 子项 width: 100%）？
□ 灰底按钮块是否为：宽 342px / 高 40px / 圆角 4px / `--o-color-fill3` 底 / link 距左 24px + 上下居中？
□ 内层 OLink medium 是否严格按 link.md：整体高 **34px**、Auto Layout Padding **上 5px / 下 5px / 左 0px / 右 0px**、文字 14px/22px Regular？
□ 内层 OLink medium 颜色是否 link1 → hover link2 + 下划线 → active link3 + 下划线（未用 info1 / primary2 自创色）？
□ **引用 link1/link2/link3 前，`:root` 是否已定义这三个变量**（漏定义会导致 var() 整条失效、文字发黑）？
□ 内层 OLink medium 后缀图标是否 24×24（icon_size_control-m，取 icon-外链.svg，颜色随文字）、图文间距 4px？
□ HTML 结构是否为 `<a class="o-nav__quick-link">`（灰底块）包 `<span class="o-nav__quick-link-text">`（OLink medium）两层？
□ 图+文本卡片是否**没有背后的色块**（无 background / 无 border-radius / 无 padding）？
□ 图+文本卡片是否左图右文 + **图文垂直居中**（align-items: center）、图文间距 8px、宽 342px（与色块同宽）？
□ 图+文本卡片图尺寸是否为 **121×68**、标题 **16px/24px** Semibold info1、描述 **12px/18px** Regular **info3**？
□ 图+文本卡片 hover / 按下态是否与左侧分类条目一致（hover 标题 → primary2、按下 → primary3，无背景、无字重变化，图与描述不变）？
□ 活动卡片图是否 object-fit: cover + alt + 「上线前替换真实素材」注释（禁止编造 URL）？
□ 面板 padding 是否为 PC 上下 24px / 左右 216px、MB 上下 12px / 左右 24px（栅格边距，禁止 64/40/12px）？
□ 1680~1441px 档面板左右 padding 是否仍为 216px（跟随栅格，未误用 64px）？
□ 1440~1201px 档面板左右 padding 是否仍为 216px（跟随栅格，未误用 40px）？
□ 1200~841px 档是否只改列距（16px），字号是否未重复写（已在 ≤1440px 档降为 14px）？
□ 840~601px 是否切换为 grid 双列（minmax(32px,144px) 1fr）且通栏 left: 0 + right: 0？
□ 右内容区是否 grid-row: 1 固定顶格 + max-width: 40%（黄金分割）？
□ ≤600px 是否取消 40% 收窄（max-width: none）？
□ 左列文字是否允许换行（white-space: normal）+ 最大宽度 144px？
□ 折叠态是否为 32px 最小宽度（.is-collapsed → grid-template-columns: 32px 1fr）？
□ 左列末项是否挂 .o-nav__cat-toggle（导航项过多时显示切换箭头）？
□ .o-nav__cat-toggle 是否默认 display: none（PC 隐藏）、仅在 ≤840px 档恢复显示（防箭头裸露在 PC 档）？
□ 顶导与触发器样式是否整段取自 floors/navigation.md「菜单项（触发器）完整样式」（本模板未重定义、未用 :has() 收窄、未自创状态）？
□ 是否同时支持 hover 与 click 两种展开途径，且单开互斥（CSS :has() + JS）？
□ 关闭面板时是否调用 link.blur()？
□ 点击菜单项时 is-current 是否随之迁移（单选不做 toggle）？
□ 所有间距/字号/图标尺寸是否取自硬约束白名单（PC {4,8,12,16,24,32,40,48,64,72} / 图标 {16,20,24,32,40,48,56,64}）？
□ 颜色是否全部使用 var(--o-color-*) 语义 Token（无 #fff / rgb() 硬编码、无色板变量）？
□ 是否引用了未定义的语义 Token（改色值后须回头核对 :root 清单）？
□ **是否做过 CSS 变量定义/引用比对**：页面里每个 `var(--o-color-*)` 都必须在 `:root` 有定义（改完 CSS 必须跑，详见踩坑 #44）？
```

---

## CSS 变量定义/引用比对（改完 CSS 必做）

改色值 / 加状态最容易漏的是**只改引用、漏加定义**：`var()` 指向未定义变量时，该属性**直接失效**（不是回落默认值），表现为"hover 无变化""图标消失"这类看不出原因的故障。本楼层历史上已因漏定义 `link1/2/3` 导致右侧图标与快捷链接"不是蓝色"（已确认 #17④）。

```bash
# 以 check 页为输入：列出"已定义"与"已引用"的变量集，取差集
f="references/examples/.nav-dropdown-categorized-check.html"
defined=$(grep -o -- '--o-color-[a-z0-9-]*:' "$f" | sed 's/:$//' | sort -u)
used=$(grep -o 'var(--o-color-[a-z0-9-]*)' "$f" | sed 's/var(//;s/)//' | sort -u)
comm -13 <(echo "$defined") <(echo "$used")   # 有输出 = 漏定义，必须补进 :root
```

输出为空才通过；有输出则把缺的变量补进 `:root`（取值查 components/navigation.md Appendix A 或 opendesign-tokens，禁止随手写 hex）。

---

## 十二、待确认项（规范未覆盖 / 设计稿歧义）

> 按 SKILL.md「规范未覆盖时的分级处理」，以下各点**禁止静默取值**，生成前需向用户确认：

| # | 待确认点 | 状态 |
|---|---------|------|
| 2 | **面板内边距 ≥1681px 取 24px**（间距 5）为推断值 | ⏳ 待确认 |
| 5 | **快捷链接栏剩余推断值**：按钮块**间距 8px**（纯文本项的块宽 342px / 块高 40px / 圆角 4px / fill3 底 / link 距左 24px / OLink medium 内层规格 / 栏内左对齐 / 卡片无底色均已确认，见已确认 #13、#15、#17） | ⏳ 待确认 |
| 6 | **tooltip 组件引用来源**：design skill `components/` 无 popover 文档，当前指向 opendesign-components skill 的 `references/popover.*.md` | ⏳ 待确认 |
| 7 | **触发器 hover 色与 components/navigation.md Part C 冲突**（Part C 原文规定 hover 用 primary1）。**2026-10-15 已裁决：hover 色统一为 `primary2`，两种面板样式一致**；触发器全套样式（含全状态 Regular(400) 裁决）已沉淀到 floors/navigation.md「菜单项（触发器）完整样式」，两份下拉模板均不重定义 | ✅ 已裁决并落地（唯一定义源 = floors/navigation.md） |
| 8 | **601~840px 为 hard-constraints 规范空档**（§1 只定义 ≤600px 与 ≥841px），本模板按设计稿实现为独立档 | ⏳ 待确认 |
| 9 | **条目是否真的没有 hover 背景**（设计稿仅规定文字三态，未标注背景） | ⏳ 待确认 |
| 10 | **红标签垂直 padding**：tag.md small 为 2px，与 20px 固定高冲突，当前取 0 | ⏳ 待确认 |
| 11 | **列头与条目列表间距 12px、标题↔说明间距 4px** 均为推断值 | ⏳ 待确认 |
| 12 | **快捷链接栏右对齐的精确含义**：当前实现为「整栏 `margin-left: auto` 贴栅格内容区右缘 + 栏内 `align-items: flex-end`」，PC 端距视口右边固定 216px；MB 端跟随 MB 栅格（距右 24px）。若要求 MB 端也保持 216px 需再次确认（360px 画布下物理上不可能） | ⏳ 待确认 |

### ✅ 已确认（2026-10-14）

| # | 确认内容 |
|---|---------|
| 1 | 适用场景 = 有层级分类、但条目数量较多时使用 |
| 2 | 桌面端 = 浮层面板贴顶栏下方，多列平铺（列头 + 列内条目「标题 + 说明」） |
| 3 | 典型列数：开发 Tab 5 列（贡献社区/构建/发布/分析/项目）；社区 Tab 4 列 + 右侧快捷链接栏 |
| 4 | 条目内元素：标题右侧可带外链图标 ↗；标题后可挂红色小标签（BETA / 新） |
| 5 | 列底部可有「更多项目 >」入口；快捷链接栏为灰底按钮块或带图活动卡片 |
| 6 | 文字 Token（PC 1920）：分类列头 14px Regular / #000000 60%；条目标题 16px Regular / #000000 100%（默认）→ Semibold / #5177FA（悬浮）→ Semibold / #002793（按下）→ Semibold / #002FA7（选中）→ Regular / #000000 40%（禁用）；条目说明 12px Regular / #000000 80%（禁用同） |
| 7 | 顶栏导航面板展开时：当前选中项 16px / #002FA7 + 下划线；其他悬停项 16px / #5177FA 无下划线（⚠️ Semibold 部分 2026-10-15 用户否决：字重随交互切换会肉眼跳动，触发器全状态 Regular(400)；颜色部分继续有效；**hover 色 primary2 已由 2026-10-15 统一裁决为两种面板样式通用**） |
| 8 | MB（360）：默认 14px Regular / #000000 80%；按下 14px Semibold / #002FA7；禁用 14px Regular / #000000 40% |
| 9 | 色板映射：info1=#000000、info2=#000000 80%、info3=#000000 60%、info4=#000000 40%、primary1=#002FA7、primary2=#5177FA、primary3=#002793 |
| 10 | 六档断点：≥1681（左右24/上下24）/ 1680~1441（菜单间距32、面板左右64）/ 1440~1201（内容区距左右40）/ 1200~841（左右16、上下24、标题16→14）/ 840~601（条目距左右12、右内容区最大40%、左一级导航列+右内容区）/ 600~360（条目距左右12、左菜单左对齐、中心导航距左右24、菜单最大宽144px超长换行） |
| 11 | 截断规则：下拉标题最多 2 行；下拉选项最多 6 行；超出均显示 ...；悬浮 tooltip 显示完整内容（复用设计系统既有 tips 组件） |
| 12 | 溢出规则：导航项过多时最后一项显示切换箭头；左侧折叠态最小宽度 32px |
| 13 | **第二轮补充确认**：① hover 色取上游语义 Token `--o-color-primary2`（#5177CA），不写死 #5177FA；② 断点笔误修正——「内容区距左右 40px」区间为 **1440~1201px**（非 1680~1201px），修正后与 1680~1441px（64px）互不重叠；③ **≤600px 右内容区不按 40% 收窄**（`max-width: none`）；④ 快捷链接纯文本按钮 = **link.md M 尺寸**（文字 14px/22px、link 高 34px）、**块内 link 距左 24px + 上下居中**；⑤ 快捷链接带图卡片 = **左图右文**、图 **16:9**（高度与左侧文本块一致）、**图文间距 8px** |
| 14 | **第三轮确认（2026-10-15，面板定位与边距）**：① 分类平铺面板**通栏撑满页面宽度**（`left: 0` + `right: 0` + `width: auto`，包含块经 `.o-nav__item:has(> .o-nav__dropdown--categorized) { position: static }` 上移到顶导本体）；② 面板**选项内容跟随栅格内容区**，`padding-left/right` 取 hard-constraints §4.1 栅格边距——PC **216px**（216+1488+216=1920）、MB **24px**（24+312+24=360）；③ 原「1680~1441px 左右 64px / 1440~1201px 左右 40px / MB 左右 12px」**全部作废**，PC 全档统一 216px、MB 全档统一 24px |
| 15 | **第四轮确认（2026-10-15，字号分档 / 字重 / 图标 / 标签 / 快捷链接栏）**：① **快捷链接栏是可选模块，看需求挂载，默认不出现**（HTML 骨架中默认注释掉）；② 快捷链接栏**栏首必须有「快捷链接」标题**，规格同分类列头；③ 快捷链接**纯文本项 = 灰底按钮块（fill3、高 40px、圆角 4px、link 距左 24px、上下居中）+ 内部文字严格用 OLink medium**（components/link.md：14px/22px Regular、`--o-color-link1`、hover `link2` + 下划线、按下 `link3` + 下划线、后缀图标 24×24、图文间距 4px）——⚠️ **两层缺一不可**：2026-10-15 第一轮曾把灰底整块删掉只留裸 OLink 文字，用户验收时被打回（"背后的色块没了"）；禁止再犯 | ④ 快捷链接栏**右对齐**，`margin-left: auto`，始终贴栅格内容区右缘，PC 端距视口右边固定 **216px**；⑤ **分类列头字号 >1440px 16px / ≤1440px 14px，颜色统一 `info4`**（原 info3、原 14px 一档到底作废）；⑥ **选项标题字号 >1440px 16px / ≤1440px 14px，颜色 `info1`；选项说明两档均 12px / `info2`**；⑦ **选项标题字重默认态与 hover 交互全部统一 Semibold**（原「默认 Regular → hover Semibold」作废；禁用态是唯一例外，仍回落 Regular）；⑧ **图标必须从 `references/assets/public icons/` Read 后逐属性内联**（外链 = `icon-外链.svg`、更多项目 = `icon-右箭头.svg`、MB 切换 = `icon-上箭头.svg` / `icon-下箭头.svg`），16×16 + `fill="currentColor"`，**禁止手写 SVG / `<img src>` / 字符代替**；⑨ **红标签严格按 tag.md OTag `fill + danger + small`**（高 20px / 水平 padding 6px / 圆角 4px / 12px-18px / 白字 / 底 `--o-color-danger1`），类名统一为 `.o-tag.o-tag--fill.o-tag--danger.o-tag--small`；⑩ **「更多项目」改用 OButton `text` + `large`**（components/button.md：高 40px、水平内边距 0、16px/24px Regular、默认 `info1` → hover `primary2` → 按下 `primary3`、图标 24×24 在右侧、图文间距 8px），**元素必须是 `<button>`**，右箭头 24×24 取自 `icon-右箭头.svg`（原 12px-18px / info3 方案作废） |
| 16 | **第五轮确认（2026-10-15，快捷链接栏演示可见 + 间距核对）**：① 模板 HTML 骨架中快捷链接栏**默认仍注释掉**（正式落库默认无）；**验收 check 页面为演示检验需要看到，默认渲染出来**——两处刻意不同，不是遗漏；② **列间距（条目间左右间距）= 24px**（`.o-nav__dropdown { gap: 24px }`），**条目间距（条目间上下间距）= 24px**（`.o-nav__cat-list { gap: 24px }`），PC 全档条目上下间距 24px，列间 24px 仅在 1200~841px 降为 16px |
| 17 | **第六轮确认（2026-10-15，快捷链接栏对齐 / 色块宽度 / 卡片底色 / OLink 两层结构）**：① **栏内整体左对齐**（`align-items: flex-start`，标题 / 色块 / 卡片同一左缘）+ **整栏靠右**（`margin-left: auto` 贴栅格内容区右缘，PC 距视口右 216px）——⛔ 不是每个选项各自右对齐（原 `align-items: flex-end` 方案作废）；② **选项背后的色块宽度固定 342px**（原 `min-width: 200px` 作废）；③ **图+文本的快捷链接没有背后的色块**（无背景 / 无圆角 / 无内边距，原给卡片刷 `fill3` 作废）；④ **纯文本项必须分成两层**：外层灰底按钮块（宽 342px / 高 40px / 圆角 4px / `fill3` 底 / link 距左 24px / 上下居中）+ 内层 **OLink medium**（components/link.md：整体高 **34px**、Auto Layout Padding **上 5px / 下 5px / 左 0px / 右 0px**、文字 14px/22px Regular、`link1` → hover `link2` + 下划线 → active `link3` + 下划线、**后缀图标 24×24**（`icon_size_control-m`，取 `icon-外链.svg`，颜色随文字）、图文间距 4px）——⛔ 禁止把 40px 高 / 24px 左右 padding 直接写在 link 本体上；⑤ 卡片标题改 14px/22px Semibold、时间行 12px/18px；⑥ **全局字体一律 HarmonyOS Sans / HarmonyHeiTi**（hard-constraints §2 铁律），禁止 PingFang / 系统默认字体 |
| 18 | **第七轮确认（2026-10-15，图标尺寸 / link 色来源 / 卡片排版）**：① **分类条目外链图标 = 24×24**（原 16×16 作废，用户明确指定覆盖硬约束 §5 默认档），**外链图标↔文本间距 = 8px**、**红标签↔文本间距 = 8px**（`.o-nav__cat-item-title { gap: 8px }`）；② **OLink 后缀图标同为 24×24**，但其↔文字间距**保持 link.md 的 4px（gap-1）**——OLink 是组件规范，不随条目侧间距调整；③ **link 色 Token 来源已核实**：`--o-color-link1` = **#002FA7**（brand-6）、`--o-color-link2` = **#5177CA**（brand-4）、`--o-color-link3` = **#002793**（brand-7），取自 **opendesign-tokens/references/tokens-openeuler.md**「品牌色 / Primary / 链接色」表，components/link.md Part B 亦注明 link1=brand-6；**不是为某个具体社区自创的色值**；④ 快捷链接纯文本之前"不是蓝色"的原因 = check 页 `:root` 漏定义 link1/2/3 导致 var() 失效，**已修复**，模板新增「引用前必须先定义」警告；⑤ **图+文本卡片图文垂直居中**（`align-items: center`，原顶部对齐作废）、**图高 68px 宽 121px**（16:9）、**标题 16px/24px**、**描述 12px/18px + `--o-color-info3`**（原 info2 作废） |
| 19 | **第八轮确认（2026-10-15，图+文卡片 hover 态）**：**图+文本卡片原本没有任何 hover 态**（用户验收时发现「这里的 hover 态没有」），现要求**与左侧分类条目 hover 态保持完全一致**：hover 标题色 → `--o-color-primary2`、按下 → `--o-color-primary3`；**无背景色块、无字重变化，图片与描述行各态均不变**。实现为 `.o-nav__quick-card:hover .o-nav__quick-card-title { color: var(--o-color-primary2) }` + `:active` 版本 |
| 20 | **第九轮确认（2026-10-15，描述预留两行高度）**：**条目说明预留两行位置**，`.o-nav__cat-item-desc { min-height: 36px }`（= 2 × 18px 行高）——文案只有 1 行时也占两行空间，后续文案延展成 2 行时不会挤压其他条目布局；⛔ 不是把 line-clamp 从 6 改成 2，截断上限仍为 6 行不变 |
| 21 | **第十轮确认（2026-10-15，条目无选中态 + 顶导骨架复用）**：① **面板条目没有选中态**——导航条目是**直接点击跳走的链接**，不存在"当前停在这一项"的持久状态。条目状态集合收敛为**四种**：默认 / hover（`primary2`）/ 按下（`primary3`）/ 禁用（Regular + `info4`）；**禁止给条目加 `.is-selected` 类或 `primary1` 文字色**（原「已选 = primary1」方案作废）。② **这与顶导触发器 `is-current` 是两回事，禁止混用**：触发器 `is-current` 表达「当前所在页面」，保留 `primary1` + 底部 2px 指示线（字重全状态 Regular(400)，Semibold 已于 2026-10-15 否决）；条目跳走即离开，不记录位置。③ **顶导骨架唯一定义源 = floors/navigation.md「菜单项（触发器）完整样式」**：平铺式与分类平铺式两份下拉模板**均不定义顶导骨架**，生成 HTML 时该段必须整段复制且禁止改写 / 删减（`align-self: flex-end`、`line-height` 等易漏项必须原样带上） |
| 22 | **第十一轮确认（2026-10-15，面板与顶导零间距 · 抽屉式展开）**：① **面板与顶导之间没有间距**，`top: 100%` 紧贴顶导底边展开，**像抽屉一样**——⚠️ 覆盖继承平铺式基类的 `top: calc(100% + 4px)`。② **上圆角必须同时压平为 `border-radius: 0 0 4px 4px`**：零间距后若保留 4px 全圆角，面板左上 / 右上会各露出一道月牙形缺口，视觉上面板与顶导又"裂开"了，抽屉式贴合白做；下圆角保留（面板底边是自由边）。③ **基类的 `::before` 4px 热区桥接必须显式 `display: none`**：桥接本是补鼠标穿过间隙的路径，零间距后无间隙可穿，只剩白占顶导底部 4px 的副作用。④ **不要**把隐藏态 `translateY(-8px)` 当成间距去改 `top`——那只是入场位移，展开终态是 `translateY(0)`，改了反而会在展开态留下真实间隙。⑤ MB 档不覆盖 `top`，同样零间距 |

---

← [楼层模板索引](../SKILL.md#楼层模板索引) · [组件规范](../components/navigation.md) · [平铺式模板](navigation-dropdown.md)