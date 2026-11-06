← [SKILL.md](../SKILL.md)

# 设计师使用指南

> 本文档面向**第一次使用 OpenDesign Skills 的设计师**，帮你快速理解工作流、避开常见坑、高效产出合规设计稿。

---

## 一、角色决策树

你是设计师，接到了高保真设计需求。先回答一个问题：

```
你的产出物是什么？
│
├─ "我要一个能在浏览器打开的 HTML 页面原型"
│   → 使用 opendesign-design skill（你正在这里）
│   → 生成的 HTML 遵循 CodeGen 工程规范（--o-* 语义 Token、BEM、响应式 Token）
│
└─ "我要直接出生产级 Vue 代码"
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

### 输入清单：第一轮消息给什么信息

还原度稳定的前提是把需求说清楚。第一轮消息里给足以下信息——意图级信息（①~④）缺失时 AI 会停下询问，与其被追问，不如一次给齐：

| 信息 | 必要性 | 缺了会怎样 |
|------|--------|-----------|
| ① 目标社区（openEuler / openGauss / openUBMC / 鲲鹏 / 昇腾…） | 必给 | AI 停下询问；用错社区 = 整页主题色错（最高优先级错误） |
| ② 目标设备（PC / MB；双端分两次生成） | 必给 | 规格表走错列，全页字号 / 间距 / 栅格错 |
| ③ 场景 + 站点地址：全新落地页，还是已有站点增改？增改给 URL | 必给 | 增改场景的导航 / 页脚无法按线上真实结构还原 |
| ④ 页面语言（zh / en / zh-en） | 必给（可默认 zh） | 文案语言不符预期 |
| ⑤ 楼层构成意图（顺序 + 每块放什么） | 越具体越稳 | AI 规划楼层时自行发挥——这是还原度参差的第一个入口 |
| ⑥ 每层内容素材（菜单项、slogan、按钮文字、卡片标题 / 描述、页脚信息） | 决定生成质量 | AI 禁止编造内容，缺了会在每层开工时被索要 |
| ⑦ 配图意图（场景 + 调性，如「数据库机房、紫色调、16:9」） | 建议给 | AI 按文案语义自行发挥配图 |
| ⑧ 每层验收点（如「每层 solid 按钮最多一个」） | 可选 | 走默认自检清单 |
| ⑨ 品牌红线（logo、禁用色、特殊约束） | 可选 | 走默认规范 |

**不用给的信息**：楼层类型、Banner 等级、栅格列数、组件选型——AI 规划时会自己映射并在需求单里请你确认；导航下拉面板样式 AI 会单独问你；一切像素数值（所有几何数值都来自规范白名单，你给的值反而可能不合法）。

**第一轮消息示例**（十几行自然语言即可，不需要任何 YAML 或术语）：

```
做一个 openGauss 社区的 PC 端新首页，中文。这是全新落地页，
导航和页脚参考现有站 www.opengauss.org 的结构。

大致结构：顶部导航 → 主视觉大图 → 三张核心能力卡片 → 页脚。

内容：
- Banner：slogan「高性能企业级开源数据库」，副标题「面向企业级场景的开源数据库」，
  按钮「立即下载」（实心）+「查看文档」（描边），配图要数据库机房、紫色调
- 三张卡片：
  1. 标题「高性能」 描述「……」
  2. 标题「高可用」 描述「……」
  3. 标题「高安全」 描述「……」
- 页脚按线上站的分组和版权信息

验收点：每层 solid 按钮最多一个。
```

### 确认流程：需求单与楼层工单

你用自然语言说需求，AI 会生成结构化草稿请你确认——你不需要学格式，会改、会批即可：

1. **页面需求单**：楼层规划完成后，AI 输出需求单草稿（含楼层类型、Banner 等级等），你确认或调整后进入逐层生成；
2. **楼层工单**：每层开工前，AI 输出本层工单（组件选型 + 内容素材 + 验收点），你确认 / 补充后该层开始生成；
3. **导航面板样式**：导航楼层有硬门禁，AI 会问下拉面板用平铺式还是分类平铺式，如实选择即可。

两种单据的完整格式见 [page-brief-template.md](page-brief-template.md)。熟练用户可以走**快捷通道**：会话首轮直接粘贴填好的页面需求单，AI 校验必填与枚举后跳过规划确认、直接进入逐楼层循环。

### ❌ 低质量方式（信息不足）

```
"帮我生成一个 openEuler 社区首页，包含导航、Banner、3个功能楼层和页脚"
```

没有社区 / 设备 / 场景 / 内容素材。AI 要么逐项停问，要么规划楼层时自行发挥——两种结果都不稳定。

### ✅ 高质量方式（信息充足 + 逐楼层）

```
第 1 轮：按「第一轮消息示例」给足信息
         → AI 输出页面需求单草稿 → 你确认或调整
第 2 轮："确认，先做导航楼层" → AI 问面板样式 → 你选择 → 生成导航
第 3 轮："继续 Banner 楼层" → AI 输出楼层工单 → 你确认 → 生成 Banner
第 4 轮："继续三张能力卡片" → AI 输出楼层工单 → 你确认 → 生成卡片
第 5 轮："做页脚" → …
第 6 轮："所有楼层确认完毕，组装完整页面" → AI 组装并做最终验证
```

每一步 AI 只需处理 1~2 个组件的规范，细节精准落地。

### 三个过程纪律

1. **逐楼层生成，别催一次性整页**——见第二节「最重要的一条规则」；
2. **AI 停下来确认时，认真回答**——面板样式二选一、组件确认这些停顿是对齐设计，回「都行 / 你看着办」等于放弃这次对齐机会；
3. **指出问题时说维度**——说「按钮颜色不对」，AI 会按「同类全查」机制复查该楼层所有组件的颜色 / 尺寸 / 圆角 / 变体 / 状态；只说「感觉不太对」，AI 只能猜。

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
| 导航下拉面板 · 平铺式 | [floors/navigation-dropdown.md](../floors/navigation-dropdown.md) | 顶导菜单项展开的平铺式下拉面板（**必须与导航楼层搭配生成**；全套导航统一 `.o-nav__*` 命名，无需映射） |
| 导航下拉面板 · 分类平铺式 | [floors/navigation-dropdown-categorized.md](../floors/navigation-dropdown-categorized.md) | 顶导菜单项展开的分类平铺式下拉面板（多列 + 列头 + 条目说明 + 快捷链接栏，**必须与导航楼层搭配生成**；与平铺式共享 `.o-nav__dropdown` 基类，可混挂同一顶导） |
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

以下是一个设计师从零开始做 openEuler 社区首页的完整交互流程（第一轮消息怎么写，见第三节「输入清单」）：

```
👤 设计师：用 opendesign-design 做一个 openEuler 社区的 PC 端新首页，中文，
          全新落地页。大致结构：顶部导航 → 主视觉 Banner → 功能介绍
          （3 列封面卡片）→ 数据统计 → 页脚。素材：Banner slogan
          「开源驱动数字基础设施」、按钮「立即下载」+「了解更多」、
          配图要数据中心机房、蓝色调；三张卡片文案如下：……（略）

🤖 AI：[读取 SKILL.md + hard-constraints.md，识别社区 openEuler，拉取 Token]
     [输出页面需求单草稿]
     # ========== 页面需求单 ==========
     meta:
       community: openEuler
       device: PC
       scenario: new-page
       language: zh
     floors:
       - id: nav
         type: navigation
         summary: 主导航，logo + 一级菜单 + 搜索
       - id: banner
         type: banner
         variant: XL
         summary: 首页主视觉，slogan + 双按钮
       - id: features
         type: card-grid
         columns: 3
         summary: 功能介绍——3 列封面卡片
       - id: stats
         type: card-grid
         columns: 4
         summary: 数据统计楼层——OCard icon 统计卡片栅格（规格见 components/card.md「组合搭配 · 统计卡片」）
       - id: footer
         type: footer
         summary: 站点地图 + 版权 + 友链
     请确认或调整楼层规划。

👤 设计师：确认，先做导航楼层

🤖 AI：[读取 floors/navigation.md + components/navigation.md]
     [生成顶导骨架 HTML]

🤖 AI：顶导骨架已完成。请选择下拉面板样式：
       ① 平铺式——垂直列表，选项独占一行
       ② 分类平铺式——多列 + 列头 + 条目说明 + 快捷链接栏

👤 设计师：选 ② 分类平铺式

🤖 AI：[读取 floors/navigation-dropdown-categorized.md + floors/navigation-dropdown.md]
     [一并生成全套导航：顶导 + 下拉面板]
     [自检通过]

👤 设计师：继续做 Banner 楼层

🤖 AI：[输出楼层工单草稿——组件选型 OBanner + OButton；内容素材：
        标题 / 副标题 / 双按钮文字 / 配图意图（从首轮素材带入）]

👤 设计师：确认

🤖 AI：[读取 floors/banner.md + components/banner.md + components/button.md]
     [生成 Banner 楼层 HTML]
     [自检通过]

👤 设计师：继续做功能介绍楼层

🤖 AI：[输出楼层工单草稿——组件选型 OCard（封面卡片，vertical）+ OTag；
        三张卡片标题 / 描述清单]

👤 设计师：第 2 张卡的描述改成「……」

🤖 AI：[更新工单 → 读取 floors/card-grid.md + components/card.md + components/tag.md]
     [生成卡片栅格楼层 HTML]
     [自检通过]

...（逐楼层继续）...

👤 设计师：所有楼层都完成了，组装完整页面

🤖 AI：[按楼层顺序组装，楼层间距统一 72px]
     [最终验证：结构、间距、组件组合]
     [输出完整 HTML]
```