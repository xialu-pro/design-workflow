---
name: opendesign-design
description: OpenDesign HTML 页面生产指南。当需要生成符合 OpenDesign 设计规范的 HTML 页面（按钮、输入框、卡片、导航等 UI 组件）、应用设计规范（栅格/颜色/字号/间距/圆角）、搭建页面框架时使用此 skill。包含 PC/MB 双断点的变量映射硬约束（字号/行高/间距/栅格/图标尺寸的合法取值白名单），覆盖 27 个组件设计规范。生成的 HTML 遵循 opendesign-codegen 工程规范（--o-* 语义 Token、OpenDesign 组件命名、BEM、响应式 Token），可直接作为设计原型或进一步转译为 Vue SFC 生产代码。
last_update: 2026-10-16
---

# Design Skill · HTML 页面生成指南

> 🎨 **设计师第一次使用？** → 先读 [references/designer-guide.md](references/designer-guide.md)，里面有角色决策树、规范速览卡和推荐交互方式。核心建议：**逐楼层生成，不要一次性生成整页**——逐楼层还原度远高于整页一次性生成。

> 💡 本文件为 AI 执行入口，仅包含生成 HTML 页面所需的指令与约束。完整使用说明、目录结构、协议信息请查看 [README.md](README.md)；请勿全量加载所有子文件，按工作流按需读取即可。
>
> 产出物是**手写 HTML** 的设计原型（遵循 OpenDesign 设计规范、可转译 Vue SFC），不渲染真实组件库；若要的是渲染**真实 OpenDesign 组件**（UMD + CDN）的可运行单文件或工程内代码，走 [`../opendesign-codegen/SKILL.md`](../opendesign-codegen/SKILL.md)。

> 🔴 **生成任何页面前，必读以下硬约束文档**：
> 1. [global/hard-constraints.md](global/hard-constraints.md) — **变量映射硬约束 + 组件组合规则**（合并版）：PC / MB 双断点下，字号 / 行高 / 间距 / 栅格 / 图标尺寸的合法取值白名单，以及按钮对齐 / 排列 / 间距、组件栅格挂靠、卡片间距、禁止组合黑名单及自动校验清单。所有几何数值必须严格选自该白名单，禁止四舍五入、推断或造数。
>
> 以上约束**最高优先级**，与下文 [#图标处理规范（核心约束）](#图标处理规范核心约束) 并列，缺一不可。
>
> ⚖️ **规则冲突裁决**：楼层模板（floors/）、组件规范（components/）、示例（references/examples/）与 hard-constraints.md 或本文件「图标处理规范」「通用生成约束」冲突时，**一律以 hard-constraints.md 与本文件为准**。发现冲突 → 停下向用户报告冲突点，禁止各取所需或自行折中。

## 数据资源

栅格、响应式断点、设计变量等数据来自 atomgit 上游仓库。**使用时通过 WebFetch 实时拉取最新版本**，不要 bundle 到本地；**WebFetch 拉取失败时，降级用本地 `../opendesign-tokens/references/` 下的六主题 Token 文件（`tokens-{社区}.md`，离线可用，注意可能落后于上游最新值）**：

| 用途 | URL |
|---|---|
| 栅格规范 | `https://raw.atomgit.com/openeuler/opendesign-token/raw/master/packages/opendesign-token/tokens/grid-token.json` |
| 响应式断点 | `https://raw.atomgit.com/openeuler/opendesign-token/raw/master/packages/opendesign-token/tokens/responsive-token.json` |
| **主题 Token（按目标社区选择）** | 见下表，同一目录下按社区命名 |

**⚠️ 各社区主题 Token 相互独立（openGauss 品牌色为紫色，openEuler 为蓝色），用错社区属最高优先级错误。禁止不识别目标社区就默认拉取 openeuler-token：**

| 社区 | Token URL |
|------|-----------|
| openEuler | `https://raw.atomgit.com/openeuler/opendesign-token/raw/master/packages/opendesign-token/tokens/openeuler-token.json` |
| openGauss | `https://raw.atomgit.com/openeuler/opendesign-token/raw/master/packages/opendesign-token/tokens/opengauss-token.json` |
| 其他社区 | 同一目录下按社区名查找 `{community}-token.json`（先访问仓库确认存在）；仓库无该社区 Token 时停下询问用户，禁止以 openEuler Token 代替 |

本 skill 内 bundled 的资源文件（HTML 生产专用，无上游真源）：

- [references/assets/](references/assets/) — SVG 图标资源（按组件名分子目录，用于 HTML 内联）

## 组件索引

每个组件的详细设计规范见 `components/{name}.md`，涵盖适用场景、变体说明、布局规格、颜色/字体 Token 映射、识别特征。

| 组件 | 说明 | 组件 | 说明 |
|------|------|------|------|
| [OAnchor](components/anchor.md) | 锚点 | [OMessage](components/message.md) | 全局消息 |
| [OBanner](components/banner.md) | 横幅 | [ONavigation](components/navigation.md) | 导航 |
| [OBreadcrumb](components/breadcrumb.md) | 面包屑 | [OPagination](components/pagination.md) | 分页 |
| [OButton](components/button.md) | 按钮 | [ORadio](components/radio.md) | 单选框 |
| [OCard](components/card.md) | 卡片 | [OScrollbar](components/scrollbar.md) | 滚动条 |
| [OCarousel](components/carousel.md) | 幻灯片指示器 | [OSelect](components/select.md) | 选择器 |
| [OCheckbox](components/checkbox.md) | 复选框 | [OStep](components/step.md) | 步骤条 |
| [ODataTable](components/data-table.md) | 数据表格 | [OSwitch](components/switch.md) | 开关 |
| [ODialog](components/dialog.md) | 对话框 | [OTab](components/tab.md) | 标签页 |
| [ODivider](components/divider.md) | 分割线 | [OTag](components/tag.md) | 标签 |
| [ODropdown](components/dropdown.md) | 下拉菜单 | [OToggle](components/toggle.md) | 选择块 |
| [OInput](components/input.md) | 输入框 | | |
| [OLink](components/link.md) | 链接 | | |
| [OLoading](components/loading.md) | 加载 | | |
| [OMenu](components/menu.md) | 菜单 | | |
| [OSearch](components/search.md) | 搜索框 | | |

## 图标处理规范（核心约束）

> ⚠️ 本节规则优先级最高，所有生成步骤必须遵守。

### 图标使用规则

**SVG 来源优先级（所有场景统一）**：① `references/assets/` 已有文件（Read 内联）→ ② 手写符合线性风格的 SVG 路径 → ③ CSS（兜底，需注释）

**SVG 内嵌方式**：从 `references/assets/` 读取 SVG 后，**逐属性原样搬运**写入 HTML。

| 属性 | 规格 |
|------|------|
| 尺寸 | 与设计系统图标一致（通常 24×24px，使用硬约束中的图标尺寸 Token） |
| 风格 | 线性（stroke），与 OpenEuler 图标库保持一致 |
| 颜色 | `currentColor`（继承父元素文字色）或 `var(--o-color-*)` 语义 Token |
| 写法 | `<svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" ...>` 直接内嵌 |
| **搬运** | **逐属性原样搬运：`fill-rule`、`clip-rule`、`opacity`、`fill-opacity`、`transform` 一律保留；禁止仅拷贝 `path` 的 `d` 数据重写标签** |

> **风格判定（强制）**：以浏览器渲染结果为准，不得仅凭源码 `fill`/`stroke` 属性下结论。`fill + fill-rule="evenodd"` 的环形构造，渲染上是线性细线。

---

## 执行工作流

> ⚠️ **核心原则：逐楼层生成，不要一次性生成整页。** 一次性处理整页所有组件规范会导致上下文溢出、细节丢失。逐楼层生成时，AI 每次只需处理 1~2 个组件规范，还原度显著更高。

### 第零步：读取硬约束文档 + 识别目标社区拉取对应Token（强制起手式）

```
读取 global/hard-constraints.md
   — 锁定当前目标设备（MB 或 PC）
   — 明确该设备下字号 / 行高 / 间距 / 栅格 / 图标尺寸的合法取值集合
   — 明确按钮对齐 / 排列优先级 / 间距规则
   — 明确组件栅格挂靠要求、卡片间距规则
   — 明确禁止组合黑名单（Banner 嵌套、多尺寸 Banner 并排、按钮位置等）

【社区识别（必做，禁止跳过）】从需求/PRD/用户输入中识别目标社区
   （openEuler / openGauss / openUBMC / 鲲鹏 / 昇腾…）
   — ⚠️ 各社区主题 Token 相互独立（openGauss 品牌色为紫色、openEuler 为蓝色）
   — ⚠️ 禁止不识别社区就默认拉取 openeuler-token！用错社区主题 Token 属最高优先级错误
   — ⚠️ 无法确定目标社区时停下询问用户，禁止猜测

WebFetch 拉取目标社区主题 Token（必须执行！URL 见 #数据资源 社区映射表）
   — 将 JSON 中的颜色/字号/间距变量翻译为 :root CSS 变量
   — ⚠️ 禁止凭训练数据"猜测"颜色值！尤其是 --o-color-fill1（页面背景=浅灰，不是纯白）
   — 拉取失败时：降级用本地 `../opendesign-tokens/references/tokens-{社区}.md`（六主题全量 Token + Dark 值反查，离线可用；栅格 / 响应式见同目录 [tokens.md](../opendesign-tokens/references/tokens.md)）；designer-guide.md 速查表仅作 openEuler Light 辅助
   — 目标社区无本地 Token 文件时：停下询问用户，禁止借用其他社区 Token

【站点框架基准（路由A页面级增改场景必做）】需求是在已有站点上新增/修改页面、不动整站框架时：
   — 抓取目标社区线上站点的真实主导航与页脚结构（导航常为 JS 渲染，
     优先用 curl 拉原始 HTML 提取 nav / footer 节点，WebFetch 提取不到时换 curl）
   — 导航楼层与页脚楼层必须按线上实际结构还原：导航项名称/顺序/链接、
     页脚分组/链接/友情链接/底部信息均以线上为准，仅新增入口高亮；
     组件规格仍遵循 floors/navigation.md，但内容结构禁止套用模板编造
   — 新增导航入口在导航中的位置作为待确认项输出给用户
   — 线上结构抓取不到时，停下向用户索要导航/页脚结构，禁止凭空编造导航项
```

后续所有步骤的几何数值必须落在硬约束的白名单内，组件排布必须符合组合规则。无法对应到约束的需求 → 停下询问用户，禁止造数或静默放行。

### 第一步：规划页面楼层结构

根据用户需求，将页面拆解为楼层序列。所有落地页遵循固定结构（见 [hard-constraints.md §8](global/hard-constraints.md)）：

```
导航楼层（必选）→ [Banner 楼层]（可选）→ 楼层 1 → 楼层 2 → … → 页脚楼层（必选）
```

向用户输出楼层规划，**等待确认后才能继续**：

```
我理解本次页面由以下楼层组成，请确认或调整：
  1. 导航楼层（ONavigation）
  2. Banner 楼层（OBanner，XL/L/M/无）
  3. 功能介绍楼层（OCard × N 栅格）
  4. 数据统计楼层（自定义数字 + 描述）
  5. 页脚楼层（ONavigation Footer）

请确认每个楼层的类型和内容，确认后我将逐楼层读取规范并生成。
```

楼层模板速查见 [#楼层模板索引](#楼层模板索引)。用户确认后，进入逐楼层循环。

### 第二步：确认 Token 与组件规范（全局，仅执行一次）

```
已通过第零步获取上游 Token（WebFetch），确认：
   — :root 中所有 --o-* 语义 Token 变量已就绪
   — 响应式 Token（--o-r-font_size-*、--o-r-gap-*、--o-r-grid-*）已就绪
   — 栅格规范与断点定义已就绪

如需补充对照上游 Token 真源（语义化变量名、栅格断点），通过 WebFetch 拉取 [#数据资源](#数据资源) 中的 atomgit URL。
```

---

### 逐楼层循环（第三步 ~ 第六步，每个楼层执行一轮）

> 🔄 对第一步规划中的每个楼层，依次执行第三步到第六步。**每次只处理一个楼层**，完成验证后再进入下一个楼层。

> 🛑 **导航楼层专属门禁（强制，禁止跳步）**：当前楼层为「导航楼层」时，本楼层固定按以下三步执行：
>
> 1. **先生成顶导骨架**——按 `floors/navigation.md` 生成顶导（Logo / 菜单项 / 右侧操作区，含「菜单项（触发器）完整样式」）
> 2. **停下询问用户（硬门禁）**——向用户提问下拉面板样式：**平铺式**（`floors/navigation-dropdown.md`，垂直列表、选项独占一行）还是**分类平铺式**（`floors/navigation-dropdown-categorized.md`，多列 + 列头 + 条目说明 + 快捷链接栏）？**未经用户明确选择，禁止自行决定样式、禁止跳过本步**
> 3. **确认后一并生成全套**——读取用户选定的下拉面板模板，将「顶导 + 下拉面板」作为**一套完整的导航模式**完整生成（`navigation.md` + `navigation-dropdown` 或 `navigation-dropdown-categorized` = 全套），再进入第六步验证
>
> ⛔ 禁止把裸顶导（无下拉面板）当作导航楼层的最终交付；⛔ 禁止顶导与面板分多轮生成；⛔ 禁止未经用户确认自选面板样式。

#### 第三步：确认本楼层涉及的组件

仅列出**当前楼层**用到的组件，不是整页所有组件：

```
当前楼层：[楼层名称]
涉及组件：
  - OCard（封面卡片，vertical）
  - OButton（solid + outline）
  - OTag（分类标签）

确认后我将读取对应规范。
```

用户确认后，逐一读取每个组件的 `components/{name}.md`，**全部读取完毕才能进入下一步**。

#### 第四步：读取本楼层组件规范与资源

```
1. 读取本楼层涉及的每个 components/{name}.md
2. 如有楼层模板（floors/{type}.md），必须整本读完再落笔，以下四节缺一不可：
   - 「生成前必读」——搭配文件清单
   - 「已确认清单」——用户历轮确认过的规格，优先级高于模板正文旧值与规范默认值
   - 「冲突声明」——与上游组件规范的差异点（以模板为准并已隔离的）
   - 「踩坑记录」——已验证错误，禁止再犯
   ⚠️ 用户后续要求与模板旧值冲突时：先更新模板再生成，禁止按旧稿落笔
   ⚠️ 模板与 hard-constraints 冲突时：按「规则冲突裁决」停下向用户报告，禁止自行折中
3. 检查图标资源：
   ls references/assets/<组件名>/
   有文件 → Read 每个文件，**逐属性原样内联**到 HTML 对应位置（保留 fill-rule / clip-rule / transform / opacity 等所有属性，禁止仅拷贝 path.d）
   无文件 → 手写线性风格 SVG 路径，注释注明「assets 中无资源」
4. 独立图标：查 references/assets/public icons/ 或 components/{name}.md 中的图标表按中文名匹配
```

> ⚠️ **导航楼层的读取范围（防幻觉）**：`components/navigation.md` 是**设计视角**文档（变体 / 布局树 / 颜色 / 资源入仓清单），**HTML 实现规范已被楼层模板取代**——生成导航时，`components/navigation.md` **只读 Part C（触发器交互状态）与 Appendix A（Token 速查表）**，HTML 结构 / CSS / 图标内联方式一律以 `floors/navigation.md` 及下拉面板模板为准。禁止把 Part A~G 的设计视角描述（布局树、`<img>` 引用等）当作 HTML 落笔依据。

#### 第五步：生成本楼层

生成本楼层的独立 HTML 片段（含内联 CSS + `--o-*` 语义 Token），后续在第七步组装为完整页面。

**HTML 生成规范（必须遵循，对齐 opendesign-codegen 工程标准）**：

| 维度 | 要求 |
|------|------|
| 颜色 | 全部使用 `var(--o-color-*)` 语义 Token，禁止 `#fff`/`rgb()` 硬编码 |
| 色板变量 | **严禁**直接使用 `--o-kleinblue-*`/`--o-grey-*` 等色板变量，只能用语义 Token（`--o-color-info1` 而非 `--o-grey-14`） |
| 字号/行高 | 优先使用 `var(--o-r-font_size-*)` + `var(--o-r-line_height-*)` 响应式 Token |
| 间距 | 页面级使用 `var(--o-r-gap-*)`；组件内使用 `var(--o-gap-*)` |
| 栅格 | 页面级布局使用 24 栅格（`--o-r-grid-N` + 水槽 `--o-r-grid-column-gutter`） |
| 圆角/阴影 | `--o-radius-*`、`--o-shadow-*` |
| 组件命名 | 容器类名使用 BEM（如 `.navigation`、`.card-grid`），组件语义使用 `o-` 前缀（如 `.o-button`、`.o-card`） |
| 响应式 | 优先响应式 Token 免写媒体查询；确需断点时用 `@media` |
| 无内联 style | 禁止 `style="..."` 内联样式（极特殊动态值除外并注释） |
| 无 !important | 除非硬约束中的布局强制要求，否则禁止 `!important` |

#### 第六步：验证本楼层

```
逐项核对本楼层的硬约束自检清单：
□ 字号是否来自该设备列的字号集合？字号与行高是否成对？
□ 间距是否来自该设备列的间距集合？
□ 卡片间距是否为 PC 32px / MB 12px？
□ 按钮是否左对齐、solid 唯一、间距 24px？
□ 所有组件是否挂靠栅格容器？
□ 【HTML栅格】楼层内容容器 width + max-width 是否为 1488px（PC）/ 312px（MB）？（禁止 1200px / 1140px / 960px）
□ 【HTML栅格】楼层内容容器是否使用 margin-left: 216px + margin-right: 216px（PC）/ margin-left: 24px + margin-right: 24px（MB）？（禁止 `margin: 0 auto` + `padding: 0 216px` 组合）
□ 【HTML栅格】楼层内容容器 padding 是否为 0？（禁止用 padding 控制楼层边距，padding 仅用于组件内部间距）
□ 【HTML栅格】验证：margin-left + width + margin-right = 画布宽度（PC: 216+1488+216=1920 / MB: 24+312+24=360）
□ 【HTML颜色】`:root` 中的颜色值是否来自上游Token（openeuler-token.json），而非凭训练数据猜测？（尤其检查 `--o-color-fill1` 是否误写为纯白 `#FFFFFF`）
□ 【HTML颜色】`body` 的 `background-color` 是否使用 `var(--o-color-fill1)`？（禁止 `#FFFFFF` / `white`）
□ 【SVG搬运】图标 SVG 是否逐属性原样搬运（fill-rule / clip-rule / transform / opacity 未丢失）？
□ 【CodeGen对齐】颜色是否全部使用 `var(--o-color-*)` 语义 Token，无 `#fff`/`rgb()` 硬编码？
□ 【CodeGen对齐】是否未直接使用色板变量（`--o-kleinblue-*`/`--o-grey-*` 等）？
□ 【CodeGen对齐】字号/行高是否使用 `var(--o-r-font_size-*)` + `var(--o-r-line_height-*)`？
□ 【CodeGen对齐】是否无内联 `style="..."` 和非必要 `!important`？
□ 【CodeGen对齐】类名是否使用 BEM 命名规范？
□ 【视觉合理性】数值虽全部合规，视觉是否仍然异常（内容重心偏移 / 对比度不足 / 留白失衡 / 信息色与背景色颠倒）？——数值自洽不等于符合设计意图，禁止只验算式不验视觉；发现疑点必须在交付说明中显式提出，禁止静默放行

渲染验证（按环境能力分级，禁止静默跳过）：
   — 有浏览器能力（Playwright / Puppeteer MCP、IDE 预览等）→ 打开 HTML 片段截图目检
   — 无浏览器能力（headless / CLI 环境）→ 逐项静态自检，并在交付说明中显式标注「未做渲染目检，需人工在浏览器复核」

验证不通过 → 修正后重验，**通过后才进入下一个楼层**。
```

> 🔴 **同类全查（禁止挤牙膏式修复）**：当自检或用户指出本楼层任一处不合规时，**必须对本楼层同维度规格做全量复查**——例如发现按钮颜色不对，须同时复查该楼层所有组件的颜色 / 尺寸 / 圆角 / 变体 / 状态，一次性列全问题清单再修。禁止用户指出一条就只修一条，把多轮返工成本转嫁给用户。

---

### 第七步：组装所有楼层为完整页面

所有楼层验证通过后，按楼层顺序组装为完整页面：

```
导航楼层 → Banner 楼层 → 楼层 1 → 楼层 2 → … → 页脚楼层

楼层间距统一使用间距 10（PC 72px / MB 32px）
```

将所有楼层 HTML 片段组装为完整 HTML 文件，包含统一的 `<head>` / CSS Reset / Token 引入。

### 第八步：最终验证

```
□ 页面结构是否符合：导航 → [Banner] → 楼层(s) → 页脚？
□ Banner 以下楼层标题（及描述）是否水平居中对齐（Banner 内容为左对齐）？
□ 楼层间距是否统一为间距 10（PC 72px / MB 32px）？
□ 全页组件组合是否无黑名单违规？
□ 整体视觉一致性检查
□ 【社区Token】:root 颜色是否来自目标社区主题 Token（对照第零步社区识别结论，禁止 openEuler 默认值混入）？
□ 【站点框架】路由A页面级增改场景：导航/页脚是否与社区线上实际结构一致（对照第零步抓取的站点框架基准）？
□ 【渲染验证】组装完成后逐楼层目检完整页面（有浏览器能力 → 打开截图目检；无 → 按第六步降级为静态自检并显式标注「未做渲染目检」）——重点排查固定 width/height 容器内的长文案是否折行溢出堆叠（步骤条标题、卡片标题、按钮、标签）
□ 【图标风格】关键图标是否与设计系统一致（以浏览器渲染为准，非仅看源码 fill/stroke 属性）？
□ 【CodeGen对齐】全页颜色是否全部使用 `var(--o-color-*)` 语义 Token，无硬编码？
□ 【CodeGen对齐】全页字号/行高/间距是否使用响应式 Token？
□ 【CodeGen对齐】全页是否无语义色板变量直接引用？
□ 【CodeGen对齐】全页类名是否符合 BEM 规范？
□ 【视觉合理性】全页数值虽合规，视觉是否仍然异常（各楼层内容重心偏移 / 信息色与背景色颠倒 / 品牌色对比度不足）？——禁止只验数值不验视觉，发现疑点显式提出
```

---

### 通用生成约束（所有楼层共用）

> 🚨 以下约束无论哪个楼层均强制生效，违反即违规：
>
> - **必须逐一读取**当前楼层涉及组件的规范文档 `components/{name}.md`
> - 合法的参数来源只有三个：① 对应组件规范文档 ② 硬约束白名单（第零步） ③ 上游 Token 真源
> - 三者之外的任何取值均属**自定义内容，一律禁止**，包括但不限于：近似色值、估算间距、推断圆角、猜测默认变体
> - **规格表必须逐行对照，禁止凭先验落笔**：颜色 / 尺寸 / 圆角 / 变体等每一处取值，都必须回到本楼层 `components/{name}.md` 的规格表逐行确认后再写。当「我习惯的写法 / 训练数据中的常见样式」与规范表冲突时，**一律以规范表为准**——读过了规范不等于对照过规范，禁止边写边凭印象填充
> - **规范未覆盖时的分级处理**（禁止静默取值）：
>   - 规范文档缺少某项参数（如 PC 端未写明 Banner 的垂直对齐方式）→ **先显式向用户指出该缺失**，并给出你打算采用的设计常识做法
>   - 确需按设计常识补位时，必须在交付说明中标注「**此点为规范未覆盖的推断，需你确认**」
>   - **禁止把推断值当规范值静默使用**；也禁止因为规范没写，就默默抓取文档里任意一个看似相关的数值充当布局模型
> - 如果规范文档中找不到所需参数且无法向用户确认 → 停下，询问用户，**禁止自行填充**
> - 图标：直接内嵌 `<svg>` 路径，禁止色块占位符或 `<img src>` 引用
> - SVG 颜色：`currentColor` 继承父元素色值，或 `var(--o-color-*)` 语义 Token
> - SVG 搬运（强制）：从 assets 读取 SVG 时逐属性原样内联，保留 `fill-rule` / `clip-rule` / `opacity` / `fill-opacity` / `transform`，禁止仅拷贝 `path` 的 `d` 数据重写标签
> - **位图素材（案例封面 / Banner 图等）优先生成、不留占位色块**：若当前环境提供文生图能力（内置工具 / MCP 服务 / 可用 API，端点以所用 AI 工具为准，本 skill 不绑定任何厂商端点），生成贴合内容语义的图片填充——prompt 为 URL 编码的具体场景描述、按容器比例选尺寸、调性贴合目标社区主题色与开发者风格；以 `<img>` + alt 填充，HTML 注释注明「示意配图，上线前替换真实素材」；**生成后必须校验**返回的是真实生成图（固定兜底图 / 默认占位图属静默失败，换 prompt 重试）。**环境无文生图能力时**：用占位色块 + 同样的 HTML 注释，禁止因缺能力中断流程，禁止编造图片 URL
>
> 🎯 **CodeGen 工程对齐约束（强制）**：
> - 颜色全部使用 `var(--o-color-*)` 语义 Token，**严禁**使用色板变量（`--o-kleinblue-*`/`--o-grey-*` 等）
> - 字号/行高优先 `var(--o-r-font_size-*)` + `var(--o-r-line_height-*)` 响应式 Token
> - 间距使用 `var(--o-r-gap-*)` 或 `var(--o-gap-*)` Token
> - 圆角/阴影使用 `--o-radius-*` / `--o-shadow-*` Token
> - 类名使用 BEM 命名规范（`block__element--modifier`）
> - 禁止非必要 `!important` 和内联 `style="..."`
> - 页面使用 24 栅格布局（`--o-r-grid-N` + `--o-r-grid-column-gutter`）

### 验收页（check page）生成约定

> 交付高保真验收页供用户逐轮验收时，以下约定强制生效——多轮返工的最高频来源：

1. **默认渲染全部可选模块**：楼层模板中默认注释掉的可选模块（如分类平铺式的快捷链接栏），验收页必须默认渲染出来。默认不渲染会导致用户「验收时看不到」，规格无法验证；模板本体仍保持默认注释（两处刻意不同，不是遗漏）
2. **辅助元素不得遮挡验证区**：图例 / 标注 / 规格说明等辅助元素，禁止 `position: fixed` 固定在面板或验证区之上；默认收起（`<details>`）或移到不遮挡位置
3. **验收页与模板逐条一致**：验收页 CSS 必须与楼层模板规格一致，唯一允许差异 = 可选模块的默认渲染状态；模板每轮修正后必须同步验收页，交付前做双向 diff（本轮曾因模板改了 `min-height` 验收页没同步，多跑一轮）

## 楼层模板索引

每个楼层模板是一个**自包含的生成单元**——AI 读了这个文件就能精确生成该楼层，无需再拼凑多个组件规范。模板包含：布局结构、精确尺寸/间距/字号值（直接取自硬约束白名单）、PC 和 MB 双端规格。

| 楼层 | 模板文件 | 典型用途 |
|------|---------|---------|
| 导航 | [floors/navigation.md](floors/navigation.md) | 页面顶部导航栏（PC/MB） |
| 导航下拉面板 · 平铺式 | [floors/navigation-dropdown.md](floors/navigation-dropdown.md) | 顶导菜单项展开的平铺式下拉面板（垂直列表，与导航楼层搭配） |
| 导航下拉面板 · 分类平铺式 | [floors/navigation-dropdown-categorized.md](floors/navigation-dropdown-categorized.md) | 顶导菜单项展开的分类平铺式下拉面板（多列 + 列头 + 条目说明 + 快捷链接栏，与导航楼层搭配） |
| Banner | [floors/banner.md](floors/banner.md) | 首页/栏目页顶部横幅（XL/L/M） |
| 卡片栅格 | [floors/card-grid.md](floors/card-grid.md) | 功能展示、产品列表、特性介绍 |
| 图文特色 | [floors/feature-section.md](floors/feature-section.md) | 左图右文/左文右图特色介绍 |
| 页脚 | [floors/footer.md](floors/footer.md) | 站点地图、版权信息、社交入口 |

## 与 CodeGen skill 的关系

本 skill 专注于**设计规格到 HTML 页面**的映射——确保输出的 HTML 视觉上严格符合 OpenDesign 设计规范。当需要将 HTML 原型进一步转化为生产级 Vue SFC 代码时，应使用 `opendesign-codegen` skill。两者的关系：

| 维度 | opendesign-design（本 skill） | opendesign-codegen |
|------|------|------|
| 产物 | HTML 页面（设计原型） | Vue 3 + OpenDesign SFC（生产代码） |
| Token 使用 | `--o-*` CSS 变量 + `:root` | `--o-*` CSS 变量（同一套 Token） |
| 组件表达 | `o-` 前缀 CSS 类名 + BEM | 真实 `<OButton>`/`<OCard>` 等组件 |
| 布局 | 24 栅格 + 硬约束白名单 | 24 栅格 + 响应式 Token |
| 适用场景 | 设计评审、原型演示、视觉还原 | 直接进生产工程 |

生成的 HTML 中的 `--o-*` Token 命名、色值语义、组件类名约定与 CodeGen 保持一致，确保从 HTML 到 Vue SFC 的转译顺畅。

## 注意事项

- 当你准备使用某一个组件来生成页面时，你应该读取 opendesign-components 的 skill，来看他对应的组件开发 skill 中说明的传参是否足以支持你的想法
- **组件文档中所有色值、字号、间距必须匹配上游 Token 的变量名**，禁止使用硬编码值
- 编写组件文档时，参见 [global/component-doc-spec.md](global/component-doc-spec.md) 中的 Token 匹配性要求和格式规范

## 异常与降级策略

| 异常场景 | 处理方式 |
|----------|----------|
| WebFetch 拉取上游 Token 失败（超时 / 404） | 使用 `references/designer-guide.md` 中的 Token 语义速查表作为降级数据源，在输出中注明「Token 真源未拉取，使用降级数据源」 |
| `components/{name}.md` 不存在 | 停下告知用户该组件暂无设计规范，询问是否继续（无规范 = 无合法参数来源，禁止自行推断） |
| 硬约束白名单无法覆盖需求值 | 停下告知用户该值不在合法取值集合内，给出最接近的白名单值供选择，禁止静默取近似值 |
| `references/assets/` 中无目标 SVG | 按图标使用规则降级：手写线性风格 SVG 路径，并在 HTML 注释中注明「assets 中无资源」 |

<!-- 以下内容仅为人类维护者参考，AI 执行时请忽略 -->
### 📎 人工参考附录
- 更新日志：[CHANGELOG.md](CHANGELOG.md)
- 项目说明与目录结构：[README.md](README.md)
- 组件文档编写规范：[global/component-doc-spec.md](global/component-doc-spec.md)