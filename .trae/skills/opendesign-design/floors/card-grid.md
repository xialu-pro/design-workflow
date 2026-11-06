← [楼层模板索引](../SKILL.md#楼层模板索引) · [组件规范](../components/card.md)

# 卡片栅格楼层模板

> 本模板为自包含生成单元。AI 读取本文件后即可精确生成卡片栅格楼层，无需再拼凑多个组件规范。这是落地页中最常见的楼层类型。

---

## 涉及组件

| 组件 | 规范文档 | 本楼层使用的变体 |
|------|---------|----------------|
| OCard | [components/card.md](../components/card.md) | type=cover, layout=vertical 或 type=icon, layout=vertical |
| OButton | [components/button.md](../components/button.md) | size=medium, variant=text（卡片 Footer） |
| OTag | [components/tag.md](../components/tag.md) | fill/stroke × small/medium（状态标记/分类标签） |

---

## 一、楼层结构

```
楼层容器（1920px 宽 / 360px 宽）
│  内容区: PC 1488px（距两边 216px）/ MB 312px（距两边 24px）
│
├── [楼层标题]（必选）
│   水平对齐: 居中对齐（页面级规范，硬约束 §8.3）
│   字号: PC 40px/56px / MB 18px/26px, 字重: SemiBold
│
├── [楼层标题描述]（可选）
│   水平对齐: 居中对齐（跟随标题）
│   字号: PC 16px/24px / MB 14px/22px, 字重: Regular
│   颜色: --o-color-info3
│
├── [标题到内容间距] 间距 7: PC 40px / MB 12px
│
└── [卡片栅格区]
    ├── PC: 24 列栅格，卡片间距 32px
    └── MB: 4 列栅格，卡片间距 12px
```

### ⚠️ 楼层容器 CSS 实现（必须严格遵守）

> 🔴 生成HTML时，楼层内容容器**必须**使用以下CSS，禁止使用1200px/1140px等框架默认值。**禁止使用 `margin: 0 auto` + `padding: 0 216px` 组合**（border-box下padding吃进max-width，内容区变窄）。

**PC端**：
```css
.floor-card-grid {
    width: 1488px;                    /* 内容区固定宽度，撑满 */
    max-width: 1488px;                /* 禁止 1200px / 1140px / 960px */
    margin-left: 216px !important;    /* 固定左边距 */
    margin-right: 216px !important;   /* 固定右边距 */
    padding: 0 !important;            /* 禁止用padding控制楼层边距 */
}
```

**MB端**：
```css
.floor-card-grid-mb {
    width: 312px;
    max-width: 312px;                 /* 禁止 360px（那是画布宽度） */
    margin-left: 24px !important;
    margin-right: 24px !important;
    padding: 0 !important;
}
```

---

## 二、PC 端卡片栅格布局

### 常见列数配置

| 列数 | 每张卡片占栅格 | 卡片宽度计算 | 适用场景 |
|------|-------------|------------|---------|
| 3 列 | 8 列/卡 | (1488 - 32×2) / 3 ≈ 474.67px | 产品展示、特性介绍 |
| 4 列 | 6 列/卡 | (1488 - 32×3) / 4 = 348px | 功能入口、图标卡片 |
| 2 列 | 12 列/卡 | (1488 - 32) / 2 = 728px | 图文特色、大卡片 |

> 卡片宽度 = (内容区宽度 - 间距 × (列数-1)) / 列数

### 封面卡片（cover + vertical）内部结构

```
OCard（自适应宽高）
│  cornerRadius: var(--o-radius_control-m)（openEuler 4px / Ascend 12px——按目标社区 Token，2026-10-29 修正写死值）
│  fill: --o-color-fill2
│  padding: 8px
│
├── [封面图片] aspect-ratio: 16/9, 宽度 100%
│
├── [Body 文字区] padding: 0 16px 16px
│   ├── [标题] 22px / 30px, SemiBold, --o-color-info1
│   ├── [描述] 16px / 24px, Regular, --o-color-info3
│   └── [Tag 组]（可选）OTag stroke small
│
├── [间距] 24px
│
└── [Footer]（可选，多跳转时才显示）
    padding: 0 8px 8px
    按钮左对齐, OButton medium text
    顺序: solid → outline → text（solid 有且仅 1 个）
```

### 图标卡片（icon + vertical）内部结构

```
OCard（自适应宽高）
│  cornerRadius: var(--o-radius_control-m)（openEuler 4px / Ascend 12px——按目标社区 Token）
│  fill: --o-color-fill2
│  padding 与内部间距：按 card.md「icon 变体五档分档表」取值（2026-10-23 修订口径：
│    容器 padding 五档 24/24/16/12/12，图标→标题 12px 不分档、标题→描述 8px 分段 margin 直读，
│    禁止容器统一 gap——旧文「padding: 24px 32px / gap: 16px」为已废止口径，2026-10-29 修正）
│
├── [图标容器] 32px × 32px（≤1200px 档 24px）, 圆角 --o-radius_control-xs
│   fill: --o-color-primary1-light
│   └── [图标] fill: --o-color-primary1
│
├── [标题] 22px / 30px, SemiBold, --o-color-info1
│
├── [描述] 16px / 24px, Regular, --o-color-info3
│
└── [Footer]（可选）
    按钮左对齐, OButton medium text
```

---

## 三、MB 端卡片栅格布局

### 列数配置

| 列数 | 每张卡片占栅格 | 卡片宽度计算 | 适用场景 |
|------|-------------|------------|---------|
| 1 列 | 4 列/卡 | 312px | 封面卡片、图文卡片 |
| 2 列 | 2 列/卡 | (312 - 12) / 2 = 150px | 图标卡片、功能入口 |

### MB 端字号映射

| PC 字号/行高 | MB 字号/行高 | 角色 |
|-------------|------------|------|
| 22px / 30px | 16px / 24px | 卡片标题 |
| 16px / 24px | 14px / 22px | 卡片描述 |
| 14px / 22px | 12px / 18px | Footer 按钮文字 |

---

## 四、交互状态

| 卡片类型 | 默认 | 悬浮/按下 |
|---------|------|---------|
| 封面卡片 | 标题 --o-color-info1 | 标题 → --o-color-primary1, 封面图 scale(1.05) |
| 图标卡片 | 标题 --o-color-info1 | 标题 → --o-color-primary1 |
| 复合卡片（含按钮） | 按钮 outline + primary1 文字 | 主按钮 → primary1 填充 + 白字 |

> 封面图 hover 放大：容器 overflow:hidden 不变，仅内部图片 transform:scale(1.05)

---

## 五、Tag 位置规则

| Tag 类型 | 位置 | 实现方式 |
|---------|------|---------|
| 状态标记（HOT/NEW） | 卡片右上角 | position:absolute; top:12px; right:12px; z-index:1 |
| 分类标签 | 内容区标题下方 | 嵌入 card-body，flex 横排 |

---

## 六、楼层自检清单

```
□ 楼层标题字号是否为 PC 40px/56px / MB 18px/26px？
□ 楼层标题（及描述）是否水平居中对齐（Banner 以下楼层）？
□ 标题到内容间距是否为间距 7（PC 40px / MB 12px）？
□ 卡片间距是否为 PC 32px / MB 12px？
□ 卡片圆角是否为目标社区 --o-radius_control-m 值（openEuler 4px / Ascend 12px）？
□ 封面图比例是否为 16:9（禁止竖图）？
□ Footer 按钮是否左对齐？
□ Footer 按钮是否为 OButton medium text 变体？
□ Solid 按钮是否最多 1 个？
□ 按钮间距是否为 24px？
□ 状态 Tag 是否在右上角？
□ 所有间距是否来自硬约束白名单？
□ 所有字号/行高是否成对且来自白名单？
```

---

## 七、生成前必读（SKILL.md 第四步要求，2026-10-29 验证轮补齐）

| 必读项 | 文件 | 说明 |
|---|---|---|
| 卡片内部样式唯一真源 | [components/card.md](../components/card.md) | 封面 / 图标 / 文字 / Footer 的全部内部规格以 card.md 为准，本模板只定义楼层级架构 |
| icon 变体分档表 | card.md「icon 变体」 | 容器 padding 与内部间距的五档取值（本模板 §二 旧固定值口径已废止） |
| 楼层容器四件套 | 本模板 §一 | PC 1488/216/padding 0 · MB 312/24/padding 0 |
| Footer / Tag（可选） | [components/button.md](../components/button.md) · [components/tag.md](../components/tag.md) | 卡片 Footer 按钮与 Tag 的规格 |

## 八、已确认清单（用户历轮确认过的规格，优先级高于本模板正文旧值）

| # | 规格 | 确认轮次 |
|---|------|---------|
| 1 | 卡片圆角按目标社区 `--o-radius_control-m`（openEuler 4px / Ascend 12px），不写死 4px | 2026-10-29 验证轮（features 楼层裁定） |
| 2 | 图标卡容器 padding / 内部间距按 card.md 五档分档表，禁止容器统一 gap | 2026-10-23 card.md 修订（本模板 §二 旧口径同步废止，2026-10-29 修正） |
| 3 | 3 列卡片的卡片标题取 h3（22px/30px，Semibold 600）——hard-constraints §2「三级标题（≤ 3 列）」 | 2026-10-29 验证轮（原「< 3 列」空档修正） |
| 4 | 楼层标题颜色规范未定义时取 `--o-color-info1`（推断值，产物标注「需确认」） | 2026-10-29 验证轮 |

## 九、冲突声明

- 本模板与 [components/card.md](../components/card.md) 冲突时：**卡片内部样式以 card.md 为准**（本模板曾滞后于 card.md 2026-10-23 修订，2026-10-29 已同步；再遇冲突按 SKILL.md「规则冲突裁决」停下报告）
- 本模板与 [global/hard-constraints.md](../global/hard-constraints.md) 冲突时：按 SKILL.md「规则冲突裁决」执行——hard-constraints 优先，发现冲突停下向用户报告
- 旧文「卡片圆角 4px」「icon 卡 padding: 24px 32px / gap: 16px」为已废止口径（2026-10-29 修正），禁止按旧稿落笔

## 十、踩坑记录（已验证错误，禁止再犯）

| # | 坑 | 禁令 |
|---|---|---|
| 1 | 模板滞后：card.md 已废止的旧口径（统一 padding / gap）在本模板残留，生成侧按旧稿落笔 | 生成前对照 card.md 分档表，模板旧值一律不采信 |
| 2 | 圆角写死 4px，Ascend / Mindspore 等递进圆角社区生成错误圆角 | 圆角一律走目标社区 `--o-radius_control-m` |
| 3 | 3 列卡片的标题档位在 §2 表找不到（原「< 3 列」未覆盖），生成侧自行降档到四级标题 | 3 列取 h3 22px/30px（已确认清单 #3） |