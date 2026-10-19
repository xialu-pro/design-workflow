← [楼层模板索引](../SKILL.md#楼层模板索引) · [组件规范](../components/navigation.md)

# 页脚楼层模板

> 本模板为自包含生成单元。AI 读取本文件后即可精确生成页脚楼层，无需再拼凑多个组件规范。

---

## 涉及组件

| 组件 | 规范文档 | 本楼层使用的变体 |
|------|---------|----------------|
| ONavigation | [components/navigation.md](../components/navigation.md) | type=PC, 属性2=openEuler / type=Mb, 属性2=openEuler |

---

## 一、PC 页脚

### 布局结构

```
ONavigation Footer PC（1920px × 460px）
│  background: rgba(18,18,20,1)（始终深色）
│  padding: 24px 216px 34px 216px（内容区宽 1488px）
│
├── [顶部说明区] 居中, column, gap: 16px, margin-bottom: 32px
│   ├── [说明文字] 20px / 28px, Regular, white, 居中
│   └── [基金会 Logo] 160px × 32px, <img>
│
├── [导航列区] 6 列等宽, margin-bottom: 24px
│   ├── 关于openEuler（成员单位/组织架构/社区章程/贡献看板/社区介绍）
│   ├── 新闻与资讯（新闻/博客/白皮书）
│   ├── 获取与下载（获取openEuler操作系统/最新社区发行版/商业发行版/软件中心）
│   ├── 支持与服务（FAQ/联系我们/反馈问题）
│   ├── 互动与交流（邮件列表/活动/论坛）
│   └── 贡献与成长（SIG中心/贡献攻略/课程中心）
│
├── [友情链接区] margin-bottom: 24px
│   ├── [标题"友情链接"] 12px, UltraLight(200), white
│   └── [链接] 12px, white@0.6, 用 "/" 分隔
```

### ⚠️ 楼层容器 CSS 实现（必须严格遵守）

> 🔴 页脚楼层为全宽深色背景+内部内容区模式。**禁止使用 `margin: 0 auto` + `padding: 0 216px` 组合**（border-box下padding吃进max-width，内容区变窄）。

**PC端**：
```css
.footer-section {
    width: 100%;
    padding: 24px 0 34px 0;           /* 上下padding，左右为0 */
    background: rgba(18, 18, 20, 1);
}
/* 内层内容区 */
.footer-content {
    width: 1488px;                    /* 内容区固定宽度，撑满 */
    max-width: 1488px;                /* 禁止 1200px / 1140px / 960px */
    margin-left: 216px !important;    /* 固定左边距 */
    margin-right: 216px !important;   /* 固定右边距 */
    padding: 0 !important;            /* 禁止用padding控制楼层边距 */
}
```

**MB端**：
```css
.footer-section-mb {
    width: 100%;
    padding: 24px 0 34px 0;
    background: rgba(18, 18, 20, 1);
}
.footer-content-mb {
    width: 312px;
    max-width: 312px;                 /* 禁止 360px */
    margin-left: 24px !important;
    margin-right: 24px !important;
    padding: 0 !important;
}
```
│
├── [分隔线] 1487.91px × 2px, rgba(229,229,229) opacity 0.12
│
└── [底部信息行] 1487.91px × 78px, space-between
    ├── [Logo 区] 150px × 68px, <img>
    ├── [版权区] 居中, column
    │   ├── 品牌 | 隐私政策 | 法律声明（14px white）
    │   ├── 版权所有 © 2024 openEuler 保留一切权利（14px white@0.6）
    │   └── 遵循 木兰宽松许可证第2版（MulanPSL2）（14px white@0.6）
    └── [社交媒体区] 354px × 52px, <img>
```

### 精确数值速查

| 规格项 | 值 | 来源 |
|--------|---|------|
| 画布宽度 | 1920px | 硬约束 §4.1 |
| 页脚高度 | 460px | navigation.md |
| 内边距 | 24px 216px 34px 216px | navigation.md |
| 顶部说明文字 | 20px / 28px, Regular | 硬约束 §2（四级标题） |
| 导航列标题 | 20px / 28px, Regular, white | navigation.md |
| 导航链接 | 14px / 22px, Regular, white@0.6 | 硬约束 §2（提示文本1） |
| 友情链接标题 | 12px / 24px, UltraLight(200), white | navigation.md |
| 友情链接文字 | 12px / 18px, Regular, white@0.6 | 硬约束 §2（提示文本2） |
| 底部链接 | 14px / 22px, Regular, white | 硬约束 §2 |
| 版权声明 | 14px / 22px, Regular, white@0.6 | 硬约束 §2 |
| 分隔线 | 2px, rgba(229,229,229) opacity 0.12 | navigation.md |
| 底部信息行高度 | 78px | navigation.md |
| Logo 区 | 150px × 68px | navigation.md |
| 社交媒体区 | 354px × 52px | navigation.md |

### 资源引用

| 用途 | 路径 | 用法 |
|------|------|------|
| 基金会 Logo | `references/assets/navigation/footer-pc-基金会logo.svg` | `<img>` |
| 底部 Logo（含邮箱） | `references/assets/navigation/footer-pc-底部logo.svg` | `<img>` |
| 社交媒体 | `references/assets/navigation/footer-pc-社交媒体logo.svg` | `<img>` |

### 颜色

| 区域 | 颜色 |
|------|------|
| 背景 | rgba(18,18,20,1) |
| 说明/导航列标题 | rgba(255,255,255,1) |
| 导航链接/友情链接 | rgba(255,255,255,0.6) |
| 友情链接标题 | rgba(255,255,255,1) |
| 底部链接 | rgba(255,255,255,1) |
| 版权声明 | rgba(255,255,255,0.6) |
| 分隔线 | rgba(229,229,229) opacity 0.12 |
| 平台 Logo 背景 | rgba(43,43,47,1), borderRadius 4px |

---

## 二、MB 页脚

### 布局结构

```
ONavigation Footer Mb（360px × 458px）
│  background: rgba(18,18,18,1)
│
├── [顶部说明文字] 14px / 22px, Medium, white, 居中
├── [基金会 Logo] 150px × 30px, <img>, 居中
├── [分隔线] 312px × 1px, rgba(229,229,229) opacity 0.12
├── [底部链接行] 品牌 | 隐私政策 | 法律声明 | 服务状态, 12px white
├── [版权声明] 12px, white, 居中
├── [Logo GROUP] 86px × 34px, <img>, 居中
├── [QR 码区] 243px × 102px
│   ├── openEuler公众号 78px × 78px
│   └── openEuler小助手 78px × 78px
└── [平台 Logo 行] 232px × 49px
    第一行: OSCHINA / CSDN / 新土提金
    第二行: bilibili / 头条
```

### MB 端精确数值

| 规格项 | 值 | 来源 |
|--------|---|------|
| 画布宽度 | 360px | 硬约束 §4.1 |
| 页脚高度 | 458px | navigation.md |
| 顶部说明文字 | 14px / 22px, **Medium** | navigation.md（注意：字重与 PC 不同） |
| 底部链接 | 12px / 18px, white | 硬约束 §2 |
| 版权声明 | 12px / 18px, white | 硬约束 §2 |
| 联系邮箱 | 8px, white | navigation.md |
| QR 码尺寸 | 78px × 78px | navigation.md |
| 分隔线宽度 | 312px | 硬约束 §4.1（内容区宽度） |

### MB 端资源引用

| 用途 | 路径 | 用法 |
|------|------|------|
| 基金会 Logo | `references/assets/navigation/footer-mb-基金会logo.svg` | `<img>` |
| 底部 Logo | `references/assets/navigation/footer-mb-底部logo.svg` | `<img>` |
| 二维码 1（公众号） | `references/assets/navigation/footer-mb-二维码1.svg` | `<img>` |
| 二维码 2（小助手） | `references/assets/navigation/footer-mb-二维码2.svg` | `<img>` |
| 平台 Logo | `references/assets/navigation/footer-mb-平台logo.svg` | `<img>` |

---

## 三、常见错误清单

| # | 错误 | 正确 |
|---|------|------|
| 1 | 页脚 padding 顶部 40px | 顶部 **24px**，底部 34px |
| 2 | 顶部说明区放邮箱文字 | 放**基金会 Logo img**，邮箱在底部 Logo SVG 内 |
| 3 | 导航列标题 16px/weight 600 | **20px/weight 400**（Regular） |
| 4 | 列名含多余空格："关于 openEuler" | "关于openEuler"（无空格） |
| 5 | 省略友情链接区 | 分隔线前必须有**友情链接**区 |
| 6 | 底部 Logo 用自绘 SVG | `<img>` 引用 footer-pc-底部logo.svg |
| 7 | 版权顺序：版权文字在上，法律链接在下 | **品牌\|隐私政策\|法律声明在上**，版权文字在下 |
| 8 | MB 页脚背景与 PC 相同 | MB rgba(18,18,18,1) ≠ PC rgba(18,18,20,1) |
| 9 | MB 顶部说明字重 Regular | MB 为 **Medium**（与 PC 的 Regular 不同） |
| 10 | MB 页脚缺"服务状态"链接 | MB 比 PC **多**"服务状态"一项 |

---

## 四、楼层自检清单

```
□ 页脚高度是否为 PC 460px / MB 458px？
□ 内边距是否为 PC 24px 216px 34px 216px？
□ 背景色是否为 PC rgba(18,18,20,1) / MB rgba(18,18,18,1)？
□ 导航列标题是否为 20px / 28px, Regular（非 16px/600）？
□ 导航链接是否为 14px / 22px, white@0.6？
□ 友情链接区是否未遗漏？
□ 分隔线是否为 2px（PC）/ 1px（MB），rgba(229,229,229) opacity 0.12？
□ 底部 Logo 是否用 <img> 引用 SVG（禁止自绘）？
□ 版权顺序是否为：品牌|隐私|法律（上）→ 版权文字（下）？
□ MB 端是否多"服务状态"链接？
□ MB 端顶部说明字重是否为 Medium（非 Regular）？
□ 所有图形资源是否用 <img> 引用（禁止自绘 SVG）？
```