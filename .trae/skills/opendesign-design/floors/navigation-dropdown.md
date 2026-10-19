← [楼层模板索引](../SKILL.md#楼层模板索引) · [组件规范](../components/navigation.md)

# 导航下拉面板楼层模板（平铺式）

> 本模板为自包含生成单元。AI 读取本文件后即可精确生成「顶导菜单项点击/hover 展开的平铺式下拉面板」，无需再拼凑多个组件规范。
>
> 📑 **下拉面板有两种样式，分为两份模板**：平铺式 = **本文件**；分类平铺式 = [navigation-dropdown-categorized.md](navigation-dropdown-categorized.md)（多列 + 列头 + 条目说明 + 快捷链接栏）。生成前先确认用户要哪一种，对应关系见 §〇。
>
> 🔗 **搭配关系**：本模板**不单独构成页面楼层**，必须与 [floors/navigation.md](navigation.md)（顶导 Header）配合生成——顶导由 navigation 楼层产出，下拉面板由本模板产出，二者挂在同一个菜单项节点上。组件规格以 [components/navigation.md](../components/navigation.md) 为准，本模板只补充「顶导菜单项 → 下拉面板」这一层规范。
>
> 🔴 **顶导与触发器（菜单项）的全部样式唯一定义源是 [floors/navigation.md](navigation.md)**（`.o-nav__*` 命名，含容器 / Logo / 菜单 / 触发器五态 / 右侧操作区，可整段取用）：本模板**不定义、不重定义、不改写任何顶导与触发器样式**——无论下拉面板是哪种样式，顶导与触发器的样式完全一致，本模板只定义**下拉面板本身**（面板几何 / 选项 / 面板三态 / 定位显隐 / 交互 JS / 自适应）。
>
> ⚠️ **全套导航统一使用 `.o-nav__*` 命名，无需映射**：顶导骨架（`.o-nav` / `.o-nav__left` / `.o-nav__logo` / `.o-nav__menu` / `.o-nav__item` / `.o-nav__link` / `.o-nav__right` / …）整段取自 navigation.md「菜单项（触发器）完整样式」；本模板只新增面板相关 class（`.o-nav__dropdown` / `.o-nav__dropdown-item`），面板挂在 `.o-nav__item` 节点内。

---

## 〇、模板定位与两种面板样式

下拉面板共两种样式，**均已覆盖，分为两份模板**：

| 样式 | 模板文件 | 特征 |
|------|---------|------|
| **平铺式** | **本文件** `navigation-dropdown.md` | 垂直列表、选项独占一行、无分类标题（⛔ 不是横向 flex-wrap 平铺，见踩坑 #14） |
| **分类平铺式** | [navigation-dropdown-categorized.md](navigation-dropdown-categorized.md) | 多列平铺：分类列头 + 列内条目（标题 + 说明）+ 快捷链接栏 |

本文件仅定义**平铺式**。生成前确认用户要的是哪一种；要分类平铺式 → 读 `navigation-dropdown-categorized.md`，**禁止用平铺式顶替**。

**两份模板的协作方式**（生成分类平铺式时必读）：

| 项 | 说明 |
|---|---|
| 基类共享 | 两份模板的面板都挂 `.o-nav__dropdown` 基类——面板定位（`top: calc(100% + 4px)`、居中触发器）、显隐三规则（hover / focus-within / data-open）、4px 热区桥接、单开互斥 `:has()`、MB 左对齐、交互 JS **全部由本文件定义**，分类平铺式模板只写修饰类与内部结构，禁止重复定义 |
| 修饰类 | 分类平铺式追加 `.o-nav__dropdown--categorized`，用复合选择器 `.o-nav__dropdown.o-nav__dropdown--categorized` 覆盖布局（flex-direction / gap / padding / width） |
| 混挂互斥 | 同一顶导内两种样式可混挂（如「开发」挂分类平铺式、「下载」挂平铺式），单开互斥规则指向基类，**同时覆盖两者**，无需另写 |
| 漏基类后果 | 分类平铺式若只挂修饰类不挂基类，定位/显隐/互斥全部失效 |

> 🚨 **生成后必须实测，不许"写完就交"**：本楼层的踩坑表（§九）里每一条——面板闪烁关闭、选项 hover 色不对、高度被写死裁切、面板溢出视口——**都只有渲染出来才看得见**。交付前至少：① 无头浏览器截图，宽度覆盖 **1920 / 1440 / 1200 / 840 / 600** 五档；② hover 与点击锁定态各截一次（只截默认态会漏掉全部交互样式问题）；③ 顶导与面板交界处取一列像素确认阴影渐变存在（纯白 = 阴影没画出来，修法见 floors/navigation.md §顶导阴影）。⚠️ 尤其注意：**父级 `z-index` 盖不住自己的后代、`height: 0` 不渲染 `box-shadow`**，这两条反直觉特性只能靠实测发现。

---

## 涉及组件

| 组件 | 规范文档 | 本楼层使用的变体 |
|------|---------|----------------|
| ONavigation | [components/navigation.md](../components/navigation.md) | type=PC 顶导（面板挂载于顶导菜单项） |
| OTab | [components/tab.md](../components/tab.md) | text 变体 · medium 尺寸（顶导菜单项 = 面板触发器） |
| ODropdown | [components/dropdown.md](../components/dropdown.md) | 仅参照其「展开面板」通用规格（间距 4px / 圆角 4px / 阴影 / 内边距） |
| OLink | [components/link.md](../components/link.md) | 面板菜单项文字链接 |

---

## 一、面板定位与触发规则

```
ONavigation 顶导（见 floors/navigation.md，高 72px）
│
├── [Logo] [导航菜单 FRAME]
│              │
│              └── [菜单项 × N]← 触发器（完整样式见 floors/navigation.md「菜单项（触发器）完整样式」，position: relative 由骨架提供）
│                   │  data-open="true" 时展开面板
│                   │
│                   └── [下拉面板]（position: absolute, top: calc(100% + 4px)）
│                       │  display: flex | flex-direction: column | align-items: stretch
│                       │  gap: 0（选项容器间距 0，无间隙）
│                       │  padding: 4px（四边 → 色块左右自然留 4px）
│                       │  left: 50% + transform: translate(-50%, …)（水平居中于触发器）
│                       │  width: auto（内容撑开，不超过 1488px）
│                       │  background: var(--o-color-fill2)
│                       │  border-radius: 4px
│                       │
│                       └── [选项容器 × N]（垂直排列，一项一行，width: 100%）
│                            height: 40px（>1440px）/ 32px（841~1440px）
│                            padding: 0 12px | border-radius: 4px
│                            background: transparent（仅交互态填充）
│                            16px / 24px | color: var(--o-color-info2)
│                            └─ hover / 点击态时色块撑满整行（左右各让出面板 padding 4px）
│
└── [右侧操作区]（搜索 / 源码下拉 / 图标）
```

> ⚠️ **是垂直列表，不是横向平铺**：每个选项独占一行，`flex-direction: column` + `gap: 0` + `width: 100%`。「平铺式」指的是**选项不分层、无分类标题**（与第二轮的「分类平铺式」相对），不是横向排列。

### 触发与展开规则

| 项 | 规则 |
|----|------|
| 触发器 | **顶导菜单项本身**（不是右侧「源码」ODropdown 按钮） |
| 展开方式 | **两种途径并存，语义不同**（详见下方「交互模型」表）：hover = 临时展开；click = 锁定展开 |
| 与顶导间距 | **4px**（与 ODropdown 面板一致，见 dropdown.md「展开面板规格」） |
| 水平定位 | **面板水平居中于触发器**（`left: 50%` + `translateX(-50%)`）；居中后预计溢出视口/内容区时，改为左对齐触发器（`left: 0`）或收紧宽度兜底 |
| 面板宽度上限 | `max-width: 1488px`（内容区宽）；⚠️ 居中定位下面板可能左右都溢出，须同时校验左右两侧不超出内容区 |
| 同时展开 | 同一时刻**只允许一个面板展开**（鼠标移入其他菜单项时切换）。⚠️ hover 与 `data-open` 锁定是两套独立机制，仅靠 click JS 关其他面板不够——hover B 时 A 的锁定面板仍会显示导致重叠，必须同时用 CSS `:has()` 压制（见 §八「单开互斥」） |
| z-index | 面板 `z-index: 100`；⚠️ 顶导容器**禁止** `overflow: hidden`（会裁剪面板） |
| 顶导下阴影 | 面板是 `.o-nav` 的后代，必然整条盖住顶导的下阴影。⚠️ **不要试图用"顶导 z-index 101 > 面板 100"解决**——z-index 只在同一父级的定位兄弟之间比较，父子之间无意义（实测无效）。必须由 `.o-nav::after` 补偿层（`top: 0` + `height: 100%` 与顶导等高 + `z-index: 102`）重画阴影叠在面板之上；`height: 0` 不渲染阴影、`height: 1px` 会被模糊稀释，详见 floors/navigation.md §顶导阴影 |

#### 交互模型（hover 临时态 vs click 锁定态）

| 途径 | 面板是否固定 | 何时消失 | 用途 |
|------|-----------|---------|------|
| **hover** | 否，**临时展开** | 鼠标一移走就消失 | 鼠标用户快速浏览 |
| **click** | 是，**锁定展开** | 不随鼠标移走消失；需**再次点击同一菜单项**才关闭 | 触屏设备无 hover；用户想固定面板操作 |

**行为要点**：

1. **hover 不改变锁定状态**：hover 其他菜单项只会临时显示其面板，不会取消已有 click 锁定；鼠标移出整个菜单后，被互斥临时隐藏的锁定面板**恢复显示**（锁定态本就该保持）
2. **click 是切换（toggle）**：点已锁定的菜单项 → 关闭；点未锁定的 → 关闭其他所有锁定项后锁定当前项（单开）
3. **锁定项再次被 hover 时面板保持**：鼠标移走仍显示（锁定优先于 hover 的临时性）
4. **`:focus-within` 仅作键盘兜底**，不构成第三种触发语义；关闭时须 `link.blur()`，否则 `:focus-within` 会立即使面板重新显示，导致点击关不掉

> ⚠️ **4px 间隙的热区断桥问题（极易踩坑）**：面板与触发器之间有 4px 物理间隙，鼠标从触发器移向面板会穿过「死区」导致面板闪烁关闭。必须给面板补一个透明桥接热区：
> ```css
> .o-nav__dropdown::before {
>     content: '';
>     position: absolute;
>     top: -4px; left: 0; right: 0; height: 4px;  /* 填补 4px 间隙 */
> }
> ```

---

## 二、面板内容排布

平铺式面板 = **垂直列表**，每个选项独占一行，**无分类标题、无分组**。

**示例 · 「下载」菜单项下的选项列表**：

```
┌──────────────────────────────┐
│ 组织结构                      │  ← 默认：透明底
│ 成果获奖                      │
│ 下载链接                      │
│ 下载镜像                      │
│ 全部镜像                      │
│ 社区发行版                    │  ← 点击态（:active）：色块撑满整行（左右各让 4px）
│ 社区镜像                      │
│ 商业操作系统                  │
│ 开源方案                      │
│ 归档版本                      │  ← 禁用：info4 文字
└──────────────────────────────┘
   ↑                        ↑
   面板 padding 4px，色块左右各让出 4px
```

**核心规则**：

1. 每个选项 = 一个**透明容器**（`background: transparent`），只有 hover / 已选时才填充色块
2. 色块**撑满整行**（`width: 100%`），**不受文字长短影响**，左右始终各让出面板 padding 的 4px
3. 容器高度**固定**：`40px`（>1440px）/ `32px`（841~1440px）
4. 容器之间间距 **`gap: 0`**，色块上下相接无空隙
5. 面板宽度由**最长选项文字**撑开（`width: auto`），不是固定值

> 面板高度由**选项数量**决定，不是固定值。按当前规格反推：面板高 = `8px(padding) + 40px × N`（>1440px）或 `8px + 32px × N`（841~1440px）。生成时一律 `height: auto`，**禁止写死高度**。

---

## 三、精确数值速查

| 规格项 | 值 | 来源 |
|--------|---|------|
| 面板与顶导间距 | 4px | dropdown.md 展开面板规格 |
| 面板圆角 | 4px | dropdown.md |
| 面板背景 | `var(--o-color-fill2)` | dropdown.md |
| 面板阴影 | `0 6px 24px rgba(18,20,23,0.08)` | dropdown.md DROP_SHADOW |
| 面板内边距 | **4px（上下左右均 4px）** | 用户确认 |
| 面板布局方向 | `flex-direction: column`（垂直列表，一项一行） | 用户确认 |
| **选项容器间距（gap）** | **0px** | 用户确认 |
| **选项容器高度** | **40px（>1440px）/ 32px（841~1440px）** | 用户确认 |
| 选项容器宽度 | `100%`（撑满面板内容区，左右各让出 4px） | 用户确认 |
| 选项容器默认背景 | `transparent`（仅 hover / 已选时填充） | 用户确认 |
| 选项文本左右内边距 | 12px | 待确认 |
| 选项字号 / 行高 | 16px / 24px（>1440px）· 12px / 18px（841~1440px）· 14px / 22px（MB） | 用户确认 |
| 选项文字色 | `var(--o-color-info2)` | 设计稿标注 `#000000 80%` |
| 选项圆角 | 4px | — |
| 选项 hover 背景 | `var(--o-color-control2-light)`（`#F0F5FF` 极浅蓝） | 用户确认 |
| 选项 hover 文字色 | `var(--o-color-info2)`（**保持不变**） | 用户确认 |
| 选项点击态背景 | `var(--o-color-control3-light)`（`#E6EDFF` 浅蓝） | 用户确认 |
| 选项点击态文字色 | `var(--o-color-primary1)` | 用户确认 |
| 选项禁用文字色 | `var(--o-color-info4)` | 用户确认 |
| 面板宽度 | `auto`（最长选项文字撑开），`max-width: 1488px` | 设计稿自适应规则 |
| 面板高度 | `auto`（= `8px + 容器高 × N`） | 规格反推 |
| 触发器全部样式 | 见 [floors/navigation.md](navigation.md)「菜单项（触发器）完整样式」——**本模板不定义** | — |

---

## 四、状态矩阵（触发器 + 面板菜单项）

> 🔴 **触发器（顶导菜单项）的状态不在本模板定义**：五态（默认 / Hover / 展开 / 选中 / 禁用）的完整样式——字号字重、颜色、指示线、全部禁令——**唯一定义源是 [floors/navigation.md](navigation.md)「菜单项（触发器）完整样式」**（由 components/navigation.md Part C 派生），生成顶导时必须从该处整段取用，本模板不重定义、不改写。面板菜单项（选项）的状态由本模板定义，见 §4.2。二者必须分开实现，禁止把触发器状态搬到选项上。

### 4.1 触发器（顶导菜单项）——样式归 navigation.md，本模板只列与面板相关的行为

与面板相关的两条行为规则（属交互模型，非样式定义）：

1. **面板展开时，触发器视觉同 Hover**（仅文字 `primary2`、背景透明、无指示线、字重不变）——完整五态与禁令（任何状态无背景色块 / 全状态 Regular(400) / 禁止 `border-radius` / 选中优先）见 [floors/navigation.md](navigation.md) 踩坑 #9/#17/#18/#19
2. **`is-current` 随点击迁移**（单选、不做 toggle），JS 见 §八

### 4.2 面板选项（OLink · 透明容器）

| 状态 | 字号 / 行高 | 容器高度 | 背景 | 文字色 |
|------|-----------|---------|------|--------|
| 默认 | 16px / 24px | 40px | **透明** | `var(--o-color-info2)` |
| Hover | 16px / 24px | 40px | `var(--o-color-control2-light)`（#F0F5FF）**撑满整行** | `var(--o-color-info2)`（**保持不变**） |
| 点击态（按下瞬间） | 16px / 24px | 40px | `var(--o-color-control3-light)`（#E6EDFF）**撑满整行** | `var(--o-color-primary1)` |
| 禁用 Disabled | 16px / 24px | 40px | 透明 | `var(--o-color-info4)` |

> ⚠️ **选项没有持久选中态**：面板选项是**点击即跳走**的链接，不存在「当前停在这一项」的持久状态——与顶导触发器的 `is-current`（当前页 + 2px 指示线）是两回事，禁止把 `is-current` 语义搬到选项上。
>
> ⚠️ **`is-selected` = 点击态，不是选中态**：`is-selected` 表达的是**鼠标按下的那一瞬间**（`:active`）的视觉反馈——`control3-light` 色块 + `primary1` 文字，鼠标松开即恢复默认态。禁止把它当「已选中项」常驻显示。
>
> ⚠️ **容器默认必须透明**：色块只在 hover / 点击态时出现，默认态看不到任何背景。禁止给选项容器加常驻底色。
>
> ⚠️ **色块撑满整行**：`width: 100%`，与文字长短无关，左右始终各让出面板 padding 的 4px。禁止用 `display: inline-block` 或让容器收缩包裹文字。
>
> ⚠️ **hover 文字色不变**：hover 只换背景，文字**保持 `info2`**。
>
> ⚠️ **hover 与点击态用两个不同色阶**：hover = `control2-light`（#F0F5FF 极浅蓝），点击态 = `control3-light`（#E6EDFF 浅蓝），后者更深一级，表达「按下」的力度反馈。
>
> ⚠️ **容器间距 0**：`gap: 0`，相邻色块上下相接无空隙。禁止加 `gap` 或 `margin-bottom`。

---

## 五、颜色（全量语义 Token，禁止硬编码 rgba / hex）

| 区域 | Token |
|------|-------|
| 面板背景 | `var(--o-color-fill2)` |
| 菜单项文字（默认 / hover） | `var(--o-color-info2)` |
| 菜单项点击态文字 | `var(--o-color-primary1)` |
| 菜单项 hover 背景 | `var(--o-color-control2-light)`（#F0F5FF 极浅蓝） |
| 菜单项点击态背景 | `var(--o-color-control3-light)`（#E6EDFF 浅蓝） |
| 菜单项禁用文字 | `var(--o-color-info4)` |

> 🔴 **触发器相关颜色（默认 `info1` / hover·展开态 `primary2` / 选中态 `primary1` + 2px 指示线 / 禁用 `info4`）不在本表定义**——唯一定义源是 [floors/navigation.md](navigation.md)「菜单项（触发器）完整样式」。本表只列**面板**颜色。⛔ 触发器任何状态都无背景 Token（背景一律透明），详见 navigation.md 踩坑 #18。

### ⚠️ control2 / control3 的取值消歧（必读）

`--o-color-control2-light` / `--o-color-control3-light` 在 components/navigation.md 附录 B 中各有**多种语义**，直接引用极易取错。本楼层（顶导 mega 下拉面板菜单项）的取值固定为：

| Token | 本楼层含义 | 值 | ❌ 不要用它的其他含义 |
|-------|-----------|-----|---------------------|
| `--o-color-control2-light` | 菜单项 **hover 背景** | `#F0F5FF`（rgb(240,245,255) 极浅蓝） | ~~`#002FA7`（OSearch Hover 描边）~~ |
| `--o-color-control3-light` | 菜单项 **点击态背景** | `#E6EDFF`（rgb(230,237,255) 浅蓝） | ~~用户名区 Press 背景~~ |

### ⚠️ primary2 的取值消歧（必读，2026-10-15 新增）

**同一个 `--o-color-primary2` 在不同组件规范里有两个值**，导航楼层只认上游真源：

| 来源 | primary2 值 | 处置 |
|------|-----------|------|
| **opendesign-tokens/references/tokens-openeuler.md**（上游真源） | **`#5177CA`**（rgb 81,119,202） | ✅ **本楼层唯一取值** |
| components/dropdown.md 附录 / components/search.md 附录（来源各自的 demo 页） | `#0047D9` | ❌ **废弃值，禁止用于导航楼层** |

> ⚠️ 生成顶导时若需要给「源码」按钮（ODropdown text 变体）或「更多项目」按钮（OButton text + large）写 hover 色，**同样用 `#5177CA`**，禁止从 dropdown.md / search.md 附录抄 `#0047D9`——否则同一顶导内会出现两种 hover 蓝。引用一律写 `var(--o-color-primary2)`，禁止把色值写死进 CSS。

> 面板阴影 `rgba(18,20,23,0.08)` 为 DROP_SHADOW 组件特例值，上游语义 Token 无对应变量，允许保留规格值并加 HTML 注释「组件特例值，来源 components/dropdown.md」，禁止扩散到其他区域。

---

## 六、自适应规则（三档断点）

| 视口宽度 | 选项容器高度 | 选项字号 / 行高 | 面板布局 |
|---------|-----------|--------------|---------|
| ≥ 1441px | **40px** | **16px / 24px** | PC：`auto` 宽 + 居中触发器 |
| **841 ~ 1440px** | **32px** | **12px / 18px** | **保持 PC 布局不变** |
| ≤ 600px | 40px（推断，待确认） | 14px / 22px | MB：100% 宽 + 左对齐 |

**核心规则**：

1. 面板宽度默认 `auto`（最长选项文字撑开），**不超过内容区宽度**（PC 1488px）
2. **841~1440px 只改两件事：容器高度 40→32px、字号 16→12px**；`gap: 0`、`padding: 4px`、`width: 100%`、居中定位**全部保持不变**——这是最容易做错的一档
3. MB 端（≤600px）面板全宽 + 左对齐，从顶导下方展开
4. 面板高度 = `8px + 容器高 × N`，随选项数量增长，**禁止裁切、禁止内部滚动条**

> ⚠️ **601~840px 为规范空档**：hard-constraints §1 只定义了 ≤600px（MB）与 ≥841px（PC）两侧，601~840px 区间无定义。当前暂归入 PC 档（32px 容器 + 12px 字号），**需你确认**（见「待确认项」#6）。

---

## 七、MB 端规格（≤600px）

| 规格项 | PC（≥1441px） | 841~1440px | MB（≤600px） |
|--------|-------------|-----------|-------------|
| 面板宽度 | auto（最长文字撑开） | auto（同 PC） | 100%（312px 内容区） |
| 面板内边距 | 4px（四边） | 4px（同 PC） | 4px（四边） |
| 面板布局 | column + gap 0 | 同 PC | column + gap 0 |
| 选项容器高度 | **40px** | **32px** | 40px（推断，待确认） |
| 选项容器宽度 | 100% | 100% | 100% |
| 选项文本左右内边距 | 12px | 12px | 12px |
| 选项字号 / 行高 | 16px / 24px | 12px / 18px | 14px / 22px |
| 选项圆角 | 4px | 4px | 4px |
| 面板水平定位 | 居中触发器 | 居中触发器（同 PC） | 左对齐 `left: 0` |

> **面板内边距四边均 4px、gap 0、容器高度 40px/32px** 已由用户确认（2026-10-14）。
>
> MB 端菜单项字号取 MB 列「常规正文」= 14px/22px（硬约束 §2 PC↔MB 映射）；MB 下面板改**左对齐**（`left: 0`，去掉 `translateX(-50%)`），因为 312px 内容区下居中已无意义且易溢出。
>
> ⚠️ **选项文本左右内边距 12px 与 MB 容器高度 40px 为推断值，需你确认**（见「待确认项」#2、#7）。

---

## 八、HTML / CSS 实现骨架

```html
<!-- 顶导菜单项（触发器）：.o-nav__item 即 floors/navigation.md 顶导骨架的菜单项节点
     （全套导航统一 .o-nav__* 命名，无需映射；顶导与触发器样式由 navigation.md 整段提供，此处不重定义）
     · is-current = 当前页选中态（底部 2px 指示线），只能有一个，点击时 JS 迁移
     · data-open = 面板锁定展开态，点击切换，单开互斥 -->
<div class="o-nav__item is-current" data-open="false">
    <a class="o-nav__link" href="#">下载</a>

    <!-- 平铺式下拉面板（垂直列表，选项独占一行） -->
    <div class="o-nav__dropdown" role="menu">
        <a class="o-nav__dropdown-item" href="#">组织结构</a>
        <a class="o-nav__dropdown-item" href="#">成果获奖</a>
        <a class="o-nav__dropdown-item" href="#">下载链接</a>
        <a class="o-nav__dropdown-item" href="#">下载镜像</a>
        <a class="o-nav__dropdown-item" href="#">全部镜像</a>
        <a class="o-nav__dropdown-item" href="#">社区发行版</a>
        <a class="o-nav__dropdown-item" href="#">社区镜像</a>
        <a class="o-nav__dropdown-item" href="#">商业操作系统</a>
        <a class="o-nav__dropdown-item is-disabled" aria-disabled="true" href="#">开源方案</a>  <!-- 禁用态 -->
    </div>
</div>
```

> ⚠️ 面板选项状态类名共两种：默认（无类）/ `.is-disabled`（禁用）；点击态（`control3-light` 色块 + `primary1` 文字）由 CSS `:active` 伪类实现，**不需要也不允许写 `is-selected` 类名**（选项点击即跳走，无持久选中态）。触发器状态类名：`.is-current`（选中）+ `data-open`（锁定展开）+ `:hover`（临时展开）。

```css
/* ⚠️ 本模板不定义 :root 变量——hover/点击态背景直接用语义 Token
   var(--o-color-control2-light) / var(--o-color-control3-light)，
   取值消歧见「五、颜色」节，禁止另建 --o-nav-dropdown-item-hover-bg 之类的私有变量 */

/* 🔴🔴🔴 顶导骨架 + 触发器状态【本模板不定义，一处都不许写】 🔴🔴🔴
   .o-nav / .o-nav__left / .o-nav__logo / .o-nav__menu / .o-nav__item /
   .o-nav__link（基础几何 + hover / 展开态 / 选中态全部规则）/
   .o-nav__right / .o-nav__search / .o-nav__source / .o-nav__icon
   ——唯一定义源是 floors/navigation.md「菜单项（触发器）完整样式」（.o-nav__* 命名）。
   生成 HTML 时该段需整段复制进页面（验收页为自包含单文件，无共享 CSS 机制），
   ⛔ 复制时禁止任何改写 / 删减（align-self: flex-end、line-height 等易漏项必须原样带上），
   ⛔ 禁止在本模板重定义 / 收窄 / "顺手优化" 任何顶导与触发器规则。
   本模板的 CSS 从 .o-nav__dropdown（面板）开始，往上一律不碰。 */

/* ===== 平铺式下拉面板（垂直列表） ===== */
.o-nav__dropdown {
    position: absolute;
    top: calc(100% + 4px);                      /* 与顶导间距 4px */
    left: 50%;                                  /* ⭐ 水平居中于触发器 */
    z-index: 100;
    display: flex;
    flex-direction: column;                     /* ⭐ 垂直列表，一项一行 */
    align-items: stretch;                       /* ⭐ 选项撑满面板内容区宽度 */
    gap: 0;                                     /* ⭐ 选项容器间距 0，色块上下相接 */
    width: auto;                                /* 最长选项文字撑开 */
    max-width: 1488px;                          /* 不超过内容区；居中定位须同时校验左右不溢出 */
    height: auto;                               /* ⚠️ 禁止写死高度 */
    padding: 4px;                               /* ⭐ 四边均 4px → 色块左右自然让出 4px */
    background: var(--o-color-fill2);
    border-radius: 4px;
    box-shadow: 0 6px 24px rgba(18, 20, 23, 0.08);  /* 组件特例值，来源 dropdown.md */
    opacity: 0;
    visibility: hidden;
    transform: translate(-50%, -8px);           /* ⚠️ 居中 + 入场位移必须合并 */
    transition: opacity .2s ease, transform .2s ease, visibility .2s;
}

/* 4px 间隙热区桥接，防止鼠标穿越时面板闪烁 */
.o-nav__dropdown::before {
    content: '';
    position: absolute;
    top: -4px; left: 0; right: 0; height: 4px;
}

/* 三条显示规则对应三种语义：hover=临时 / focus-within=键盘兜底 / data-open=锁定（固定） */
.o-nav__item:hover > .o-nav__dropdown,
.o-nav__item:focus-within > .o-nav__dropdown,
.o-nav__item[data-open="true"] > .o-nav__dropdown {
    opacity: 1;
    visibility: visible;
    transform: translate(-50%, 0);              /* ⚠️ 保持 translateX(-50%)，否则面板右移 */
}

/* ===== 单开互斥（规范：同一时刻只允许一个面板展开） ===== */
/* ⚠️ hover 与 data-open 是两套独立机制，只写上面三条规则会出现
   「锁定展开的菜单项」与「鼠标 hover 的其他菜单项」面板同时显示、互相重叠。
   必须用 :has() 压制：菜单内任意【带面板】的菜单项被 hover / focus-within 时，
   其余项（含 data-open 锁定项）的面板一律隐藏 */
.o-nav__menu:has(> .o-nav__item:hover > .o-nav__dropdown) > .o-nav__item:not(:hover) > .o-nav__dropdown,
.o-nav__menu:has(> .o-nav__item:focus-within > .o-nav__dropdown) > .o-nav__item:not(:focus-within) > .o-nav__dropdown {
    opacity: 0;
    visibility: hidden;
    transform: translate(-50%, -8px);           /* 与入场态一致，保留 translateX(-50%) */
}
/* ⚠️ 选择器限定 :hover > .o-nav__dropdown：hover 无面板的菜单项（如当前页）不会误关锁定面板；
   鼠标移出整个菜单后，data-open 锁定项的面板恢复显示（锁定态本就应保持） */

/* ===== 选项容器（默认透明，仅交互态填充） ===== */
.o-nav__dropdown-item {
    display: flex;
    align-items: center;                        /* 文字垂直居中于固定高度容器 */
    width: 100%;                                /* ⭐ 撑满整行，色块不受文字长短影响 */
    height: 40px;                               /* ⭐ >1440px 固定 40px */
    flex: none;                                 /* 禁止被 flex 压缩 */
    padding: 0 12px;                            /* 文本左右内边距 */
    border-radius: 4px;
    background: transparent;                    /* ⭐ 默认透明 */
    font-size: 16px;
    line-height: 24px;
    font-weight: 400;
    color: var(--o-color-info2);                /* ⚠️ 80% 黑，不是 info1 */
    text-decoration: none;
    white-space: nowrap;
    transition: background-color .15s ease, color .15s ease;
}

/* hover：仅换背景，文字色保持 info2 */
.o-nav__dropdown-item:hover {
    background: var(--o-color-control2-light);   /* #F0F5FF 极浅蓝，撑满整行 */
    color: var(--o-color-info2);                 /* ⚠️ 保持不变，不要变 primary1 */
}

/* 点击态（:active 按下瞬间）：浅一档背景 + primary1 文字；选项点击即跳走，无持久选中态 */
.o-nav__dropdown-item:active {
    background: var(--o-color-control3-light);   /* #E6EDFF，撑满整行 */
    color: var(--o-color-primary1);
}

/* 禁用 */
.o-nav__dropdown-item.is-disabled {
    color: var(--o-color-info4);
    cursor: not-allowed;
}

/* ===== 响应式：841~1440px 容器降为 32px、字号 12px，布局不变 ===== */
@media (max-width: 1440px) and (min-width: 841px) {
    .o-nav__dropdown-item { height: 32px; font-size: 12px; line-height: 18px; }
    /* ⚠️ 只改容器高度与字号：gap / padding / width / 居中定位全部保持 PC 值 */
}

/* ===== MB 端（≤600px）：全宽 + 左对齐 ===== */
@media (max-width: 600px) {
    .o-nav__dropdown {
        left: 0;
        width: 100%;
        max-width: 100%;
        transform: translateY(-8px);             /* MB 左对齐，去掉 translateX(-50%) */
    }
    .o-nav__item:hover > .o-nav__dropdown,
    .o-nav__item:focus-within > .o-nav__dropdown,
    .o-nav__item[data-open="true"] > .o-nav__dropdown {
        transform: translateY(0);
    }
    /* MB 端互斥规则同步覆盖 transform（无 translateX） */
    .o-nav__menu:has(> .o-nav__item:hover > .o-nav__dropdown) > .o-nav__item:not(:hover) > .o-nav__dropdown,
    .o-nav__menu:has(> .o-nav__item:focus-within > .o-nav__dropdown) > .o-nav__item:not(:focus-within) > .o-nav__dropdown {
        transform: translateY(-8px);
    }
    .o-nav__dropdown-item { height: 40px; font-size: 14px; line-height: 22px; }
}
```

> ⚠️ **MB 端容器高度 40px 为推断值**（触屏目标区不小于 40px），设计稿未标注，**需你确认**（见「待确认项」#7）。

**交互 JS 最小实现**（hover 由 CSS 负责；JS 只做 click 锁定 + 单开 + 关不掉兜底）：

```js
document.querySelectorAll('.o-nav__item').forEach((item) => {
    const link = item.querySelector('.o-nav__link');
    link.addEventListener('click', (e) => {
        e.preventDefault();
        // click = 锁定展开（toggle）：固定显示，不随鼠标移走消失，需再次点击才关闭
        const willOpen = item.dataset.open !== 'true';
        // 单开：先关闭其他所有面板
        document.querySelectorAll('.o-nav__item[data-open="true"]')
            .forEach((i) => { i.dataset.open = 'false'; });
        item.dataset.open = String(willOpen);
        // 关闭时移出焦点：否则 :focus-within 会立即使面板重新显示，导致点击关不掉
        if (!willOpen) link.blur();
        // 选中态随点击迁移：点谁谁成为当前页；先移除其他项，单选不做 toggle
        // （无选中项会让顶导丢失「当前位置」信息）
        document.querySelectorAll('.o-nav__item.is-current')
            .forEach((i) => { i.classList.remove('is-current'); });
        item.classList.add('is-current');
    });
});
```

> ⚠️ JS 只负责 click 锁定与单开；**hover 临时展开与 hover/锁定态的重叠压制均由 CSS 完成**（`:hover` + `:has()`），两者缺一不可。

---

## 九、踩坑记录（面板部分；触发器踩坑见 floors/navigation.md）

> 🔴 **触发器（顶导菜单项）相关踩坑已统一收入 [floors/navigation.md](navigation.md) 踩坑记录**：背景色块 #18、指示线形态/漏写 #9、`is-current` 迁移 #10、字重与 `:has()` 收窄 #17、hover 抢选中态 #19。本表只列**下拉面板**踩坑。

| # | 问题现象 | 根因 | 正确做法 |
|---|---------|------|---------|
| 1 | 面板高度被写死，项多时裁切、项少时空洞 | 用了固定 height | 一律 `height: auto`，高度随选项数量增长 |
| 2 | 面板宽度写死，窄屏溢出画布 | 固定 width | `width: auto`（最长文字撑开）+ `max-width: 1488px`；≤600px 改 `100%` |
| 3 | 鼠标移向面板时面板闪烁关闭 | 触发器与面板间 4px 死区 | 面板加 `::before` 透明桥接热区（`top:-4px; height:4px`） |
| 4 | 面板被顶导裁剪或页面内容盖住 | 顶导 `overflow: hidden` / z-index 不足 | 顶导禁止 `overflow: hidden`；面板 `z-index: 100` |
| 5 | 选项 hover 背景色不对 | 误用用户名区的 `#EBF1FA` 或 OSearch 的 `#E8F0FE` | 本楼层 hover = `var(--o-color-control2-light)`（#F0F5FF） |
| 6 | 选项 hover 时文字也变蓝 | 照搬 ODropdown 规则把 hover 文字改成 primary1 | 本楼层 hover **只换背景，文字保持 `info2`** |
| 7 | 点击态与 hover 态背景色相同 | 二者都用了 control2-light | hover = control2-light（#F0F5FF），点击态 = control3-light（#E6EDFF） |
| 8 | 选项文字色过深/过浅 | 用了 info1 或 info3 | 必须是 `var(--o-color-info2)`（80% 黑） |
| 9 | **多个面板同时展开（重叠）** | hover 与 `data-open` 是两套独立机制：点开 A 锁定后再 hover B，两条显示规则同时命中，A、B 面板重叠 | click JS 先关其他面板（单开）+ **CSS `:has()` 压制**：菜单内任意带面板的项被 hover/focus 时，其余项（含锁定项）面板隐藏 |
| 10 | 选项内部文字折行 | 未禁止换行 | 选项 `white-space: nowrap`；容器高度固定，文字单行垂直居中 |
| 11 | 居中定位后面板整体右移/偏移 | `left: 50%` 后又用 `transform: translateY(0)` 覆盖掉了 `translateX(-50%)` | 入场与展开态的 transform 必须都写成 `translate(-50%, …)` |
| 12 | 居中定位下面板左右都溢出内容区 | 只校验了右侧 | 居中定位须**同时**校验左右两侧；溢出则改左对齐或收紧宽度 |
| 13 | 禁用态无视觉区分 | 只设了 `pointer-events: none` | 禁用文字色 `var(--o-color-info4)` |
| 14 | **选项做成横向 flex-wrap 平铺** | 误解「平铺式」= 横向排列 | 平铺式 = **垂直列表**，`flex-direction: column` + `gap: 0`，一项一行 |
| 15 | **色块只包裹文字、不撑满整行** | 选项用了 `inline-block` / `width: auto` / `align-self: flex-start` | 选项必须 `width: 100%` + `align-items: stretch`，色块左右各让出面板 padding 4px |
| 16 | **选项容器默认带底色** | 给容器加了常驻 background | 默认 `transparent`，仅 hover / 点击态时填充 |
| 17 | **选项容器高度被内容撑开** | 没写 height，靠 padding 撑 | 显式 `height: 40px`（>1440px）/ `32px`（841~1440px）+ `flex: none` |
| 18 | **选项之间出现空隙** | 加了 `gap` 或 `margin-bottom` | `gap: 0`，相邻色块上下相接 |
| 19 | **841~1440px 档把布局也改成了 MB** | 以为窄屏就该换 MB 布局 | 该档只改容器高度（40→32px）与字号（16→12px），其余全部不变 |
| 20 | **click 后面板关不掉** | 关闭时未移出焦点，`:focus-within` 立即使面板重新显示 | 关闭时 `link.blur()` |
| 21 | **把 hover 也做成锁定（或反之）** | 混淆两种触发语义 | hover = 临时展开（移走即隐，不改锁定态）；click = 锁定展开（固定，需再次点击关闭） |
| 22 | **选中态点不动，始终停在初始项** | `is-current` 只写死在 HTML 上，§八 的 JS 未处理 | 点击菜单项时把 `is-current` 迁移到该项（先移除其他项）；单选、**不做 toggle**，顶导始终保留「当前位置」 |
| 23 | **改了色值后 hover / 展开态"没反应"**（2026-10-15 实战踩中） | 把 `primary1` 改成 `primary2` 时**只改了 CSS 引用、漏在 `:root` 加定义**——`var()` 指向未定义变量时该属性**直接失效**（不是回落默认值），`color` 不生效，文字仍是默认色，看不出报错只能看到"hover 无变化" | 改完 CSS **必须跑定义/引用比对**（见「CSS 变量定义/引用比对」节的 comm 命令），差集为空才算过；缺的变量补进 `:root`，取值查 components/navigation.md Appendix A。同类历史坑：categorized 已确认 #17④ 漏定义 link1/2/3 导致图标/文字失效 |

---

## 十、楼层自检清单

```
□ 面板与顶导间距是否为 4px？
□ 面板圆角是否为 4px？
□ 面板背景是否为 var(--o-color-fill2)？
□ 面板阴影是否为 0 6px 24px rgba(18,20,23,0.08)（组件特例值）？
□ 面板是否 height: auto（禁止写死高度）？
□ 面板宽度是否为 auto（最长文字撑开）+ max-width: 1488px（≤600px 时 100%）？
□ 面板布局是否为 flex-direction: column（垂直列表，非横向平铺）？
□ 面板 align-items 是否为 stretch？
□ 面板 gap 是否为 0（选项容器上下相接无空隙）？
□ 面板内边距是否为四边 4px？
□ 选项容器是否 width: 100%（色块撑满整行，不受文字长短影响）？
□ 色块左右是否各让出面板 padding 的 4px？
□ 选项容器默认背景是否为 transparent（仅交互态填充）？
□ 选项容器高度是否固定 40px（>1440px）/ 32px（841~1440px）？
□ 选项容器是否设置 flex: none（防止被压缩）？
□ 选项文字是否垂直居中（align-items: center）？
□ 选项字号行高是否为 >1440px 16px/24px、841~1440px 12px/18px、MB 14px/22px（三档）？
□ 选项文字色是否为 var(--o-color-info2)（不是 info1）？
□ 选项是否设置 white-space: nowrap？
□ 选项 hover 背景是否为 var(--o-color-control2-light)（#F0F5FF）？
□ 选项点击态（:active）背景是否为 var(--o-color-control3-light)（#E6EDFF，与 hover 不同阶）？
□ 选项 hover 时文字色是否保持 info2（没有误改为 primary1）？
□ 选项是否未写 is-selected 类名（点击态由 :active 伪类实现，无持久选中态）？
□ 选项禁用文字色是否为 var(--o-color-info4)？
□ 面板是否水平居中于触发器（left: 50% + translate(-50%, …)）？
□ 居中定位的 transform 是否在入场态与展开态都保留了 translateX(-50%)？
□ 居中定位是否校验了左右两侧均不溢出内容区？
□ 4px 间隙是否加了 ::before 热区桥接？
□ 顶导是否未设置 overflow: hidden？
□ 面板 z-index 是否 ≥ 100？
□ 是否实现单开互斥（同时只展开一个面板）：click JS 先关其他 + CSS :has() 压制 hover 与锁定态重叠？
□ 顶导与触发器样式是否整段取自 floors/navigation.md「菜单项（触发器）完整样式」（本模板未重定义、未收窄、未自创状态）？
□ 是否同时支持 hover 与 click 两种展开途径（触屏可用）？
□ hover 是否为【临时展开】：鼠标移走面板立即消失，且不改变 click 锁定状态？
□ click 是否为【锁定展开】：面板固定不随鼠标移走消失，需再次点击同一菜单项才关闭？
□ 鼠标移出整个菜单后，被互斥临时隐藏的锁定面板是否恢复显示？
□ 关闭面板时是否调用 link.blur()（否则 :focus-within 会使面板重新显示、点击关不掉）？
□ 点击菜单项时 is-current 是否随之迁移（选中态可切换，单选不做 toggle）？
□ 841~1440px 档是否只改容器高度(40→32px)与字号(16→12px)、其余未动？
□ MB 端面板是否 100% 宽、左对齐、字号 14px/22px？
□ 所有间距/字号是否取自硬约束白名单？
□ **是否做过 CSS 变量定义/引用比对**：页面里每个 `var(--o-color-*)` 都必须在 `:root` 有定义（改完 CSS 必须跑，详见踩坑 #23）？
```

---

## CSS 变量定义/引用比对（改完 CSS 必做）

改色值 / 加状态最容易漏的是**只改引用、漏加定义**：`var()` 指向未定义变量时，该属性**直接失效**（不是回落默认值），表现为"hover 无变化""图标消失"这类看不出原因的故障。

```bash
# 以 check 页为输入：列出"已定义"与"已引用"的变量集，取差集
f="references/examples/.nav-dropdown-check.html"
defined=$(grep -o -- '--o-color-[a-z0-9-]*:' "$f" | sed 's/:$//' | sort -u)
used=$(grep -o 'var(--o-color-[a-z0-9-]*)' "$f" | sed 's/var(//;s/)//' | sort -u)
comm -13 <(echo "$defined") <(echo "$used")   # 有输出 = 漏定义，必须补进 :root
```

输出为空才通过；有输出则把缺的变量补进 `:root`（取值查 components/navigation.md Appendix A，禁止随手写 hex）。

---

## 十一、待确认项（规范未覆盖 / 设计稿歧义）

> 按 SKILL.md「规范未覆盖时的分级处理」，以下各点**禁止静默取值**，生成前需向用户确认：

| # | 待确认点 | 状态 |
|---|---------|------|
| 1 | 设计稿「默认」与「常态」标注值完全相同（16px Regular + 透明背景），未给出二者差异 | ⏳ 待确认 |
| 2 | **选项文本左右内边距 12px** 为推断值 | ⏳ 待确认 |
| 3 | **面板圆角 4px** 是否准确 | ⏳ 待确认 |
| 4 | 面板高度反推值与实测存在偏差 | 高度一律 `auto`，反推值仅作 QA 参考 |
| 5 | **601~840px 区间归属**：hard-constraints 只定义 ≤600px 与 ≥841px，该区间无定义 | ⏳ 待确认 |
| 6 | **分类平铺式**已补充完成（2026-10-14 第二轮输入）→ 见 [navigation-dropdown-categorized.md](navigation-dropdown-categorized.md) | ✅ 已完成 |
| 7 | **MB 端选项容器高度**（当前推断 40px） | ⏳ 待确认 |

### ✅ 已确认（2026-10-14，用户口述）

| # | 确认内容 |
|---|---------|
| 1 | 面板与顶导间距 = **4px** |
| 2 | 面板水平定位 = **水平居中于触发器** |
| 3 | 选项字号 = **16px/24px**（>1440px） |
| 4 | hover 背景 = **`--o-color-control2-light`**（#F0F5FF），文字色**保持 `info2`** |
| 5 | 点击态（`:active`）背景 = **`--o-color-control3-light`**（#E6EDFF），文字色 **`primary1`** |
| 6 | 禁用态文字色 = **`info4`** |
| 7 | 面板内边距 = **四边均 4px** |
| 8 | **面板为垂直列表**，选项容器间距 **gap = 0px** |
| 9 | 选项容器高度 = **40px（>1440px）/ 32px（841~1440px）** |
| 10 | **hover / 点击态色块撑满整行**，不受文字长短影响，左右各让出 4px |
| 11 | 选项容器**默认透明**，仅交互态填充 |
| 12 | **841~1440px** = 保持 PC 样式，仅字号降为 **12px** |
| 13 | 移动端断点 = **600px** |
| 14 | **触发器（顶导菜单项）任何状态都无背景色块**：展开态视觉同 Hover（仅文字 primary2），选中态 = 底部 2px primary1 指示线；hover 与 click 锁定**必须单开互斥**，不得出现两个面板重叠 |
| 15 | **交互模型**：hover = 临时展开（移走即消失、不改变锁定状态）；click = 锁定展开（面板固定，需再次点击同一菜单项才关闭） |