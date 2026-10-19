---
name: opendesign-design
description: OpenDesign Pixso 设计稿生产指南。当需要在 Pixso 中创建/编辑 UI 组件（按钮、输入框、卡片、导航等）、应用设计规范（栅格/颜色/字号/间距/圆角）、搭建页面框架、调用 Pixso 组件库（Symbol）或读取设计变量（Tokens）时使用此 skill。包含 PC/MB 双断点的变量映射硬约束（字号/行高/间距/栅格/图标尺寸的合法取值白名单），覆盖 26 个组件设计规范、536 个 componentKey 变体与 187 个图标 componentKey。本 skill 仅生产 Pixso 设计稿，不输出代码。
last_update: 2026-08-18
---

# Design Skill · Pixso 组件系统

> 🎨 **设计师第一次使用？** → 先读 [references/designer-guide.md](references/designer-guide.md)，里面有角色决策树、规范速览卡和推荐交互方式。核心建议：**逐楼层生成，不要一次性生成整页**——逐楼层还原度远高于整页一次性生成。

> 💡 本文件为 AI 执行入口，仅包含生成设计稿所需的指令与约束。完整使用说明、目录结构、协议信息请查看 [README.md](README.md)；请勿全量加载所有子文件，按工作流按需读取即可。

> 🔴 **生成任何设计稿前，必读以下硬约束文档**：
> 1. [global/hard-constraints.md](global/hard-constraints.md) — **变量映射硬约束 + 组件组合规则**（合并版）：PC / MB 双断点下，字号 / 行高 / 间距 / 栅格 / 图标尺寸的合法取值白名单，以及按钮对齐 / 排列 / 间距、组件栅格挂靠、卡片间距、禁止组合黑名单及自动校验清单。所有几何数值必须严格选自该白名单，禁止四舍五入、推断或造数。
>
> 以上约束**最高优先级**，与下文 [#图标处理规范（核心约束）](#图标处理规范核心约束) 并列，缺一不可。

## 数据资源

栅格、响应式断点、设计变量等数据来自 atomgit 上游仓库。**使用时通过 WebFetch 实时拉取最新版本**，不要 bundle 到本地：

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

本 skill 内 bundled 的索引文件（设计稿生产专用，无上游真源）：

- [references/pixso-mcp-adapter/component-keys.md](references/pixso-mcp-adapter/component-keys.md) — 536 个 UI 组件变体的 componentKey 索引
- [references/pixso-mcp-adapter/icon-keys.md](references/pixso-mcp-adapter/icon-keys.md) — 187 个图标的 componentKey 索引

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
| [ODropdown](components/dropdown.md) | 下拉菜单 | [OToggle](components/toggle.md) | 切换按钮 |
| [OInput](components/input.md) | 输入框 | | |
| [OLink](components/link.md) | 链接 | | |
| [OLoading](components/loading.md) | 加载 | | |
| [OMenu](components/menu.md) | 菜单 | | |
| [OSearch](components/search.md) | 搜索框 | | |

## 图标处理规范（核心约束）

> ⚠️ 本节规则优先级最高，所有生成步骤必须遵守。

### 图标存储机制

OpenEuler 设计系统中，图标以 `svgSha` 外部引用方式存储在 Pixso 服务器上（非内嵌 SVG 路径）。这意味着：

- 图标 SVG 数据**不存在于 DSL 中**，无法通过工具直接提取路径
- `code_to_design` 内嵌的任何 SVG 均为**通用占位符**，不属于设计系统图标
- `create_instance` 生成的组件实例**自带正确图标**，由 Pixso 在渲染时从服务器解析

### 图标库资源

OpenEuler 拥有独立图标库 Pixso 文件 `kbqInwBrCTGnM0MsPJDgvA`，包含 **187 个线性图标**（24×24px）。完整索引见 [references/pixso-mcp-adapter/icon-keys.md](references/pixso-mcp-adapter/icon-keys.md)。

### 图标使用规则

**SVG 来源优先级（所有场景统一）**：① `references/assets/` 已有文件（Read 内联）→ ② 手写符合线性风格的 SVG 路径 → ③ CSS（兜底，需注释）

**SVG 内嵌方式**：直接将 `<svg>` 标签写入 HTML，不使用 `<img src>`。图标颜色跟随设计系统 Token，默认使用 `currentColor` 继承父元素色值。

| 属性 | 规格 |
|------|------|
| 尺寸 | 与设计系统图标一致（通常 24×24px，使用 `icon_size-m` Token） |
| 风格 | 线性（stroke），与 OpenEuler 图标库保持一致 |
| 颜色 | `currentColor`（继承父元素文字色）或对应 Token 色值 |
| 写法 | `<svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" ...>` 直接内嵌 |

> `create_instance` 仍可用于需要 live Symbol 绑定或 Token 联动的特殊场景，但不再作为图标的强制路径。

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
   — 拉取失败时：使用 references/designer-guide.md 中的 Token 语义速查表作为降级数据源

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

### 第二步：读取设计规范（全局，仅执行一次）

```
使用 get_variable_sets 获取当前文件所有变量集合
使用 get_variables(variableSetId) 读取具体变量值（颜色/字号/间距等）
使用 get_local_styles 获取本地样式（填充色/描边/文字样式）
```

如需对照上游 Token 真源（语义化变量名、栅格断点），通过 WebFetch 拉取 [#数据资源](#数据资源) 中的三个 atomgit URL。

---

### 逐楼层循环（第三步 ~ 第六步，每个楼层执行一轮）

> 🔄 对第一步规划中的每个楼层，依次执行第三步到第六步。**每次只处理一个楼层**，完成验证后再进入下一个楼层。

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
2. 如有楼层模板（floors/{type}.md），读取模板获取完整规格
3. 检查图标资源：
   ls references/assets/<组件名>/
   有文件 → Read 每个文件，全部内联到 HTML 对应位置
   无文件 → 手写线性风格 SVG 路径，注释注明「assets 中无资源」
4. 独立图标：查 references/pixso-mcp-adapter/icon-keys.md 按中文名匹配
```

#### 第五步：生成本楼层

选择生成模式（全页统一，首次确定后后续楼层沿用）：

| 条件 | 选择 |
|------|------|
| 用户需要 Pixso 可编辑画布、Token 绑定或交互原型 | 模式 A |
| 用户需要可在浏览器直接打开的网站页面 / 未明确说明 | 模式 B（默认） |

**模式 A**：`code_to_design(htmlStr)` — 当前楼层的 HTML 片段，包含精确位置与像素级样式

**模式 B**：生成当前楼层的独立 HTML 片段（含内联 CSS），后续在第七步组装为完整页面

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

模式 A：get_node_dsl(itemId) + get_image(itemId) 验证
模式 B：在浏览器中打开 HTML 片段验证
```

验证不通过 → 修正后重验，**通过后才进入下一个楼层**。

---

### 第七步：组装所有楼层为完整页面

所有楼层验证通过后，按楼层顺序组装为完整页面：

```
导航楼层 → Banner 楼层 → 楼层 1 → 楼层 2 → … → 页脚楼层

楼层间距统一使用间距 10（PC 72px / MB 32px）
```

**模式 A**：将所有楼层 HTML 拼接为完整页面，通过 `code_to_design` 输出

**模式 B**：将所有楼层 HTML 片段组装为完整 HTML 文件，包含统一的 `<head>` / CSS Reset / Token 引入

### 第八步：最终验证

```
□ 页面结构是否符合：导航 → [Banner] → 楼层(s) → 页脚？
□ 楼层间距是否统一为间距 10（PC 72px / MB 32px）？
□ 全页组件组合是否无黑名单违规？
□ 整体视觉一致性检查
□ 【社区Token】:root 颜色是否来自目标社区主题 Token（对照第零步社区识别结论，禁止 openEuler 默认值混入）？
□ 【站点框架】路由A页面级增改场景：导航/页脚是否与社区线上实际结构一致（对照第零步抓取的站点框架基准）？
□ 【渲染验证】模式 B：组装完成后必须在浏览器中打开完整页面逐楼层目检——重点排查固定 width/height 容器内的长文案是否折行溢出堆叠（步骤条标题、卡片标题、按钮、标签），禁止仅靠静态代码检查放行
```

---

### 通用生成约束（所有楼层共用）

> 🚨 以下约束无论哪种模式、哪个楼层均强制生效，违反即违规：
>
> - **必须逐一读取**当前楼层涉及组件的规范文档 `components/{name}.md`
> - 合法的参数来源只有三个：① 对应组件规范文档 ② 硬约束白名单（第零步） ③ 上游 Token 真源
> - 三者之外的任何取值均属**自定义内容，一律禁止**，包括但不限于：近似色值、估算间距、推断圆角、猜测默认变体
> - 如果规范文档中找不到所需参数 → 停下，询问用户，**禁止自行填充**
> - 图标：直接内嵌 `<svg>` 路径，禁止色块占位符或 `<img src>` 引用
> - SVG 颜色：`currentColor` 继承父元素色值，或对应 Token CSS 变量
> - **位图素材（案例封面 / Banner 图等）禁止留占位色块**：用文生图 API 生成贴合内容语义的图片填充 `https://console.enterprise.trae.cn/api/ide/v1/text_to_image?prompt={prompt}&image_size={image_size}`（prompt 为 URL 编码的具体场景描述，按容器比例选 image_size，如 16:9 封面用 `landscape_16_9`；调性贴合目标社区主题色与开发者风格）；以 `<img>` + alt 填充，HTML 注释注明「文生图示意配图，上线前替换真实素材」；**生成后必须校验**：curl 检查重定向目标，落在 `default.jpeg` 属静默失败（prompt 触发内容审核），换 prompt 重试直至返回真实生成图；API 失败时才降级占位色块

## 楼层模板索引

每个楼层模板是一个**自包含的生成单元**——AI 读了这个文件就能精确生成该楼层，无需再拼凑多个组件规范。模板包含：布局结构、精确尺寸/间距/字号值（直接取自硬约束白名单）、PC 和 MB 双端规格。

| 楼层 | 模板文件 | 典型用途 |
|------|---------|---------|
| 导航 | [floors/navigation.md](floors/navigation.md) | 页面顶部导航栏（PC/Mb） |
| Banner | [floors/banner.md](floors/banner.md) | 首页/栏目页顶部横幅（XL/L/M） |
| 卡片栅格 | [floors/card-grid.md](floors/card-grid.md) | 功能展示、产品列表、特性介绍 |
| 图文特色 | [floors/feature-section.md](floors/feature-section.md) | 左图右文/左文右图特色介绍 |
| 页脚 | [floors/footer.md](floors/footer.md) | 站点地图、版权信息、社交入口 |

## 节点 ID 获取方式

- 从 Pixso 画布 URL 提取：`?item-id=1:2` → itemId 为 `1:2`
- 使用 `mcp__pixso-desktop__get_all_components` 或 `mcp__pixso-desktop__get_local_styles` 返回结果中查找对应 key/id

## 注意事项

- 当你准备使用某一个组件来生成设计稿时，你应该读取 opendesign-components 的 skill，来看他对应的组件开发 skill 中说明的传参是否足以支持你的想法
- 所有 `itemId` 格式为 `"数字:数字"`，例如 `"123:456"`
- 若未指定 itemId，工具默认操作当前 Pixso 画布中已选中的节点
- 导出图片时使用 `get_export_image` 并配置 exportSettings（PNG/SVG/PDF 等）
- **组件文档中所有色值、字号、间距必须匹配上游 Token 的变量名**，禁止使用硬编码值
- 编写组件文档时，参见 [global/component-doc-spec.md](global/component-doc-spec.md) 中的 Token 匹性要求和格式规范

## 异常与降级策略

| 异常场景 | 处理方式 |
|----------|----------|
| WebFetch 拉取上游 Token 失败（超时 / 404） | 使用 Pixso 本地 `get_variables` / `get_local_styles` 作为降级数据源，在输出中注明「Token 真源未拉取，使用本地变量」 |
| `components/{name}.md` 不存在 | 停下告知用户该组件暂无设计规范，询问是否继续（无规范 = 无合法参数来源，禁止自行推断） |
| componentKey 在索引中找不到匹配变体 | 使用 `get_all_components` 实时搜索组件库，按名称模糊匹配；仍无结果 → 停下询问用户 |
| 硬约束白名单无法覆盖需求值 | 停下告知用户该值不在合法取值集合内，给出最接近的白名单值供选择，禁止静默取近似值 |
| `references/assets/` 中无目标 SVG | 按图标使用规则降级：手写线性风格 SVG 路径，并在 HTML 注释中注明「assets 中无资源」 |
| Pixso MCP 工具调用失败（连接断开 / 权限不足） | 停下告知用户具体报错，建议检查 Pixso 桌面端连接状态；不可跳过该步骤继续生成 |

## componentKey 速查

> - 完整 536 个 UI 组件变体：[references/pixso-mcp-adapter/component-keys.md](references/pixso-mcp-adapter/component-keys.md)
> - 完整 187 个图标 componentKey：[references/pixso-mcp-adapter/icon-keys.md](references/pixso-mcp-adapter/icon-keys.md)

常用高频变体（Light 模式默认变体，已验证）：

| 组件 | 变体 | componentKey |
|------|------|-------------|
| ONavigation/顶部导航 | PC, Dark=off | `561d3014fbe6f361abfecc203a06f1ab6cad43af` |
| ONavigation/顶部导航 | PC, Dark=on | `c4522744b94335f4bc8fbc358c2316d3c447b771` |
| ONavigation/顶部导航 | Mb, Dark=off | `7675ede074069cf358a0f164f76e1c6c119a592f` |
| 导航 Navigation/底部导航 | PC | `5d66a734f21bfcce1fbe4fce1d79f832b7f08f02` |
| 导航 Navigation/底部导航 | Mb | `62383dfdd93fd29e6dfcb7def1861d22d005324a` |
| OBreadcrumb 面包屑 | Dark=off | `7838d75ba480e2c51fe0f741109ba6c9745a6f86` |
| 搜索框 Search | large, Enabled, Light | `504d78f315ae03447c67e6019f787e5656b4aa1a` |
| 搜索框 Search | medium, Enabled, Light | `bfb0631d6779aa79179e27e84534dd68acd2fa0e` |
| OButton 按钮 | solid, medium, Light | `eab0e6c545d27376ba67aef7dd233fc58c476234` |
| OButton 按钮 | outline, medium, Light | `3cd0c2f583c60180725e297d3529d67ed62fcad8` |
| OButton 按钮 | text, medium, Light | `2b717cf5c4d422ff4385f6f215d52fa4dcc1c04a` |
| OButton 按钮 | solid, large, Light | `1a36bbfd374263c7b4644d4490e8bb65450c2f57` |
| ODropdown 下拉菜单 | medium, solid, Light | `647e1f27461028010d0828c17a8dfb6f245bd449` |
| ODropdown 下拉菜单 | medium, outline, Light | `6c12a6d3766a2055cd8f300e001c589084b2decd` |
| ODropdown 下拉菜单 | medium, text, Light | `cdf3700ce2d84a8bcf81720f9d64a0c867909268` |
| OPagination 分页 | All, Dark=off | `35bb7a0ef9189834048e73d6832418da0796d993` |
| OPagination 分页 | Simple, Dark=off | `c8d2772ff9159d6bc99498f30ba24bb001039139` |
| OCard 卡片 | cover, vertical, Light | `a22044aaa494bba137bd3ebba74319449b998694` |
| OCard 卡片 | cover, horizontal, Light | `0fceafcdae43547862ffb167bdcce89f510c3c35` |
| OCard 卡片 | icon, vertical, Light | `6103b3265e752087a546bf1b77a0f1b5bfef888b` |
| OCard 卡片 | icon, horizontal, Light | `e2a10a804dc8cb9ab507d237c712f65c7cf12f5e` |

<!-- 以下内容仅为人类维护者参考，AI 执行时请忽略 -->
### 📎 人工参考附录
- 更新日志：[CHANGELOG.md](CHANGELOG.md)
- 项目说明与目录结构：[README.md](README.md)
- 组件文档编写规范：[global/component-doc-spec.md](global/component-doc-spec.md)