# Changelog

本文件记录 `opendesign-design` skill 的变更，供使用者判断是否需要重新安装。`SKILL.md` frontmatter 的 `last_update` 字段与本文件中最新条目日期对应。

分类：新增 / 更新 / 修正 / 移除 / ⚠️ 破坏性（破坏性表示按旧版 skill 已生成的产物失效或违规，需复核）。

---

## 2026-10-16

### 分类平铺式 MB 切换箭头 PC 档裸露修正

> 用户验收 B 分类平铺式截图时发现 PC 档「项目」列末尾突然冒出一个 `^` 箭头。根因：`.o-nav__cat-toggle`（MB 左列折叠/展开开关）被 HTML 骨架无条件渲染，但模板 CSS 只写了图标状态切换，漏写 PC 档隐藏规则。

#### 修正内容

1. **`floors/navigation-dropdown-categorized.md` §7.3 基础样式补 `display: none`**：`.o-nav__cat-toggle` 默认隐藏，仅在 `@media (max-width: 840px) and (min-width: 601px)` 里恢复 `display: block`
2. **§八规则表 #4 补充显隐约束 + 新增通用警告「MB 专属元素在 PC 档必须显式隐藏」**：HTML 骨架无条件渲染全量节点，靠 CSS 分档显隐——只写「MB 显示」不写「PC 隐藏」，PC 档就会多出不该有的元素
3. **自检清单新增 1 项**：`.o-nav__cat-toggle` 是否默认 `display: none`、仅在 ≤840px 档恢复显示
4. 同步修正验收页 `opendesign-navigation-categorized-check.html`，PC 档箭头消失、MB 档功能不受影响

#### 说明

- 本条为**生成侧约束补充**，不改动 MB 双列布局与折叠态规格本身
- 与同日「max-width 覆盖」「踩坑 #20 盒模型」并列，共同构成本次流程沉淀

### 分类平铺式通栏定位补 `max-width` 覆盖规则

> 生成 B 分类平铺式下拉面板并用 HTML 验收时，1920/1680 档快捷链接栏不靠右（距视口右缘不是 216px，悬在页面中间）。根因：基类 `.o-nav__dropdown` 的 `max-width: 1488px` 未被修饰类覆盖，通栏 `left: 0; right: 0` 实际被限宽。

#### 修正内容

1. **`floors/navigation-dropdown-categorized.md` §一定位代码块补 `max-width: none`**：复合选择器 `.o-nav__dropdown.o-nav__dropdown--categorized` 中显式覆盖基类 `max-width: 1488px`，让通栏真正撑满页面
2. **新增警告「通栏必须显式覆盖基类 `max-width`」**：归纳通用规则——凡基类有而修饰类不适应的约束（`max-width` / `transform` / `top` / `border-radius`）都必须逐条显式覆盖，不能只写自己新增的属性
3. 同步修正本次生成的验收页，六档（1920/1680/1440/1200/840/600）像素级验证通过：快捷链接栏宽 342px、距视口右缘精确 216px、三处内容左缘对齐

#### 说明

- 本条为**生成侧约束补充**，不改动分类平铺式的几何规格本身（通栏定位、216px 栅格边距均不变）
- 与同日「踩坑 #20 盒模型冲突」共同构成本次流程沉淀：下次生成前对照 navigation.md 自检清单 + 本模板的覆盖规则，可一步到位

### 导航顶导盒模型冲突修正 · 踩坑 #20

> 按导航流程生成全套并用 HTML 验收时，顶导整体歪斜（Logo 偏低、菜单文字偏高、选中下划线悬在导航栏中间）。定位为盒模型冲突，非对齐属性写错。

#### 修正内容

1. **`floors/navigation.md` 踩坑记录新增 #20**：`.o-nav__logo` 写 `height: 32px` 时，若页面存在全局 `* { box-sizing: border-box }`，32px 会被当作**含 padding 的总高**（本意是内容高），Logo 实际仅 32px、`.o-nav__left` 只剩 48px → 菜单 `align-self: flex-end` 贴到 y=60 而非导航底边 y=72（指示线落在导航栏正中）、Logo 在 48px 内居中偏至 y=22~54
2. **明确正确做法**：Logo **禁止写显式 height**，由 SVG 自身 32px + `padding: 20px 0` 撑出 **72px 总高**（与导航等高）；仓库已验证示例 `.nav-dropdown-check.html` 正是此写法（不写 height），故 border-box 下依然正确
3. **归纳通用规则**：凡依赖「height + padding 相加得总高」的节点（Logo 32+20+20、菜单项 48+24）都会在 border-box 下被压扁；生成时先查页面是否有全局 `box-sizing`，有则该类节点一律不写显式 height
4. **自检清单同步新增 2 项**：Logo 是否未写 height；生成前是否确认全局 box-sizing 并调整该类节点

#### 说明

- 已同步修正本次生成的验收页（删除 `height: 32px`），三层对齐恢复准确
- 本条为**生成侧约束**，不改动导航几何规格本身（Logo 仍为 136×32、padding 20px 0、总高 72px）

---

## 2026-10-15

### 源码下拉按钮 B.2 与下拉面板选项状态规则统一（续）

> 用户裁决：「B.2 ODropdown text + small 变体（源码下拉按钮）这部分应该是要与面板选项状态保持统一」。B.2 原为通用 ODropdown 规范，其面板菜单项规则与 floors 已裁决的顶导下拉面板规则正面冲突，且两边都叫「面板内菜单项状态矩阵」、无边界声明，AI 极易混用。

#### ⚠️ 破坏性（需复核的生成物）

1. **`components/navigation.md` B.2 的 Selected 持久选中态已作废**。此前按 B.2 生成的源码下拉面板若带 `.selected`（`rgba(0,47,167,0.08)` 底 + Medium 字重、持续显示到面板关闭），需改为按下瞬间态（`:active`，松开恢复）

#### 统一内容（B.2 五处）

1. **节首加适用边界声明**：B.2 只描述顶导右侧「源码」按钮（`.o-nav__source`）及其面板；**顶导菜单项**（`.o-nav__item` / `.o-nav__link`）挂载的下拉面板以 `floors/navigation-dropdown.md` §4.2 为唯一真源，禁止把 B.2 的 primary1 文字 hover / Semibold / Selected 搬到顶导菜单项面板上
2. **触发按钮 Hover 色 `primary1 #002FA7` → `primary2 #5177CA`**（对齐 dropdown.md text 变体与 floors 导航楼层；`navigation-dropdown.md` §五 早已明确禁抄 `#0047D9`，本次消除 B.2 的 primary1 误导）
3. **面板内菜单项 Hover 文字色 `primary1` → 保持 `info1`**（只换背景 `#F0F5FF`）
4. **按下态去字重与微缩**：Semibold(600) + `scale(0.98)` → **Regular(400) 无微缩**（保留背景 `#E6EDFF` + 文字 `primary1`，仅按住瞬间）
5. **删除 Selected 行与 `.o-dropdown__item.selected` CSS**：无持久选中态（选项点击即跳走/关闭面板）；「常见错误清单」同步换血——删「缺少 Selected 状态」，新增 4 条禁令（hover 文字不得变色 / 不得加持久选中态 / 不得切字重或加微缩 / 触发按钮 hover 必须 primary2）

#### 保持不变

- `B.1.1 搜索建议面板`（OSearch 专用，`#E8F0FE` hover 背景、40px 项高、8px 项间距、关键词 Semibold 高亮）与下拉面板选项规则不同域，已带差异标注，本次不动
- `Part E 用户名区`菜单项 hover 背景 `#EBF1FA` 与按下/选中态 Semibold 仍为原规范（与面板选项规则尚未统一，待后续确认）

### 导航架构重构：顶导/触发器样式唯一源 = floors/navigation.md（续）

> 用户裁决：「两个 dropdown 模板不应该涵盖顶部导航的样式，因为顶部导航的样式和触发样式都应该在 navigation.md 里就已经写完整了——无论下面的 dropdown 是什么类型，顶部导航的样式一定是一致的，仅仅只是下面下拉面板的区别而已。」

#### ⚠️ 破坏性（需复核的生成物）

1. **两份下拉模板不再包含任何顶导/触发器样式定义**。按旧版模板生成的全套导航，若顶导部分是照下拉模板复制的，需按 `floors/navigation.md`「菜单项（触发器）完整样式」复核（旧模板里的触发器规则已被删除，不再维护）

#### 架构调整（三份楼层模板的职责重划）

1. **`floors/navigation.md` 补全为顶导/触发器唯一样式源**：新增「菜单项（触发器）完整样式」整段可交付 CSS——顶导容器（含 `.o-nav::after` 阴影补偿层）→ 左侧区域（Logo/菜单 Flexbox 三层对齐）→ 菜单项五态（默认/hover/展开/选中/禁用，含选中优先于 hover 的优先级选择器）→ 右侧操作区（搜索框/源码下拉/图标）；原独立拆分的搜索框/源码下拉实现整段并入。全套导航统一 `.o-nav__*` 命名，**删除 class 映射表**（无需映射）
2. **`floors/navigation-dropdown.md`（平铺式）只定义下拉面板**：删除 §4.1 触发器状态矩阵、颜色表中的触发器行、CSS 骨架中全部触发器规则；CSS 从 `.o-nav__dropdown` 起；文件头加 🔴 架构约束块（顶导与触发器「一处都不许写」，唯一定义源 navigation.md，复制时禁止改写删减）
3. **`floors/navigation-dropdown-categorized.md`（分类平铺式）同样只定义面板**：删除 §4.1 状态矩阵与两段触发器 CSS（hover/选中态），改为只列与面板相关的两条行为规则；删除 class 映射表，改为统一命名说明
4. **踩坑/自检去重防幻觉**：触发器相关踩坑统一收入 navigation.md（#9 指示线形态/漏写、#10 `is-current` 迁移、#17 字重与 `:has()` 收窄、#18 背景色块、#19 hover 抢选中态）；两份下拉模板删除对应重复行并**整表重编号**（平铺式 26→23 条、分类平铺式 47→44 条），表头加「触发器踩坑见 navigation.md」指引；自检清单的触发器逐项检查改为一条「顶导与触发器样式是否整段取自 navigation.md」

#### 规则裁决落定（同步全链路）

5. **触发器 hover 色统一为 `primary2`**（#5177CA，上游真源）：此前两份模板记不同值，现由 navigation.md 骨架统一定义，两份下拉模板均不涉及
6. **面板选项无持久选中态**：选项是点击即跳走的链接，不存在「当前停在这一项」的状态；平铺式的 `control3-light` 色块仅是 `:active` 按下瞬间的**点击态**（松开即恢复），禁止 `is-selected` 类名、禁止 `primary1` 选项文字色；`is-current` 只属于顶导触发器（当前页 + 2px 指示线）
7. **CSS 变量完整性校验固化为流程**：改色值后必须跑「已定义 vs 已引用」差集检查（comm 命令，见两份模板「CSS 变量定义/引用比对」节）——漏定义时 `var()` 整条声明**静默失效**（不是回落默认值），表现为「hover 无变化」「图标消失」

#### 保持不变

8. `SKILL.md` 导航专属门禁三步（先生成顶导骨架 → 停下询问用户面板样式 → 确认后一并生成全套）已绑定，本次架构调整后流程不变；`components/navigation.md` Part H 继续指向楼层模板为 HTML 实现唯一真源

### 分类平铺式第四~九轮验收修正 + 防复现约束

#### 修正（`floors/navigation-dropdown-categorized.md`，用户逐轮验收确认）

1. **第四~九轮规格全部落定并同步模板 ↔ 验收页**：① 分类列头 / 条目标题按 **>1440px 16px / ≤1440px 14px** 分档、颜色统一 `info4` / `info1`，选项说明两档均 12px / `info2`；② **条目标题全状态统一 Semibold**（默认态即 600，hover / 按下 / 已选只改 color，仅禁用态回落 Regular）；③ 外链图标 **24×24**（覆盖硬约束 §5 默认档）、外链图标↔文本与红标签↔文本间距统一 **8px**；④ 红标签严格 OTag `fill + danger + small`；⑤「更多项目」改 **OButton `text` + `large`**（`<button>`，右箭头 24×24 在右侧）；⑥ 快捷链接纯文本项 = **灰底按钮块（342px / 40px / fill3 / link 距左 24px）+ 内层 OLink medium**（34px 高 / 上下 padding 5px / 左右 0 / 14px-22px / link1→link2→link3）**两层缺一不可**；⑦ 栏内 **`align-items: flex-start` 左对齐** + 整栏 **`margin-left: auto` 靠右**（贴栅格右缘，PC 距右 216px）；⑧ 图+文本卡片**无背后色块**、图文垂直居中、图 **121×68**、标题 16px/24px、描述 12px/18px **info3**、hover / 按下态与左侧分类条目一致；⑨ **条目说明 `min-height: 36px` 预留两行高度**（2 × 18px 行高，文案 1 行时也占两行空间，后续延展不挤压布局）；⑩ link 色 token 来源核实为 `opendesign-tokens/references/tokens-openeuler.md` 品牌色·链接色表（link1 #002FA7 / link2 #5177CA / link3 #002793），并修复验收页 `:root` 漏定义导致 `var()` 失效文字发黑的问题
2. **踩坑记录扩充至 41 条**：新增 #39 验收页与模板双文件同步（默认渲染可选模块 / 双向 diff / 辅助元素不得遮挡验证区）、#40 同类元素状态集合一致性（卡片漏 hover 态）、#41 描述文本预留两行高度
3. **`floors/navigation.md` 三处文档缺陷修正**（用户质询「顶导是否严格按 navigation.md 生成」后逐条核对发现）：① 源码下拉 CSS 片段 `line-height: 24px` → **22px**（违反 hard-constraints §2 的 14px↔22px 成对锁定，且与本文档速查表的 14px/22px 自相矛盾）；② 自检清单「源码按钮颜色 `info2`」→ **`info1`**（与本文档「颜色」节及 dropdown.md Part B「text 变体文字与图标均 info1」一致）；③ 资源表补「源码下拉箭头 = `public icons/icon-下箭头.svg`（保留 transform）」一行（原表缺此项，生成方无据可依只能手写 SVG）
4. **`.nav-dropdown-categorized-check.html` 顶导补齐 `align-self: flex-end`**：`.o-nav__item` 原只写 `position: relative`，漏了 navigation.md「菜单项完整样式」要求的 `align-self: flex-end`（贴底对齐）
5. **两份模板互标「顶导骨架共用」复用关系**（用户质询「分类平铺式顶导为何不直接复用平铺式」后确认）：两份下拉面板模板本就不定义顶导骨架——唯一定义源是 `floors/navigation.md`「菜单项（触发器）完整样式」，两份共用同一份；两份模板 §九/§八 CSS 骨架头部均已加 🔴 注释明示「顶导骨架本模板不定义、生成 HTML 时整段复制且禁止改写删减（含 `align-self: flex-end`、`line-height` 等易漏项）」。同时修正复制产生的两处偏差：`.nav-dropdown-check.html`（平铺式）body 字体原为 `'HarmonyOS Sans SC', 'HarmonyHeiTi', 'PingFang SC', sans-serif`（**违反 hard-constraints §2 铁律**）已改为 `'HarmonyOS Sans', 'HarmonyHeiTi'`，`.o-nav__item` 同样补齐 `align-self: flex-end`

#### 更新（防复现：多轮返工的流程根因是「规则未执行」而非「规则缺失」，补 4 个拦截点）

1. **`SKILL.md` 第四步**：读楼层模板从「读取获取规格」升级为**整本读完再落笔**——「生成前必读 / 已确认清单 / 冲突声明 / 踩坑记录」四节缺一不可；用户后续要求与模板旧值冲突时**先更新模板再生成**，与 hard-constraints 冲突时**停下报告**禁止自行折中
2. **`SKILL.md` 新增「验收页（check page）生成约定」**：① 验收页**默认渲染全部可选模块**（模板本体仍默认注释，两处刻意不同）；② 图例 / 标注等辅助元素禁止 `fixed` 遮挡验证区（默认 `<details>` 收起或移走）；③ 验收页 CSS 与模板逐条一致，唯一允许差异 = 可选模块默认渲染状态，每轮修正后必须双向 diff
3. **`global/hard-constraints.md` §6 自检清单扩充至 26 条**：新增 #22 `:root` Token 完整性（漏定义 → `var()` 静默失效 → 文字发黑 / 图标消失）、#23 图标必须 assets Read 后逐属性内联（禁止手写 SVG / `<img>` / 字符代替）、#24 OTag / OButton / OLink 必须按组件规范落笔、#25 同类元素状态集合一致、#26 描述文本预留两行高度

### 分类平铺式第十二轮 · 顶导阴影被面板遮挡 · z-index 层级修正（续）

#### 修正（`floors/navigation.md`）——阴影规范原本就有，缺的是层级规则

1. **确认顶导阴影规范已存在**：`box-shadow: var(--o-shadow-1)`（`0 3px 9px 0 rgba(0,18,85,0.078)`，一级阴影·浮层）在本文档「颜色」表、`--o-shadow-1` Token 定义（`:root`）、结构树中均已定义，来源 components/navigation.md。**不需要新增阴影值，缺的是让阴影可见的层叠规则**
2. **新增「顶导 z-index 必须 > 下拉面板」规则**（用户验收发现「顶导没有阴影」后确认根因）：顶导 `.o-nav` 取 **`z-index: 101`**，下拉面板为 100。根因 = 面板 DOM 位于顶导**内部**（后绘制），两者 z-index 相同时**面板整条盖住顶导的下阴影**，表现为交界处"平贴"、无悬浮感。顶导元素本身只占 0~72px，提到 101 不遮挡面板内容（72px 以下），只是让下阴影（72~81px）叠在面板顶部
3. **新增「顶导容器 CSS」完整片段**（`.header` 的 position / top / z-index / background / box-shadow），并标注 ⛔ 禁止 `overflow: hidden`（裁剪发生在层叠之前，z-index 再高也救不回被裁的面板）
4. **速查表新增两行**：顶导阴影值、顶导 z-index 101
5. **自检清单新增 3 条**：阴影是否设置、z-index 是否 101、顶导容器是否无 overflow:hidden
6. **踩坑记录新增 #15 / #16**：#15 = 阴影被面板盖住（遮挡 vs 样式缺失的排查区分）、#16 = overflow:hidden 裁掉面板

#### 修正（`floors/navigation-dropdown-categorized.md`）

7. **差异表新增「面板 z-index 100 且必须低于顶导 101」**；自检清单新增对应检查项；踩坑表新增 #44（阴影被盖）/ #45（overflow:hidden 裁面板），均回指 navigation.md #15/#16

#### 修正（`.nav-dropdown-categorized-check.html` / `.nav-dropdown-check.html`）

8. **两份验收页 `.o-nav` 的 `z-index: 100` → `101`**（阴影原本就写了，是被面板挡住看不见）；分类平铺式验收页新增「顶导阴影 / 层叠层级验证」卡片，图例新增「层叠」行

### 分类平铺式第十一轮 · 面板与顶导零间距 · 抽屉式展开（续）

#### 修正（`floors/navigation-dropdown-categorized.md`）

1. **面板与顶导间距 4px → 0px**（用户确认「没有间距，就像抽屉一样展开」）：`.o-nav__dropdown--categorized { top: 100% }`，紧贴顶导底边展开——⚠️ 覆盖继承平铺式基类的 `top: calc(100% + 4px)`。同步修改结构树、§一 继承规则表 + 差异表、§三 速查表、§九 CSS 骨架、§十一 自检清单（新增 3 条）、§十 踩坑（新增 #43）、已确认清单（新增 #22）；§「生成前必读」第 3 项补注本模板 top 与热区桥接的覆盖关系
2. **上圆角压平为 `border-radius: 0 0 4px 4px`**（零间距的必然推论，已向用户标注）：面板零间距紧贴顶导后，若保留基类的 4px 全圆角，面板左上 / 右上会各露出一道月牙形缺口，视觉上面板与顶导又"裂开"了。下圆角保留（面板底边是自由边）
3. **基类 `::before` 4px 热区桥接显式 `display: none`**：桥接本是补鼠标穿过面板与顶导之间间隙的路径，零间距后无间隙可穿，只剩白占顶导底部 4px 的副作用
4. **新增两条防错警告**：① 不要把隐藏态 `translateY(-8px)` 当成间距去改 `top`——那只是入场位移，展开终态是 `translateY(0)`，改了反而会在展开态留下真实间隙；② 零间距必须与压平上圆角同时做，只做一半视觉上仍然裂开

#### 修正（`.nav-dropdown-categorized-check.html`）

5. **同步 `top: 100%` + `border-radius: 0 0 4px 4px` + `::before` 关闭**；原「热区桥接 / 浮层遮挡验证」卡片改写为「抽屉式贴合 / 浮层遮挡验证」，图例「几何」行补零间距说明

### 分类平铺式第十轮 · 条目无选中态 + 顶导骨架复用（续）

#### 修正（`floors/navigation-dropdown-categorized.md`）

1. **面板条目删除「选中态」**（用户确认：导航是直接点击跳走的，没有选中态）：条目状态集合由五种收敛为**四种**（默认 / hover / 按下 / 禁用），删除 `.is-selected` 类与 `primary1` 条目文字色。同步清理 §2.2 元素构成表、§三 速查表、§4.3 状态矩阵、§五 颜色表、§九 HTML 骨架（`代码扫描` 去掉 `is-selected`）与 CSS（删 `.o-nav__cat-item.is-selected` 规则）、§十 踩坑（#6 三色分级改两色分级、新增 #42）、§十一 自检清单（原「条目已选是否为 primary1」改为「条目是否无选中态」反向检查项）；三色分级说明改为两色分级
2. **明确区分「条目无选中态」与「触发器 is-current」**：新增双向警示——条目禁止 `primary1` / `.is-selected`；触发器保留 `is-current`（当前页 + 2px 指示线 + primary1）。二者语义不同，禁止混用
3. **顶导骨架复用关系写进两份下拉模板**（用户质询「分类平铺式顶导为何不直接复用平铺式」后确认）：两份模板本就不定义顶导骨架，唯一定义源是 `floors/navigation.md`「菜单项（触发器）完整样式」；两份模板 CSS 骨架头部均加 🔴 注释——生成 HTML 时顶导段必须整段复制且禁止改写 / 删减（点名 `align-self: flex-end`、`line-height` 等易漏项）

#### 修正（`floors/navigation.md`）

4. **源码下拉 CSS 片段 `line-height: 24px` → `22px`**：原值违反 hard-constraints §2 的「14px 文字 ↔ 22px 行高」成对锁定，且与同文件速查表的 14px/22px 自相矛盾
5. **自检清单「源码按钮颜色 info2」→ `info1`**：原值与本文档颜色节、dropdown.md Part B 均冲突，照自检清单做反而会错
6. **资源表补「源码下拉箭头 = `icon-下箭头.svg`」**：原表只有 5 个图标、独缺此项，生成方查不到来源只能手写 SVG（即踩坑记录 #7 的成因）

#### 修正（`.nav-dropdown-categorized-check.html` / `.nav-dropdown-check.html`）

7. **分类平铺式验收页去掉 `代码扫描` 的 `is-selected`** 及对应 CSS 规则，图例「条目已选」色块改为「✕ 条目无选中态」警示，「三色分级验证」卡片改为「两色分级验证（条目无选中态）」
8. **平铺式验收页 body 字体改为 `'HarmonyOS Sans', 'HarmonyHeiTi'`**：原为 `'HarmonyOS Sans SC', 'HarmonyHeiTi', 'PingFang SC', sans-serif`，违反 hard-constraints §2 铁律（禁止 PingFang）
9. **两份验收页 `.o-nav__item` 补齐 `align-self: flex-end`**：复制顶导段时漏掉的 navigation.md 贴底对齐属性

---

## 2026-10-14

### 分类平铺式第二轮四项确认（续）

#### 修正（`floors/navigation-dropdown-categorized.md`）

1. **primary2 色值定案**：设计稿标注的 hover 色 `#5177FA` 与上游 `openeuler-token.json` 的 `--o-color-primary2` = `#5177CA` 不一致，用户确认**以上游语义 Token 为准**。§五「色值消歧」标题由 ⚠️ 改为 ✅，触发器冲突声明与条目三色分级注释中的 `#5177FA` 同步改为 `var(--o-color-primary2)`（#5177CA）。
2. **断点区间笔误修正**：原稿「1680~1201px 内容区距左右 40px」为笔误，正确区间为 **1440~1201px**。修正后 `1680~1441px`（左右 64px）与 `1440~1201px`（左右 40px）**互不重叠**，§六 断点表、实现要点（原第 3 条拆为两条）、CSS 媒体查询（原「响应式 2」拆为两条、后续编号顺延）全部同步；同时明确 1200~841px 档左右内边距沿用 40px（须显式写出，否则回退基准档 24px）。
3. **≤600px 右内容区不按 40% 收窄**：用户确认取 `max-width: none`，CSS 注释由「待确认 #4」改为「用户确认」，§七 MB 规格表与踩坑 #17 同步。
4. **快捷链接栏规格落定**：
   - **纯文本按钮**：内部 link 用 [components/link.md](../components/link.md) 的 **M（medium）尺寸**（文字 14px/22px、link 高 34px），**块内 link 距左 24px、上下居中**；块高 40px 仍为推断值
   - **带图卡片**：改为**左图右文**布局，图片 **16:9 比例**、高度与左侧文本块一致（高 36px / 宽 64px），**图文间距 8px**；替换原「上下结构 + 卡片宽 200px + 图高 96px」的推断实现
   - 涉及 §2.3、§三 速查表、§九 HTML 注释与 CSS、§十 踩坑（原 #24 拆为 #24 字号/比例 + #25 图片变形）、§十一 自检清单同步更新；**生成前必读**新增第 5 项（link.md 的 M 尺寸规格）
5. **待确认项收敛**：原 #1、#3、#4 移入「已确认」清单（新增第 13 条记录四项确认）；#5 收窄为「剩余推断值」（块高 40px、块间距 8px、卡片背景色、栏 min-width 200px、按钮文字色 info1 vs link1）。

### 分类平铺式第二轮输入

#### 新增
- **新增楼层模板 `floors/navigation-dropdown-categorized.md`（导航下拉面板 · 分类平铺式）**：按用户提供的设计稿与规格整理稿完成，与 `floors/navigation.md` + `floors/navigation-dropdown.md` 搭配生成。覆盖：
  - **多列平铺结构**：分类列头（14px/22px Regular / `info3`，PC 端无交互）+ 列内条目（标题 16px/24px `info1` + 说明 12px/18px `info2`）+ 外链图标（16×16 currentColor）+ 红色小标签（OTag fill + danger + small）+ 列底部「更多项目 >」+ 快捷链接栏（`fill3` 灰底按钮块 / 带图活动卡片）；典型列数 开发 Tab 5 列、社区 Tab 4 列 + 右侧快捷链接栏
  - **完整状态矩阵**：条目标题 默认 Regular `info1` → hover Semibold `primary2` → 按下 Semibold `primary3` → 选中 Semibold `primary1` → 禁用 Regular `info4`；说明文字仅禁用态变 `info4`；**全程无背景色块**（与平铺式的 `control2-light` / `control3-light` 色块是两种交互表达，从源头规避 control Token 取值歧义）
  - **六档自适应**（≥1681 / 1680~1441 / 1200~841 / 840~601 / 600~360）+ **MB 双列**（左一级导航列 + 右内容区，`grid: minmax(32px,144px) 1fr`，右列 `grid-row:1` 固定顶格 + `max-width:40%` 黄金分割，折叠态 32px）
  - **截断规则**：标题 `line-clamp: 2`、说明 `line-clamp: 6`（四件套写法）、tooltip 复用 OPopover；导航项过多时左列末项挂切换箭头
  - **HTML/CSS 骨架 + MB 交互 JS**、24 条踩坑记录、57 项楼层自检清单、11 项待确认项 + 12 项已确认清单
- **架构决策：两种下拉面板样式共享 `.o-nav__dropdown` 基类**。平铺式模板继续定义面板定位（`top: calc(100% + 4px)`、居中触发器）、显隐三规则（hover / focus-within / data-open）、4px 热区桥接、单开互斥 `:has()`、MB 左对齐与交互 JS；分类平铺式只追加修饰类 `.o-nav__dropdown--categorized`（复合选择器覆盖布局）。因此**同一顶导内两种样式可混挂**，单开互斥规则同时覆盖两者，无需另写。
- **触发器状态显式冲突隔离**：分类平铺式的顶导菜单项按用户规格为 hover/展开态 = Semibold + `primary2`（无下划线）、选中态 = Semibold + `primary1` + 2px 指示线，与 `components/navigation.md` Part C（Regular + primary1、无字重变化）不一致。已按「用户显式输入优先」写入模板，并用 `:has(> .o-nav__dropdown--categorized)` 限定**只作用于挂分类平铺面板的菜单项**，禁止污染平铺式与普通顶导；Part C 同步更新列入待确认项 #7。

#### 更新
- `floors/navigation-dropdown.md`：§〇 改为「两种样式 / 两份模板」分工表 + 协作方式表（基类共享、修饰类、混挂互斥、漏基类后果）；头部增加样式导航；待确认项 #6 标记为已完成并指向新模板。
- `floors/navigation.md`：头部互指说明改为两种样式二选一（含混挂说明）；「生成前必读」第 2 步改为按样式二选一；class 对应表补 `.o-nav__dropdown`（基类）与 `.o-nav__dropdown--categorized`（修饰类）两行。
- `SKILL.md` 楼层模板索引：`导航下拉面板` 一行拆为「平铺式 / 分类平铺式」两行（楼层模板数 6 → 7）。
- `references/designer-guide.md` 楼层模板速查、`README.md` 目录结构与按需加载策略：同步登记两份下拉面板模板，模板数 6 → 7。

### 平铺式导航下拉面板首版与当天多轮验收修正

#### 新增
- **新增楼层模板 `floors/navigation-dropdown.md`（导航下拉面板 · 平铺式）**：将顶导菜单项展开的下拉面板从组件规范中拆出，封装为独立自包含生成单元，与 `components/navigation.md` + `floors/navigation.md` 搭配使用。覆盖：面板定位与触发规则（hover + click 双途径、单开互斥、4px 间距热区桥接）、平铺式布局（flex-wrap 自动换行、`height: auto`）、双层状态矩阵（顶导菜单项触发器 默认/常态/点击态/选中态 + 面板菜单项 默认/hover/已选）、设计稿实测自适应断点（1518 / 1440 / 1280 / MB）、PC 与 MB 双端规格、HTML/CSS 实现骨架与最小交互 JS、10 条踩坑记录、楼层自检清单。
  - **搭配关系**：`floors/navigation.md` 只生成顶导骨架，下拉面板必须读本模板；已在 navigation 楼层头部增加互指说明，并在 SKILL.md / designer-guide.md / README.md / 根 AGENTS.md / 根 README.md 的楼层模板索引中登记（楼层模板数 5 → 6）。
  - **Token 冲突隔离**：`--o-color-control2-light` / `--o-color-control3-light` 在本仓多文档中各有多种取值，模板内以**取值消歧表**固定本楼层含义（hover 背景 = control2-light #F0F5FF；已选背景 = control3-light #E6EDFF），并显式排除 `#002FA7`（OSearch Hover 描边义）等误用。
  - **待确认项显式列出**：设计稿「默认」与「常态」标注值相同、点击态蓝底圆角未标注、面板内边距与菜单项 gap 是否实测值、MB 端内边距与 gap 为推断值、MB 断点设计稿 655px 与 hard-constraints §1（360~600px）冲突、面板高度实测值与公式推导存在偏差（高度一律 auto，实测值仅作 QA 参考）、**分类平铺式样式待第二轮输入后补充**。以上各点按 SKILL.md「规范未覆盖时的分级处理」禁止静默取值。（其中间距 4px、面板居中触发器、菜单项字号 16px、hover/已选/禁用态配色已于同日经用户口述确认，见下方「修正」节。）

#### 更新
- `floors/navigation.md` 头部新增与 `floors/navigation-dropdown.md` 的搭配说明：顶导骨架由 navigation 楼层生成，菜单项展开的下拉面板（平铺式 / 分类平铺式）必须搭配 navigation-dropdown 模板，触发器状态与面板规格以该模板为准。

#### 修正
- **`floors/navigation-dropdown.md` 面板规格按用户口述校正**（首版按 ODropdown 通用规格推导，与 mega 面板实际规格不符）：
  - 菜单项字号 14px/22px → **16px/24px**（常规正文），MB 对应 12px/18px → **14px/22px**
  - 菜单项 hover 背景 `#EBF1FA` → **`var(--o-color-control2-light)`（#F0F5FF 极浅蓝）**；已选背景 → **`var(--o-color-control3-light)`（#E6EDFF 浅蓝）**，与 hover 明确分级
  - 删除私有变量 `--o-nav-dropdown-item-hover-bg`，改为直接引用语义 Token，并在「五、颜色」新增 **control2/control3 取值消歧表**（标注 `#002FA7` 是 OSearch Hover 描边义、`#E6EDFF` 也是用户名区 Press 义，均非本楼层用法）
  - 面板水平定位 左对齐触发器 → **水平居中于触发器**（`left: 50%` + `translate(-50%, …)`），MB 端回落左对齐；同步补踩坑 #12（transform 覆盖丢 `translateX(-50%)`）与 #13（居中须同时校验左右溢出）
  - 补 **禁用态**规格（文字 `var(--o-color-info4)`）与踩坑 #14
  - 踩坑 #5/#6 重写：原写「hover 背景不是 #F0F5FF」结论错误，实际正是 control2-light；原写 hover 文字变 primary1 亦错误，实际保持 info2

#### 修正（续 · 2026-10-14 用户复核后发现）
- **`floors/navigation-dropdown.md` 触发器状态严重偏离 components/navigation.md Part C（用户指出「导航选中部分怎么有 primary 色块」）**：
  - **删除首版自创的「点击态 = SemiBold + primary1 实心蓝底 + 白字」**——navigation.md 全文中不存在该状态，系误读设计稿所致。正确规格：**展开态 = SemiBold + primary1 文字 + `--o-color-control3-light`（#EBF1FA）浅蓝背景**。
  - **修正选中态**：原写「SemiBold + primary1 蓝字（无背景）」，遗漏了 navigation.md Part C 与 floors/navigation.md 均明确规定的**底部 2px 品牌蓝指示线**（`border-bottom-color: var(--o-color-primary1)`）。现已改为「指示线 + 文字加深」，并显式区分「选中态（指示线）」与「展开态（浅蓝背景）」两套机制。
  - §八 CSS 同步重写：`.o-nav__link` 基础几何声明移除（归 floors/navigation.md 管），只保留 hover / 展开态 / 选中态三个面板相关态。
- **`floors/navigation-dropdown.md` 面板间距与自适应按用户口述重定**：
  - 面板内边距 24px 32px → **四边均 4px**
  - 菜单项 gap 24px → **12px**
  - 自适应从「1518/1440/1280/MB 四档」改为**三档**：≥1441px（PC 16px）/ **841~1440px（保持 PC 样式，仅字号降为 12px）** / ≤600px（MB 14px）
  - 面板高度反推值由「1 行 ≈84px / 2 行 ≈144px」修正为「1 行 ≈48px、每多 1 行 +52px」（按 padding 4px + 项高 40px 推导），实测值标注为仅 QA 参考
  - 601~840px 区间 hard-constraints 无定义，新增为待确认项
- 踩坑记录扩至 16 条，新增 #8（展开态做成 primary1 实心蓝底）、#9（选中态漏指示线）、#16（841~1440px 误改 MB 布局）；自检清单同步扩充。

#### 修正（再续 · 2026-10-14 用户指出布局方向错误）
- **`floors/navigation-dropdown.md` 面板布局从「横向 flex-wrap 平铺」改为「垂直列表」**（用户指出 hover 色块应撑满面板、选项为固定高度透明容器）：
  - 面板 `display: flex; flex-wrap: wrap; align-items: center; gap: 12px` → **`flex-direction: column; align-items: stretch; gap: 0`**
  - 新增「平铺式 = 垂直列表、选项不分层无分类标题」的定义澄清（与第二轮「分类平铺式」相对，非横向排列）
  - **选项容器规格重定**：`width: 100%`（色块撑满整行，不受文字长短影响，左右各让出面板 padding 4px）+ 固定高度 **40px（>1440px）/ 32px（841~1440px）** + `flex: none` + `display:flex; align-items:center`（文字垂直居中）+ `background: transparent`（默认透明，仅交互态填充）
  - 选项间距 gap 12px → **0px**（相邻色块上下相接）
  - 选项内边距 8px 12px → `0 12px`（垂直方向由固定高度 + 居中控制）
  - 自适应表改为按容器高度分档：≥1441px = 40px/16px；841~1440px = **32px/12px**（该档从「只改字号」修正为「改容器高度 + 字号」）；≤600px = 40px/14px（推断）
  - 面板高度公式修正为 `8px + 容器高 × N`
- 踩坑记录扩至 **21 条**，新增 #16（误解平铺式为横向）、#17（色块未撑满整行）、#18（容器默认带底色）、#19（容器高度靠 padding 撑）、#20（选项间出现空隙）、#21（841~1440px 漏改容器高度）；自检清单同步扩充。
- 新增待确认项 #8（MB 端容器高度 40px 为推断值）、#3（选项文本左右内边距 12px 为推断值）。

#### 修正（续 · 2026-10-14 顶部导航按 navigation.md 规范复核）
- **`floors/navigation.md` 源码下拉文字色修正 info2 → info1**：原颜色表与「源码下拉实现」CSS 均写 `var(--o-color-info2)`，与 `components/dropdown.md` Part B「text 变体：文字/图标均为 `--o-color-info1`」及 `components/navigation.md`「源码下拉颜色 --o-color-info1」矛盾（两份组件规范一致，楼层模板为孤例）。已改为 info1 并注明来源。
- **`.nav-dropdown-check.html` 顶导骨架 4 处偏差修正**（复核 navigation.md 后发现）：
  - `.o-nav` box-shadow 硬编码 `0 3px 9px 0 rgba(0,18,85,0.078)` → `:root` 定义 `--o-shadow-1`（值取 components/navigation.md 顶导 boxShadow 规格，加来源注释）后改为 `box-shadow: var(--o-shadow-1)`
  - `.o-nav__source` 文字色 info2 → **info1**（ODropdown text 变体，同上）
  - `.o-nav__source` gap 4px → **8px**，补 `min-width: 48px`（floors/navigation.md 源码下拉实现规格）
  - **图标 fill 字面量 → 语义 Token**（SKILL.md 图标处理规范「颜色用 currentColor 或 var(--o-color-*)」）：logo 黑 `rgb(0,0,0)` → `var(--o-color-info1)`（9 处）、logo 蓝 `rgb(0,47,167)` → `var(--o-color-primary1)`（1 处）、各图标黑 → `var(--o-color-info1)`（7 处，`fill-opacity="0.800000012"` 等属性按规范原样保留，浅色模式渲染不变、深色模式可自适应）；遮罩内结构色 `rgb(196,196,196)` 保留不动
- 复核确认无偏差项：三层对齐（外层 center / 左侧 flex-end / Logo align-self:center / 菜单 flex-end / 右侧 center）、72px 高 + padding 0 216px、左侧 gap 40px、菜单 gap 32px、右侧 gap 20px、菜单项 48px + padding-bottom 24px + border-bottom 2px transparent、16px/24px/400、搜索框 160×32（border + 背景 + 内部绝对定位 + 占位 info4 + 图标 16×16）、源码下拉箭头 16×16（components/navigation.md 明确规格）、操作图标 24×24、7 个菜单项、右侧 5 元素顺序、图标 path 数据与 assets 逐字符一致。

#### 修正（续 · 2026-10-14 用户指出选中态背景色块 + 面板互斥失效）
- **`floors/navigation-dropdown.md` 触发器「展开态」自创背景色块删除**（用户指出「选中怎么可能会有个背景色块垫在后面」）：
  - 核对 `components/navigation.md` Part C「顶导交互状态」：仅定义默认 / Hover / Active 三态，**全部背景透明**，无任何背景态。第二版给展开态自创的 `control3-light`（#EBF1FA）浅蓝背景块、SemiBold 字重全部删除；首版自创的 primary1 实心蓝底白字早已删除，本轮将三次误写全部显式列为禁令
  - 展开态规格改为：**文字 `primary1` + 背景透明**（视觉同 Hover，规范未单独定义展开态）；选中态维持底部 2px primary1 指示线 + 文字加深
  - §三 速查表「触发器展开态背景」行删除；§4.1 状态表背景列全部标注透明并加 ⛔ 禁令块；§五 颜色表删除「触发器点击态背景/文字」（primary1 底 + 白字，亦为自创），新增「触发器无任何背景 Token」禁令
- **单开互斥从「仅 JS」升级为「JS + CSS `:has()` 双保险」**（用户指出 hover 与选中触发会面板重叠）：
  - 根因：hover（`:hover`）与 click 锁定（`data-open`）是两套独立 CSS 机制，点开 A 后再 hover B，两条显示规则同时命中，A、B 面板重叠
  - 新增互斥规则：`.o-nav__menu:has(> .o-nav__item:hover > .o-nav__dropdown) > .o-nav__item:not(:hover) > .o-nav__dropdown`（focus-within 同理）——菜单内任意**带面板**的项被 hover/focus 时，其余项（含锁定项）面板隐藏；选择器限定 `:hover > .o-nav__dropdown`，hover 无面板项（如当前页）不会误关锁定面板；MB 端同步覆盖 transform
  - JS 补 `link.blur()`：关闭时若不移出焦点，`:focus-within` 会立即使面板重新显示，导致点击关不掉
  - 踩坑 #8 重写（背景色块）、#11 重写（重叠根因与双保险解法）；自检清单同步更新；待确认项重编号（原 #2「展开态圆角」随背景删除失效）
- `.nav-dropdown-check.html` 同步修正：删除展开态背景与 SemiBold、补 PC/MB 互斥 CSS、JS 补 blur；新增「单开互斥验证」「触发器无背景验证」两张验证卡片，图例补充触发器状态说明。

#### 更新（续 · 2026-10-14 交互模型显式固化）
- **`floors/navigation-dropdown.md` 新增「交互模型（hover 临时态 vs click 锁定态）」章节**（用户确认 hover 与 click 的语义差异后补入规范，此前仅在「展开方式」一行带过，未讲清两种触发语义的区别）：
  - 明确表格：hover = **不固定**，鼠标移走即消失，用于鼠标用户快速浏览；click = **固定**，不随鼠标移走消失，需再次点击同一菜单项才关闭，用于触屏与固定面板操作
  - 四条行为要点：① hover 不改变锁定状态，鼠标移出整个菜单后被互斥临时隐藏的锁定面板**恢复显示**；② click 是 toggle（点已锁定项关闭、点未锁定项先关其他再锁定当前项）；③ 锁定项再次被 hover 时面板保持（锁定优先）；④ `:focus-within` 仅作键盘兜底，关闭须 `link.blur()`
  - §四 状态矩阵 Hover 行「面板」列标注「显示（**临时**，鼠标移走即隐）」；§八 CSS 三条显示规则注释改为「hover=临时 / focus-within=键盘兜底 / data-open=锁定（固定）」；JS 注释补 click = 锁定展开（toggle）语义
  - 踩坑记录扩至 **23 条**：#22（click 后面板关不掉 = 缺 blur）、#23（混淆 hover/click 两种触发语义）
  - 自检清单新增 4 条（hover 临时性、click 锁定性、锁定面板恢复、blur 兜底）；已确认清单新增 #15
- `.nav-dropdown-check.html` 说明文案改为显式描述交互模型；页面标题标注「· 交互模型」。

#### 修正（续 · 2026-10-14 用户指出选中态不可切换 + 指示线被圆角截断）
- **`.nav-dropdown-check.html` 选中态指示线圆角截断修复**（用户指出「选中底部的指示线怎么有个被截断的圆角？不应该只是一根线吗？」）：
  - 根因：`.o-nav__link` 上误加了 `border-radius: 4px`（把搜索框 `.search-box` 的圆角搬到了菜单项上），`border-bottom` 指示线两端被圆角裁出弧形缺口
  - 该圆角**无任何视觉效果**——顶导菜单项所有状态背景一律透明（navigation.md Part C），圆角本来的用途是给背景色块收边，而触发器根本没有背景色块
  - 已删除 `.o-nav__link` 的 `border-radius` 及 `transition` 中多余的 `background-color`，指示线恢复为完整一根水平直线（宽 = 菜单项宽 32px − 左右 padding 4px = 24px）
  - 规范核对确认：`floors/navigation.md` 中 `border-radius: 4px` 仅属于 `.search-box`，菜单项从未定义圆角——属生成时自创
- **`.nav-dropdown-check.html` 选中态改为可切换**（用户指出「当前选中是文档，点击下载时选中态应该切换为下载」）：
  - 根因：`is-current` 只写死在 HTML 上（`<li class="o-nav__item is-current">文档`），JS 完全未处理，点击其他菜单项选中态不动
  - JS 补点击迁移：先移除所有 `.is-current`，再给当前点击项添加；**单选、不做 toggle**——再次点击已选中项不取消选中，否则顶导会丢失「当前位置」信息
  - 选中态与展开态两套机制独立：点击「下载」后它同时持有 `is-current`（指示线）与 `data-open`（面板），互不冲突
- **`floors/navigation.md` 补两条踩坑记录**（#9 菜单项 border-radius 导致指示线圆弧缺口、#10 is-current 写死不可切换）+ 自检清单 2 条；精确数值速查新增「菜单项 border-radius = 禁止」并附原因说明块
- **`floors/navigation-dropdown.md` 同步**：踩坑记录扩至 **25 条**（#24 圆角截断、#25 选中态不可切换）、自检清单 +2 条、JS 最小实现补 `is-current` 迁移代码
- `.nav-dropdown-check.html` 新增「选中态切换验证」「指示线形态验证」两张验证卡片。

#### 更新（续 · 2026-10-14 用户要求汇总问题并确保下次生成一步到位）
本轮排查出 **4 个会导致下次生成再次出错的规范缺口**，已全部修复：
- **缺口 1（严重）：顶导菜单项基础样式「责任悬空」**
  - 症状：`navigation-dropdown.md` 写「`.o-nav__link` 的 height/padding-bottom/border-bottom 基础样式由 floors/navigation.md 提供，此处严禁重定义」，但 **navigation.md 里根本没有这段 CSS**——只有 `.nav-menu { align-self: flex-end; }` 一行
  - 后果：生成方被要求「严禁重定义」却无据可依，只能自行编写，必然自创 `border-radius: 4px`、SemiBold 字重、背景色块等错误属性（本轮 17 个问题中有 4 个直接源于此）
  - 修复：navigation.md 新增 **「菜单项（触发器）完整样式」** 独立章节，给出完整 CSS（`.nav-menu > li` + `.nav-menu a` + hover + `.is-current` 选中态），并在 CSS 注释内嵌「⛔ 绝对禁止 border-radius」「⛔ 禁止改 SemiBold」两处禁令；附「选中态必须可切换」说明（单选、不做 toggle）
- **缺口 2：两份楼层模板 class 命名不统一且无映射表**
  - 症状：navigation.md 用 `.header` / `.nav-menu` / `.logo`，navigation-dropdown.md 用 `.o-nav__menu` / `.o-nav__item` / `.o-nav__link` / `.o-nav__dropdown`，无任何说明
  - 后果：生成方需自行猜测两份模板的对应关系，易混用或漏挂面板
  - 修复：**两份文件互相插入同一张 class 对应表**，并注明「禁止混用」「菜单项基础样式归 navigation.md」
- **缺口 3：dropdown 模板 HTML 骨架只有默认态**
  - 症状：骨架 9 个选项全无状态类名，而 CSS 骨架已定义 `.is-selected` / `.is-disabled` 三态
  - 修复：HTML 骨架补 `.is-selected`（已选）/ `.is-disabled` + `aria-disabled`（禁用）/ 触发器 `.is-current` 示例，并加状态类名汇总说明
- **缺口 4：无「生成前必读」清单**
  - 修复：navigation.md 开头新增「生成前必读（按顺序读取，缺一不可）」表，列明 3 份文件各自必须取用的内容，并点明「只读一份会漏什么」
- **designer-guide.md**：导航下拉面板行补充「必须与导航楼层搭配生成，两份模板 class 命名不同需按对应表映射」。

#### 修正（续 · 2026-10-14 用户指出搜索图标跑到右侧）
- **`.nav-dropdown-check.html` 搜索图标位置修复**（用户指出「搜索框里面的图标怎么到了右边？？这个错误很严重！！」）：
  - 根因：`.o-nav__search svg` 设了 `right: 12px`，把搜索图标推到占位文字右侧。**OSearch small 规范明确「搜索图标 位置：左侧；占位文字 位置：搜索图标右侧」**（components/search.md），components/navigation.md 的参考实现 HTML 也是图标在前、文字在后
  - 修复：图标改 `left: 16px`，占位文字改 `left: 40px`（= 左内边距 16px + 图标 16px + gap 8px）；HTML DOM 顺序也调整为 `<svg>` 在前、`<span>` 在后
  - 顺带修复图标填充色：搜索图标应为 `var(--o-color-info4)`（`rgba(0,0,0,.4)`），原误用 `info1` + `fill-opacity="0.8"`（资源 SVG 切图残留值）；国际化/主题切换/用户三个图标应为 `info3`（`rgba(0,0,0,.65)`），原同样误用 `info1` + `fill-opacity="0.8"`
- **`floors/navigation.md` 修正搜索框规范**：
  - `.search-box svg` 由 `right: 12px` 改为 `left: 16px`，并内嵌「⛔ 严禁 right」注释；`.search-box span` 由 `left: 16px` 改为 `left: 40px` 并注明计算式
  - 新增「搜索框内部顺序固定为图标→文字」+「HTML 中图标须写在文字前」说明
  - 颜色表新增**搜索图标 = info4**、**国际化/主题/用户 = info3** 两行，并新增四图标 Light/Dark 色值对照表，明确「资源 SVG 的 `fill-opacity="0.8"` 是切图残留，内联时须换成语义 Token 并去掉 fill-opacity，否则深浅色模式无法自适应」
  - 踩坑记录扩至 **12 条**（#11 搜索图标跑到右侧、#12 四图标填充色统一用 info1）
  - 自检清单新增 4 条（搜索图标位置、搜索图标 info4、其余三图标 info3）

#### 修正（续 · 2026-10-14 用户指出右侧三个图标全部消失）
- **`.nav-dropdown-check.html` 右侧国际化/主题切换/用户三图标全部消失修复**（用户指出「右边的 icon 全部都不见了？？捡了芝麻丢了西瓜！」）：
  - **根因**：上一轮把这三个图标的 fill 从 `info1` 改成 `info3`（符合 Part H 规范，改动本身是对的），但 demo 的 `:root` 变量表里**只有 info1/info2/info4，从来没有定义 `--o-color-info3`** → `fill="var(--o-color-info3)"` 整条声明无效 → `<path>` 回退继承外层 `<svg fill="none">` → **图标完全不可见**（HTML 结构完好，只是渲染不出来）
  - 附带根因：`--o-color-info2` / `--o-color-info4` 原用 `rgba(0,0,0,0.8)` / `rgba(0,0,0,0.4)`，与 components/navigation.md Appendix A 的 Token 定义（#333333 / #999999）不一致
  - 修复：`:root` 补齐 `--o-color-info3: #666666`，并同时把 info2/info4 校准为 Token 速查表权威值（#333333 / #999999）；`info1/fill1/fill2/control1/control2-light/control3-light/primary1` 保持不变
  - 已交叉校验：全文件引用的 10 个 Token（fill1、fill2、info1~4、control1、control2-light、control3-light、primary1）全部在 `:root` 有定义
- **`floors/navigation.md` 新增「顶导所需语义 Token 完整清单」**（根本解法，防止再犯）：
  - 给出**可直接抄入 `:root` 的完整 CSS**（13 个 Token，逐个标注用途），生成方无需自行拼装变量表
  - 新增铁律「**⛔ Token 必须先定义再使用**」：引用未定义 `var()` 时该 fill/color/background 声明整体无效，SVG path 会继承 `<svg fill="none">` 导致元素完全不可见；并强调「**每次修改色值后必须回头核对 `:root`**」
  - 踩坑记录扩至 **13 条**（#13 右侧图标全部消失——引用未定义 Token）
  - 自检清单新增 1 条（是否引用了未定义的语义 Token）
- **复盘结论**：本轮故障 skill 欠缺占主因（无完整 Token 清单、无"先定义再使用"铁律），粗心占次因（改 fill 色值时未回头检查 `:root`）。Token 清单已入规范，下次生成直接抄录即可规避。

#### 修正（续 · 2026-10-14 用户指出图标色应为 info2 而非 info3）
- **`.nav-dropdown-check.html` 三个右侧图标 `info3` → `info2`**（用户指出「不是 info3，是 info2」）：
  - **根因在 skill 文档，不在 demo**：`components/navigation.md` Part H 的「顶导 Icon SVG」表把国际化/主题切换/用户三个图标标为 info3（`rgba(0,0,0,.65)` / `#666666`），我照抄了该表
  - **实际资源文件核对**（`references/assets/navigation/` 三个 SVG）：填充均为 `fill="rgb(0,0,0)"` + `fill-opacity="0.800000012"` = **80% 黑 = `#333333` = info2**，与 info3（60% `#666666`）不符——**文档表格有误，用户纠正正确**
  - 修复：demo 三图标 fill 改 `var(--o-color-info2)`（`info2` 上一轮已补入 `:root`，无再次消失风险）
- **`components/navigation.md` Part H 三处错误全部修正**（这是本轮错误的源头，必须改）：
  1. 「顶导 Icon SVG」表：三个图标 Light 色 info3 → **info2**（`rgb(0,0,0)` @80%）；尺寸 20×20 → **24×24**（与自身结构图、速查表、floors/navigation.md 及资源 viewBox 一致）
  2. 表格下方新增修正说明：「**图标颜色与尺寸以 `references/assets/navigation/` 资源文件为准，本表为辅**」
  3. Part H「顶导 Icon CSS mask」参考实现：`.nav-icon` 默认 `background-color` 由 `rgba(0,0,0,.65)` → **`rgba(0,0,0,.8)`**（80% 黑 = info2）；`.nav-icon-lang` / `.nav-icon-theme` / `.nav-icon-user` 尺寸由 20×20 → **24×24**；`.nav-icon-search` 补充注释「info4，比其余三图标更浅」
  - Dark 侧 `rgba(255,255,255,.85)`（info1-dark）经核对与表格一致，未改动
- **`floors/navigation.md` 同步**：颜色表与四图标对照表 info3 → info2、尺寸 20×20 → 24×24；新增铁律「**勿照抄 components/navigation.md Part H 的 info3，图标颜色以 assets 资源文件实际填充值为准**」；` :root` 清单注释调整（info2 = 次要文字 + 三图标 fill；info3 = 页脚次要文字，顶导不使用）；踩坑记录扩至 **14 条**（#14 三图标填成 info3——照抄文档表格未核对资源文件）；自检清单同步
- **复盘结论**：本轮 100% 是 skill 欠缺——Part H 表格数据本身错误，我照抄即错。已把「**资源文件 > 文档表格**」的优先级写进规范，后续遇冲突以资源文件为准。

---

## 2026-10-10

### ⚠️ 破坏性
- **楼层模板图标引用方式统一为内联 SVG**：`floors/navigation.md`、`floors/footer.md` 原要求 Logo / 图标用 `<img>` 或 CSS mask 引用 assets 资源，现对齐 SKILL.md「图标处理规范」——一律 Read 资源后逐属性原样内联 `<svg>`，禁止 `<img src>` / CSS mask / 凭记忆手绘。
  - **影响评估**：仅影响按旧版模板生成的 HTML（以 navigation / footer 楼层为主）——按新版自检清单复核时 `<img>` 引用判为违规形式。**渲染效果不变，无功能性破坏**：同一 SVG 内联前后视觉一致；且内联后图标可用 `currentColor` / 语义 Token 随深浅模式自动适配，原型移出本仓库目录后不再因相对路径（含中文/空格文件名）失效。旧规则本身与 SKILL.md 既有「禁止 `<img src>`」冲突，本次为消解矛盾而非新增禁令。
  - **复核动作（机械性）**：对已生成 HTML 执行 `grep -n "<img"`，命中即取对应 assets SVG 逐属性内联替换；布局与其他代码无需改动。

### 新增
- **SKILL.md 新增「规则冲突裁决」条款**：楼层模板（floors/）、组件规范（components/）、示例与 hard-constraints.md 或 SKILL.md「图标处理规范」「通用生成约束」冲突时，一律以后者为准；发现冲突停下向用户报告，禁止各取所需。

### 更新
- **SKILL.md 渲染验证按环境能力分级**：有浏览器能力（Playwright / Puppeteer MCP、IDE 预览）→ 截图目检；无（headless / CLI）→ 静态自检并在交付说明中显式标注「未做渲染目检，需人工在浏览器复核」，禁止静默跳过。覆盖第六步与第八步。
- **SKILL.md 位图素材文生图解除厂商绑定**：移除硬编码的 Trae 文生图端点，改为「环境具备文生图能力则用之（端点以所用 AI 工具为准），无能力则占位色块 + 注释」，保留生成后校验与禁止编造图片 URL 的要求。
- **SKILL.md Token 降级数据源指向本地六主题文件**：WebFetch 拉取上游 Token 失败时，降级用 `../opendesign-tokens/references/tokens-{社区}.md`（六主题全量 + Dark 值反查，离线可用）；designer-guide.md 速查表降级为 openEuler Light 辅助；目标社区无本地 Token 文件时停下询问。
- **floors/navigation.md、floors/footer.md、floors/banner.md 颜色全量语义 Token 化**：rgba / hex 硬编码改为 `var(--o-color-*)` 语义 Token（fill2 / info1~4 / control1 / shadow-1 / color-white），深浅模式统一由 `data-o-theme` 切换自动适配，删除 Light/Dark 双列色值。上游 Token 体系无对应变量的组件特例值（页脚固定深色、MB 顶导半透明背景、footer 分隔线等）显式标注「组件特例值」并要求加 HTML 注释，禁止扩散。
- **navigation.md 踩坑 #7、#8 与自检清单对齐全局规范**：图标改为「Read 后逐属性内联」；「内联 style + !important」改为「提高选择器优先级，宿主覆盖时允许带注释的 !important（硬约束布局强制豁免场景），禁止内联 style」。
- **README.md、根 AGENTS.md、根 README.md 中 `opendesign-design` 的描述**由「Pixso 设计稿生产 / componentKey 速查」更新为「HTML 设计原型生产（26 组件 + 5 楼层模板）」，global/ 两个速查副本在目录树中标注真源归属。

### 修正
- **floors/footer.md MB 端 CSS 背景值笔误**：`.footer-section-mb` 背景 rgba(18,18,20,1) → rgb(18,18,18)，与 MB 布局结构树及常见错误清单 #8（MB ≠ PC）对齐。
- **global/interaction-rules.md 图标规则对齐 SKILL.md**：移除过时的 icon-keys.md 引用，补充手写 SVG 兜底注释要求与「逐属性原样搬运」强制条款；与 layout-rules.md 一并在文件头部增加「真源声明」（内容已并入 hard-constraints.md，不一致时以 hard-constraints.md 为准，本文档仅作速查副本）。

---

## 2026-10-08

### 修正
- `components/dialog.md` 组合搭配中 `OTable 操作` 改为 `ODataTable 操作`（OTable 已弃用，表格统一用 ODataTable）。

---

## 2026-09-24

### 新增
- **SKILL.md 图标处理规范新增 SVG 搬运硬约束**：在「图标使用规则」表格中新增「搬运」属性行，强制逐属性原样搬运（保留 `fill-rule` / `clip-rule` / `opacity` / `fill-opacity` / `transform`），禁止仅拷贝 `path` 的 `d` 数据重写标签。
- **SKILL.md 图标处理规范新增风格判定规则**：新增「风格判定（强制）」条款，明确以浏览器渲染结果为准，不得仅凭源码 `fill`/`stroke` 属性下结论；`fill + fill-rule="evenodd"` 的环形构造渲染上是线性细线。
- **SKILL.md 通用生成约束新增 SVG 搬运条款**：在「通用生成约束（所有楼层共用）」中新增 SVG 搬运强制条款，覆盖所有楼层生成场景。
- **SKILL.md 第四步读取资源新增搬运提醒**：在「第四步：读取本楼层组件规范与资源」的图标资源检查步骤中，新增逐属性原样内联提醒。
- **SKILL.md 第六步验证清单新增 SVG 搬运检查项**：新增「图标 SVG 是否逐属性原样搬运」校验项，确保搬运问题在楼层验证阶段即被拦截。
- **SKILL.md 第八步最终验证新增图标风格检查项**：新增「关键图标是否与设计系统一致」校验项，以浏览器渲染为准。

---

## 2026-08-18

### 新增
- **hard-constraints.md 新增 §0 颜色 Token 常见错误**：针对 AI 生成 HTML 时最常出错的 Token（如 `--o-color-fill1` 误写为纯白 `#FFFFFF`），新增强制规则——生成 HTML 前必须先 WebFetch 拉取上游 `openeuler-token.json`，禁止凭训练数据猜测颜色值。
- **hard-constraints.md 新增 §4.1.1 HTML/CSS 实现翻译**：为 PC/MB 双端定义楼层内容容器的精确 CSS 规则（`width: 1488px` + `margin-left/right: 216px` + `padding: 0`），并附 7 条常见错误对照表（如误用 1200px、`margin: 0 auto` + `padding: 0 216px` 组合等）。
- **SKILL.md 起手式新增 WebFetch 拉取 Token 强制步骤**：第零步必须执行 `WebFetch` 拉取 `openeuler-token.json`，将颜色/字号/间距变量翻译为 `:root` CSS 变量，拉取失败时降级到 `designer-guide.md` 中的 Token 语义速查表。
- **SKILL.md 验证步骤新增 HTML 栅格/颜色检查项**：第六步验证清单新增 6 条 HTML 专属校验（楼层容器 width/max-width、margin-left/right、padding: 0、验证公式 216+1488+216=1920、颜色值来源、body 背景色）。
- **各 floors/ 模板新增楼层容器 CSS 实现规则**：navigation.md、banner.md、card-grid.md、feature-section.md、footer.md 均新增 `⚠️ 楼层容器 CSS 实现（必须严格遵守）` 章节，明确禁止 `margin: 0 auto` + `padding` 组合，强制使用固定 margin-left/margin-right 方案。
- **hard-constraints.md 自检清单新增 HTML 栅格/颜色检查项**：§6 AI 自检清单从 10 项扩充至 16 项，新增 §10~§16 覆盖 HTML 栅格容器验证、框架默认值排查、颜色 Token 来源校验。

### 更新
- 优化各组件的交互状态描述（Hover / Pressed / Focus / Disabled 等状态视觉属性细化）。
- `global/interaction-rules.md` 页面生成自动校验新增 HTML 栅格相关检查项（§9、§10）。
- `global/component-doc-spec.md` 颜色 Token 对照表补充 `--o-color-` 前缀格式示例。

---

## 2026-07-21

### 新增
- 新增多个交互示例 demo HTML 文件：card-demo、card-list、checkbox、dropdown-demo、input、interactive-card-demo、navigation-demo、radio、search-demo、select-demo、skills-list-page、skills-list-table、switch-debug、toggle，用于展示各组件的实际使用效果。
- 新增 `global/qa-checklist/component-html-qa-checklist.md`——组件 HTML 实现质量保证清单，覆盖设计参数提取、实现完整性、交互状态、无障碍、浏览器兼容等检查项。

---

## 2026-06-30

### ⚠️ 破坏性
- 颜色变量命名统一加 `--o-color-` 前缀（如 `color-primary1` → `--o-color-primary1`、`color-white` → `--o-color-white`），影响 26 个组件设计规范文档。按旧变量名在 Pixso 中建立的变量需同步更新。

### 更新
- 调整 ONavigation 登录与未登录的交互规则，以及 anchor / breadcrumb / button / link / menu / step / tab 等组件的交互规则。

---

## 2026-06-29

### 更新
- 按"融合而非补丁"原则清理对不存在组件的误引用，重写相关章节。

---

## 2026-06-05

### 新增
- 新增 `components/banner.md`——OBanner 横幅组件设计规范（XL / L / M / 无 四个等级）。
- 新增落地页模板规则文档（`floors/` 目录）：navigation.md、banner.md、card-grid.md、feature-section.md、footer.md，每个模板为自包含生成单元。
- 新增组件组合规则硬约束（禁止组合黑名单、Banner 嵌套限制等），合并入 `global/hard-constraints.md`。

### 更新
- 修改 OMessage 消息提示组件的 icon SVG 引用路径，对齐 `references/assets/message/` 目录。
- 调整 `SKILL.md` 工作流，将 `global/hard-constraints.md` 列为必读文档（起手式强制读取）。

---

## 2026-06-03

### 更新
- 修改 ODataTable 数据表格组件规范，嵌入 assets 资源引用。
- 修改 OLoading 加载组件规范，嵌入 assets 资源引用。

### 修正
- 修正 OCarousel 幻灯片组件中文名命名（原"走马灯"→"幻灯片"）。
- 修正 ODropdown 下拉菜单组件中文名命名不一致问题。

---

## 2026-06-02

### 新增
- 新增 `references/assets/public icons/` 目录，包含 22 个公共线性图标 SVG（上/下/左/右箭头、关闭、删除、刷新、搜索、排序、提示、日历、时间、添加、筛选、链接文件、占位、图片、外链、loading 加载、dark、light）。
- 新增 ODataTable 数据表格组件设计规范 `components/data-table.md`。
- 新增 OCarousel 幻灯片指示器组件设计规范 `components/carousel.md`。

### 更新
- 调整 `SKILL.md` 工作流，图标资源优先从 `references/assets/` 目录读取（优先级：assets 已有文件 → 手写 SVG → CSS 兜底）。

---

## 2026-06-01

### 新增
- 新增 `references/examples/` 目录下的指导页面模式示例 HTML 文件。
- 新增 `references/assets/checkbox/` 目录（Selected.svg、Unselected.svg、indeterminate.svg）。
- 新增 `references/assets/radio/` 目录（Selected.svg、Unselected.svg）。

### 更新
- 调整 `references/assets/navigation/` 中 SVG 资源命名，统一为 `header-pc-*` / `footer-pc-*` / `footer-mb-*` 前缀格式。
- 更新 ONavigation 导航组件规范 `components/navigation.md`，对齐最新 assets 命名。
- 更新 OCard 卡片组件规范 `components/card.md`。

---

## 2026-05-29

### 新增
- 新增 `global/hard-constraints.md`——变量映射硬约束文档，定义 PC / MB 双断点下字号 / 行高 / 间距 / 栅格 / 图标尺寸的合法取值白名单。
- 新增 `global/interaction-rules.md`——通用交互规范文档（按钮对齐 / 排列优先级 / 间距 / 禁止组合黑名单）。
- 新增 `global/layout-rules.md`——栅格 / 断点 / 间距规范速查手册。
- 新增 `global/component-doc-spec.md`——组件文档编写规范（Token 匹性要求）。
- 新增 `references/assets/navigation/` 目录，存放导航相关 SVG 资源（header Logo、search icon 等）。

### 更新
- 修正 `references/assets/navigation/` 中 SVG 命名：原 `logos` 统一重命名为 `header-pc-*`，语义更准确。
- 新增 footer 相关 SVG 资源（footer-pc-基金会logo、footer-pc-底部logo、footer-mb-* 系列）。
- 更新 ONavigation 导航组件规范 `components/navigation.md`，补充 footer 资源引用。

---

## 2026-05-28

### 新增
- **opendesign-design** skill 初始创建：SKILL.md 总调度入口、README.md 使用说明、26 个组件设计规范（`components/` 目录）、Pixso MCP 适配器索引（component-keys.md 536 个变体 + icon-keys.md 187 个图标）、设计师使用指南 `references/designer-guide.md`。

### 更新
- 调整 `references/pixso-mcp-adapter/component-keys.md` 和 `icon-keys.md` 中的 openEuler 命名规范，统一组件/图标标识符格式。

---

> 本 CHANGELOG 自 2026-07-25 起从仓库根目录迁移至各 skill 目录，并于 2026-08-18 补全历史变更记录。