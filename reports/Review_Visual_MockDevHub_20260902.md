# 视觉还原度检视报告：MockDevHub

> 交付检视器（delivery-reviewer）产出 · 视觉还原度检视为必做项

## 1. 概览

| 项 | 内容 |
|----|------|
| 检视对象 | 测试环境页面 `mock/testenv_MockDevHub_20260902.html` |
| 比对基准 | DEMO 设计稿 `design/prototype_MockDevHub_20260902.html` |
| 访问方式 | 本地文件静态走查（dry run：读取源码比对样式规则与 DOM 结构，未产生浏览器截图） |
| 检视范围 | 单页 7 楼层：导航 / 搜索区 / 标签筛选 / 工具列表工具栏 / 卡片网格 / 页脚 / 提交对话框 |
| 问题统计 | **P0 ×1（波及 4 处）、P1 ×3、P2 ×3** |

## 2. 逐维度检视表

| # | 维度 | 结论 | 说明 |
|---|------|------|------|
| 1 | 布局结构 | ✅ | 楼层顺序、栅格分栏（3 列卡片）、断点行为与 DEMO 一致 |
| 2 | 组件规格 | ❌ P1 | 卡片 hover 状态缺失（见 V-04） |
| 3 | 间距节奏 | ❌ P1 | 网格间距与主区间距被压缩，组内/组外节奏被破坏（见 V-05） |
| 4 | 字号层级 | ✅ | 字号阶梯 13/14/16/18/20 与 DEMO 一致，≤ 4 级 |
| 5 | 色彩 | ❌ P2 | 标签激活色微差（见 V-08）；正文对比度达标 |
| 6 | 图标与图片 | ❌ P0 | 4 处黑色图标直出深色/品牌色背景（见 V-01） |
| 7 | 视觉细节 | ❌ P1 ×2 | 导航垂直居中被破坏（V-02）；提交按钮文本截断（V-03） |

## 3. 问题清单

### P0（必须修复）

| 编号 | 位置 | 描述 | 证据 | 修复建议 | 对应 DEMO 模块 |
|------|------|------|------|---------|---------------|
| V-01 | 导航 Logo / 搜索按钮 / 提交按钮 / 页脚 | **黑色图标直出深色/品牌色背景**：测试环境全文件 0 处 `filter:invert`，DEMO 的 4 条反色规则整体丢失——① Logo 图标黑底黑图（L54）；② 搜索按钮深色底 #1D2129 内黑图标（L67）；③ 提交按钮品牌蓝 #0052D9 内黑图标（L81）；④ 页脚深色底黑图标（L89） | 证据 E-01~E-04；DEMO 基准 E-05 | 恢复 DEMO 中 4 条规则：`.logo svg / .search-bar button svg / .btn-submit svg / footer svg { filter: invert(1); }` | 导航 / 搜索区 / 工具栏 / 页脚 |

### P1（必须修复）

| 编号 | 位置 | 描述 | 证据 | 修复建议 |
|------|------|------|------|---------|
| V-02 | 导航 | **Logo 与导航文字未垂直居中**：`header` 使用 `align-items: flex-end`，`.logo` 用 `padding-bottom:10px`、`nav a` 用 `align-items:flex-end + padding:0 16px 14px` 模拟贴底——即 DEMO 明令禁止的 flex-end + padding hack，box-sizing 下高度计算失真导致错位 | E-06（L12/13/15）vs DEMO E-07 | header 改 `align-items:center`，去掉子项 padding hack；导航项 `height:100% + border-bottom:2px` 实现选中下划线贴底 |
| V-03 | 工具栏"提交工具"按钮 | **文本截断**：`width:88px; padding:0 8px; overflow:hidden; white-space:nowrap`，图标+文字总宽超出 88px，"提交工具"文字被裁切（用户偏好红线：操作按钮不得截断） | E-08（L27、L80-82） | 移除固定 width 与 overflow，改用 DEMO 的 `padding:0 20px` 自适应 |
| V-04 | 卡片网格 | **卡片 hover 状态缺失**：DEMO 有 `.card:hover`（品牌色描边 + 阴影），测试环境无此规则，交互状态不完整 | E-09（L29 无 hover 规则） | 补 `.card:hover { border-color:#0052D9; box-shadow:0 4px 12px rgba(0,82,217,.08); }` |
| V-05 | 卡片网格 / 主区 | **间距节奏破坏**：网格 `gap:12px`（DEMO 24px）、主区 `padding-top:24px`（DEMO 32px）、搜索区 `margin-bottom:16px`（DEMO 24px）——组间间距被压缩至接近组内间距，亲密性层级丢失 | E-10（L17/18/28） | 三处恢复 DEMO 值：gap 24px / padding-top 32px / margin-bottom 24px |

### P2 备忘（记录不修复）

| 编号 | 位置 | 描述 | 证据 |
|------|------|------|------|
| V-06 | 全局滚动条 | 自定义 webkit 滚动条样式，DEMO 无此实现（合理实现差异） | L9-10 |
| V-07 | 卡片圆角 | 6px vs DEMO 8px，目视难辨 | L29 |
| V-08 | 标签激活色 | #165DFF vs DEMO #0052D9，色相差细微且均达标 | L23 |

## 4. 修复复检记录

P0/P1 修复后按编号逐项复检（只复检修改影响范围），更新于此。

| 问题编号 | 复检日期 | 结果 | 备注 |
|---------|---------|------|------|
| V-01 (P0) | 2026-09-02 | ✅ 复检通过 | 4 条反色规则全部恢复（R-01：L14/L20/L30/L38），Logo/搜索按钮/提交按钮/页脚图标均为白色 |
| V-02 (P1) | 2026-09-02 | ✅ 复检通过 | header 改 `align-items:center`（R-02：L12），padding hack 全部移除，导航项 `height:100% + border-bottom` 贴底下划线保留 |
| V-03 (P1) | 2026-09-02 | ✅ 复检通过 | 固定 width/overflow 移除，改 `padding:0 20px` 自适应（R-03：L29），"提交工具"文字完整显示 |
| V-04 (P1) | 2026-09-02 | ✅ 复检通过 | `.card:hover` 规则已补（R-04：L33），品牌色描边 + 阴影与 DEMO 一致 |
| V-05 (P1) | 2026-09-02 | ✅ 复检通过 | 三处间距恢复 DEMO 值（R-05：gap 24px / padding-top 32px / margin-bottom 24px） |

**复检结论：P0 ×1、P1 ×4 全部关闭，视觉还原度检视完成。** 测试环境实现与 DEMO 交付物一致，功能检视（functional-reviewer）前置条件已满足。

## 5. 风险与声明

- **dry run 限制**：本次为本地演练，访问方式为源码静态走查而非浏览器截图实测；真实检视中 P0/P1 判定须附页面截图证据
- 视觉高频问题（深色背景图标、导航垂直居中、元素截断）三项全部命中，符合"历史高频问题必查"预期
- 未覆盖项：无（单页全楼层已覆盖）

---

*证据索引（测试环境 `mock/testenv_MockDevHub_20260902.html`，标注 L 行号；DEMO `design/prototype_MockDevHub_20260902.html`）*

- E-01：L54 Logo SVG `fill="#000"`，深色 header 内
- E-02：L67 搜索按钮内 SVG `fill="#000"`，按钮底色 #1D2129（L20）
- E-03：L81 提交按钮内 SVG `stroke="#000"`，按钮底色 #0052D9（L27）
- E-04：L89 页脚 SVG `fill="#000"`，页脚底色 #1D2129（L33）
- E-05：DEMO 具备 4 条 `filter:invert(1)` 规则；测试环境全文件 grep `filter:invert` 0 命中
- E-06：L12 `header{align-items:flex-end}`、L13 `.logo{padding-bottom:10px}`、L15 `nav a{align-items:flex-end; padding:0 16px 14px}`
- E-07：DEMO L14-18 header 用 `align-items:center`，无 padding hack
- E-08：L27 `.btn-submit{width:88px;overflow:hidden;white-space:nowrap}`；L80-82 按钮含图标+"提交工具"文字
- E-09：L29 `.card` 规则无 :hover；DEMO L52-53 有 `.card:hover`
- E-10：L28 `gap:12px`、L17 `padding:24px`、L18 `margin-bottom:16px`
