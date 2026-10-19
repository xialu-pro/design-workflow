# Changelog

本文件记录 `opendesign-design` skill 的变更，供使用者判断是否需要重新安装。`SKILL.md` frontmatter 的 `last_update` 字段与本文件中最新条目日期对应。

分类：新增 / 更新 / 修正 / 移除 / ⚠️ 破坏性（破坏性表示按旧版 skill 已生成的产物失效或违规，需复核）。

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