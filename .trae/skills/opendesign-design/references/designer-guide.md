← [SKILL.md](../SKILL.md)

# 设计师使用指南

> 本文档面向**第一次使用 OpenDesign Skills 的设计师**，帮你快速理解工作流、避开常见坑、高效产出合规设计稿。

---

## 一、角色决策树

你是设计师，接到了高保真设计需求。先回答一个问题：

```
你的产出物是什么？
│
├─ "我要在 Pixso 里做可编辑的设计稿"
│   → 使用 opendesign-design skill（你正在这里）
│   → 需要 Pixso 桌面端 + MCP 连接
│
├─ "我要一个能在浏览器打开的网页原型"
│   → 使用 opendesign-design skill，选模式 B（HTML 网站）
│
└─ "我要直接出生产级代码"
    → 使用 opendesign-codegen skill
    → 需要目标仓库已使用 @opensig/opendesign
```

---

## 二、⚠️ 最重要的一条规则

**逐楼层生成，不要一次性生成整页。**

| 方式 | 还原度 | 原因 |
|------|--------|------|
| 一次性生成整页 | ❌ 低 | AI 上下文溢出，声称读了所有文件但细节丢失 |
| 逐楼层生成 | ✅ 高 | AI 每次只处理 1~2 个组件规范，细节精准落地 |

---

## 三、推荐交互方式

### ❌ 低质量方式（容易漏细节）

```
"帮我生成一个 openEuler 社区首页，包含导航、Banner、3个功能楼层和页脚"
```

AI 会声称读了所有文件，但一次性处理太多规范，细节必然丢失。

### ✅ 高质量方式（逐楼层）

```
第 1 轮："我要做一个 openEuler 社区首页，先帮我规划楼层结构"
第 2 轮："先做导航楼层"
第 3 轮："再做 Banner 楼层，等级 XL"
第 4 轮："做第一个功能楼层——3列封面卡片栅格"
第 5 轮："做页脚"
第 6 轮："把所有楼层组装成完整页面"
```

每一步 AI 只需处理 1~2 个组件的规范，细节精准落地。

---

## 四、页面结构模板

所有落地页都遵循固定结构（不可调换顺序）：

```
导航楼层（必选）→ [Banner 楼层]（可选）→ 楼层 1 → 楼层 2 → … → 页脚楼层（必选）
```

楼层间距统一为：**PC 72px / MB 32px**。

---

## 五、设计规范速览卡（2 分钟版）

### 你最常需要的 5 个规则

1. **画布尺寸**：PC 1920px / MB 360px，内容区 PC 1488px / MB 312px
2. **楼层间距**：PC 72px / MB 32px（间距 10）
3. **卡片间距**：PC 32px / MB 12px
4. **按钮**：左对齐，solid→outline→text 排列，间距 24px，solid 最多 1 个
5. **字号必须从白名单选**：PC {56,48,40,32,24,22,20,18,16,14,12}，MB {22,20,18,16,14,12,10}

### 最常违反的 3 条规则

❌ 用了不在白名单里的间距（如 20px、36px）
❌ 字号和行高没有成对使用（如 32px 配了 42px 行高）
❌ 按钮没有左对齐或 solid 按钮超过 1 个

### PC 端字号/行高完整对照

| 角色 | 字号 / 行高 |
|------|------------|
| 一级数据展示 | 56px / 80px |
| 二级数据展示 | 48px / 64px |
| 楼层标题 | 40px / 56px |
| 一级标题 | 32px / 44px |
| 二级标题 | 24px / 32px |
| 三级标题 | 22px / 30px |
| 四级标题 | 20px / 28px |
| 大号正文 | 18px / 26px |
| 常规正文 | 16px / 24px |
| 提示文本 1 | 14px / 22px |
| 提示文本 2 | 12px / 18px |

### PC 端间距完整对照

| Token | 值 | 典型用途 |
|-------|---|---------|
| 间距 1 | 4px | 微间距 |
| 间距 2 | 8px | 紧凑间距 |
| 间距 3 | 12px | 小间距 |
| 间距 4 | 16px | 基础间距 |
| 间距 5 | 24px | 组件左右间距 |
| 间距 6 | 32px | 组件上下间距 / 卡片间距 |
| 间距 7 | 40px | 楼层标题到内容 |
| 间距 8 | 48px | 大间距 |
| 间距 9 | 64px | 超大间距 |
| 间距 10 | 72px | 楼层之间 |

---

## 六、楼层模板速查

每个楼层模板是一个**自包含的生成单元**——告诉 AI "用 XX 楼层模板"，它就能精确生成。

| 楼层 | 模板文件 | 典型用途 |
|------|---------|---------|
| 导航 | [floors/navigation.md](../floors/navigation.md) | 页面顶部导航栏 |
| Banner | [floors/banner.md](../floors/banner.md) | 首页/栏目页顶部横幅 |
| 卡片栅格 | [floors/card-grid.md](../floors/card-grid.md) | 功能展示、产品列表、特性介绍 |
| 图文特色 | [floors/feature-section.md](../floors/feature-section.md) | 左图右文/左文右图特色介绍 |
| 页脚 | [floors/footer.md](../floors/footer.md) | 站点地图、版权信息、社交入口 |

---

## 七、设计意图→组件速查

| 我要画... | 用这个组件 | 规范文档 |
|----------|-----------|---------|
| 可点击的按钮 | OButton | [components/button.md](../components/button.md) |
| 信息卡片（有封面图） | OCard (cover) | [components/card.md](../components/card.md) |
| 功能卡片（有图标） | OCard (icon) | [components/card.md](../components/card.md) |
| 页面顶部导航栏 | ONavigation (Header) | [components/navigation.md](../components/navigation.md) |
| 页面底部页脚 | ONavigation (Footer) | [components/navigation.md](../components/navigation.md) |
| 首页大横幅 | OBanner (XL) | [components/banner.md](../components/banner.md) |
| 栏目页横幅 | OBanner (L/M) | [components/banner.md](../components/banner.md) |
| 状态标签（HOT/NEW） | OTag (fill) | [components/tag.md](../components/tag.md) |
| 分类标签 | OTag (stroke) | [components/tag.md](../components/tag.md) |
| 分页控件 | OPagination | [components/pagination.md](../components/pagination.md) |
| 标签页切换 | OTab | [components/tab.md](../components/tab.md) |
| 面包屑导航 | OBreadcrumb | [components/breadcrumb.md](../components/breadcrumb.md) |
| 输入框 | OInput | [components/input.md](../components/input.md) |
| 下拉选择 | OSelect | [components/select.md](../components/select.md) |
| 开关 | OSwitch | [components/switch.md](../components/switch.md) |
| 数据表格 | ODataTable | [components/data-table.md](../components/data-table.md) |

---

## 八、Token 语义速查（设计师版）

| 我想要... | 用这个 Token | 值（openEuler Light） |
|----------|-------------|---------------------|
| 品牌主色 | --o-color-primary1 | #002FA7 |
| 一级文字/标题色 | --o-color-info1 | #000000 |
| 正文色 | --o-color-info2 | rgba(0,0,0,0.8) |
| 辅助文字色 | --o-color-info3 | rgba(0,0,0,0.6) |
| 禁用文字色 | --o-color-info4 | rgba(0,0,0,0.4) |
| 页面背景色 | --o-color-fill1 | rgb(245,245,245) |
| 卡片/容器背景色 | --o-color-fill2 | rgb(255,255,255) |
| 链接色 | --o-color-link1 | #002FA7 |
| 成功色 | --o-color-success1 | #00A858 |
| 警告色 | --o-color-warning1 | #E69B00 |
| 危险色 | --o-color-danger1 | #CF2E2E |

---

## 九、设计师 QA 清单

### 必查项（5 个）

□ 字号是否全部来自白名单？（PC: {56,48,40,32,24,22,20,18,16,14,12}）
□ 字号和行高是否成对？（如 PC 32px 必须配 44px 行高）
□ 间距是否全部来自白名单？（PC: {4,8,12,16,24,32,40,48,64,72}）
□ 楼层间距是否为间距 10？（PC 72px / MB 32px）
□ 按钮是否左对齐、solid 唯一、间距 24px？

### 常查项（3 个）

□ 卡片间距是否为 PC 32px / MB 12px？
□ 画布宽度是否为 PC 1920px / MB 360px？
□ 是否有不在白名单的"看起来合理"的数值？

---

## 十、完整工作流示例

以下是一个设计师从零开始做 openEuler 社区首页的完整交互流程：

```
👤 设计师：我要用 opendesign-design 做一个 openEuler 社区首页

🤖 AI：[读取 SKILL.md + hard-constraints.md]
     我理解本次页面由以下楼层组成，请确认或调整：
       1. 导航楼层（ONavigation Header）
       2. Banner 楼层（XL 等级）
       3. 功能介绍楼层（3 列封面卡片栅格）
       4. 数据统计楼层（4 列图标卡片栅格）
       5. 页脚楼层（ONavigation Footer）

👤 设计师：确认，先做导航楼层

🤖 AI：[读取 floors/navigation.md + components/navigation.md]
     [生成导航楼层 HTML]
     [自检通过]

👤 设计师：继续做 Banner 楼层

🤖 AI：[读取 floors/banner.md + components/banner.md]
     [生成 Banner 楼层 HTML]
     [自检通过]

👤 设计师：继续做功能介绍楼层

🤖 AI：[读取 floors/card-grid.md + components/card.md + components/tag.md]
     [生成卡片栅格楼层 HTML]
     [自检通过]

...（逐楼层继续）...

👤 设计师：所有楼层都完成了，组装完整页面

🤖 AI：[按楼层顺序组装，楼层间距统一 72px]
     [最终验证：结构、间距、组件组合]
     [输出完整 HTML]
```