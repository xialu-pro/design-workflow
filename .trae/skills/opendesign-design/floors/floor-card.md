← [楼层模板索引](../SKILL.md#楼层模板索引) · [组件规范](../components/card.md)

# 通用楼层卡片模块模板

> 本模板为自包含生成单元。AI 读取本文件后即可精确生成「通用楼层卡片模块」——页面中可复用的标准楼层容器，覆盖 **≥1681px / 1201~1680px / 841~1200px / 601~840px / 340~600px 五档断点**。

> 🔴 **本模板的间距表为固定值（非 `--o-r-gap-*` Token），是未来所有页面生成中该楼层类型的约束条件**：禁止四舍五入、禁止推断、禁止"就近取 Token 近似值"。无法对应时停下询问用户。

---

## 涉及组件

| 组件 | 规范文档 | 本楼层使用的变体（按断点 ≥1681 / 1201~1680 / 841~1200 / 601~840 / 340~600） |
|------|---------|------|
| OTab | [components/tab.md](../components/tab.md) | size = L / M / M / **S** / **S**；交互态必须含 tab.md Part C 全量（Default / Hover / Press / Disabled） |
| OCard | [components/card.md](../components/card.md) | 各断点一致（内部样式以 card.md 为唯一真源） |
| OButton | [components/button.md](../components/button.md) | variant = text，size = L / M / M / S / S |

> 🔴 **子组件引用规则（硬约束）**：Tab、卡片、Button **不写内部样式**，统一引用独立组件规范（`tab.md` / `card.md` / `button.md`），仅选择对应尺寸变体。本模板只定义楼层级架构、字号与间距。

> 🔴 **「仅选尺寸变体」≠ 只画选中态**：子组件规范里的**交互态矩阵**属于组件规格的一部分，必须一并落地。典型遗漏是 Tab 只写 `.is-active`（选中）而漏掉 Hover / Press / Disabled——视觉上「像真的」，实际鼠标放上去毫无反馈。引用 `tab.md` 时必须包含 Part C 全部状态；引用 `button.md` / `card.md` 同理（按下态、禁用态不得省略）。

> 🔴 **hover / press 一律不改字重（[hard-constraints.md](../global/hard-constraints.md) §2.1 全局字重铁律）**：Tab 的字重只表达「是否选中」（非选中 Regular / 选中 SemiBold）。hover、press 只改颜色（press 外加同色下划线），**一条 `font-weight` 都不写**——理由与正确写法见 [tab.md Part C](../components/tab.md)（与 [navigation.md](navigation.md) 踩坑 #17、[navigation-dropdown-categorized.md](navigation-dropdown-categorized.md) 踩坑 #30 同源：实测拉丁标签 400→600 文字宽 +1.9px，下划线变宽、后续页签被推挤）。**该规则不限 Tab，适用于所有交互元素。**

> 🔴 **601~840px / 340~600px 两档 Tab 用 S（small）**：即 [tab.md](../components/tab.md) 的 small 规格——**竖向 56×56**、字号 14px/22px（`font_size-tip1`）、选中指示器为 **16px 宽居中短线**（圆角 9px，非全宽）。禁止在这两档继续用 M（44px 横排）。

---

## 一、楼层架构

```
楼层标题（必选）+ 楼层描述（可选）+ 二级标题（可选）+ Tab 页签（可选）+ 卡片布局（必选，仅示例）+ 操作·查看更多（可选）
```

```
通用楼层卡片模块
│  内容容器: ≥1681px 1488px（距两边 216px）/ ≤600px 312px（距两边 24px）
│          中间三档按楼层栅格规则：Laptop = grid-22（一侧空白 1 列，边距 = 留白+1列+1水槽，1440px 时 ≈98px）
│          Pad 两档 = grid-full（占满，边距 = 留白 32px）
│
├── [楼层标题]（必选）
├── [楼层描述]（可选）
├── [二级标题]（可选）
├── [Tab 页签]（可选）           ← 引用 tab.md，仅选尺寸变体
├── [卡片布局]（必选，仅示例）    ← 引用 card.md；卡片数量灵活可变：一卡、两卡、三卡及以上均可（图中三卡仅为示例），无论多少均按栅格排布（PC 24 列 / MB 4 列）；≤5 卡单行横排（≤600px 一排两卡横滑），>5 卡一排四列换行；≤600px：≤6 卡横滑，>6 卡平铺为默认，横滑为备选
└── [操作·查看更多]（可选）       ← 引用 button.md，text 变体，仅选尺寸变体
```

| 元素 | 必选/可选 | 说明 |
|------|----------|------|
| 楼层标题 | 必选 | 楼层名称，信息层级最高 |
| 楼层描述 | 可选 | 标题下方的一句/段说明 |
| 二级标题 | 可选 | 描述与 Tab 之间的分组标题 |
| Tab 页签 | 可选 | 卡片前的分类切换，引用 tab.md |
| 卡片布局 | 必选 | 楼层内容主体；卡片数量灵活可变——一卡、两卡、三卡及以上均可（图中三卡仅为示例），无论多少均按栅格排布（PC 24 列 / MB 4 列）：≤5 卡单行横排，>5 卡一排四列换行；≤600px：≤6 卡横滑，>6 卡平铺为默认，横滑为备选；内部样式以 card.md 为准 |
| 操作·查看更多 | 可选 | 卡片容器下方的跳转操作，引用 button.md |

**对齐方式**（依适配规则示意图）：楼层标题、楼层描述、二级标题、Tab 页签均**水平居中**；卡片容器通栏；查看更多按钮居中。

---

## 二、断点定义

| 断点 | 视口范围 | 示例宽度 | 媒体查询写法 |
|------|---------|---------|-------------|
| Desktop | ≥1681px | 1920 | 基准档（默认值） |
| Laptop | 1201~1680px | 1440 | `@media (min-width: 1201px) and (max-width: 1680px)` |
| Pad 横屏 | 841~1200px | 1080 | `@media (min-width: 841px) and (max-width: 1200px)` |
| Pad 竖屏 | 601~840px | 740 | `@media (min-width: 601px) and (max-width: 840px)` |
| Phone | 340~600px | 360 | `@media (max-width: 600px)` |

---

## 三、字号表（五档固定值，与上游响应式 Token 互为校验）

| 元素 | 字重 | 颜色 | ≥1681px | 1201~1680px | 841~1200px | 601~840px | 340~600px | 对应 Token |
|------|------|------|--------|-----------|-----------|----------|----------|-----------|
| 楼层标题 | semibold | `--o-color-info1` | 40px / 56 | 32px / 44 | 24px / 32 | 18px / 26 | 18px / 26 | `--o-r-font_size-display3` + `--o-r-line_height-display3` |
| 楼层描述 | regular | `--o-color-info2` | 16px / 24 | 14px / 22 | 14px / 22 | 14px / 22 | 14px / 22 | `--o-r-font_size-text1` + `--o-r-line_height-text1` |
| 二级标题 | semibold | `--o-color-info1` | 24px / 32 | 20px / 28 | 18px / 26 | 16px / 24 | 16px / 24 | `--o-r-font_size-h2` + `--o-r-line_height-h2` |

- 字号与行高**成对锁定**，直接使用上表 Token（响应式变量自动切档，无需手写断点）；表中固定值为校验基准，二者必须一致。
- Tab / 卡片 / 查看更多按钮的字号由各自组件规范决定（tab.md / card.md / button.md），本模板不重定义。

---

## 四、间距表（固定值 · 约束条件）

| 间距项 \ 断点 | ≥1681px | 1201~1680px | 841~1200px | 601~840px | 340~600px |
|--------------|--------|-----------|-----------|----------|----------|
| 楼层与楼层上下之间间距 | 72 | 56 | 40 | 32 | 32 |
| 楼层标题 → 楼层描述 | 16 | 12 | 8 | 16 | 16 |
| 楼层描述 → 二级标题 | 40 | 24 | 16 | 12 | 12 |
| 二级标题 → Tab 页签 | 24 | 16 | 12 | 12 | 12 |
| Tab 页签 → 卡片容器 | 24 | 16 | 12 | 12 | 12 |
| 卡片容器 → 查看更多按钮 | 32 | 24 | 16 | 12 | 12 |
| 查看更多 → 模块底部外边距 | 72 | 56 | 40 | 32 | 32 |
| 卡片水平间距 | 32 | 24 | 16 | 16 | 12 |
| 卡片垂直间距 | —（≤5 卡） / 32（>5 卡） | — / 24 | — / 16 | — / 16 | —（≤6 卡横滑）/ 12（>6 卡平铺） |
| 卡片列数 | ≤5 卡单行横排；**>5 卡一排四列换行** | 同左 | 同左 | 同左 | ≤6 卡一排两卡横滑；>6 卡两列平铺为默认，横滑为备选 |

### 实现要点

1. **间距一律用固定值 + 媒体查询**，不使用 `--o-r-gap-*` Token（Token 默认映射与本表不完全一致，见下方差异声明）。
2. **「楼层与楼层上下之间间距」= 「查看更多 → 模块底部外边距」**：同一约束的两种表述——模块底部外边距即楼层间距。实现时每个楼层只保留一处（楼层容器 `margin-bottom` 或父容器 `gap`），相邻楼层总间距恒为该档值，**禁止叠成 2 倍**；模块顶部外边距取同值。
3. **缺省元素合并规则**：任一可选元素缺省时，其前后两段间距合并为一档，**取相邻两行值的较大者**。例：
   - 无楼层描述：楼层标题 → 二级标题 = max(16, 40) = **40**（≥1681px 档）
   - 无 Tab：二级标题 → 卡片容器 = max(24, 24) = **24**
   - 无查看更多：卡片容器 → 模块底部 = max(32, 72) = **72**
   - 推论：标题组到卡片容器的间距恒为 40/24/16/12/12（与硬约束「间距 7 · 楼层标题到内容」一致）
4. **卡片水平间距不随 Grid Token 漂移**：固定按上表执行（32/24/16/16/12），即使视口落入上游栅格的其他分档也不改用 `--o-r-grid-column-gutter`。
5. **Tab 交互态三条硬规则**：
   - **hover / press 一律不改字重**，只改 `color`（press 外加同色下划线）。字重只由「是否选中」决定（非选中 Regular / 选中 SemiBold）。⛔ 禁止给 hover 加 `font-weight: 600`——实测拉丁/数字标签 400→600 文字宽 +1.9px，页签宽由文字撑开 + 指示器全宽，下划线随之变宽、后续页签被推挤位移。详见 [tab.md Part C](../components/tab.md)。
   - **Press 规则特异性必须 ≥ Hover 规则**：按下时鼠标必然同时处于 hover，若 Press 写作 `.otab__item:active`（0,2,0）而 Hover 写作 `.otab__item:hover:not(.is-active)`（0,3,0），Press 色会被 Hover 色盖掉——表现为「按下毫无变化」。写法：`.otab__item:active:not(.is-active), .otab__item.is-active:active { … }`（0,3,0 且位于 Hover 之后）。
   - **hover / press 必须带 `:not(:disabled)`**：禁用是终态，交互态对禁用项一律不得生效。⛔ 若 Hover 写作 `.otab__item:hover:not(.is-active)`（0,3,0），会直接盖掉 `.otab__item:disabled`（0,2,0）——禁用页签被鼠标划过就变色（实测 hover → primary2、press → primary3 且指示器变色）。写法见 §六 Tab 交互态骨架；详见 [hard-constraints.md §2.2](../global/hard-constraints.md)。

### ⚠️ 与上游 `--o-r-gap-*` 的差异声明（有意覆盖，禁止"修正"）

- 除**「楼层标题 → 楼层描述」在 601~840px / 340~600px 两档为 16px**（上游 `--o-r-gap-4` 在 ≤840px 为 8px）外，本表其余各行与上游 gap Token 逐档一致。
- 该 16px 为设计裁决：移动端标题与描述之间需要更大呼吸空间。**禁止**将其"对齐"回 8px。

---

## 五、卡片布局规则

### 5.1 卡片数量分档（硬约束）

| 卡片数量 | 布局规则 |
|---|---|
| 1~5 卡 | **单行横排**，卡片 `flex: 1` 均分；340~600px 档一排两卡，第 3 张起横滑（见 5.2） |
| **>5 卡（6 张及以上）** | **一排四列、换行排布**（`grid-template-columns: repeat(4, 1fr)`），7 卡 = 第一行 4 张 + 第二行 3 张，共两行；**340~600px 档按数量定默认模式：≤6 卡横滑，>6 卡平铺为默认，横滑为备选**（见下方细则） |

**>5 卡规则细则**：

- **列数固定 4**（PC 24 列中每卡占 6 列，与 [card-grid.md](card-grid.md) PC 四列口径一致）：≥1681px 卡宽 = `(1488 − 3×32) / 4 = 348px`。⛔ 不是 5 列、也不是「铺满一行」——数量超过 5 时按 4 列切，避免单卡过窄。
- **第二行左对齐**：末行不满 4 张时占据左侧列，右侧留空（grid 自动行为）。⛔ 禁止把末行卡片拉伸铺满整行（如 3 张各占 1/3 宽）——两行卡宽不一致会破坏栅格对齐。
- **换行后必须有垂直间距**：卡片垂直间距 = 该档卡片水平间距（32 / 24 / 16 / 16 / 12）。⛔ 禁止写 `gap: 0 32px` 之类只留水平间距的写法——两行卡片会上下贴死。
- **601~840px 同样四列**：卡宽 = `(676 − 3×16) / 4 = 157px`（740 视口）。该档与 ≤5 卡单行时的卡宽同级（≈122~157px），属可接受下限。
- **340~600px：横滑与两列平铺二选一，默认模式按卡片数量决定**：该档内容容器仅 312px，放 4 列只有 69px/卡不可用，故 >5 卡在此档不沿用四列，改按以下两种模式之一落地——
  - **≤6 卡：默认横滑**——与 ≤5 卡同规则：一排两卡（卡宽 150px）+ `overflow-x: auto` + 右端外扩 36px（规则见 5.2），第 3 张起横滑。
  - **>6 卡（7 张及以上）：默认两列平铺**：`grid-template-columns: repeat(2, 1fr)`，卡宽 = `(312 - 12) / 2 = 150px`（与「一排两卡」同宽），全部平铺展示、不横滑；7 卡 = 2+2+2+1 共四行。数量多时逐张横滑成本高，故平铺为默认；**横滑仍为合法备选**（首屏只做引导、不追求全部可见时）。
  - 两种模式卡宽相同（150px），区别只在「横滑承载」还是「全部平铺」；默认值只解决「用户没提诉求时怎么落地」，最终按用户诉求选择。
- **卡片内部样式**仍以 [components/card.md](../components/card.md) 为唯一真源，本模板不重定义。

### 5.1.1 cover + horizontal 专项规则（2026-10-21 新增）

水平布局卡（`type=cover` 且 `layout=horizontal`）的封面宽度由**内容列高度按 16:9 反推**（见 card.md「封面尺寸推导规则」），同等视口下比 vertical 卡更占横向空间，因此列数规则**独立于**上方 5.1 的数量分档：

| 断点 | cover + horizontal 每行列数 | 理由 |
|------|---------------------------|------|
| ≥1681px / 1201~1680px / 841~1200px | 按 5.1 数量规则（≤5 单行横排 / >5 四列换行） | 卡宽 348 / ≈293 / 242px，封面反推宽 387 / 320 / 295px 均小于双列宽，文字列充足 |
| **601~840px** | **一排只放一张**（独占整行） | 双列宽 330px 时封面已占 241.8px，文字列只剩 ~160px 已到极限；再并排会恶化到不可读 |
| **340~600px** | **一排只放一张**（卡宽 312px） | 单列仅 150px，封面即便按窄档封顶也要 148px，只占一列会把文字列压成 0 |

**实现要点（踩坑记录）**：

- **flex 行内必须写 `flex: 0 0 100%`，只写 `width: 100%` 不生效**——`width: 100%` 会被 `flex-shrink` 收缩回 `(容器宽 − 间距)/2`（601~840px 档 = `(676−16)/2 = 330px`），两张卡仍并排。
- **两列平铺（340~600px）下水平卡跨满整行**：该档为 `grid-template-columns: repeat(2, 1fr)`，须写 `grid-column: 1 / -1` 让卡片跨两列（卡宽 312px）。
- **水平卡封面的上下内缩由封面自身 `margin` 承担**（≥841px `8px 0`，≤840px `4px 0`），容器**只留封面所在侧的 padding**（封面在左 → 只留 `padding-left`）。若给容器写上/下 padding，会同时压在内容区上，使「标题→卡片顶」「操作按钮底→卡片底」变成 32px 而非 24px。详见 card.md「内边距规格」。
- **≤600px 一律不显示操作区**（所有类型、两种布局），见 card.md 规则 6。

### 5.2 ≤5 卡单行规则

- **列数**：所有断点均为**单行横排**；340~600px 档**一排两卡**，第 3 张起依次往右加并**横向滑动**（`overflow-x: auto`），不换行、不加垂直间距。
- **340~600px 卡片宽度 = 150px**：由「内容容器 312px − 卡片水平间距 12px」对半分得，即 `(312 - 12) / 2 = 150`（与 card-grid.md MB 端两列口径一致）。**禁止**把卡片写成 216px 之类的任意值——容器只有 312px，一张卡超过 156px 就会挤成一排一卡，第二张被裁掉，「一排两卡」直接失效。
- **340~600px 横滑容器的右侧间距（硬约束，超出部分同样按卡片水平间距规则排布）**：横滑序列的**右端也要有 12px**——即 ① 容器 `padding-right: 12px`（滚到最右时末卡与右缘同为 12px，与卡间距一致）；② 容器向右外扩 `width: calc(100% + 36px)`，使**第 3 张正好露出一个间距（12px）**，让用户看出"右边还有卡、可横滑"。**禁止**把容器写成 `overflow-x: auto` 就不管右端——实测不处理时第 3 张被完全裁掉、卡片2 右缘紧贴容器边缘，中间那条 12px 缝露在容器外变成白缝，既没有"可横滑"提示、也不符合间距规则。外扩量用 `width` 撑开（不占文档流宽度），**不会挤占「查看更多」按钮的位置**。
- **卡片水平间距**：32 / 24 / 16 / 16 / 12（按断点）。
- **卡片垂直间距**：无（单行）。
- **卡片内部样式**（封面/图标/文字/Footer/圆角/padding）一律引用 [components/card.md](../components/card.md)，本模板不重定义。其中**内边距按 card.md 的单层口径 + 断点分档**：vertical 容器 `padding: 8px 8px 0`（只服务封面，底部 0），**601~840px 与 340~600px 两档容器收到 4px**；Body 按档自写（≥1681px `24px 24px 24px` → 601~840 / 340~600 `12px 8px 12px`）。⚠️ **水平方向会叠加**：「标题→卡片左」= 容器左 + Body 左，Body 左右不是最终可见值。校验直接用 card.md「实际可视间距（直读值）」表。⚠️ **horizontal 卡容器只留封面所在侧**（封面在左 → `padding-left` 8px / ≤840 档 4px，上/右/下全 0），封面另外两边内缩由封面 `margin` 承担，见 card.md「内边距规格」与 §5.1.1。
- **卡片文本行数上限（硬约束，按断点分档）**：**≥841px** 卡片标题**最多 2 行**、卡片描述**最多 3 行**；**≤840px**（601~840px / 340~600px）标题收紧为 **1 行**、描述收紧为 **2 行**——超出末尾以 `...` 截断。用 line-clamp **四件套**：`display: -webkit-box` + `-webkit-box-orient: vertical` + `-webkit-line-clamp: N` + `overflow: hidden`——**缺 `overflow: hidden` 时省略号不出现、文字直接溢出**。写法见 §六 CSS 骨架；分档表与窄屏收紧理由见 [card.md](../components/card.md) 硬约束规则 8。

---

## 六、楼层容器 CSS

**≥1681px（Desktop）与 ≤600px（Phone）** 按硬约束 §4.1 落实：

```css
/* ≥1681px */
.floor-card {
    width: 1488px;                    /* 内容区固定宽度，撑满 */
    max-width: 1488px;                /* 禁止 1200px / 1140px / 960px */
    margin-left: 216px !important;    /* 固定左边距 */
    margin-right: 216px !important;   /* 固定右边距 */
    padding: 0 !important;            /* 禁止用 padding 控制楼层边距 */
}
/* ≤600px */
.floor-card-mb {
    width: 312px;
    max-width: 312px;                 /* 禁止 360px（那是画布宽度） */
    margin-left: 24px !important;
    margin-right: 24px !important;
    padding: 0 !important;
}
```

**中间三档（1201~1680 / 841~1200 / 601~840）**：按上游「楼层栅格适配规范」执行（`--o-r-grid-section-width` 已在 token 包预定义，`margin: 0 auto` 居中）——

- **Laptop（1201~1680px）**：楼层宽度 = `--o-r-grid-22`（24 列中**一侧空白 1 列**），边距 = `--o-r-grid-padding` + 1 列 + 1 水槽。以 1440px 为例：40 + 33.7 + 24 ≈ **98px**，内容宽 = 1440 − 2×98 = **1244px**。⛔ **不是 40px**——40px 只是栅格外侧留白，楼层还要再退 1 列 + 1 水槽（≥1681px 档的 216px 同源：64 + 2×(44+32) = 216，对应 grid-20 两侧各空 2 列）。
- **Pad 横屏 / Pad 竖屏（841~1200 / 601~840）**：楼层宽度 = `--o-r-grid-full`（占满），边距 = `--o-r-grid-padding` = 32px。
- 卡片水平间距仍固定按本模板间距表执行（见 §四 实现要点 4），不随水槽 Token 漂移。

```css
/* 1201~1680px · Laptop：楼层 = grid-22（一侧空白 1 列） */
@media (min-width: 1201px) and (max-width: 1440px) {
    /* 小桌面栅格：留白 40 / 水槽 24 / col = (视口−632)/24；边距 = 40 + col + 24（1440px 时 ≈98px） */
    .floor-card { width: calc((100vw - 632px) * 11 / 12 + 504px); max-width: none; margin-left: auto; margin-right: auto; }
}
@media (min-width: 1441px) and (max-width: 1680px) {
    /* 桌面栅格：留白 64 / 水槽 32 / col = (视口−864)/24；边距 = 64 + col + 32（1680px 时 = 130px） */
    .floor-card { width: calc((100vw - 864px) * 11 / 12 + 672px); max-width: none; margin-left: auto; margin-right: auto; }
}
/* 841~1200px / 601~840px · Pad：楼层 = grid-full（占满），边距 = 留白 32px */
@media (min-width: 841px) and (max-width: 1200px) {
    .floor-card { width: calc(100vw - 64px); max-width: none; margin-left: 32px; margin-right: 32px; }
}
@media (min-width: 601px) and (max-width: 840px) {
    .floor-card { width: calc(100vw - 64px); max-width: none; margin-left: 32px; margin-right: 32px; }
}
```

### 间距骨架示例（固定值 + 媒体查询）

```css
/* 基准档 ≥1681px */
.floor-card__title    { margin: 0 0 16px; }   /* 楼层标题 → 楼层描述 */
.floor-card__desc     { margin: 0 0 40px; }   /* 楼层描述 → 二级标题 */
.floor-card__subtitle { margin: 0 0 24px; }   /* 二级标题 → Tab 页签 */
.floor-card__tabs     { margin: 0 0 24px; }   /* Tab 页签 → 卡片容器 */
.floor-card__cards    { margin: 0 0 32px; display: flex; gap: 32px; }  /* 卡片容器 → 查看更多；卡片水平间距 */
.floor-card__more     { margin: 0 0 72px; }   /* 查看更多 → 模块底部 */

/* 1201~1680px */
@media (min-width: 1201px) and (max-width: 1680px) {
    .floor-card__title    { margin-bottom: 12px; }
    .floor-card__desc     { margin-bottom: 24px; }
    .floor-card__subtitle { margin-bottom: 16px; }
    .floor-card__tabs     { margin-bottom: 16px; }
    .floor-card__cards    { margin-bottom: 24px; gap: 24px; }
    .floor-card__more     { margin-bottom: 56px; }
}
/* 841~1200px */
@media (min-width: 841px) and (max-width: 1200px) {
    .floor-card__title    { margin-bottom: 8px; }
    .floor-card__desc     { margin-bottom: 16px; }
    .floor-card__subtitle { margin-bottom: 12px; }
    .floor-card__tabs     { margin-bottom: 12px; }
    .floor-card__cards    { margin-bottom: 16px; gap: 16px; }
    .floor-card__more     { margin-bottom: 40px; }
}
/* 601~840px */
@media (min-width: 601px) and (max-width: 840px) {
    .floor-card__title    { margin-bottom: 16px; }
    .floor-card__desc     { margin-bottom: 12px; }
    .floor-card__subtitle { margin-bottom: 12px; }
    .floor-card__tabs     { margin-bottom: 12px; }
    .floor-card__cards    { margin-bottom: 12px; gap: 16px; }
    .floor-card__more     { margin-bottom: 32px; }
}
/* 340~600px：一排两卡（卡宽 (312-12)/2 = 150px），第 3 张起依次往右加并横向滑动 */
@media (max-width: 600px) {
    .floor-card__title    { margin-bottom: 16px; }
    .floor-card__desc     { margin-bottom: 12px; }
    .floor-card__subtitle { margin-bottom: 12px; }
    .floor-card__tabs     { margin-bottom: 12px; }
    .floor-card__cards    { margin-bottom: 12px; gap: 12px; overflow-x: auto; width: calc(100% + 36px); padding-right: 12px; }  /* 外扩 36px = 右内边距 12px（滚到最右时末卡右缘间距）+ 第3张露出 12px（一个间距单位，提示可横滑）；width 外扩不占文档流宽度，不挤占「查看更多」 */
    .floor-card__cards > .ocard { flex: 0 0 auto; width: 150px; }  /* 一排两卡 (312-12)/2=150，禁止 216px 等任意值 */
    .floor-card__more     { margin-bottom: 32px; }
}
```

### >5 卡（一排四列换行）骨架示例

> 仅当卡片数量 **>5 张** 时启用；≤5 卡沿用上方 flex 单行骨架。末行不满 4 张时占据左侧列、右侧留空（grid 自动行为），**禁止**拉伸铺满。

```css
/* >5 卡：四列换行；gap 单一值 = 行距 = 列距（32/24/16/16/12），禁止只写水平间距 */
.floor-card__cards { display: grid; grid-template-columns: repeat(4, 1fr); gap: 32px; margin: 0 0 32px; }
/* 1201~1680px：列距/行距 24 */
@media (min-width: 1201px) and (max-width: 1680px) {
    .floor-card__cards { gap: 24px; margin-bottom: 24px; }
}
/* 841~1200px：列距/行距 16 */
@media (min-width: 841px) and (max-width: 1200px) {
    .floor-card__cards { gap: 16px; margin-bottom: 16px; }
}
/* 601~840px：仍四列（卡宽 (676-48)/4 = 157px @740） */
@media (min-width: 601px) and (max-width: 840px) {
    .floor-card__cards { gap: 16px; margin-bottom: 12px; }
}
/* 340~600px：横滑与平铺二选一；≤6 卡默认横滑，>6 卡默认平铺（标 .is-tile），卡宽均为 (312-12)/2 = 150px */
@media (max-width: 600px) {
    /* 模式 A（≤6 卡默认 / >6 卡备选）：一排两卡 + 横滑，规则同 5.2（含右端外扩 36px） */
    .floor-card__cards { display: flex; overflow-x: auto; width: calc(100% + 36px); padding-right: 12px; gap: 12px; margin-bottom: 12px; }
    .floor-card__cards > .ocard { flex: 0 0 auto; width: 150px; }
    /* 模式 B（>6 卡默认）：两列平铺换行，全部展示、不横滑 */
    .floor-card__cards.is-tile { display: grid; grid-template-columns: repeat(2, 1fr); width: auto; padding-right: 0; overflow-x: visible; }
    .floor-card__cards.is-tile > .ocard { flex: none; width: auto; }
}
```

### 卡片内间距骨架示例（单层口径 + 分档，引用 card.md 内边距规格）

```css
/* 基准档 ≥1681px：容器 8/8/0/8、Body 24/24/24/24 */
.ocard       { padding: 8px 8px 0; }        /* 只服务封面（封面距卡片边缘 8px），底部收口 0 */
.ocard__body { padding: 24px 24px 24px; }   /* 上=封面→标题 24；左右=24（+容器 8 = 文字距卡片边缘 32）；下=文字底→卡底 24 */
.ocard__desc { margin-top: 12px; }          /* 标题→描述 */

/* 1201~1680px：文字距卡片左/右 24、文字底→卡底 16 */
@media (min-width: 1201px) and (max-width: 1680px) {
    .ocard__body { padding: 16px 16px 16px; }
    .ocard__desc { margin-top: 8px; }
}
/* 841~1200px：文字距卡片左/右 16 */
@media (min-width: 841px) and (max-width: 1200px) {
    .ocard__body { padding: 12px 8px 12px; }
    .ocard__desc { margin-top: 8px; }
}
/* 601~840px：容器收到 4px、文字距卡片左/右 12 */
@media (min-width: 601px) and (max-width: 840px) {
    .ocard       { padding: 4px 4px 0; }
    .ocard__body { padding: 12px 8px 12px; }
    .ocard__desc { margin-top: 8px; }
}
/* 340~600px：容器收到 4px、文字距卡片左/右 12 */
@media (max-width: 600px) {
    .ocard       { padding: 4px 4px 0; }
    .ocard__body { padding: 12px 8px 12px; }
    .ocard__desc { margin-top: 8px; }
}
```

> ⚠️ **Body 左右不是最终可见值**：Body 左/右边缘本身已被容器 padding 推离卡片边缘，故「标题→卡片左」= 容器左 + Body 左（如 ≥1681px：8 + 24 = 32）。校验一律用 card.md「实际可视间距（直读值）」表，**禁止**把 Body padding 左右直接当成文字距卡片边缘。

/* ===== Tab 尺寸骨架（L / M / M / S / S，规格以 tab.md 为唯一真源）===== */
/* ≥1681px = L（48px 高、20px/28px）；841~1200px / 1201~1680px = M（44px 高、18px/26px） */
.otab__item { height: 48px; padding: 0 0 16px; font-size: var(--o-font_size-h4); line-height: var(--o-line_height-h4); }
/* ⚠️ M / S 两处媒体查询必须成对编写：M 档不写 min-width: 841px 时仍会命中 601~840px，且源码在后盖掉下面的 S */
@media (min-width: 841px) and (max-width: 1680px) {
    .otab__item { height: 44px; padding: 2px 0 16px; font-size: var(--o-font_size-text2); line-height: var(--o-line_height-text2); }
}
/* 601~840px / 340~600px = S：竖向 56×56、14px/22px、16px 居中短线指示器（圆角 9px，非全宽） */
@media (max-width: 840px) {
    .otab__item {
        width: 56px; height: 56px; padding: 4px 0 2px;
        flex-direction: column; justify-content: center; align-items: center;
        font-size: var(--o-font_size-tip1); line-height: var(--o-line_height-tip1);
    }
    .otab__item::after { position: static; left: auto; right: auto; bottom: auto; width: 16px; border-radius: 9px; margin-top: 4px; }
}

/* ===== Tab 交互态骨架（三条硬规则：不改字重 / Press 特异性 ≥ Hover / 必须 :not(:disabled)）===== */
.otab__item:hover:not(.is-active):not(:disabled) { color: var(--o-color-primary2); }
/* 特异性必须 ≥ hover 规则：按下时鼠标必然同时处于 hover，否则 Press 色被 Hover 色盖掉 */
.otab__item:active:not(.is-active):not(:disabled),
.otab__item.is-active:active:not(:disabled) { color: var(--o-color-primary3); }
.otab__item:active:not(:disabled)::after { background: var(--o-color-primary3); }
/* 禁用是终态：hover/press 已在选择器层被 :not(:disabled) 排除，此处只需写禁用默认态 */
.otab__item:disabled { font-weight: 400; color: var(--o-color-info4); cursor: not-allowed; }
/* 视觉移除指示器但保留占位：601~840 / 340~600 两档 S 档竖排（::after 为 16px 短线 + margin-top:4px，占文档流），
   display:none 会连占位删掉 → 禁用项只剩文字、在 column+center 下垂直居中，文字比其他页签低一截（实测错位约 10px）。
   visibility:hidden 保留 16px+4px 占位，文字与其他页签对齐；L/M 档指示器为绝对定位，不受影响。⛔ 禁写 display:none */
.otab__item:disabled::after { visibility: hidden; }
/* ⚠️ 若漏写 :not(:disabled)，`.otab__item:hover:not(.is-active)` (0,3,0) 会盖掉
   `.otab__item:disabled` (0,2,0) —— 禁用页签被鼠标划过就变色（实测 hover→primary2、
   press→primary3 且指示器变色）。见 hard-constraints.md §2.2 */

/* ===== 卡片文本约束（card.md 硬约束规则 8，按断点分档）===== */
.ocard__title { display: -webkit-box; -webkit-box-orient: vertical; -webkit-line-clamp: 2; overflow: hidden; }  /* ≥841px 标题最多 2 行 */
.ocard__desc  { display: -webkit-box; -webkit-box-orient: vertical; -webkit-line-clamp: 3; overflow: hidden; }  /* ≥841px 描述最多 3 行 */
@media (max-width: 840px) {   /* ≤840px：标题 1 行 / 描述 2 行，只改数值 */
    .ocard__title { -webkit-line-clamp: 1; }
    .ocard__desc  { -webkit-line-clamp: 2; }
}
```

> 可选元素缺省时按 §四 实现要点 3 合并间距：把合并值写到**缺口前最后一个实际存在的元素**上（如无楼层描述时，`.floor-card__title` 的 `margin-bottom` 取 40/24/16/12/12）。

---

## 七、楼层自检清单

```
□ 楼层标题字号/行高是否为 40/56 · 32/44 · 24/32 · 18/26 · 18/26（semibold / info1）？
□ 楼层描述字号/行高是否为 16/24 · 14/22 · 14/22 · 14/22 · 14/22（regular / info2）？
□ 二级标题字号/行高是否为 24/32 · 20/28 · 18/26 · 16/24 · 16/24（semibold / info1）？
□ Tab 尺寸是否为 L / M / M / **S** / **S**（tab.md；601~840px 与 340~600px 为竖向 56×56、14px/22px、16px 居中短线指示器）？
□ Tab 是否含 tab.md Part C 全量交互态（Hover 非选中仅变 primary2 / Press primary3 + 同色下划线 / Disabled info4 去下划线），而非只有 `.is-active`？
□ Tab 的 hover / press 是否**未改字重**（字重只由「是否选中」决定；⛔ 禁止 hover 加粗——实测拉丁标签 400→600 文字宽 +1.9px，下划线变宽、后续页签被推挤）？
□ Tab 的 Press 规则特异性是否 ≥ Hover 规则（否则按下时 Press 色被 Hover 色盖掉，表现为「按下毫无变化」）？
□ Tab 的 hover / press 规则是否**都带 `:not(:disabled)`**——鼠标划过禁用页签时，颜色/字重/指示器/背景与禁用默认态逐项相同（⛔ `.item:hover:not(.is-active)` (0,3,0) 会盖掉 `.item:disabled` (0,2,0)，见 hard-constraints.md §2.2）？JS 侧禁用项是否不响应点击、不切换 is-active？
□ Tab 禁用态指示器是否写 `visibility: hidden`（而非 `display: none`）——601~840 / 340~600 两档 S 档竖排时，禁用页签文字是否与同排其他页签**同一水平线对齐**（display:none 会删掉 16px+4px 占位，文字在 column+center 下垂直居中、低一截，实测错位约 10px）？
□ 卡片文本是否按断点分档截断——≥841px 标题 line-clamp: 2 / 描述 3；≤840px 标题 1 / 描述 2（四件套齐全含 overflow: hidden，超出末尾 ...）？
□ hover / press 是否一条 `font-weight` 都没写（全局字重铁律，见 hard-constraints.md §2.1）？Press 规则特异性是否 ≥ Hover 规则？
□ 查看更多按钮是否为 text 变体 L / M / M / S / S（button.md）？
□ 七项间距是否逐档等于间距表固定值（未取 --o-r-gap-* 近似）？
□ 「楼层标题 → 楼层描述」移动两档是否为 16px（未被"修正"为 8px）？
□ 可选元素缺省时是否按"取相邻两行较大值"合并（标题组到卡片容器恒为 40/24/16/12/12）？
□ 相邻楼层总间距是否为 72/56/40/32/32（未叠成 2 倍）？
□ 340~600px 卡片是否**一排两卡**（卡宽 150px = (312−12)/2，前两张完整可见不裁切）、第 3 张起是否横向滑动（未换行、未变成一排一卡）？
□ 340~600px 横滑容器右侧是否按卡片水平间距规则排布——`width: calc(100% + 36px)` + `padding-right: 12px`：第 3 张是否露出一个间距（12px）、滚到最右时末卡右缘是否距容器右缘 12px（未处理则第 3 张被完全裁掉、卡片2 右缘贴边，12px 缝露在容器外变白缝）？
□ >5 卡是否四列换行（7 卡 = 4+3 两行、末行左对齐不拉伸）；340~600px 档是否按数量选模式——≤6 卡横滑、>6 卡平铺为默认（横滑为备选）？
□ ≥1681px 容器是否为 1488px + 216px 边距；≤600px 是否为 312px + 24px 边距？
□ Tab / 卡片 / Button 是否未写内部样式（仅引用组件规范选尺寸变体）？
□ 所有间距值是否来自本模板固定表（禁止造数、禁止四舍五入）？
```