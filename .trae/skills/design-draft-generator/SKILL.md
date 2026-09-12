---
name: "design-draft-generator"
description: "Design draft generator for Huawei computing ecosystem designers. Routes PRD-based design work to opendesign-design skill (design-system-guarded UI) or generates free-form HTML interactive prototypes. Invoke when user asks to create 设计稿/交互原型 from a PRD or business requirement."
---

# 设计稿生成器（设计师原型助手）

面向华为计算生态产品团队的设计师，承接 PRD 之后的下游设计环节：读取需求文档（PRD），判定设计需求类型，路由到对应的设计生产流程，产出可评审的设计稿。

**业务背景**：鲲鹏、昇腾等计算生态产品，以及 openEuler（欧拉）、openGauss（高斯）、openUBMC 等开源项目。

## 安装方式

本 skill 遵循 Agent Skills 开放标准（一个目录 + 一个 SKILL.md），支持 Claude Code、OpenCode、Codex 主流 AI 编程平台。`<SKILL_SOURCE>` 指本 skill 目录（含 SKILL.md 的 `design-draft-generator/` 文件夹）。

**项目级安装（推荐，随仓库分发，团队共享）**

```bash
# 在项目根目录执行，按所用平台复制到对应目录
cp -R <SKILL_SOURCE> .claude/skills/    # Claude Code
cp -R <SKILL_SOURCE> .opencode/skills/  # OpenCode 原生
cp -R <SKILL_SOURCE> .agents/skills/    # Codex（OpenCode 亦兼容此路径）
```

**全局安装（本机所有项目生效）**

| 平台 | 全局路径 |
|------|---------|
| Claude Code | `~/.claude/skills/design-draft-generator/SKILL.md` |
| OpenCode | `~/.config/opencode/skills/design-draft-generator/SKILL.md`（兼容 `~/.claude/skills/`、`~/.agents/skills/`） |
| Codex | `~/.codex/skills/design-draft-generator/SKILL.md`（新版亦支持 `~/.agents/skills/`） |

**验证安装**：Claude Code 会话输入 `/` 查看 skills 列表；OpenCode 由 agent 通过原生 skill 工具按需加载；Codex 用 `/skills` 查看，或提示词中 `$design-draft-generator` 显式调用。

**路由 A 依赖**：使用设计系统看护流程（路由 A）需同时安装 `opendesign-design` skill，从 atomgit 获取：`https://atomgit.com/openeuler/opendesign-skills/tree/master/skills/opendesign-design`，按上述相同方式安装。

**注意**：目录名必须与 frontmatter `name` 一致（`design-draft-generator`）；修改后未生效时重启对应 CLI。

## 两类设计需求

**路由的唯一判据是"是否使用 OpenDesign 设计系统"，由用户确认，不靠 PRD 措辞推断**——新建社区同样可以选择用设计系统搭建（走路由 A），已有社区增改也可能被要求出自由视觉稿（走路由 B）。

| 类型 | 定义 | 典型场景 | 产出路径 |
|------|------|---------|---------|
| 设计系统看护 | **使用 OpenDesign 设计系统**搭建 | 已有社区（已接入）的功能迭代、页面改版、新增页面/专区/子站；新建社区选择按设计系统规范建站 | 路由 A：委托 opendesign-design skill |
| 非设计系统看护 | **不使用设计系统**，视觉自由度大 | 新社区概念探索、暂不受设计系统约束的自由原型 | 路由 B：生成 HTML 交互原型 |

## 设计纲领

> 提炼自《开发者调性》v0.3，**两条路由均强制遵守**，生成前逐条自查。

### 三大理念

1. **Developers over sales**：以开发者为核心而非营销导向。不为营销转化牺牲功能可达性与信息清晰度；用专有名词、参数、版本号、代码块等专业信息填充设计，拒绝过度装饰与引导式设计
2. **Simple over easy**：简单明了优于傻瓜易用。逻辑层级清晰、规则一致可复用，不刻意隐藏复杂度做保姆式引导；开发者用过一次即可形成记忆并迁移
3. **Doable and searchable**：功能可执行、内容可检索。功能从发现到操作全链路闭环；信息结构化呈现，人可快速定位，AI 工具可精准识别与调用

### 五大原则

| 原则 | 维度 | 核心要求 |
|------|------|---------|
| 简洁 | 外观呈现 | 去装饰化，营销元素让位核心内容；关键功能与提示常驻展示，不依赖 Hover 才显示；字号/字重/色彩区分层级，控制单屏信息密度 |
| 准确 | 内容表意 | 图标/操作命名用开发者通用认知，避免小众与网络化表达；关键状态禁止仅靠颜色或单一图标传义，必须配明确文本 |
| 连贯 | 操作逻辑 | 操作入口与当前场景强绑定；多尺寸响应式、跨端体验一致；逻辑相关内容模块化成组，关联通过显性链接/锚点表达 |
| 高效 | 操作效率 | 搜索入口清晰常驻；核心功能多入口可达，不强制线性流程；标题层级规范，锚点与内容一一对应 |
| 闭环 | 操作结果 | 耗时任务提供进度与状态；异常提示同步成因与处理建议；版本变更可溯源；命令/代码/链接等可执行内容便捷提取复制 |

### 设计四原则（CRAP）

视觉组织的底层法则，与五大原则配合使用：五大原则决定"设计什么"，四原则决定"怎么摆"。

| 原则 | 含义 | 应用要点 |
|------|------|---------|
| **亲密（Proximity）** | 相关元素在空间上靠近，组成视觉单元 | 逻辑同组的内容靠拢（配合纲领"模块化成组"），无关内容拉开距离；组内间距 < 组间间距 |
| **对比（Contrast）** | 拉开差异，制造层级 | 字号/字重/颜色/留白至少一项形成明显反差；页面层级 ≤ 4 级；两个元素不同就让他们截然不同，避免"看起来差不多"的模糊层级 |
| **重复（Repetition）** | 相同性质元素使用一致的处理方式 | 同级标题、卡片、按钮、间距在整个页面重复同一规格；一致性让开发者形成记忆（呼应 Simple over easy） |
| **对齐（Alignment）** | 任何元素都不随意放置，与页面其他元素建立视觉联系 | 统一对齐基准（左对齐优先，数字可右对齐）；跨组元素也保持对齐线贯通；栅格挂靠本质是对齐的系统化 |

## 工作流程

### 第 1 步：读取需求输入

优先读取 PRD（默认位于 `docs/`，命名 `PRD_[主题]_[YYYYMMDD].md`，通常由 requirement-doc-generator 产出）。从 PRD 提取设计输入：

| PRD 章节 | 提取的设计输入 |
|---------|--------------|
| 1.3 目标与成功指标 | 设计的价值锚点（体验类指标优先） |
| 2. 用户与场景 | 页面使用者与使用情境 |
| 3.1 功能清单 + 4. 功能详述 | 页面模块与交互说明（重点读"交互与原型说明"小节） |
| 3.2 适配范围 | 目标设备（PC / MB / 双端） |
| 5. 非功能性需求 | 性能与兼容约束 |

无 PRD 时：新建社区优先使用设计简报（见 3B-0，最终走 A 还是 B 以第 2 步判定为准）；其他情况建议用户先用 requirement-doc-generator 生成 PRD，用户坚持直接设计则先补齐四项关键信息（页面目标、目标用户、功能模块清单、目标设备）再继续。

### 第 2 步：需求类型判定（必须问用户，禁止自行拍板）

**是否使用 OpenDesign 设计系统只能由用户确认，不能从 PRD 措辞推断**。用 AskUserQuestion 向用户提问：

> 本需求是否使用 OpenDesign 设计系统搭建？
> - **使用** → 路由 A（委托 opendesign-design，按设计规范生产）
> - **不使用** → 路由 B（自由视觉，HTML 交互原型）

提问时附上建议路由作为推荐项，建议依据下表：

| 判定信号 | 建议路由 |
|---------|------|
| 需求发生在已接入 OpenDesign 的社区（openEuler / openGauss / openUBMC 等），无论页面修改、新增页面还是**新增专区/子站/一组新页面** | A |
| 新建社区/站点从 0 到 1 搭建，暂无设计系统诉求 | B（交付时注明未来接入设计系统的可能性） |
| 新建社区但用户选择用设计系统搭建 | A |

**易错警示**："新增 Agent 专区"这类需求是已有社区的常规增改，属路由 A——PRD 里的"新增/搭建/从零"等措辞**不构成**走 B 的依据，曾有团队因此误走 B 产出不合规视觉。判定依据是社区接入事实 + 用户确认，两者缺一不可。

### 第 3 步 A：使用设计系统 → 委托 opendesign-design

本 skill 不重复设计系统规范，职责是把 PRD 预消化为 opendesign-design 的输入：

1. 检查环境中是否已安装 opendesign-design skill；未安装时提示从 atomgit 获取：`https://atomgit.com/openeuler/opendesign-skills/tree/master/skills/opendesign-design`
2. 从 PRD 提炼以下材料并输出，交由用户确认：
   - **目标社区与 Token 来源**：明确目标社区（openEuler / openGauss / openUBMC / 鲲鹏 / 昇腾…）及其主题 Token 文件。各社区主题色不同（如 openGauss 紫、openEuler 蓝），禁止默认 openEuler；无法从 PRD 判断时询问用户
   - **站点框架基准**：路由 A 属于页面级增改、不动整站框架——导航与页脚必须按社区线上站点实际结构还原（抓取线上页面提取主导航项名称/顺序/链接与页脚分组/链接，导航为 JS 渲染时用 curl 拉原始 HTML），仅新增入口高亮；新增导航入口的位置作为待确认项输出；抓取不到时向用户索要，禁止编造。**新建社区走 A 时无线上站点可抓取**，站点框架（导航/页脚结构）作为待确认项与用户共同确定（可参考设计系统默认框架或同类社区结构），同样禁止编造
   - **页面楼层规划**：[Banner] → 楼层 1..N（导航/页脚已由站点框架基准确定），每楼层标注来源功能编号（PRD-XXX）与内容摘要。楼层取舍遵守纲领"简洁"原则：拒绝纯装饰/营销楼层，信息密集型内容楼层优先
   - **组件清单**：每楼层涉及的 O 组件（OButton / OCard / ODataTable…）。组件选型遵守纲领"准确"原则：同类信息聚合优先选语义承载强的标准组件（列表用 ODataTable / OTag 而非纯文本堆砌，代码内容用规范代码块组件），禁止用纯视觉样式模拟结构化内容
3. 确认后引导进入 opendesign-design 的标准工作流（硬约束 → 社区识别与 Token → 站点框架基准 → 楼层确认 → 逐楼层生成 → 验证），不在本 skill 内执行

### 第 3 步 B：不使用设计系统 → HTML 交互原型

**3B-0 输入：设计简报（无 PRD 时的标准输入）**

将简报模板 [templates/new-community-brief.md](file:///Users/xialu/Desktop/trae%20project/design%20workflow/.trae/skills/design-draft-generator/templates/new-community-brief.md)（位于本 skill 目录 `templates/` 下）交给设计师填写；设计师不便填写时，按简报结构（任务 / 目标用户 / 内容域 / 内容素材 / 关键功能 / 风格与方案数量）通过 AskUserQuestion 代填。内容素材越真实评审越有效（Developers over sales：用数字，不用形容词），无素材条目留空以占位内容补齐。

**3B-1 信息架构（必须先确认）**

输出页面模块结构：楼层/分区序列 + 每模块承载的功能（对应 PRD 编号或简报素材来源）与内容，等待用户确认后再生成，禁止跳过确认直接产出。简报"业界参考"留空时，先调研业界同类网站（WebSearch）补充建议模块，并入信息清单一起确认。

**3B-2 布局交互方向与视觉方向**

- 简报指定方案数量 N > 1（默认 3）时，不做视觉提问，直接生成 N 套方案：信息架构（业务内容与功能模块）统一，各方案在**布局结构、交互方式**两维度差异化——多方案的价值在于对比**同一业务的不同信息组织与操作流**，而非视觉风格变体。差异化示例：方案 A 门户式（顶部搜索 + 卡片栅格平铺）/ 方案 B 工作台式（左侧分类导航 + 右侧内容区）/ 方案 C 快捷入口式（首屏全功能入口 + 分组向导），可按简报关键功能调整
- 视觉调性可在各方案间存在差异，但必须落在**开发者调性**范围内（终端质感、代码/文档/数据密度、克制的色彩与留白等），禁止商务风、营销风（如大图 Banner 配文案口号、渐变光效、促销式强调、销售导向的引导设计）
- N = 1 时，AskUserQuestion 提问（一轮 ≤4 题）：视觉调性 / 主色与品牌 / 参考风格 / 强调元素

**3B-3 生成规范**

视觉规范：
- 自包含单 HTML 文件：内联 CSS，浏览器直接打开即可评审；确需交互（Tab 切换、表单校验、弹层）可内联少量原生 JS，不引入外部依赖
- 按 PRD 3.2 适配范围实现响应式；未指明时默认 PC 优先，设计宽度 1440px
- 字号阶梯 ≤ 4 级，间距取 4/8 的倍数，保持节奏一致
- 文字对比度满足 WCAG AA（正文 ≥ 4.5:1）
- 占位内容使用中文真实文案（禁止 lorem ipsum）；**案例封面、Banner 图等位图素材禁止留占位色块**——用文生图 API 生成贴合内容语义的图片填充：`https://console.enterprise.trae.cn/api/ide/v1/text_to_image?prompt={prompt}&image_size={image_size}`（prompt 为 URL 编码的具体场景描述，`image_size` 按容器比例选 `landscape_16_9` / `landscape_4_3` / `square` 等；图片调性遵循开发者风格，贴合目标社区主题色）；以 `<img>` 填充并配 alt 说明，HTML 注释注明「文生图示意配图，上线前替换真实素材」；**生成后必须校验**：curl 检查响应重定向目标，落在 `default.jpeg` 属静默失败（prompt 可能触发内容审核），须换 prompt 重试直至返回真实生成图；API 请求本身失败时降级为占位色块 + alt 说明
- 页面内以 HTML 注释标注关键交互说明（如 `<!-- 交互：点击展开筛选面板 -->`），便于评审对照

纲领语义化落地（强制，保障人机双端可识别）：
- **语义地标**：用 `<header>` / `<nav>` / `<main>` / `<aside>` / `<footer>` 划分页面区块，核心内容置于 `<main>`，AI 与读屏可直达
- **结构化内容**：同类条目聚合用 `<ul>` / `<ol>`；命令、代码、配置片段用 `<pre><code>` 承载并配代码类型标识与复制按钮
- **可交互元素**：纯图标按钮必须配可见文字或 hover 提示文本，并添加 `aria-label`；输入控件用 `<label>` 绑定，禁止仅用 placeholder 传义
- **关键信息不裸传**：状态、校验、错误信息禁止仅靠颜色或单一图标表达，必须配明确文本说明
- **模块化成组**：逻辑相关内容聚合为带标题的模块（层级归属清晰），模块间关联用显性链接/锚点表达，不依赖视觉邻近暗示

**3B-4 交付**

- 输出到 `design/` 目录：单方案命名 `prototype_[主题]_[YYYYMMDD].html`；多方案命名 `prototype_[主题]_方案A/B/C_[YYYYMMDD].html`
- 交付后提示：原型路径可回填 PRD 第 4 章"交互与原型说明"（或设计简报），形成需求-设计闭环

**3B-5 对抗评审（生成后必须执行，交付前的最后一道关卡）**

生成本身不是终点。对抗评审由独立的 **adversarial-reviewer** skill 执行（7 项检查表 + 评审报告 + 修复闭环），本 skill 不内嵌评审流程，只负责交接：

- **被 design-workflow 编排时**：编排层以 subagent 派发 adversarial-reviewer，生成者与评审者上下文物理隔离，消除确认偏误；❌ 项修复回到本 skill（主对话）执行，复检再派发
- **独立使用本 skill 时**：交付前引导调用 adversarial-reviewer（位于同一 skills 目录）
- **评审规范来源**：本文件的"设计纲领"与"质量标准"两节是 adversarial-reviewer 的判定依据，修改时保持同步
- **交付前置条件**：评审未通过（❌ 项未关闭且无显式风险声明）禁止执行 3B-4 的交付动作

## 质量标准（生成前自查 + 生成后对抗评审基准）

1. **需求可溯**：每个页面模块可对应到 PRD 功能编号（PRD-XXX）或设计简报的信息清单条目；两者皆无时对应到已确认的功能清单
2. **信息架构先行**：先确认结构再生成，禁止跳过确认直接产出
3. **设备适配**：与 PRD 3.2 适配范围一致；双端需求出响应式单文件或双端两份原型
4. **可评审**：真实占位文案、可点的交互、注释标注清晰，评审者无需追问即可理解
5. **视觉基本盘**：层级清晰、间距有节奏、对比度达标，自由发挥也守住设计素养底线
6. **纲领合规**：生成前对照"三大理念 + 五大原则"逐条自查；路由 B 额外检查语义化落地清单（语义地标 / 结构化内容 / aria-label / label 绑定 / 模块化成组）
7. **路由正确**：是否使用设计系统以第 2 步的用户确认为准（判定记录须含用户答复）；设计系统看护类绝不走自由生成路径（会产生不合规视觉）
8. **视觉细节对齐**：① 导航栏 Logo 与导航文字必须垂直居中——禁止 `align-items: flex-end` + 子项 `padding-bottom` + `align-self: center` 的组合模拟贴底对齐（`box-sizing: border-box` 下高度计算失真导致错位），正确做法是导航区 `align-items: center` 整体居中、导航项 `height: 100%` + `border-bottom: 2px` 实现选中下划线贴底；② 深色/品牌色背景上的图标必须与文字同色（白色）——SVG 图标原始填充为黑色且禁止修改文件本身，须在 CSS 中用 `filter: invert(1)` 反色（如 solid 按钮、深色代码块头部内的图标），禁止让黑色图标直接出现在深色背景上；③ 承载文本的容器禁止固定尺寸卡死——固定 `width` 遇长文案会折行、固定 `height` 遇折行文案会溢出堆叠（如步骤条节点标题与数字圆圈叠压），承载动态文本的容器用 `min-width`/`min-height` 或不设尺寸，单行标题补 `white-space: nowrap`

## 与其他 skill 的关系

- **requirement-doc-generator**：上游，产出 PRD；本 skill 消费 PRD 并回填原型路径
- **design-workflow**：编排器，把 requirement-doc-generator（阶段 1）与本 skill（阶段 2-3）串成端到端流水线（PRD 生成 → 确认门 → 设计稿 → 对抗评审 → 回填闭环）；独立使用本 skill 时不经过该编排
- **adversarial-reviewer**：下游，3B-5 对抗评审的执行者（独立上下文派发）；本 skill 的"设计纲领"与"质量标准"是其评审规范来源
- **opendesign-design**：下游，路由 A 的执行者（Pixso 设计稿生产，含 Token 硬约束与逐楼层工作流）
- **opendesign-tokens**：路由 B 不强制其 Token 约束，但涉及已有主题的产品时建议参考其品牌色保持一致性
