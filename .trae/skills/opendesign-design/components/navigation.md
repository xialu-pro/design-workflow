> ← [组件索引](../SKILL.md#组件索引) · [README](../README.md)

# ONavigation 导航 · 设计 Skill

> 组件集合节点：`1303:10353` · 组件名：ONavigation 导航 · 变体总数：6

---

## Part A：设计使用卡

### 组件概览

**ONavigation 导航**：openEuler 官网全局导航组件，分为**顶部导航栏**（Header）和**底部页脚**（Footer）两类。

> 🧩 **卡槽（Slot）容器设计**
>
> 顶部导航栏采用**卡槽容器**设计模式——整体作为一个容器，通过插槽（Slot）组合不同子组件：
>
> | 插槽区域 | 组件引用 | 说明 |
> |---------|---------|------|
> | 左侧 Logo 区 | 占位插槽 | 默认使用 openEuler Logo，可由调用方传入自定义 SVG/PNG 替换（详见 [Part F](#part-flogo-自定义仅顶导)） |
> | 中间菜单区 | [OTab 标签页](./tab.md) | 使用 OTab `text` 变体（medium 尺寸），菜单项的样式、交互状态（默认/Hover/Active）均遵循 OTab 规则 |
> | 右侧操作区 · 搜索框 | [OSearch 搜索框](./search.md) | 使用 OSearch `small` 尺寸变体，搜索框的样式、交互（Focus/清除/建议面板）均遵循 OSearch 规则 |
> | 右侧操作区 · 源码下拉 | [ODropdown 下拉菜单](./dropdown.md) | 使用 ODropdown `small` 尺寸 `text` 变体，无背景无描边，下拉触发与面板交互均遵循 ODropdown 规则 |
> | 右侧操作区 · 其他图标 | 内置图标插槽 | 国际化、主题切换、用户头像等为独立图标插槽，通过 CSS mask 着色适配主题 |

顶部导航支持 PC 端宽屏和移动端两种布局，均提供 Light/Dark 主题切换；底部页脚为深色通栏样式，包含站点地图、版权信息和社交媒体入口。

---

### 适用场景

- ✅ **PC 顶导**：1920px 宽屏页面的全局顶部导航，卡槽容器组合 Logo 插槽 + [OTab](./tab.md) 菜单 + [OSearch](./search.md) 搜索 + [ODropdown](./dropdown.md) 源码下拉 + 图标插槽
- ✅ **移动端顶导**：360px 移动页面的全局顶部导航，包含汉堡菜单、Logo、搜索、用户头像
- ✅ **PC 页脚**：PC 端站点底部通栏，包含分类导航链接、版权声明、社交媒体二维码
- ✅ **移动端页脚**：移动端底部页脚，适配小屏垂直排版
- ❌ **不适合**：侧边导航（用 OSideNav）、面包屑（用 OBreadcrumb）、标签页（用 OTab）

---

### 变体说明

**type（设备/位置）**
- `PC` — PC 端（1920px），顶导高度 72px，页脚高度 460px
- `Mb` — 移动端（360px），顶导高度 76px（含 18px 状态栏），页脚高度 458px

**Dark（主题，仅顶导）**
- `off` — 浅色模式，背景白色（顶导默认）
- `on` — 深色模式，背景 `rgba(36,36,39,1)` (**`--o-color-fill2-dark`**)，底部加 1px 白色分割线

**属性 2（内容类型）**
- *(不填)* — 顶部导航栏（Header）
- `openEuler` — 底部页脚（Footer），始终深色

> **说明**：6 个变体 = 顶导 4 个（PC×2 + Mb×2）+ 页脚 2 个（PC + Mb）。页脚没有 Light 变体。

---

### 布局结构

#### PC 顶导（type=PC）

```
ONavigation Header PC（SYMBOL）
│  node_id: 1303:10846（Light）/ 1303:10896（Dark）
│  Width: 1920px | Height: 72px
│  display: flex | justify: space-between | align: center
│  padding: 0px 216px
│  background: `rgba(255,255,255,1)` (**`--o-color-white`**)(Light) / `rgba(36,36,39,1)` (**`--o-color-fill2-dark`**)(Dark)
│  boxShadow: 0px 3px 9px 0px `rgba(0,18,85,0.078)`
│  backdropFilter: blur(4.53px)
│  [Dark 专属] borderBottom: 1px solid `rgba(255,255,255,0.149)`
│
├── [左侧区域 FRAME]（gap 40px，align: flex-end）
│   ├── [Logo FRAME]（padding: 20px 0）
│   │   └── [OpenEuler LOGO GROUP]（136px × 32px）📌 **可替换插槽** → [Part F](#part-flogo-自定义仅顶导)
│   │         图标（28.86px × 32px）+ 文字（100.2px × 18.75px）
│   │         默认内容：OpenEuler Logo；可由调用方传入自定义 SVG/PNG 切图替换
│   │
│   └── [主导航菜单 FRAME → OTab 插槽]（gap 32px）
│       └── [OTab text medium] × 7（32px × 48px，padding-bottom: 24px）
│             🔗 组件引用：[OTab 标签页](./tab.md) · text 变体 · medium 尺寸
│             text: 下载 / 开发 / 文档 / 学习 / 支持 / 社区 / 动态
│             fontSize: 16px | lineHeight: 24px | fontWeight: 400
│             fill: `rgba(0,0,0,1)` (**`--o-color-black`**)(Light) / `rgba(255,255,255,1)` (**`--o-color-white`**)(Dark)
│             交互状态（默认/Hover/Active/指示线）均遵循 OTab 规则
│
└── └── [右侧操作区 FRAME]（gap 20px，align: center）
    ├── [搜索框 → OSearch 插槽]（160px × 32px）
    │   🔗 组件引用：[OSearch 搜索框](./search.md) · small 尺寸
    │   Light: vector 背景图片 + 绝对定位图标/文字
    │   Dark:  FRAME，background `rgba(36,36,39,1)` (**`--o-color-fill2-dark`**)，borderRadius 4px
│          padding: 5px 16px，gap 8px（图标 16px + 文字 flex-grow）
│          占位文字 fill: `rgba(255,255,255,0.4)` (**`--o-color-info4-dark`**)，14px
    │   交互状态（默认/Focus/清除/建议面板）均遵循 OSearch 规则
    ├── [源码下拉 → ODropdown 插槽]（48px × 24px）
    │   🔗 组件引用：[ODropdown 下拉菜单](./dropdown.md) · small 尺寸 · text 变体
    │   text dropdown "源码" 14px lineHeight 24px + 下箭头图标（16px × 16px）
    │   无背景无描边，颜色 --o-color-info1；交互（Hover/Press/面板弹出）均遵循 ODropdown 规则
    ├── [国际化图标 GROUP]（24px × 24px）
    ├── [主题切换图标 FRAME]（24px × 24px）
    └── [用户图标 FRAME]（24px × 24px）
```

#### 移动端顶导（type=Mb）

```
ONavigation Header Mb（SYMBOL）
│  node_id: 1303:11169（Light）/ 1303:11391（Dark）
│  Width: 360px | Height: 76px
│  position: relative（绝对定位内部子节点）
│  background: `rgba(255,255,255,0.95)` (**`--o-color-white@0.95`**)(Light) / Dark 待核实
│  boxShadow: 0px 3px 8px 0px `rgba(0,0,0,0.078)`
│  backdropFilter: blur(1.67px)
│
├── [状态栏 FRAME]（348.5px × 18px，overflow: hidden，top: 0
│   信号图标（38.34px × 10px）+ 时间"9:41 AM" + 蓝牙/电量图标
│
├── [汉堡菜单图标 FRAME]（24px × 24px）
│   position: absolute | left: 24px | top: 36px
│
├── [OpenEuler Logo GROUP]（85px × 20px）📌 **可替换插槽** → [Part F](#part-flogo-自定义仅顶导)
│   position: absolute | left: 138px | top: 38px
│   默认内容：OpenEuler Logo；可由调用方传入自定义 SVG/PNG 切图替换
│
└── [右侧操作 GROUP]（64px × 24px）
    position: absolute | left: 272px | top: 36px
    ├── [搜索图标 FRAME]（24px × 24px，left: 0px）
    └── [用户头像 GROUP]（24px × 24px，left: 40px）
          圆形裁剪头像（31.5px × 31px 含出血区）
```

#### PC 页脚（type=PC, 属性 2=openEuler）

```
ONavigation Footer PC（SYMBOL）
│  node_id: 1303:12847
│  Width: 1920px | Height: 460px
│  background: `rgba(18,18,20,1)` (**`--o-color-footer-bg-pc`**)（始终深色）
│  padding: 24px 216px 34px 216px（内容区宽 1488px）
│
├── [顶部说明 FRAME]（居中，column，gap 12px，再外层 gap 16px）
│   ├── [文字 PARAGRAPH]（20px，Regular，lineHeight 28px，**`white`** (**`--o-color-white`**)，居中）
│   │   "openEuler 是由开放原子开源基金会（OpenAtom Foundation）孵化及运营的开源项目"
│   └── [基金会 Logo RECTANGLE]（160.43px × 32px，图片填充）📌 → Part G
│
├── [导航列 GROUP]（1122.5px × 180px）
│   6 列绝对定位，各列左起位置：0 / 221.5 / 409.5 / 646.5 / 834.5 / 1022.5px
│   ├── 关于openEuler（成员单位/组织架构/社区章程/贡献看板/社区介绍）
│   ├── 新闻与资讯（新闻/博客/白皮书）
│   ├── 获取与下载（获取openEuler操作系统/最新社区发行版/商业发行版/软件中心）
│   ├── 支持与服务（FAQ/联系我们/反馈问题）
│   ├── 互动与交流（邮件列表/活动/论坛）
│   └── 贡献与成长（SIG中心/贡献攻略/课程中心）
│   [列标题] fontSize: 20px | lineHeight: 28px | fontWeight: Regular | fill: **`white`** (**`--o-color-white`**)
│   [链接文字] fontSize: 14px | lineHeight: 22px | fill: `rgba(255,255,255,0.6)` (**`--o-color-info3`**)
│
├── [友情链接区 GROUP]（897px × 24px）
│   [标题"友情链接"] fontSize: 12px，fontWeight: UltraLight，fill: `rgba(255,255,255,1)` (**`--o-color-white`**)
│   [链接列] fontSize: 12px，fill: `rgba(255,255,255,0.6)` (**`--o-color-info3`**)
│   木兰开源社区 / 鲲鹏社区 / 鲲鹏小智 / 鹏城实验室 / InfoQ /
│   开源社 / 中科微澜 / Authing / openGauss / 昇思MindSpore / Ebaina
│
├── [分隔线 FRAME + RECTANGLE]
│   Width: 1487.91px | Height: 2px
│   fill: `rgba(229,229,229,1)` (**`--o-color-grey-1`**) | opacity: 0.12
│
└── [底部信息行 GROUP]（1487.91px × 78px）
    ├── [Logo区 GROUP]（150px × 68px，left: 0）📌 → Part G
    │   OpenEuler 白色 Logo（130px × 24.53px）
    │   "contact@openeuler.org" fontSize: 14px，fill: **`white`** (**`--o-color-white`**)，top: 46px
    ├── [版权信息区 GROUP]（266px × 78px，left: 611px）
    │   品牌 | 隐私政策 | 法律声明（14px **`white`** (**`--o-color-white`**)，分隔线 1px×14px **`white`**）
    │   "版权所有 © 2024 openEuler 保留一切权利"（14px **`white@0.6`** (**`--o-color-info3`**))
    │   "遵循 木兰宽松许可证第2版（MulanPSL2）"（14px **`white@0.6`** (**`--o-color-info3`**))
    └── [社交媒体区 GROUP]（353.91px × 52px，left: 1134px）📌 → Part G
        二维码行（top: 0）：openEuler小助手 + openEuler公众号（各 126.96px × 20px）
        平台Logo行（top: 32px）：OSCHINA / CSDN / 新土提金 / bilibili / 头条
        各平台 Logo 背景：`rgba(43,43,47,1)` (**`--o-color-control2-dark`**)，borderRadius: 4px
```

#### 移动端页脚（type=Mb, 属性 2=openEuler）

```
ONavigation Footer Mb（SYMBOL）
│  node_id: 1303:12977
│  Width: 360px | Height: 458px
│  background: `rgba(18,18,18,1)` (**`--o-color-footer-bg-mb`**)（注意：与 PC 页脚 `rgba(18,18,20,1)` (**`--o-color-footer-bg-pc`**) 有细微差异）
│
├── [顶部说明文字]（14px，Medium，**`white`** (**`--o-color-white`**)，居中，left: 68px，top: 24px）
│   "openEuler 是由开放原子开源基金会"
│   "（OpenAtom Foundation）"
│   "孵化及运营的开源项目"
│
├── [基金会 Logo RECTANGLE]（150.4px × 30px，left: 105px，top: 102px，图片填充）📌 → Part G
│
├── [分隔线 GROUP]（312px × 1px，left: 24px，top: 146px）
│   fill: `rgba(229,229,229,1)` (**`--o-color-grey-1`**) | opacity: 0.12
│
├── [底部链接行 GROUP]（207px × 18px，left: 77px，top: 161px）
│   品牌 | 隐私政策 | 法律声明 | 服务状态（12px **`white`** (**`--o-color-white`**)，分隔线 1px×12px **`white`**)
│   ⚠️ 注意：移动端比 PC 端多"服务状态"一项
│
├── [版权声明 PARAGRAPH]（12px，**`white`** (**`--o-color-white`**)，居中，left: 66.5px，top: 183px）
│   "版权所有 © 2022 openEuler 保留一切权利"
│   ⚠️ 注意：移动端版权年份为 2022，PC 端为 2024
│
├── [OpenEuler Logo GROUP]（86px × 34px，left: 137px，top: 213px）📌 → Part G
│   Logo 图形（86px × 20px）+ "contact@openeuler.org"（8px **`white`** (**`--o-color-white`**)，top: 24px）
│
├── [QR 码区 GROUP]（243px × 102px，left: 60px，top: 264px）📌 → Part G
│   openEuler公众号（78px × 78px 白底二维码 + 12px 标题）
│   openEuler小助手（78px × 78px 二维码 + 12px 标题，left: 151px）
│
└── [平台 Logo 行 GROUP]（232px × 49px，left: 64px，top: 385px）📌 → Part G
    第一行（top: 0）：OSCHINA（77px）/ CSDN（58px）/ 新土提金（73px）
    第二行（top: 29px）：bilibili（48px，left: 57px）/ 头条（48px，left: 117px）
    各平台 Logo 背景：`rgba(43,43,47,1)` (**`--o-color-control2-dark`**)，borderRadius: 4px
```

> ### 📱 移动端页脚可视化速查表
>
> ```
> 移动端页脚 (360×458px) | background: `rgba(18,18,18,1)` (**`--o-color-footer-bg-mb`**)
> ┌─────────────────────────────────────┐
> │         [顶部说明文字]               │ top:24px │ 14px Medium **`white`** (**`--o-color-white`**)
> │     openEuler是由开放原子开源        │
> │     基金会（OpenAtom Foundation）    │
> │     孵化及运营的开源项目             │
> │                                     │
> │       [基金会Logo 150.4×30]         │ top:102px │ 图片填充
> │                                     │
> │ ─────────────────────────────────── │ 分隔线 312×1 │ top:146px
> │                                     │
> │ 品牌│隐私政策│法律声明│服务状态      │ 链接行 207×18 │ top:161px
> │       (1px竖线分隔)                 │ 12px **`white`** (**`--o-color-white`**)
> │                                     │
> │   © 2022 openEuler 保留一切权利      │ 版权声明 │ top:183px
> │                           12px **`white`** (**`--o-color-white`**) │
> │                                     │
> │     [OpenEuler Logo 86×34]          │ 含邮箱 │ top:213px
> │     contact@openeuler.org            │ 8px **`white`** (**`--o-color-white`**)
> │                                     │
> │   [公众号QR 78×78] [小助手QR 78×78]  │ QR码区 │ top:264px
> │   openEuler公众号   openEuler小助手  │ 12px标题
> │                                     │
> │ OSCHINA CSDN 新土提金                │ 平台Logo第1行 │ top:385px
> │ bilibili 头条                       │ 平台Logo第2行
> └─────────────────────────────────────┘
> ```
>
> **关键差异提醒：**
> - ⚠️ 背景色与PC不同：`rgba(18,18,18,1)` (**`--o-color-footer-bg-mb`**) vs PC `rgba(18,18,20,1)` (**`--o-color-footer-bg-pc`**)
> - ⚠️ 版权年份：2022 vs PC 2024
> - ⚠️ 链接多一项："服务状态"
> - ⚠️ 邮箱字号：8px vs PC 14px

---

### 组合搭配

> 🔗 **常见搭配**：
> - **完整页面框架**：ONavigation Header（顶部）+ 页面内容区 + ONavigation Footer（底部）
> - **PC 页面**：`type=PC` Header + 内容 + `type=PC, openEuler` Footer
> - **移动端页面**：`type=Mb` Header + 内容 + `type=Mb, openEuler` Footer
> - **主题统一**：顶导 Dark=on 时，配合页面深色背景；页脚始终深色，无需切换

---

### 设计稿识别指南

> 🔍 **识别特征**：
> - PC 顶导 Light：1920×72px，白色背景，无底部线条
> - PC 顶导 Dark：1920×72px，rgba(36,36,39) 深色背景，**底部 1px 白色@0.149 分割线**
> - 移动端顶导：360×76px，含 18px 状态栏，左汉堡/中Logo/右操作
> - PC 页脚：1920×460px，`rgba(18,18,20)` (**`--o-color-footer-bg-pc`**) 深色，6 列导航
> - 移动端页脚：360×458px，`rgba(18,18,18)` (**`--o-color-footer-bg-mb`**) 深色，垂直单列布局

> 🔄 **易混淆组件**：
> - 与 **OTab 标签页**：Navigation 中间菜单区**直接引用** OTab 作为插槽组件，而非独立并列关系；OTab 单独使用时用于页面内区域切换
> - 与 **OSideNav 侧边导航**：SideNav 固定在侧边栏，Navigation 是水平顶导

---

### 响应式行为

| 断点 | 组件 | 宽度 | 内边距 |
|------|------|------|--------|
| PC（1920px） | `type=PC` Header | 1920px | padding 0 216px |
| PC（1920px） | `type=PC` Footer | 1920px | padding 24px 216px 34px 216px |
| 移动端（360px） | `type=Mb` Header | 360px | 绝对定位，左边距 24px |
| 移动端（360px） | `type=Mb` Footer | 360px | 绝对定位，左边距 24px |

---

## Part B：规格速查参考

### 变体索引表

| 变体组合 | node_id | 尺寸 | 说明 |
|---------|---------|------|------|
| type=PC, Dark=off | `1303:10846` | 1920×72px | PC 浅色顶导 |
| type=PC, Dark=on | `1303:10896` | 1920×72px | PC 深色顶导，底部有分割线 |
| type=Mb, Dark=off | `1303:11169` | 360×76px | 移动端浅色顶导 |
| type=Mb, Dark=on | `1303:11391` | 360×76px | 移动端深色顶导 |
| type=PC, 属性 2=openEuler | `1303:12847` | 1920×460px | PC 深色页脚 |
| type=Mb, 属性 2=openEuler | `1303:12977` | 360×458px | 移动端深色页脚 |

---

### 布局规格

#### PC 顶导（1303:10846 / 1303:10896）

| 规格项 | 值 |
|--------|---|
| 整体尺寸 | 1920px × 72px |
| 左右内边距 | 216px |
| Logo GROUP 尺寸 | 136px × 32px |
| Logo FRAME 上下 padding | 20px |
| 菜单项 FRAME 尺寸 | 32px × 48px（padding-bottom: 24px） |
| 菜单项间距 | 32px |
| Logo 与菜单间距 | 40px |
| 右侧操作区间距 | 20px |
| 搜索框尺寸 | 160px × 32px |
| 搜索框（Dark）padding | 5px 16px，gap 8px，borderRadius 4px |
| 源码下拉 GROUP 尺寸 | 48px × 24px |
| 操作图标尺寸 | 24px × 24px |
| boxShadow | 0px 3px 9px 0px `rgba(0,18,85,0.078)` |
| backdropFilter | blur(4.53px) |
| Dark 底部描边 | 1px bottom，`rgba(255,255,255,0.149)` |

#### 移动端顶导（1303:11169 / 1303:11391）

| 规格项 | 值 |
|--------|---|
| 整体尺寸 | 360px × 76px |
| 状态栏高度 | 18px（FRAME，348.5px 宽，overflow: hidden） |
| 汉堡图标 | 24px × 24px，left: 24px，top: 36px |
| Logo GROUP | 85px × 20px，left: 138px，top: 38px |
| 右侧操作 GROUP | 64px × 24px，left: 272px，top: 36px |
| 头像（含出血区） | 31.5px × 31px，left: 40px（相对右侧 GROUP） |
| boxShadow | 0px 3px 8px 0px `rgba(0,0,0,0.078)` |
| backdropFilter | blur(1.67px) |

#### PC 页脚（1303:12847）

| 规格项 | 值 |
|--------|---|
| 整体尺寸 | 1920px × 460px |
| 内边距 | 24px 216px 34px 216px（内容区宽 1488px） |
| 顶部说明文字 | 20px Regular，lineHeight 28px |
| 基金会 Logo | 160.43px × 32px |
| 导航列 GROUP | 1122.5px × 180px |
| 导航列标题 | 20px Regular **`white`** (**`--o-color-white`**) |
| 导航链接 | 14px Regular **`white@0.6`** (**`--o-color-info3`**) |
| 友情链接行 | 897px × 24px（标题 + 链接横排） |
| 分隔线 | 1487.91px × 2px，`rgba(229,229,229)` (**`--o-color-grey-1`**) opacity 0.12 |
| 底部信息行 | 1487.91px × 78px |
| 底部 Logo 区 | 150px × 68px，left: 0 |
| 版权区 | 266px × 78px，left: 611px |
| 社交媒体区 | 353.91px × 52px，left: 1134px |
| 二维码尺寸 | 126.96px × 20px（含文字行） |
| 平台 Logo 背景 | `rgba(43,43,47,1)` (**`--o-color-control2-dark`**)，borderRadius: 4px |

#### 移动端页脚（1303:12977）

| 规格项 | 值 |
|--------|---|
| 整体尺寸 | 360px × 458px |
| 顶部说明文字 | 14px Medium，lineHeight 22px，**`white`** (**`--o-color-white`**)，居中，left: 68px，top: 24px |
| 基金会 Logo | 150.4px × 30px，left: 105px，top: 102px |
| 分隔线 | 312px × 1px，left: 24px，top: 146px，`rgba(229,229,229)` (**`--o-color-grey-1`**) opacity 0.12 |
| 底部链接行 | 207px × 18px，left: 77px，top: 161px，12px **`white`** (**`--o-color-white`**) |
| 版权声明 | 12px **`white`** (**`--o-color-white`**)，居中，left: 66.5px，top: 183px |
| Logo GROUP | 86px × 34px，left: 137px，top: 213px |
| 联系邮箱 | 8px **`white`** (**`--o-color-white`**)，top: 24px（相对 Logo GROUP） |
| QR 码区 GROUP | 243px × 102px，left: 60px，top: 264px |
| 单个二维码 | 78px × 78px |
| 平台 Logo 行 | 232px × 49px，left: 64px，top: 385px |

---

### 颜色规格

#### 顶导颜色

| 区域 | Light 模式（rgba） | Dark 模式（rgba） |
|------|-------------------|------------------|
| 导航栏背景（PC） | `(255,255,255,1)` (**`--o-color-white`**) | `(36,36,39,1)` (**`--o-color-fill2-dark`**) |
| 导航栏背景（Mb） | `(255,255,255,0.95)` (**`--o-color-white@0.95`**) | 待核实 |
| PC Dark 底部描边 | — | `(255,255,255,0.149)` |
| 菜单文字 | `(0,0,0,1)` (**`--o-color-black`**) | `(255,255,255,1)` (**`--o-color-white`**) |
| 源码文字 | `(0,0,0,1)` (**`--o-color-black`**) | `(255,255,255,1)` (**`--o-color-white`**) |
| 搜索框背景（Dark） | — | `(36,36,39,1)` (**`--o-color-fill2-dark`**) |
| 搜索占位文字 | `(0,0,0,0.4)` (**`--o-color-info4`**) | `(255,255,255,0.4)` (**`--o-color-info4-dark`**) |
| boxShadow（PC） | `(0,18,85,0.078)` | `(0,18,85,0.078)` |
| boxShadow（Mb） | `(0,0,0,0.078)` | 待核实 |

#### 页脚颜色

| 区域 | PC 页脚（rgba） | 移动端页脚（rgba） |
|------|----------------|------------------|
| 背景 | `(18,18,20,1)` (**`--o-color-footer-bg-pc`**) | `(18,18,18,1)` (**`--o-color-footer-bg-mb`**)⚠️细微差异 |
| 顶部说明文字 | `(255,255,255,1)` (**`--o-color-white`**) | `(255,255,255,1)` (**`--o-color-white`**) |
| 导航列标题 | `(255,255,255,1)` (**`--o-color-white`**) | — |
| 导航链接/友情链接 | `(255,255,255,0.6)` (**`--o-color-info3`**) | — |
| 版权/底部链接 | `(255,255,255,0.6)` (**`--o-color-info3`**) | `(255,255,255,1)` (**`--o-color-white`**) ⚠️差异 |
| 分隔线 | `(229,229,229)` (**`--o-color-grey-1`**) × opacity 0.12 | `(229,229,229)` (**`--o-color-grey-1`**) × opacity 0.12 |
| 平台 Logo 背景 | `(43,43,47,1)` (**`--o-color-control2-dark`**) | `(43,43,47,1)` (**`--o-color-control2-dark`**) |

---

### 字体样式规格

#### PC 顶导

| 使用位置 | 字号 | 行高 | 字重 | 颜色 |
|---------|------|------|------|------|
| 主导航菜单文字 | 16px | 24px | 400 | **`black`** (**`--o-color-black`**)/**`white`** (**`--o-color-white`**)（主题） |
| 源码下拉文字 | 14px | 24px | 400 | **`black`** (**`--o-color-black`**)/**`white`** (**`--o-color-white`**)（主题） |
| 搜索占位文字 | 14px | 22px | 400 | **`black@0.4`** (**`--o-color-info4`**)/ **`white@0.4`** (**`--o-color-info4-dark`**) |

#### PC 页脚

| 使用位置 | 字号 | 行高 | 字重 | 颜色 |
|---------|------|------|------|------|
| 顶部说明文字 | 20px | 28px | Regular | **`white`** (**`--o-color-white`**) |
| 导航列标题 | 20px | 28px | Regular | **`white`** (**`--o-color-white`**) |
| 导航链接文字 | 14px | 22px | Regular | **`white@0.6`** (**`--o-color-info3`**) |
| 友情链接标题 | 12px | 24px | UltraLight | **`white`** (**`--o-color-white`**) |
| 友情链接文字 | 12px | 18px | Regular | **`white@0.6`** (**`--o-color-info3`**) |
| 底部链接（品牌/政策）| 14px | 22px | Regular | **`white`** (**`--o-color-white`**) |
| 版权/许可证声明 | 14px | 22px | Regular | **`white@0.6`** (**`--o-color-info3`**) |
| 联系邮箱 | 14px | 22px | Regular | **`white`** (**`--o-color-white`**) |
| QR 码标题 | 12px | — | Regular | **`white@0.6`** (**`--o-color-info3`**) |

#### 移动端页脚

| 使用位置 | 字号 | 行高 | 字重 | 颜色 |
|---------|------|------|------|------|
| 顶部说明文字 | 14px | 22px | **Medium** | **`white`** (**`--o-color-white`**) |
| 底部链接（品牌/政策）| 12px | 18px | Regular | **`white`** (**`--o-color-white`**) |
| 版权声明 | 12px | 18px | Regular | **`white`** (**`--o-color-white`**) |
| 联系邮箱 | 8px | — | Regular | **`white`** (**`--o-color-white`**) |
| QR 码标题 | 12px | 18px | Regular | **`white`** (**`--o-color-white`**) |

字体族：`HarmonyHeiTi`（Regular 400 / Medium 500 / UltraLight 200）

---

### 变体 componentKey 速查

| 变体 | node_id | componentKey |
|------|---------|-------------|
| type=PC, Dark=off | `1303:10846` | `561d3014fbe6f361abfecc203a06f1ab6cad43af` |
| type=PC, Dark=on | `1303:10896` | `c4522744b94335f4bc8fbc358c2316d3c447b771` |
| type=Mb, Dark=off | `1303:11169` | `7675ede074069cf358a0f164f76e1c6c119a592f` |
| type=Mb, Dark=on | `1303:11391` | `bf7bae1fb8fa033032f477251a510a0e0a247544` |
| type=PC, 属性 2=openEuler | `1303:12847` | `5d66a734f21bfcce1fbe4fce1d79f832b7f08f02` |
| type=Mb, 属性 2=openEuler | `1303:12977` | `62383dfdd93fd29e6dfcb7def1861d22d005324a` |

---

### Pixso 操作速查

1. **插入顶导**：组件面板搜索「ONavigation」→ 选择 `type=PC` 或 `type=Mb` → 拖入画布
2. **切换主题**：右侧面板 → Dark 属性 → `off`（浅色）/ `on`（深色）
3. **插入页脚**：选择 `type=PC, openEuler` 或 `type=Mb, openEuler`
4. **修改菜单文字**：双击进入组件 → 双击文字图层直接编辑
5. **调整定位**：顶导固定在画板顶部（x: 0, y: 0）；页脚固定在画板底部
6. **替换 Logo（仅顶导）**：详见 [Part F · Logo 自定义](#part-flogo-自定义仅顶导)

---

### 注意事项

- **Dark 顶导有底部分割线**：PC Dark 变体底部有 1px `rgba(255,255,255,0.149)` 描边，Light 变体无此线
- **PC 与 Mb 页脚背景色不同**：PC `rgba(18,18,20,1)` (**`--o-color-footer-bg-pc`**) vs 移动端 `rgba(18,18,18,1)` (**`--o-color-footer-bg-mb`**)，细微差异
- **移动端页脚多"服务状态"链接**：移动端底部链接栏比 PC 多出"服务状态"一项
- **版权年份不一致**：PC 页脚为 2024，移动端为 2022（设计稿历史遗留，使用时注意统一）
- **移动端页脚顶部文字字重不同**：14px Medium，而 PC 端为 20px Regular
- **PC 页脚 Logo 区联系邮箱字号**：14px；移动端为 8px（较小，注意辨认）
- **移动端含状态栏**：Mb 变体高度 76px 包含 18px 系统状态栏，导航操作区实际高度 58px
- **搜索框 Light/Dark 实现方式不同**：Light 为 vector 图片叠加，Dark 为 FRAME + 描边布局
- **毛玻璃效果**：顶导有 backdrop-filter 模糊效果，需配合透明/半透明背景

---

## Part C：交互与状态

### 顶导交互状态

| 元素 | 状态 | 视觉表现 |
|------|------|---------|
| 菜单项 | 默认 | 文字黑色（Light）/ 白色（Dark），padding-bottom 24px 留激活线空间 |
| 菜单项 | Hover | 文字变主色（brand color） |
| 菜单项 | Active（当前页） | 底部 2px 指示线（品牌蓝），文字颜色加深 |
| 搜索框 | 默认 | 背景 + 占位文字 + 搜索图标 |
| 搜索框 | Focus | 边框高亮，显示光标 |
| 汉堡菜单 | 点击 | 展开侧边导航抽屉 |
| 主题切换图标 | 点击 | Light ↔ Dark 切换 |

### 主题切换逻辑

| 元素 | Light | Dark |
|------|-------|------|
| PC 导航背景 | `rgba(255,255,255,1)` (**`--o-color-white`**) | `rgba(36,36,39,1)` (**`--o-color-fill2-dark`**) |
| PC 底部描边 | 无 | 1px `rgba(255,255,255,0.149)` |
| 菜单/操作文字 | `rgba(0,0,0,1)` (**`--o-color-black`**) | `rgba(255,255,255,1)` (**`--o-color-white`**) |
| 搜索框 | 图片背景（无描边） | FRAME + 描边布局 |
| 搜索占位文字 | `rgba(0,0,0,0.4)` (**`--o-color-info4`**) | `rgba(255,255,255,0.4)` (**`--o-color-info4-dark`**) |
| 页脚 | 不受切换影响（始终深色） | 同左 |

---

## Part D：设计变量绑定

> **注意**：当前文件无本地变量集（`get_variable_sets` 返回空）。ONavigation 使用硬编码颜色值，无 Token 绑定。如需绑定变量，参考下表手动设置。

| 元素 | 属性 | 精确颜色值 |
|------|------|-----------|
| PC 顶导背景（Light） | fill | `rgba(255,255,255,1)` (**`--o-color-white`**) |
| PC 顶导背景（Dark） | fill | `rgba(36,36,39,1)` (**`--o-color-fill2-dark`**) |
| PC Dark 底部描边 | stroke | `rgba(255,255,255,0.149)`，1px bottom |
| 菜单文字（Light） | fill | `rgba(0,0,0,1)` (**`--o-color-black`**) |
| 菜单文字（Dark） | fill | `rgba(255,255,255,1)` (**`--o-color-white`**) |
| 搜索占位文字（Light） | fill | `rgba(0,0,0,0.4)` (**`--o-color-info4`**) |
| 搜索占位文字（Dark） | fill | `rgba(255,255,255,0.4)` (**`--o-color-info4-dark`**) |
| PC 页脚背景 | fill | `rgba(18,18,20,1)` (**`--o-color-footer-bg-pc`**) |
| 移动端页脚背景 | fill | `rgba(18,18,18,1)` (**`--o-color-footer-bg-mb`**) |
| 页脚导航链接 | fill | `rgba(255,255,255,0.6)` (**`--o-color-info3`**) |
| 分隔线 | fill | `rgba(229,229,229,1)` (**`--o-color-grey-1`**)，opacity 0.12 |
| 平台 Logo 背景 | fill | `rgba(43,43,47,1)` (**`--o-color-control2-dark`**) |

---

## Part E：最佳实践

### 使用建议

1. **设备适配**：PC 页面使用 `type=PC`，移动端使用 `type=Mb`，不要混用
2. **主题一致性**：顶导主题与页面整体主题保持一致；页脚无需处理（始终深色）
3. **固定定位**：顶导应置于画板最顶层并固定，确保滚动时遮挡内容
4. **内容区宽度**：PC 端内容区 = 1920 - 216×2 = **1488px**，页面主体内容应对齐此宽度

### 设计提示

- PC Dark 顶导有底部 1px 分割线，用于视觉分层，Light 无此线
- 菜单项高度 48px（非 72px），底部 24px 留给激活指示线，整体居中对齐
- 移动端 Logo、汉堡、操作图标均为绝对定位，top 统一为 36px（状态栏 18px + 10px 居中偏移）
- PC 页脚导航列使用绝对定位，6 列左起 x 坐标：0 / 221.5 / 409.5 / 646.5 / 834.5 / 1022.5px
- 移动端页脚平台 Logo 分两行排列（第一行 3 个，第二行 2 个），注意保持间距对齐

### 用户操作区交互状态（登录后）

> 适用场景：PC 顶导右侧已登录用户区域（type=PC）
> 
> 该区域为**下拉菜单触发器**，包含：头像 + 用户名称 + 下拉箭头

#### 触发器规格（用户名区域）

| 属性 | 值 |
|------|---|
| 头像尺寸 | 20×20px（圆形裁剪） |
| 用户名称 | 字号 14px，文本限宽 96px（超出显示省略号） |
| 箭头图标 | 16×16px |
| 组成结构 | 头像 + 用户名称 + 下拉箭头 |

#### 触发器状态明细

| 状态 | 字号 | 字重 | 文字色值 | 背景色 | 下拉菜单 |
|------|------|------|---------|--------|---------|
| **默认态** | 14px | Regular | #000000 100% (--o-color-info1) | 透明 | 不显示 |
| **悬浮态(Hover)** | 14px | Regular | #000000 80% (--o-color-info2) | 透明 | 显示 |
| **按下态(Press)** | 14px | Semibold | #002FA7 (--o-color-primary1) | #EBF1FA (--o-color-control3-light) | 显示 |
| **选中态(Selected)**<br>（菜单展开时） | 14px | Semibold | #002FA7 (--o-color-primary1) | #EBF1FA (--o-color-control3-light) | 显示 |
| **禁用态(Disabled)** | 14px | Regular | #000000 40% (--o-color-info4) | 透明 | 不显示 |

#### 下拉菜单（悬浮/按下/选中态时弹出）

##### 菜单容器样式

| 属性 | 值 |
|------|---|
| 背景色 | #FFFFFF (--o-color-control5-light) |
| 阴影 | X: 0, Y: 2, Blur: 24px, 颜色: #000000 15% |

##### 菜单项列表

1. **个人中心**
2. **消息中心**（未读时显示红点提示）
3. **退出登录**

##### 菜单项样式

| 状态 | 字号 | 字重 | 文字色值 | 背景色 |
|------|------|------|---------|--------|
| 默认态 | 14px | Regular | #000000 80% (--o-color-info2) | 透明 |
| 悬浮态 | 14px | Regular | #000000 80% (--o-color-info2) | #EBF1FA (--o-color-control2-light) |

#### 未登录状态

当用户未登录时，该区域显示为**用户图标**：

| 属性 | 值 |
|------|---|
| 图标尺寸 | 24×24px |
| 图标资源 | `header-pc-头像.svg`（位于 `references/assets/navigation/`） |
| 使用方式 | CSS mask（可着色） |
| 点击行为 | 跳转至登录页面 |

> 📌 **参考位置**：PC 顶导布局> 顶导左侧 Logo 区域是**可替换插槽**。本 skill 已自带 **openEuler 默认 Logo 的 4 个 SVG 变体**（见下方「默认 Logo 资源」），与 ONavigation Symbol 内置 Logo 视觉一致；调用方也可用自家品牌的 SVG / PNG 切图替换。本节定义默认资源、自定义资源要求、目标尺寸、主题适配与 Pixso 操作流程。

### 适用范围

- ✅ **PC 顶导**（`type=PC, Dark=off/on`） — Logo GROUP 136×32px
- ✅ **移动端顶导**（`type=Mb, Dark=off/on`） — Logo GROUP 85×20px
- ❌ **页脚 Logo 不在此范围**：PC 页脚底部 Logo 区（150×68px）、移动端页脚 Logo GROUP（86×34px）、PC 页脚顶部基金会 Logo（160.43×32px）、移动端基金会 Logo（150.4×30px）**均保持 OpenEuler 默认**，不可替换；页脚所有图形资源（含底部 Logo、基金会 Logo、社交媒体、二维码、平台 Logo）详见 [Part G](#part-g页脚图形资源已入仓)

### 默认 Logo 资源（已入仓）

本 skill 自带 2 个 PC Logo SVG 和 4 个顶导 Icon SVG，位于 [`references/assets/navigation/`](../references/assets/navigation/)，覆盖 PC 顶导 Logo 亮色/暗色变体及全部顶导操作图标（移动端暂无入仓资源）。

#### PC Logo（SVG，`<img>` 引用）

| 文件 | 适用变体 | 尺寸 | 颜色 |
|---|---|---|---|
| [`header-pc-light logo.svg`](../references/assets/navigation/header-pc-light logo.svg) | PC 顶导 `Dark=off`（`1303:10846`）| 136×32 | 蓝 mark + 黑字 |
| [`header-pc-dark logo.svg`](../references/assets/navigation/header-pc-dark logo.svg) | PC 顶导 `Dark=on`（`1303:10896`）| 136×32 | 白 mark + 白字 |

#### 顶导 Icon SVG（CSS mask 着色，见 Part H）

| 文件 | 用途 | 尺寸 | Light 色 | Dark 色 |
|---|---|---|---|---|
| [`header-pc-search icon.svg`](../references/assets/navigation/header-pc-search icon.svg) | 搜索框图标 | 16×16 | `rgba(0,0,0,.4)` (**`--o-color-info4`**) | `rgba(255,255,255,.4)` (**`--o-color-info4-dark`**) |
| [`header-pc-语言切换 icon.svg`](../references/assets/navigation/header-pc-语言切换 icon.svg) | 国际化切换 | 20×20 | `rgba(0,0,0,.65)` (**`--o-color-info3`**) | `rgba(255,255,255,.85)` (**`--o-color-info1-dark`**) |
| [`header-pc-深浅模式.svg`](../references/assets/navigation/header-pc-深浅模式.svg) | 主题切换 | 20×20 | `rgba(0,0,0,.65)` (**`--o-color-info3`**) | `rgba(255,255,255,.85)` (**`--o-color-info1-dark`**) |
| [`header-pc-头像.svg`](../references/assets/navigation/header-pc-头像.svg) | 用户/登录 | 20×20 | `rgba(0,0,0,.65)` (**`--o-color-info3`**) | `rgba(255,255,255,.85)` (**`--o-color-info1-dark`**) |

> ✅ PC Logo SVG 已按主题备齐两套；Icon SVG 通过 CSS mask + background-color 控制颜色，无需准备两份文件。

### 自定义 Logo 输入资源

如需用自家品牌替换默认 openEuler Logo，按以下规格准备：

| 资源类型 | 要求 | 优先级 |
|---|---|---|
| SVG | 矢量、viewBox 完整、颜色优先用 `currentColor` 或准备两套主题版本 | ⭐ 优先 |
| PNG 切图 | ≥ @2x 高分辨率（移动端建议 ≥ @3x），透明背景 | 备选 |

### 目标容器规格（不可改）

| 变体 | Logo GROUP 尺寸 | 外层 padding / 定位 | 本仓默认 SVG viewBox |
|---|---|---|---|
| PC（`1303:10846` / `1303:10896`）| 136×32px | 左侧 FRAME 内，padding 20px 0 | 136×32（纯 Logo GROUP） |
| Mb（`1303:11169` / `1303:11391`）| 85×20px | 绝对定位 left:138px / top:38px | 85×20（纯 Logo GROUP） |

> 资源原始宽高比与目标 GROUP 不一致时，按"等比缩放后水平垂直居中"对齐到目标 GROUP，**禁止拉伸变形**。

### 主题适配

| 主题 | 建议 Logo 版本 |
|---|---|
| Light（Dark=off） | 深色版（主色或纯黑 #000） |
| Dark（Dark=on） | 浅色版（白色或品牌浅色变体） |

- 调用方应**准备两套** SVG / 切图，分别对应 Light / Dark
- 或单套 SVG 内部颜色使用 `currentColor`，由外层填充驱动，避免主题切换时露出错误色

### Pixso 替换流程

**手动流程（推荐）**：

1. 用 componentKey 插入 ONavigation 顶导实例（PC 或 Mb 对应变体）
2. 右键实例 → **拆分组件**（detach instance）
3. 进入左侧 FRAME → 选中 `[Logo GROUP]` 节点（PC 136×32 / Mb 85×20，不要选外层 Logo FRAME）
4. 删除内部原 vector 图标 + 文字图层（保留 GROUP 外框作为容器）
5. `File → Import` 导入 SVG：
   - **用 openEuler 默认 Logo**：从 [`references/assets/navigation/`](../references/assets/navigation/) 选对应变体（PC Light → `header-pc-light logo.svg`，PC Dark → `header-pc-dark logo.svg`；⚠️ 移动端暂无入仓资源）
   - **用自定义品牌 Logo**：用调用方提供的 SVG / PNG
6. 等比缩放到 Logo GROUP 尺寸（PC 136×32 / Mb 85×20），水平垂直居中对齐

**MCP 工具流程（有限）**：

- `create_instance(componentKey)` 只生成带默认 OpenEuler Logo 的实例
- 当前 Pixso MCP **暂无** `detach_instance` 与 `replace_image_fill` 接口
- ⚠️ **AI 无法自动完成 Logo 替换**，生成默认实例后必须明确交付用户在 Pixso 中手动执行上述步骤

### 注意事项

- ⚠️ **脱钩风险**：detach 后该实例与组件库脱钩，后续 ONavigation 组件库更新不会自动同步到该实例
- ⚠️ **保持外层 padding**：只替换 Logo GROUP 内部内容，**不要**修改外层 Logo FRAME 的 padding（PC 上下 20px）
- ⚠️ **颜色用 currentColor**：自定义 SVG 内避免硬编码黑/白，主题切换时会露出错误色；**本仓 PC Logo SVG 已按主题对齐**（`header-pc-light logo.svg` 黑字/蓝 mark、`header-pc-dark logo.svg` 白字/白 mark），直接选对应变体即可
- ⚠️ **切图分辨率**：移动端 < @2x 真机显示模糊；优先用 SVG
- ⚠️ **不要扩大占位**：替换后 Logo 仍占据原 136×32 / 85×20 容器，**不要**因新 Logo 长宽比不同而扩大占位（会破坏顶导整体布局与右侧操作区间距）

---

## Part G：页脚图形资源（已入仓）

> ⚠️ **重要警告（2026实战踩坑记录）：**
>
> 以下 8 个 SVG 为**"整节点粒度"资源**——它们是**不可分割的整体图片**，内部已包含完整的布局结构。
>
> ### 🚫 绝对禁止：
> - ❌ 用 `<img>` 引用后再用 HTML/CSS 拆分其内部元素（如把 Logo 和邮箱分开）
> - ❌ 把平台Logo组(232×49)拆成5个独立div再分别设置尺寸
> - ❌ 自行实现这些SVG内部的布局逻辑
>
> ### ✅ 正确做法：
> - ✅ 直接 `<img src="...footer-mb-底部logo.svg">` 整体引用
> - ✅ 仅设置外层容器的 width/height 匹配规范尺寸
>
> ### ⛔ 违规后果：
> 生成的代码将不符合设计稿，需返工重写。
>
> ---
>

本 skill 自带 8 个页脚 SVG 文件，位于 [`references/assets/navigation/`](../references/assets/navigation/)，覆盖 PC 和移动端页脚所有图形区域。与顶导 Logo（[Part F](#part-flogo-自定义仅顶导)）不同，页脚图形资源**不可替换**，仅作为跨文件库链接失联时的兜底资源。

### PC 页脚图形资源

| 文件 | 适用节点 | 尺寸 | 说明 |
|---|---|---|---|
| [`footer-pc-基金会logo.svg`](../references/assets/navigation/footer-pc-基金会logo.svg) | `[基金会 Logo RECTANGLE]` | 160×32 | 开放原子开源基金会 Logo |
| [`footer-pc-底部logo.svg`](../references/assets/navigation/footer-pc-底部logo.svg) | `[Logo区 GROUP]` | 150×68 | OpenEuler 白色 Logo + 联系邮箱 |
| [`footer-pc-社交媒体logo.svg`](../references/assets/navigation/footer-pc-社交媒体logo.svg) | `[社交媒体区 GROUP]` | 354×52 | 二维码行 + 平台 Logo 行 |

### 移动端页脚图形资源

| 文件 | 适用节点 | 尺寸 | 说明 |
|---|---|---|---|
| [`footer-mb-基金会logo.svg`](../references/assets/navigation/footer-mb-基金会logo.svg) | `[基金会 Logo RECTANGLE]` | 150×30 | 开放原子开源基金会 Logo（移动端版） |
| [`footer-mb-底部logo.svg`](../references/assets/navigation/footer-mb-底部logo.svg) | `[OpenEuler Logo GROUP]` | 86×34 | OpenEuler Logo + 联系邮箱 |
| [`footer-mb-二维码1.svg`](../references/assets/navigation/footer-mb-二维码1.svg) | `[QR 码区 GROUP]` 左（公众号） | 78×78 | openEuler 公众号二维码 |
| [`footer-mb-二维码2.svg`](../references/assets/navigation/footer-mb-二维码2.svg) | `[QR 码区 GROUP]` 右（小助手） | 78×78 | openEuler 小助手二维码 |
| [`footer-mb-平台logo.svg`](../references/assets/navigation/footer-mb-平台logo.svg) | `[平台 Logo 行 GROUP]` | 232×49 | OSCHINA / CSDN / bilibili 等平台 Logo |

> ✅ **8 个 SVG 均为整节点粒度**：每个文件对应布局树中一个 GROUP/RECTANGLE 节点，尺寸与设计稿节点精确对齐。

---

## Part H：HTML 页面实现参考

> 本节记录在 HTML 页面中实现 ONavigation 导航栏和页脚时的正确结构、CSS 值与资源路径，作为后续 HTML 生成的基准。所有数值均经设计稿核对，与 Part A/B 规格完全对应。

---

### 资源路径（相对 HTML 文件根目录）

| 用途 | 路径 | 用法 |
|---|---|---|
| PC 顶导 Light Logo | `skills/opendesign-design/references/assets/navigation/header-pc-light logo.svg` | `<img>`（SVG） |
| PC 顶导 Dark Logo | `skills/opendesign-design/references/assets/navigation/header-pc-dark logo.svg` | `<img>`（SVG） |
| 顶导搜索图标 | `skills/opendesign-design/references/assets/navigation/header-pc-search icon.svg` | CSS mask |
| 顶导国际化图标 | `skills/opendesign-design/references/assets/navigation/header-pc-语言切换 icon.svg` | CSS mask |
| 顶导主题切换图标 | `skills/opendesign-design/references/assets/navigation/header-pc-深浅模式.svg` | CSS mask |
| 顶导用户图标 | `skills/opendesign-design/references/assets/navigation/header-pc-头像.svg` | CSS mask |
| PC 页脚 基金会 Logo | `skills/opendesign-design/references/assets/navigation/footer-pc-基金会logo.svg` | `<img>`（SVG） |
| PC 页脚 底部 Logo（含邮箱） | `skills/opendesign-design/references/assets/navigation/footer-pc-底部logo.svg` | `<img>`（SVG） |
| PC 页脚 社交媒体 | `skills/opendesign-design/references/assets/navigation/footer-pc-社交媒体logo.svg` | `<img>`（SVG） |

> ⚠️ Logo 和页脚图片均用 `<img>` 直接引用；4 个顶导 Icon SVG 用 **CSS mask** 着色（见下方 CSS 片段），**禁止用自绘 SVG 路径模拟**。

---

### PC 顶导 CSS 关键值

```css
.nav-right  { display: flex; align-items: center; gap: 20px; }  /* ⚠️ gap=20 不是 16 */
.icon-btn   { width: 24px; height: 24px; }

/* OSearch small 变体：背景白色(**`--o-color-fill2`**)，描边 `rgba(0,0,0,.25)`(grey-14@0.25) */
.nav-search { width: 160px; height: 32px;
              background: rgb(255,255,255);            /* ⚠️ 不是 rgba(0,0,0,.04) */
              border: 1px solid rgba(0,0,0,.25);       /* ⚠️ 不是 rgba(0,0,0,.12) */
              border-radius: 4px; }
.nav-search span { font-size: 14px; line-height: 22px; color: rgba(0,0,0,.4); (**`--o-color-info4`**) }

/* ODropdown text 变体：无背景无描边无Padding，颜色 --o-color-info1 = rgba(0,0,0,.85) */
.nav-btn    { padding: 0; border: none; background: transparent;
              color: rgba(0,0,0,.85);                  /* ⚠️ 不是 #002FA7 */
              font-size: 14px; line-height: 22px;
              display: flex; align-items: center; gap: 4px; }
```

### 顶导 Icon CSS mask

```css
/* Icon SVG 通过 CSS mask + background-color 控制 Light/Dark 颜色，无需准备两份文件 */
.nav-icon {
  display: inline-block;
  border: none; background-color: rgba(0,0,0,.65); cursor: pointer; padding: 0;
  -webkit-mask-size: contain;
  -webkit-mask-repeat: no-repeat;
  -webkit-mask-position: center;
  mask-size: contain;
  mask-repeat: no-repeat;
  mask-position: center;
}
.nav-icon-search {
  width: 16px; height: 16px;
  background-color: rgba(0,0,0,.4);
  -webkit-mask-image: url('skills/opendesign-design/references/assets/navigation/header-pc-search icon.svg');
  mask-image:         url('skills/opendesign-design/references/assets/navigation/header-pc-search icon.svg');
}
.nav-icon-lang {
  width: 20px; height: 20px;
  -webkit-mask-image: url('skills/opendesign-design/references/assets/navigation/header-pc-语言切换 icon.svg');
  mask-image:         url('skills/opendesign-design/references/assets/navigation/header-pc-语言切换 icon.svg');
}
.nav-icon-theme {
  width: 20px; height: 20px;
  -webkit-mask-image: url('skills/opendesign-design/references/assets/navigation/header-pc-深浅模式.svg');
  mask-image:         url('skills/opendesign-design/references/assets/navigation/header-pc-深浅模式.svg');
}
.nav-icon-user {
  width: 20px; height: 20px;
  -webkit-mask-image: url('skills/opendesign-design/references/assets/navigation/header-pc-头像.svg');
  mask-image:         url('skills/opendesign-design/references/assets/navigation/header-pc-头像.svg');
}
/* Dark mode：切换 background-color 即可变色，无需换图 */
.dark .nav-icon                { background-color: rgba(255,255,255,.85); }
.dark .nav-icon-search         { background-color: rgba(255,255,255,.4); }
```

### PC 顶导 HTML 骨架

```html
<header class="nav">
  <div class="nav-left">
    <!-- Logo：固定 136×32，用 img 引用 PNG，禁止内联自绘 -->
    <a class="logo" href="#">
      <img src="skills/opendesign-design/references/assets/navigation/header-pc-light logo.svg"
           width="136" height="32" alt="openEuler">
    </a>
    <!-- 导航菜单：7 项，顺序固定 -->
    <nav class="nav-menu">
      <a>下载</a><a>开发</a><a>文档</a><a>学习</a><a>支持</a><a>社区</a><a>动态</a>
    </nav>
  </div>
  <!-- ⚠️ 顺序：搜索 → 源码(含箭头) → 国际化 → 主题切换 → 用户 -->
  <div class="nav-right">
    <div class="nav-search">
      <span class="nav-icon nav-icon-search"></span>
      <span>搜索</span>
    </div>
    <button class="nav-btn">
      源码
      <svg width="16" height="16" viewBox="0 0 16 16" fill="none">
        <path d="M4 6l4 4 4-4" stroke="#002FA7" stroke-width="1.4" stroke-linecap="round" stroke-linejoin="round"/>
      </svg>
    </button>
    <span class="nav-icon nav-icon-lang" role="button" aria-label="语言切换"></span>
    <span class="nav-icon nav-icon-theme" role="button" aria-label="主题切换"></span>  <!-- ⚠️ 不可省略 -->
    <span class="nav-icon nav-icon-user" role="button" aria-label="用户"></span>
  </div>
</header>
```

---

### PC 页脚 CSS 关键值

```css
.footer {
  background: rgba(18,18,20,1); (**`--o-color-footer-bg-pc`**)
  min-height: 460px;
  padding: 24px 216px 34px;   /* ⚠️ 顶部 24px，不是 40px */
}
/* 顶部说明区：居中列布局 */
.footer-intro { display: flex; flex-direction: column; align-items: center; gap: 16px; margin-bottom: 32px; }
.footer-intro-text { font-size: 20px; line-height: 28px; color: rgba(255,255,255,1); (**`--o-color-white`**) text-align: center; }

/* 导航列：6 列等宽，⚠️ 标题 20px Regular，不是 16px/600 */
.footer-nav   { display: flex; margin-bottom: 24px; }
.footer-col   { flex: 1; }
.footer-col-title { font-size: 20px; line-height: 28px; font-weight: 400; color: rgba(255,255,255,1); (**`--o-color-white`**) margin-bottom: 16px; }
.footer-col a { font-size: 14px; line-height: 22px; color: rgba(255,255,255,.6); (**`--o-color-info3`**) }

/* 友情链接：在导航列之后、分隔线之前 */
.footer-links { display: flex; flex-wrap: wrap; align-items: center; gap: 4px 0; margin-bottom: 24px; }
.footer-links-title { font-size: 12px; font-weight: 200; color: rgba(255,255,255,1); (**`--o-color-white`**) margin-right: 8px; }
.footer-links a { font-size: 12px; color: rgba(255,255,255,.6); (**`--o-color-info3`**) padding: 0 8px; }

/* 分隔线 */
.footer-divider { height: 2px; background: rgba(229,229,229,1); (**`--o-color-grey-1`**) opacity: .12; }

/* 底部信息行：Logo（150×68）| 版权（居中）| 社交媒体（354×52） */
.footer-bottom { display: flex; align-items: center; justify-content: space-between; height: 78px; }
.footer-logo   { width: 150px; height: 68px; }          /* ⚠️ 含邮箱，整体 68px 高 */
.footer-social { width: 354px; height: 52px; }          /* ⚠️ 含二维码+平台 Logo 两行 */

/* 版权：⚠️ 顺序：品牌/隐私/法律声明（上）→ 版权文字（下） */
.footer-copy { text-align: center; display: flex; flex-direction: column; gap: 4px; }
.footer-copy-links { font-size: 14px; color: rgba(255,255,255,1); }
.footer-copy p     { font-size: 14px; color: rgba(255,255,255,.6); }
```

### PC 页脚 HTML 骨架

```html
<footer class="footer">

  <!-- 1. 顶部说明 + 基金会 Logo -->
  <div class="footer-intro">
    <p class="footer-intro-text">openEuler 是由开放原子开源基金会（OpenAtom Foundation）孵化及运营的开源项目</p>
    <!-- ⚠️ 基金会 Logo 用 img，不是文字或自绘 SVG；⚠️ 邮箱不在此处 -->
    <img src="skills/opendesign-design/references/assets/navigation/footer-pc-基金会logo.svg"
         width="160" height="32" alt="OpenAtom Foundation">
  </div>

  <!-- 2. 导航列（6 列）-->
  <div class="footer-nav">
    <div class="footer-col">
      <div class="footer-col-title">关于openEuler</div>   <!-- ⚠️ 无空格 -->
      <a>成员单位</a><a>组织架构</a><a>社区章程</a><a>贡献看板</a><a>社区介绍</a>
    </div>
    <div class="footer-col">
      <div class="footer-col-title">新闻与资讯</div>
      <a>新闻</a><a>博客</a><a>白皮书</a>
    </div>
    <div class="footer-col">
      <div class="footer-col-title">获取与下载</div>
      <a>获取openEuler操作系统</a><a>最新社区发行版</a><a>商业发行版</a><a>软件中心</a>
    </div>
    <div class="footer-col">
      <div class="footer-col-title">支持与服务</div>
      <a>FAQ</a><a>联系我们</a><a>反馈问题</a>
    </div>
    <div class="footer-col">
      <div class="footer-col-title">互动与交流</div>
      <a>邮件列表</a><a>活动</a><a>论坛</a>
    </div>
    <div class="footer-col">
      <div class="footer-col-title">贡献与成长</div>
      <a>SIG中心</a><a>贡献攻略</a><a>课程中心</a>   <!-- ⚠️ 无空格 -->
    </div>
  </div>

  <!-- 3. 友情链接（⚠️ 不可省略） -->
  <div class="footer-links">
    <span class="footer-links-title">友情链接</span>
    <a>木兰开源社区</a><span>/</span><a>鲲鹏社区</a><span>/</span>
    <a>鲲鹏小智</a><span>/</span><a>鹏城实验室</a><span>/</span>
    <a>InfoQ</a><span>/</span><a>开源社</a><span>/</span>
    <a>中科微澜</a><span>/</span><a>Authing</a><span>/</span>
    <a>openGauss</a><span>/</span><a>昇思MindSpore</a><span>/</span><a>Ebaina</a>
  </div>

  <!-- 4. 分隔线 -->
  <div class="footer-divider"></div>

  <!-- 5. 底部信息行 -->
  <div class="footer-bottom">
    <!-- Logo + 邮箱：整体用 150×68 img，邮箱已内嵌在 SVG 里 -->
    <div class="footer-logo">
      <img src="skills/opendesign-design/references/assets/navigation/footer-pc-底部logo.svg"
           width="150" height="68" alt="openEuler">
    </div>

    <!-- 版权：⚠️ 品牌/隐私/法律声明在上，版权文字在下 -->
    <div class="footer-copy">
      <div class="footer-copy-links">品牌<span>|</span>隐私政策<span>|</span>法律声明</div>
      <p>版权所有 © 2024 openEuler 保留一切权利</p>
      <p>遵循 木兰宽松许可证第2版（MulanPSL2）</p>
    </div>

    <!-- 社交媒体：含二维码行+平台 Logo 行，整体用 354×52 img -->
    <div class="footer-social">
      <img src="skills/opendesign-design/references/assets/navigation/footer-pc-社交媒体logo.svg"
           width="354" height="52" alt="社交媒体">
    </div>
  </div>

</footer>
```

---

### 常见错误清单（已踩坑）

| # | 错误 | 正确 |
|---|------|------|
| 1 | 顶导 Logo 用自绘六边形 SVG + 文字 | `<img>` 引用 `header-pc-light logo.svg`（136×32） |
| 2 | 导航菜单 6 项，含"开始" | 7 项：下载/开发/文档/学习/支持/社区/动态 |
| 3 | `nav-left` gap=48px，align=center | gap=40px，align=**flex-end** |
| 4 | 页脚 padding 顶部 40px | 顶部 **24px**，底部 34px |
| 5 | 顶部说明区放邮件图标+邮箱文字 | 放**基金会 Logo img**（160×32），邮箱在底部 Logo SVG 内 |
| 6 | 列标题 16px/weight 600 | **20px/weight 400**（Regular） |
| 7 | 列名含多余空格："关于 openEuler"、"SIG 中心" | "关于openEuler"、"SIG中心"（无空格） |
| 8 | 链接文字："获取 openEuler" | "获取openEuler**操作系统**"（完整名称） |
| 9 | 省略友情链接区 | 分隔线前必须有**友情链接**区 |
| 10 | 底部 Logo 用自绘白色六边形 SVG | `<img>` 引用 `footer-pc-底部logo.svg`（150×68） |
| 11 | 版权顺序：版权文字在上，法律链接在下 | **品牌\|隐私政策\|法律声明在上**，版权文字在下 |
| 13 | 右侧顺序：搜索→国际化→用户→源码 | 搜索→**源码**→国际化→**主题切换**→用户 |
| 14 | 源码 height 32px，无下拉箭头 | height **24px**，含下拉箭头 16×16 |
| 15 | 主题切换图标缺失 | 必须有，24×24px，位于国际化与用户之间 |
| 16 | nav-right gap 16px | gap **20px** |
| 17 | 搜索框背景 `rgba(0,0,0,.04)`，描边 `.12` | OSearch small：背景 **`rgb(255,255,255)`**（white/--o-color-fill2），描边 **`rgba(0,0,0,.25)`**（grey-14@0.25） |
| 18 | 源码按钮蓝色描边 pill，颜色 `#002FA7` | ODropdown **text 变体**：无背景无描边，颜色 **`rgba(0,0,0,.85)`**（--o-color-info1），padding=0 |
| 19 | 外层 `.header` 设 `align-items: flex-end` | 必须设 **`center`**，否则右侧操作区也底部对齐 |
| 20 | Logo 在 flex-end 容器中被拉到底部 | Logo 必须加 **`align-self: center`** 覆盖父级 |
| 21 | Tab 菜单垂直居中，下划线不贴底 | 菜单必须加 **`align-self: flex-end`** 贴底 |
| 22 | 搜索框无 border 无 background 不可见 | 必须设 **`border: 1px solid rgba(0,0,0,.25)`** + **`background-color: #FFFFFF`** |
| 23 | 搜索框用 flex 布局导致内容错位 | Light 模式必须用 **`position: relative`** + 子元素 **`position: absolute`** |
| 24 | "源码"文字与箭头换行 | `width: auto` + **`white-space: nowrap`** |
| 25 | 右侧图标用自绘内联 SVG path | 必须用 **`<img>` 引用 `references/assets/navigation/`** 资源文件 |
| 26 | CSS 样式被覆盖不生效 | 关键对齐属性使用**内联 style + !important** |
| 27 | **使用非蓝色作为主色调** | 品牌色必须是 **#002FA7**（Light）/ **rgb(110,148,243)**（Dark） | 2026实战案例 |
| 28 | **用CSS渐变/内联SVG模拟Logo** | 必须用 `<img src="...header-pc-light logo.svg">` 引用真实资源 | 2026实战案例 |
| 29 | **ODropdown箭头用16px** | 箭头固定 **24×24px**，不随size变化 | dropdown.md 第297行 |
| 30 | **ODropdown圆角用4px或缺失** | 必须是 **100px** 全圆角（胶囊形外观） | dropdown.md 第153行 |
| 31 | **text变体Hover添加背景色** | text变体Hover **只有文字颜色变化，绝对无背景** | dropdown.md 第37行 |
| 32 | **省略菜单项交互状态** | 必须完整实现 Hover(#F0F5FF) / Active(#E6EDFF) / Selected态 | dropdown.md 第91行 |
| 33 | **猜测control2-light的值** | 官方定义为 **#F0F5FF**（极浅蓝），不是灰色！查dropdown-demo.html | 2026实战案例 |
| 34 | **OSearch缺少描边或状态机错误** | small尺寸必须有1px inside描边，Focus时变为brand-6蓝色 | search.md 状态矩阵 |
| 35 | **移动端版权/链接用0.6透明度** | 移动端必须用**white全白** `(255,255,255,1)`，不是 `rgba(255,255,255,.6)` | 颜色规格表⚠️差异 |
| 36 | **拆分整节点SVG**（底部Logo、平台Logo组） | 直接整体引用 `<img>`，禁止用HTML/CSS拆分内部元素 | Part G整节点概念 |
| 37 | **QR码1/2左右颠倒** | `二维码1.svg`=公众号(左)，`二维码2.svg`=小助手(右) | Part C.3资源表 |
| 38 | **CSS!important顺序错误**（媒体查询在前，基础样式在后） | 基础样式→媒体查询（后定义胜出） | H.1 CSS优先级规则 |

---

### H.1 CSS `!important` 使用规范（⛔ 强制）

> **⚠️ 2026实战踩坑记录：PC/Mb Footer 响应式切换失效问题**

#### 🎯 核心原则

CSS `!important` 的优先级遵循 **"后定义胜出"** 原则：当两个同选择器、同特异性的规则都使用 `!important` 时，**后定义的规则会覆盖先定义的规则**。

#### ❌ 禁止的写法

```css
/* ❌ 错误顺序：媒体查询在前，基础样式在后 */
@media (max-width: 768px) {
    .pc-footer {
        display: none !important;  /* 先定义 → 被覆盖！*/
    }
}

.pc-footer {
    display: flex !important;  /* 后定义 → 胜出！导致移动端无法隐藏PC footer */
}
```

**后果：** 在移动端视口下，`.pc-footer` 仍然显示，与 `.mobile-footer` 混在一起。

#### ✅ 正确的写法

```css
/* ✅ 正确顺序：基础样式在前，媒体查询在后 */
.pc-footer {
    display: flex;  /* 基础样式：无!important */
}

@media (max-width: 768px) {
    .pc-footer {
        display: none !important;  /* 后定义 → 生效！移动端隐藏 */
    }
    
    .mobile-footer {
        display: block;  /* 移动端显示 */
    }
}

@media (min-width: 769px) {
    .mobile-footer {
        display: none !important;  /* PC端隐藏移动端 */
    }
    
    .pc-footer {
        display: flex !important;  /* PC端显示 */
    }
}
```

#### 💡 最佳实践

1. **基础样式不加 `!important`** — 仅在媒体查询中使用 `!important`
2. **媒体查询放在基础样式之后** — 确保"后定义胜出"
3. **使用类切换代替直接操作style** — JavaScript中用 `classList.add/remove()` 切换类名
4. **避免内联 style 与 CSS `!important` 冲突** — 内联 style 无法对抗 CSS `!important`

#### 🔧 实战案例：按钮切换视图模式

```javascript
// ✅ 推荐：使用类切换
function simulateMobile() {
    document.body.classList.add('mobile-view');
    window.scrollTo(0, 0);
}

function resetView() {
    document.body.classList.remove('mobile-view');
}
```

```css
/* 对应的CSS */
body.mobile-view .pc-header,
body.mobile-view .pc-footer {
    display: none !important;  /* 用!important对抗原有的!important */
}

body.mobile-view .mobile-header,
body.mobile-view .mobile-footer {
    display: block;
}

body.mobile-view {
    max-width: 360px;
    margin: 0 auto;
}
```

---

## Part Z：硬约束与强制规则（⛔ MANDATORY）

> **⚠️ 违反以下任何一条均为严重错误，生成的代码将被拒绝！**
> 
> 本章节为最高优先级规则，优先级高于Part A-G的所有说明。生成ONavigation组件前，**必须逐条确认**。

### 🚫 绝对禁止的行为（Fatal Errors）

| # | 禁止事项 | 正确做法 | 违规等级 | 典型错误案例 |
|---|---------|---------|---------|-------------|
| **Z-1** | 使用非openEuler品牌色作为主色调 | 主色调必须是 **#002FA7 (rgb(0,47,167))** [Light] / **rgb(110,148,243)** [Dark] | 🔴 **致命** | 使用橙色#E6A23C、绿色等非蓝色系颜色 |
| **Z-2** | 使用内联SVG/CSS渐变/path模拟Logo或图标 | 必须使用 `<img>` 标签引用 `references/assets/navigation/` 目录下的真实SVG文件 | 🔴 **致命** | 用CSS渐变画六边形Logo、手写SVG path |
| **Z-3** | 自定义修改子组件(OSearch/ODropdown)的核心规格 | 严格遵循对应组件Skill文档的所有规范，不得擅自修改尺寸/颜色/间距/状态机 | 🔴 **致命** | OSearch去掉描边、ODropdown改圆角为4px |
| **Z-4** | 省略或伪造资源引用路径 | 所有图标/Logo必须明确写出相对路径，且文件必须真实存在 | 🔴 **致命** | 不写src属性、使用假路径 |
| **Z-5** | 使用错误的Token值或猜测未验证的颜色值 | 所有Token值必须来自官方示例代码(dropdown-demo.html等)或明确的规范文档 | 🔴 **致命** | control2-light写成#FFFFFF或rgba(0,0,0,0.04) |

### ✅ 强制要求的行为（Mandatory Requirements）

| # | 要求项 | 具体内容 | 参考位置 | 验证方法 |
|---|--------|---------|---------|---------|
| **R-1** | **品牌色系统一致性** | primary1 = #002FA7, primary1-dark = rgb(110,148,248), 全局统一使用蓝色系 | [button.md](./button.md) Part B, [Appendix A](#appendix-aonavigation-完整token速查表) | 搜索代码中所有color值，确保无橙/红/绿 |
| **R-2** | **资源完整性** | PC顶导至少需引用8个SVG：Logo(Light+Dark) + 搜索图标 + 清除图标 + 下拉箭头 + 国际化 + 主题切换 + 用户头像 | [Part F](#part-flogo-自定义仅顶导), [Part G](#part-g页脚图形资源已入仓) | 检查所有`<img>`和`mask-image`的src |
| **R-3** | **OSearch完全合规** | small尺寸(32px高) + 1px inside描边 + Enabled/Actived状态机 + 16×16px图标 | [search.md](./search.md) 状态矩阵表, [Appendix B](#appendix-b子组件交互状态矩阵快速参考) | 对照search.md逐项检查 |
| **R-4** | **ODropdown完全合规** | text变体 + small尺寸(28px高) + 100px圆角 + 24×24px箭头 + 无背景Hover + 完整面板交互 | [dropdown.md](./dropdown.md) 交互状态机, [Appendix B] | 对照dropdown.md逐项检查 |
| **R-5** | **交互状态完整性** | 必须实现所有状态：Default/Hover/Focus/Active/Selected/Disabled，每个状态有明确视觉反馈 | 各组件Skill的"交互状态矩阵"章节 | 测试每个状态的视觉效果 |
| **R-6** | **Light/Dark双主题适配** | 所有颜色必须定义Light/Dark两套变量，切换时无遗漏 | [Part C](#part-c颜色与主题) 颜色规格表 | 切换主题后逐元素检查 |

### 🎯 生成前必读Checklist（Pre-flight Checklist）

> **生成任何ONavigation HTML/CSS代码前，必须完成以下检查：**

#### 阶段1：文档阅读（耗时约10分钟）
- [ ] 通读navigation.md全文（特别是本Part Z + Appendices）
- [ ] 通读[search.md](./search.md)的"交互状态矩阵"部分
- [ ] 通读[dropdown.md](./dropdown.md)的"下拉面板交互"部分
- [ ] 打开`references/examples/dropdown-demo.html`查看官方Token定义
- [ ] 打开`references/examples/search-demo.html`查看OSearch示例

#### 阶段2：Token确认（耗时约5分钟）
- [ ] 确认主品牌色：primary1 = #002FA7 (Light)
- [ ] 确认control2-light = #F0F5FF（极浅蓝色，非灰色！）
- [ ] 确认control3-light = #E6EDFF
- [ ] 确认info1 = #000000 (Light) / #FFFFFF (Dark)
- [ ] 将上述值写入CSS变量`:root`

#### 阶段3：资源准备（耗时约3分钟）
- [ ] 列出所有需要的SVG资源清单（至少12个）
- [ ] 确认每个资源文件的相对路径正确
- [ ] 确认文件实际存在于`references/assets/`目录

#### 阶段4：生成后自检（耗时约5分钟）
- [ ] 浏览器打开HTML，控制台无报错
- [ ] Light模式下测试所有Hover效果是否可见
- [ ] Dark模式下测试所有Hover效果是否可见
- [ ] 点击ODropdown展开面板，测试菜单项Hover背景(#F0F5FF)
- [ ] 点击OSearch输入框，测试Focus描边变色
- [ ] 缩放窗口至≤768px，测试移动端布局

---

## Appendix A：ONavigation 完整Token速查表

> **说明**：本附录汇总了ONavigation涉及的所有设计Token，按功能分组。
> 
> ⚠️ **重要提示**：
> - 所有**品牌色均为蓝色系**（#002FA7及其衍生色），绝对禁止使用其他色相
> - Token值来源：官方组件示例代码（dropdown-demo.html、search-demo.html）、各组件Skill文档
> - 如遇冲突，以**官方示例代码**为准

### A.1 品牌色系（Brand Colors - 蓝色系全局统一）

| Token名称 | Light模式 | Dark模式 | RGB精确值 | 使用场景 | 来源 |
|---------|-----------|----------|-----------|---------|------|
| `--o-color-primary1` | **#002FA7** | rgb(73,122,248) | rgb(0,47,167) → rgb(73,122,248) | 主品牌色：Active态文字、Focus描边、选中态 | button.md, dropdown-demo.html |
| `--o-color-primary2` | #0047D9 | — | rgb(0,71,217) | Hover加深态（次要强调） | dropdown-demo.html |
| `--o-color-primary3` | #1A5FE5 | — | rgb(26,95,229) | Press/Active按下态（强反馈） | dropdown-demo.html |
| `--o-color-primary4` | #B3CFFF | — | rgb(179,207,255) | Disabled禁用态（弱化显示） | dropdown-demo.html |

**视觉特征**：
```
primary1 (#002FA7): 深邃的品牌蓝 - 用于最重要的交互反馈
primary2 (#0047D9): 中等蓝色   - 用于Hover次要反馈  
primary3 (#1A5FE5): 明亮蓝色   - 用于Press物理按压感
primary4 (#B3CFFF): 极浅蓝色   - 用于Disabled禁用态
```

### A.2 文字色系（Text Colors）

| Token名称 | Light模式 | Dark模式 | 透明度/描述 | 使用场景 | 来源 |
|---------|-----------|----------|------------|---------|------|
| `--o-color-info1` | **#000000** | **#FFFFFF** | 100%不透明 | 主要文字、默认态文字、菜单项默认文字 | navigation.md Part D, dropdown.md |
| `--o-color-info2` | #333333 | rgba(255,255,255,0.8) | **80%透明度** | Hover态次要文字、用户名区域Hover | navigation.md Part E |
| `--o-color-info3` | #666666 | rgba(255,255,255,0.6) | **60%透明度** | 页脚次要文字、版权声明、导航链接、友情链接 | navigation.md Part C |
| `--o-color-info4` | #999999 | rgba(255,255,255,0.4) | **40%透明度** | 占位符文字、禁用文字、搜索占位符 | search.md, navigation.md Part D |

**使用原则**：
- 导航菜单默认文字 → info1 (100%)
- 占位符"搜索..." → info4 (40%)
- **页脚次要文字（60%透明度）→ info3** ✅ 已修正

### A.3 控制色系（Control Colors - 交互反馈专用）

| Token名称 | Light模式 | Dark模式 | RGB精确值 | 使用场景 | 关键性 |
|---------|-----------|----------|-----------|---------|--------|
| `--o-color-control1-light` | rgba(0,0,0,**0.25**) | rgba(255,255,255,**0.25**) | — | 默认描边（OSearch默认态边框） | ⭐⭐⭐ |
| `--o-color-control2-light` | **#002FA7** | — | **rgb(0,47,167)** | Hover描边（OSearch Focus态边框） | ⭐⭐⭐ |
| `--o-color-control2-light` | **#F0F5FF** | rgba(110,148,243,0.08) | **rgb(240,245,255)** | **菜单项Hover背景（极浅蓝！）** | 🔥🔥🔥 **最易错** |
| `--o-color-control3-light` | **#E6EDFF** | — | **rgb(230,237,255)** | **菜单项Active/Press背景（浅蓝）** | 🔥🔥🔥 **常遗漏** |
| `--o-color-control4-light` | #F5F5F5 | — | rgb(245,245,245) | Disabled禁用态填充背景 | ⭐⭐ |

**⚠️ 历史错误警告**：

```
❌ 错误1: control2-light = #FFFFFF (纯白) → 与面板背景相同，不可见！
❌ 错误2: control2-light = rgba(0,0,0,0.04) (灰色调) → 不符合品牌蓝色系！
✅ 正确答案: control2-light = #F0F5FF (极浅蓝色，RGB 240,245,255)

正确记忆法："F0F5FF" = "Found Official Five For First" (找到官方的前五个字符)
```

### A.4 填充色系（Fill Colors - 背景色）

| Token名称 | Light模式 | Dark模式 | 使用场景 |
|---------|-----------|----------|---------|
| `--o-color-fill2-light` | **#FFFFFF** (纯白) | **rgb(36,36,39)** (深灰) | 输入框背景、面板背景、顶导背景 |
| `--o-color-fill3-light` | #F5F6F7 | — | 次要背景、卡片背景 |

**关键应用**：
```css
/* OSearch 背景 */
.o-search { background: var(--o-color-fill2-light); }  /* white / grey-4 */

/* ODropdown Panel 背景 */
.o-dropdown__panel { background: var(--o-color-fill2-light); }  /* white / grey-4 */
```

### A.5 特殊用途颜色（Special Purpose）

| 元素 | 属性 | Light模式精确值 | Dark模式精确值 | 备注 |
|------|------|---------------|---------------|------|
| PC顶导背景 | fill | rgba(255,255,255,**1**) | rgba(36,36,39,**1**) | 100%不透明 |
| PC顶导Dark底边框 | stroke | rgba(255,255,255,**0.149**) | — | 仅1px bottom |
| PC页脚背景 | fill | rgba(18,18,20,**1**) | 同左 | 始终深色 |
| 移动端页脚背景 | fill | rgba(18,18,18,**1**) | 同左 | 与PC页脚细微不同 |
| 页脚导航链接 | fill | rgba(255,255,255,**0.60**) | 同左 | 60%白色 |
| 页脚分隔线 | fill | rgba(229,229,229,**1**), opacity **0.12** | — | 2px高度 |
| 平台Logo背景 | fill | rgba(43,43,47,**1**) | 同左 | 社交媒体图标容器 |
| 阴影通用 | boxShadow | 0px 3px 9px rgba(**0,18,85**,0.078) | — | 顶导阴影 |
| 下拉面板阴影 | boxShadow | x=0 y=6 blur=24 rgba(**18,20,23**,0.08) | — | ODropdown面板 |

---

## Appendix B：子组件交互状态矩阵（快速参考）

> **说明**：本附录提供OSearch和ODropdown在ONavigation中的完整交互状态速查，
> 避免需要反复翻阅多个文档。所有数据均来自对应组件Skill的官方定义。

### B.1 OSearch small 尺寸（搜索框 - 160×32px）

**基本信息**：
- 尺寸：small = 32px高
- 图标：16×16px（搜索图标 + 清除按钮）
- 描边：1px INSIDE（内描边）
- 圆角：4px

**⚠️ 重要提示（2026实战总结）**：
> OSearch有**三种描边状态**，不是两种！必须实现Hover态！
> - Enabled默认 → Hover(Focus前) → Actived(Focus后)
> - 很多开发者遗漏了Hover状态！

**完整状态矩阵表（8种组合）**：

| 状态 | 文本场景 | 触发条件 | 背景色 | 描边色(Token) | 描边色(RGB值) | 清除按钮 | 占位符/文字色 | 光标 | 搜索建议面板 |
|-----|---------|---------|--------|-------------|-------------|---------|------------|------|------------|
| **Enabled** | 无文本 | 初始加载 | white | `--o-color-control1` | rgba(0,0,0,**0.25**) | ❌隐藏 | grey-14@0.40 | ❌ | ❌ |
| **Enabled** | 有文本 | 输入后失焦 | white | `--o-color-control1` | rgba(0,0,0,0.25) | ❌隐藏 | **info1黑** | ❌ | ❌ |
| **Hover** | 无文本 | 鼠标悬浮(未聚焦) | white | **`--o-color-control2`** | **#002FA7 (brand-6)** | ❌隐藏 | grey-14@0.40 | ❌ | ❌ |
| **Hover** | 有文本 | 鼠标悬浮(未聚焦) | white | **`--o-color-control2`** | **#002FA7 (brand-6)** | ❌隐藏 | info1黑 | ❌ | ❌ |
| **Actived** | 无文本 | 点击/聚焦 | white | **`--o-color-control3`** | brand-6主色调 | ❌隐藏 | grey-14@0.40 | ✅显示 | ❌ |
| **Actived** | 有文本 | 聚焦+输入中 | white | **`--o-color-control3`** | brand-6主色调 | ✅显示 | info1黑 | ✅保持 | ✅**展开** |
| **Disabled** | 无文本 | disabled属性 | control4-light | `--o-color-control4` | 低对比度灰 | ❌隐藏 | info4@灰 | ❌禁止 | ❌ |
| **Disabled** | 有文本 | disabled+预填 | control4-light | `--o-color-control4` | 低对比度灰 | ❌隐藏 | info4@灰 | ❌禁止 | ❌ |

**🔥 核心交互逻辑（必须实现）**：

```css
/* 1️⃣ Enabled默认态 */
.o-search {
    height: 32px;                              /* small尺寸 */
    background: var(--o-color-fill2-light);   /* white / grey-4 */
    border: 1px solid var(--o-color-control1);/* 默认描边: rgba(0,0,0,0.25) */
    border-radius: 4px;
}

/* 2️⃣ Hover悬浮态（仅限Enabled状态！） */
.o-search:hover:not([data-state="actived"]) {
    border-color: var(--o-color-control2);    /* Hover描边: #002FA7 brand-6蓝 ⭐新增！*/
}

/* 3️⃣ Actived激活/Focus态 */
.o-search[data-state="actived"],
.o-search[data-state="actived"]:hover {       /* 注意：Actived的Hover不改变描边！ */
    border-color: var(--o-color-control3);    /* Active描边: brand-6主色调 */
}

/* 4️⃣ 清除按钮显隐逻辑 */
.o-search__clear { display: none; }           /* 默认隐藏 */
.o-search[data-state="actived"] .o-search__clear { display: block; }  /* Focus时显示 */
```

**常见错误清单（本次实战血泪教训）**：
- ❌ 只实现了Enabled和Actived两种状态 → **必须实现3种：Enabled/Hover/Actived**
- ❌ OSearch没有Hover样式 → **Enabled时Hover要变蓝#002FA7**
- ❌ Focus时改变宽度或背景 → **应该只改变描边颜色**
- ❌ 清除按钮始终显示 → **仅Actived状态才显示**
- ❌ 缺少搜索建议面板 → **Actived+有文本时要显示下拉建议列表**

---

### B.1.1 搜索建议下拉面板规格（OSearch专用）

> **说明**：当OSearch处于Actived状态且**有输入内容**时，自动展开搜索建议面板。
> 
> ⚠️ **重要差异**：此面板与ODropdown面板规格略有不同！

**面板基本信息**：

| 规格项 | 值 | 与ODropdown的差异 |
|--------|-----|-----------------|
| **与触发元素间距** | 4px | 相同 |
| **面板圆角** | 4px | 相同 |
| **面板内边距** | 4px（四周） | 相同 |
| **面板背景(Light)** | rgb(255,255,255) | 相同 |
| **面板阴影** | x=0 y=6 blur=24 rgba(18,20,23,0.08) | 相同 |
| **菜单项高度** | **40px** (control_size-l) | ❌ **ODropdown是32px！** |
| **菜单项内边距** | 上下8px，左右12px | 相同 |
| **菜单项间距** | 8px（最后一项为0） | ❌ **ODropdown是0px！** |
| **菜单项字号** | 14px | 相同 |
| **Hover背景色** | **#E8F0FE** | ❌ **ODropdown是#F0F5FF！** |
| **Hover圆角** | 2px | 相同 |

**关键词高亮规则（硬约束）**：

| 元素 | 字重 | 颜色(Token) | RGB值 | 示例 |
|------|------|-----------|-------|------|
| **匹配关键词** | Semibold (600) | `--o-color-primary1` | rgb(0,47,167) 蓝 | **openEuler**迁移 |
| **非匹配文本** | Regular (400) | `--o-color-info2` | rgba(0,0,0,0.8) 80%黑 | openEuler**迁移** |

**面板HTML结构示例**：
```html
<div class="o-search-suggest">
    <div class="o-search-suggest__item">
        <span class="o-search-suggest__match">openEuler</span><!-- 蓝色粗体 -->
        <span class="o-search-suggest__normal">迁移</span><!-- 黑色常规 -->
    </div>
    <div class="o-search-suggest__item">
        <span class="o-search-suggest__match">openEuler</span>
        <span class="o-search-suggest__normal">安装</span>
    </div>
</div>
```

**CSS实现要点**：
```css
.o-search-suggest {
    position: absolute;
    top: calc(100% + 4px);                    /* 搜索框下方4px */
    width: 100%;                               /* 与搜索框同宽 */
    background: var(--o-color-fill2-light);
    box-shadow: 0 6px 24px rgba(18,20,23,0.08);
}

/* 仅在Actived+有内容时显示 */
.o-search[data-state="actived"]:has(input:not(:placeholder-shown)) .o-search-suggest {
    opacity: 1;
    visibility: visible;
}

.o-search-suggest__item {
    height: 40px;                              /* ⚠️ 必须是40px，不是32px！*/
    padding: 8px 12px;
    margin-bottom: 8px;                        /* ⚠️ 项间距8px！*/
}

.o-search-suggest__item:hover {
    background: #E8F0FE;                       /* ⚠️ 是#E8F0FE，不是#F0F5FF！*/
}

.o-search-suggest__match {
    color: var(--o-color-primary1-light);
    font-weight: 600;                          /* 匹配词加粗 */
}

.o-search-suggest__normal {
    color: var(--o-color-info2-light);         /* 80%黑色 */
    font-weight: 400;
}
```

**⚠️ Token冲突警告**：

```
⚠️ control2-light 在不同组件中有不同值！

ODropdown菜单项Hover背景:
  --o-color-control2-light = #F0F5FF  (用于ODropdown)

OSearch建议项Hover背景:
  --o-color-control2-light = #E8F0FE  (用于OSearch)

✅ 解决方案：使用语义化变量名
  --o-dropdown-hover-bg: #F0F5FF;     (ODropdown专用)
  --o-search-suggest-hover-bg: #E8F0FE;(OSearch专用)
```

### B.2 ODropdown text + small 变体（源码下拉按钮 - 约48×28px）

**基本信息**：
- Variant：text（无背景、无描边、无Padding左右）
- Size：small = 28px高
- 圆角：**100px**（全圆角胶囊形）
- 图标：**24×24px**（固定值，不随size变化！）
- Padding(text)：上下5px，左右**0px**
- 图标间距：4px

**触发按钮状态矩阵**：

| 状态 | 按钮背景 | 按钮文字色 | 按钮图标色 | 面板显示 | 视觉特征 |
|-----|---------|-----------|-----------|---------|---------|
| **Default(默认)** | **透明**<br>(无背景！) | info1<br>黑/白 | info1<br>与文字一致 | ❌ 隐藏 | 纯文字+下箭头 |
| **Hover(悬浮)** | **透明**<br>(**无背景！**) | **primary1 蓝**<br>#002FA7 | **primary1 蓝** | — | 仅文字变色，背景保持透明 |
| **Click/Active(点击)** | 透明 | info1 | info1 | ✅ **显示** | 面板从下方滑出 |
| **Expanded(展开)** | 透明 | info1 | info1 | ✅ 显示 | 箭头旋转180° |
| **Disabled(禁用)** | 透明 | info4@灰<br>rgba(0,0,0,0.4) | info4@灰 | ❌ 隐藏 | 整体置灰 |

**⚠️ 核心要点**：text变体的Hover **绝对不能有背景色变化**，只能通过文字颜色变化提供反馈！

**下拉面板状态矩阵**：

| 面板状态 | 面板背景 | 面板定位 | 面板阴影 | 面板圆角 | 面板内边距 |
|---------|---------|---------|---------|---------|-----------|
| **收起** | 不可见 | — | — | — | — |
| **展开** | white<br>(#FFFFFF) | 按钮下方**4px**处 | DROP_SHADOW<br>x=0 y=6 blur=24<br>rgba(18,20,23,0.08) | **4px**<br>(radius_control-xs) | **4px**<br>(四周) |

**面板内菜单项状态矩阵**：

| 菜单项状态 | 触发条件 | 背景色 | 文字颜色 | 字重 | 其他效果 |
|-----------|---------|--------|---------|------|---------|
| **Default(默认)** | 初始渲染 | 透明/无 | info1<br>黑/白 | Regular(400) | — |
| **Hover(悬浮)** | 鼠标进入 | **#F0F5FF**<br>(**极浅蓝！**) | **primary1 蓝**<br>#002FA7 | Regular(400) | cursor:pointer |
| **Active/Press(按下)** | 鼠标按下 | **#E6EDFF**<br>(**浅蓝**) | primary1 蓝 | Semibold(600) | scale(0.98)微缩 |
| **Selected(选中)** | 点击选择 | rgba(0,47,167,<br>**0.08**) | primary1 蓝 | **Medium(500)**<br>加粗 | 持续显示直到面板关闭 |
| **Disabled(禁用)** | disabled属性 | 透明 | info4@灰 | Regular(400) | cursor:not-allowed |

**关键CSS实现要点**：
```css
/* 触发按钮 - text变体 */
.o-dropdown {
    height: 28px;              /* small尺寸 */
    padding: 5px 0;            /* text变体：上下5px，左右0！ */
    background: transparent;   /* 无背景 */
    border: none;              /* 无描边 */
    border-radius: 100px;      /* 全圆角胶囊形 */
    color: var(--o-color-info1-light);
}

.o-dropdown:hover {
    color: var(--o-color-primary1-light);  /* Hover只改文字色，不改背景！ */
}

.o-dropdown__arrow {
    width: 24px;               /* 固定24×24，不随size变化！ */
    height: 24px;
}

/* 下拉面板 */
.o-dropdown__panel {
    top: calc(100% + 4px);     /* 按钮下方4px */
    background: var(--o-color-fill2-light);
    box-shadow: 0 6px 24px rgba(18, 20, 23, 0.08);
    border-radius: 4px;
    padding: 4px;
}

/* 菜单项 - 核心交互 */
.o-dropdown__item:hover {
    background: #F0F5FF;       /* 极浅蓝色Hover背景 */
    color: var(--o-color-primary1-light);
}

.o-dropdown__item:active {
    background: #E6EDFF;       /* 浅蓝色Active背景 */
    transform: scale(0.98);
}

.o-dropdown__item.selected {
    background: rgba(0, 47, 167, 0.08);  /* 选中态浅蓝 */
    font-weight: 500;
}
```

**常见错误清单（本次实战总结）**：
- ❌ 箭头用了16px → 必须是**24×24px**
- ❌ 圆角是4px或缺失 → 必须是**100px**
- ❌ text变体Hover加了背景 → 必须**只有文字变色**
- ❌ 菜单项Hover背景用了灰色 → 必须是**#F0F5FF极浅蓝**
- ❌ 缺少Selected状态 → 必须实现**选中后的持续反馈**
- ❌ 面板位置偏移 >4px → 必须精确**4px**

---

## Appendix C：资源引用完整清单（Resource Checklist）

> **说明**：生成ONavigation HTML时，必须引用以下所有资源文件。
> 
> 资源根目录：`skills/opendesign-design/references/assets/`

### C.1 PC顶导必需资源（8个）

| # | 文件名 | 相对于HTML的路径 | 尺寸 | 使用位置 | 引用方式 |
|---|--------|----------------|------|---------|---------|
| 1 | `header-pc-light logo.svg` | `../references/assets/navigation/header-pc-light logo.svg` | 136×32px | Logo区(Light模式) | `<img src="...">` |
| 2 | `header-pc-dark logo.svg` | `../references/assets/navigation/header-pc-dark logo.svg` | 136×32px | Logo区(Dark模式) | `<img src="...">` |
| 3 | `header-pc-search icon.svg` | `../references/assets/navigation/header-pc-search icon.svg` | 16×16px | OSearch搜索图标 | CSS mask 或 `<img>` |
| 4 | `icon-搜索.svg` | `../references/assets/public icons/icon-搜索.svg` | 16×16px | OSearch备选图标 | `<img>` |
| 5 | `icon-关闭.svg` | `../references/assets/public icons/icon-关闭.svg` | 16×16px | OSearch清除按钮 | `<img>` (仅Actived显示) |
| 6 | `icon-下箭头.svg` | `../references/assets/public icons/icon-下箭头.svg` | 24×24px | ODropdown下拉箭头 | `<img>` |
| 7 | `header-pc-语言切换 icon.svg` | `../references/assets/navigation/header-pc-语言切换 icon.svg` | 20×20px | 国际化按钮 | CSS mask 或 `<img>` |
| 8 | `header-pc-深浅模式.svg` | `../references/assets/navigation/header-pc-深浅模式.svg` | 20×20px | 主题切换按钮 | CSS mask 或 `<img>` |
| 9 | `header-pc-头像.svg` | `../references/assets/navigation/header-pc-头像.svg` | 20×20px | 用户头像 | CSS mask 或 `<img>` |

**引用方式说明**：
- **推荐方式**：Logo用`<img src="...">`直接引入
- **Icon两种方式均可**：CSS mask（支持动态着色）或`<img>`（简单直接）
- **禁止方式**：❌ 内联SVG path、❌ CSS渐变模拟、❌ Unicode字符替代

### C.2 PC页脚必需资源（5个）

| # | 文件名 | 路径 | 尺寸 | 使用位置 |
|---|--------|------|------|---------|
| 1 | `footer-pc-基金会logo.svg` | `references/assets/navigation/footer-pc-基金会logo.svg` | 160×32px | 页脚顶部基金会标识 |
| 2 | `footer-pc-底部logo.svg` | `references/assets/navigation/footer-pc-底部logo.svg` | 150×68px | 页脚底部OpenEuler Logo+邮箱 |
| 3 | `footer-pc-社交媒体logo.svg` | `references/assets/navigation/footer-pc-社交媒体logo.svg` | 354×52px | 二维码+平台Logo组合 |
| 4 | `footer-mb-二维码1.svg` | `references/assets/navigation/footer-mb-二维码1.svg` | 78×78px | 小助手二维码 |
| 5 | `footer-mb-二维码2.svg` | `references/assets/navigation/footer-mb-二维码2.svg` | 78×78px | 公众号二维码 |

### C.3 移动端页脚额外资源（3个）

| # | 文件名 | 路径 | 尺寸 | 使用位置 |
|---|--------|------|------|---------|
| 1 | `footer-mb-基金会logo.svg` | `references/assets/navigation/footer-mb-基金会logo.svg` | 150×30px | 移动端基金会Logo |
| 2 | `footer-mb-底部logo.svg` | `references/assets/navigation/footer-mb-底部logo.svg` | 86×34px | 移动端底部Logo |
| 3 | `footer-mb-平台logo.svg` | `references/assets/navigation/footer-mb-平台logo.svg` | 232×49px | 移动端社交平台Logo组 |

**总计：PC顶导9个 + PC页脚5个 + Mb页脚3个 = 至少17个资源文件**

---

## 技术备注

- 组件集合节点 ID：`1303:10353`
- 设计稿 URL：https://pixso.cn/app/design/JZkjW0mhmT61Mtd98dCfBw?item-id=1303:10353
- 生成日期：2026-04-28
- 变体总数：6（PC/Mb × 顶导 Light/Dark + PC/Mb 页脚）
- 顶导尺寸：PC=1920×72px，Mb=360×76px（含 18px 状态栏）
- 页脚尺寸：PC=1920×460px，Mb=360×458px
- PC 顶导背景：Light=rgba(255,255,255,1)，Dark=rgba(36,36,39,1)
- Dark 顶导底部描边：rgba(255,255,255,0.149)，1px，bottom only
- PC 页脚背景：rgba(18,18,20,1)
- 移动端页脚背景：rgba(18,18,18,1)（与 PC 页脚细微不同）
- 字体族：HarmonyHeiTi（Regular 400 / Medium 500 / UltraLight 200）
- 本地变量集：无（颜色均为硬编码值）
- 移动端页脚多"服务状态"链接、版权年份 2022（PC 为 2024）