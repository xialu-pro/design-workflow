# Changelog

本文件记录 `opendesign-design` skill 的变更，供使用者判断是否需要重新安装。`SKILL.md` frontmatter 的 `last_update` 字段与本文件中最新条目日期对应。

分类：新增 / 更新 / 修正 / 移除 / ⚠️ 破坏性（破坏性表示按旧版 skill 已生成的产物失效或违规，需复核）。

---

## 2026-10-31

### 新增
- 公共图标库扩容：`references/assets/public icons/` 新增 150 个线性风格图标（21 → 171 个），命名沿用 `icon-{中文名}.svg` 约定（另含少量英文键名：icon-APP / icon-NPS / icon-pdf / icon-hardwares / icon-library-name）。内容覆盖互动（收藏 / 分享 / 评论 / 邮件）、状态（已读 / 未读 / 已签到 / 有病毒）、操作（上传 / 下载 / 复制 / 全选 / 拖拽 / 清空）、媒体（视频 / 音频 / 二维码 / 上传图片）、业务场景（大赛 / 峰会 / 课程 / 实验 / 许可证 / 实名认证 / 制卡工具）等。纯资产新增、无规范变更，已生成产物不受影响；已安装使用者拉取更新后，生成页面时图标命中 `references/assets/` 的比例显著提高，减少降级到手写 SVG 的场景。

---

## 2026-10-30

### 第二十六轮：换社区 Token 圆角错漏的机制性修复（档位圆角全链路收口）

> 来源：维护者反馈——换非 openEuler 社区 tokens 生成页面时「只对了颜色、漏了圆角」，每次需人工逐处校对。根因是社区差异链路只押在颜色维度：起手式翻译清单、QA 检查项、跨社区覆盖条款均未覆盖圆角，而组件文档的档位圆角直值全为 openEuler 基准（多数 4px）。本轮把「社区差异」收敛到 `:root` 一个点：CSS 一律引用 `var(--o-radius_*)`，换社区只换 `:root`，全页圆角自动跟随。

#### 一、链路补全（3 处）

| # | 文件 | 修复 |
|---|------|------|
| 1 | `SKILL.md` 第零步 | Token 翻译清单「颜色/字号/间距」→ 全部变量（颜色、字号/行高、间距、栅格、圆角、阴影、控件/图标尺寸），点名圆角随社区差异最大（平直社区全 4px / 内容 xl 档 0px、递进社区 4/8/12/16——六主题口径见 §7.1）并禁止沿用 openEuler 基准值 |
| 2 | `SKILL.md` 第六步 / 第八步 + `global/hard-constraints.md` §6 | 自检清单各补圆角两项：`:root` 圆角档位取自目标社区 Token；全文无硬编码档位圆角（`border-radius: 数字px` 仅允许形状语义特例） |
| 3 | `SKILL.md` 通用生成约束 + 第五步规范表 + CodeGen 对齐约束；`global/hard-constraints.md` §0 / §6#15 | 跨社区覆盖条款从「参考色值」扩为「色值、档位圆角直值、阴影」；§0 起手式去掉「默认拉取 openeuler-token.json」的无条件表述（改为目标社区 Token + 全量变量翻译） |

#### 二、全局规则立规（1 处）

| # | 文件 | 内容 |
|---|------|------|
| 4 | `global/hard-constraints.md` §7.1（新增） | 「圆角两类与社区规则」：**档位圆角**（对应 `--o-radius-*` / `--o-radius_control-*` 档位）一律 `var()` 引用、值随目标社区 Token；**形状语义圆角**（胶囊 100px、Tab 指示器 9px、carousel 3/6px、hover 微圆角 2px 等无档位直值）不随社区变化。§7 圆角行同步指向，并修正指向已不存在目录 `atomic-components/` 的失效引用（→ `components/`）；附六主题圆角口径表（2026-10-30 维护者裁定：e/k/g/u 平直全 4px 且内容 xl 档 0px、a/m 递进 4/8/12/16 / 内容 xl 24px；g/u 与包产物不一致——v0.1.1 CSS 与上游 JSON 双源核实为递进，差异已在表中标注） |

#### 三、组件文档与楼层模板去死值（7 个文件）

| # | 文件 | 修复 |
|---|------|------|
| 5 | `floors/navigation.md` | 搜索框 `border-radius: 4px` → `var(--o-radius_control-xs)`（禁令注释与规范说明两处表述同步） |
| 6 | `floors/navigation-dropdown.md` | 面板 / 选项圆角 CSS 与结构树、规格表、自检项 → `var(--o-radius_control-xs)` |
| 7 | `floors/navigation-dropdown-categorized.md` | 面板「0 0 4px 4px」压平写法 → `0 0 var(--o-radius_control-xs) var(--o-radius_control-xs)`；OTag → `var(--o-radius_control-m)`；快捷链接灰底块 / 缩略图 → `var(--o-radius_control-xs)` |
| 8 | `components/search.md` | 搜索框 / 建议面板圆角 → `var(--o-radius_control-xs)`（hover 微圆角 2px 属形状语义，保留直值） |
| 9 | `components/navigation.md` | 搜索框 / 下拉面板圆角 CSS、B.1 要点、面板状态矩阵 → `var(--o-radius_control-xs)` |
| 10 | `components/dropdown.md` | 面板圆角 CSS、规格表、R-6 校验项、自检项 → `var(--o-radius_control-xs)`；两处错误档名 `--o-radius-control-xs`（连字符格式，token 包中不存在）→ `--o-radius_control-xs` |
| 11 | `components/dialog.md` | 🔴 档名纠正：`radius_control-sm`（**不存在的档位**，真实档位为 xs/s/m/l）→ `radius_control-s`（5 处：结构树 ×2、规格表 ×2、Token 映射表 ×1），取值改为 `var(--o-radius_control-s)`（openEuler 4px / openGauss 8px）；识别特征与状态表两处 8px 锚点同步 |

#### 四、配套更新（2 处）

| # | 文件 | 内容 |
|---|------|------|
| 12 | `global/qa-checklist/component-html-qa-checklist.md` | 「Token 引用正确」「特殊效果」两节补档位圆角检查（var 引用 + `:root` 取自目标社区 + 形状语义特例白名单） |
| 13 | `global/component-doc-spec.md` | 圆角示例行修正（错误档名 `--o-radius-control-m` → `--o-radius_control-m`，值标注随目标社区）；新增落笔规则：档位圆角标注 Token 档位、直值视为 openEuler 基准，形状语义直值保留，禁止不存在的档位名 |

#### 五、口径裁定（2026-10-30 维护者）

- **openGauss (g) / openUBMC (u) 圆角口径为平直**（全档 4px、内容 xl 档 0px），与包产物不一致——npm `@opensig/opendesign-token` v0.1.1 CSS 与上游 master JSON（双源核实）中两主题为递进值（控件 4/8/12/16、内容 4/8/12/16/24）。按裁定：上游源码不改，`opendesign-tokens` 的 `tokens-opengauss.md` / `tokens-openubmc.md` 圆角段按口径改写并标注差异（详见该 skill 同日 CHANGELOG）；本 skill 的六主题口径表（§7.1）与示例值（card-grid「openGauss 12px」、dialog「openGauss 8px」改以 Ascend 为递进示例）同步更新。
- **主路径取数覆盖（同日维护者选定）**：第零步新增 openGauss / openUBMC 圆角取数覆盖——两社区圆角档位不从上游 JSON 翻译，`:root` 按本仓口径取平直值（全档 4px、xl 档 0px），其余变量照常；第六步自检项与 §7.1 差异说明同步更新。上游对齐后撤销该覆盖。

#### 六、影响评估

⚠️ **破坏性**（硬约束变化）：按旧版规范生成的产物中，档位圆角写死 px 直值的（尤其非 openEuler 社区页面）不再合规——对照 §7.1 复核：CSS 改为 `var(--o-radius_*)`、`:root` 档位值取自目标社区 Token。另：产物中若引用过 `radius_control-sm` / `--o-radius-control-*`（均不存在，`var()` 静默失效），需按 `radius_control-s` / `--o-radius_control-xs` 复核修正。形状语义圆角（胶囊 100px 等）不受影响。

---

## 2026-10-29

### 第二十五轮：全流程验证暴露的存量规范问题批量修复（18 项）

> 来源：2026-10-29 全流程验证（openGauss PC 新首页：4 个楼层子代理 + 组装验证）暴露的存量问题 backlog，去重后按「修法有明确裁决依据」筛选 18 项直接修复（spec-backlog-round-25 分支）；另有 4 项需设计裁决（见文末遗留）。本轮不涉工作流与交互层（第二十四轮范畴），全部为规范文本层的矛盾修正、缺口补齐与口径澄清。

#### 一、A 类：文档矛盾与错误引用（12 处）

| # | 文件 | 修复 |
|---|------|------|
| 1 | `floors/navigation.md` | 「两种样式可混挂」→「二选一、禁止混挂」——2026-10-19 纠错时 SKILL.md 与两份下拉模板已改、本文件漏改的存量矛盾 |
| 2 | `floors/navigation.md` 骨架 | 删 `.o-nav__link` 显式 `height: 48px`（与自身踩坑 #20 矛盾，border-box 下 48 与 24+24+2=50 有 2px 歧义）——改行高 + padding 自然撑高并注释 |
| 3 | `floors/navigation.md` 速查表 + CSS 骨架 | 顶导阴影 `0 3px 9px 0 rgba(0,18,85,0.078)`（openEuler 稿残留）→ Token 真源 `0 3px 8px rgb(var(--o-grey-13) / 0.08)` |
| 4 | `components/navigation.md` + `floors/footer.md` | 同款 shadow 旧值同步修正；顺手修正页脚链接「white@0.6 (info3)」误标（info3 随主题翻转，页脚固定深色区禁用——改为特例写法说明） |
| 5 | `components/navigation.md` + `floors/footer.md` | 友情链接标题行高 12px/24px → 12px/18px（违反硬约束 §2 成对锁定；验证轮 footer 楼层裁定） |
| 6 | `components/button.md` | 「solid 在右（主操作）」→「从左到右 solid → outline」（与 hard-constraints §1.1 相反）；「表单底部间距 8–12px」→ 固定 24px（同款冲突） |
| 7 | `components/card.md` | 卡片标题字重 bold(700) → Semibold(600)（Part C 交互表与 Part D 映射表 600/700 并存，违反 §0.2 一致性铁律；按 4 处证据 vs 2 处统一为 600） |
| 8 | `components/search.md` Part Z-2 / R-2 / B.3 | 「必须 `<img>` 引用图标」→「Read 后逐属性内联 `<svg>`」（与 SKILL.md「禁止 `<img src>`」直接冲突） |
| 9 | `SKILL.md` 异常降级表 | 降级指引两处不一致（表内 designer-guide 速查表 vs 第零步正文 opendesign-tokens 六主题文件）→ 统一为第零步口径，速查表降为 openEuler Light 辅助 |
| 10 | `SKILL.md` 第五步组件命名 | 示例 `.o-card` 与 card.md 权威 DOM `.ocard` 不符 → 改为「组件类名以各组件规范权威 DOM 为唯一真源」 |
| 11 | `global/hard-constraints.md` §2 | 「三级标题（列表卡片 < 3 列）」→「≤ 3 列」（3 列场景两档均未覆盖的空档） |
| 12 | `../opendesign-tokens/references/tokens.md` + tokens-opengauss.md | `rgba(var(--o-grey-13), 0.08)` 逗号混写（§0 判定非法语法，浏览器整条静默丢弃）→ 斜杠语法 `rgb(var(--o-grey-13) / 0.08)`（详见 opendesign-tokens CHANGELOG 同日条目） |

#### 二、B 类：结构性缺口补齐（2 项）

| # | 文件 | 补齐 |
|---|------|------|
| 13 | `floors/card-grid.md` | 补 SKILL.md 第四步强制的四节（生成前必读 / 已确认清单 / 冲突声明 / 踩坑记录），内容从验证轮沉淀；icon 卡旧口径（padding 24px 32px / gap 16px，card.md 2026-10-23 已废止）改为指向分档表；圆角写死 4px 改按目标社区 `--o-radius_control-m`（cover / icon 两处 + 自检项） |
| 14 | `floors/footer.md` | 同款四节补齐（含 ⏳ 页脚高度 460px 不自洽的待确认登记）；友链行高修正入已确认清单 |

#### 三、C/D 类：Token 补齐与口径澄清（4 项）

| # | 文件 | 内容 |
|---|------|------|
| 15 | `../opendesign-tokens/references/tokens-opengauss.md` | 新增「文字色 Token（Info）」节：info1~4 Light 取通用定义、Dark 按 §0.1 推导（标注推导值与待复核）——验证轮三个楼层均靠临时推导落地，现固化为文档 |
| 16 | `SKILL.md` 通用生成约束 | 新增「跨社区内容适配」条款：内容示例与品牌资产随目标社区替换、无素材区域省略 + 说明、openEuler 参考值以目标社区 Token 覆盖（验证轮 openGauss 全链路实践沉淀） |
| 17 | `SKILL.md` 第五步响应式行 | 澄清：楼层模板已给出分档值的以模板为准、禁止改用 `--o-r-*` 替换（楼层模板 CSS 骨架视为已确认断点特例；如 Banner 正文 MB 档 12/18，直接套 `--o-r-font_size-h4` 会错成 16/24） |
| 18 | `global/hard-constraints.md` | 字体铁律澄清（铁律管首选字体，`--o-font_family` 回退链按上游原样）；§3 新增「组件级几何参数」例外（明示强制值如页脚底 padding 34px 不受白名单约束、引用须注明出处） |

#### 四、遗留裁决项（4 项，待维护者拍板）

1. 841~1680px 中间档楼层容器方案（验证轮采用 max-width 1488 + 栅格留白 Token 推断方案，目前规范空白）
2. 页脚高度 460px / 458px 的处理（openEuler 内容集导出值、结构值累加不自洽 ≈510px——改内容自然高度或给各内容档高度参考表；已登记 footer.md 已确认清单 ⏳ 项）
3. openGauss Dark 值补齐（info 系与 control2/3-light 的 Dark 值为本仓推导，启用 Dark 模式前需复核）
4. UltraLight(200) 无上游字重 Token（最细一档为 250）与 18px 图标不在 §5 白名单——补 Token 或改值

#### 五、影响评估

全部为规范文本修正与补齐，**无破坏性**（工作流、确认载体、Token 值本身均未变）；按旧版规范生成的产物中受影响项（按钮排列方向、卡片圆角、友链标题行高、shadow 值、`<img>` 图标引用）对照新规范时按需复核。

---

### 第二十四轮：页面需求单与楼层工单——「输入对齐」从用户手填门槛改为 AI 确认载体

> 根因：skill 原生工作流（第一步 AI 规划楼层 → 用户确认）本身低门槛，但实际流传的「任务需求单 / 楼层工单」YAML 模版把楼层分类学（type / variant / 组件选型）与流程配置（strict_check / output.format / image_policy）转嫁给了用户手填——熟练作者填得对、效果稳定；其他使用者填错枚举 / 缩进 / 选型则触发连环停问或规划偏移，还原度参差。本轮**不改任何生成规格**，只重排「谁提供什么信息」：结构化单据保留，改为 AI 生成草稿、用户确认修改。

#### 一、改动（5 处）

| # | 文件 | 改动 |
|---|------|------|
| 1 | `references/page-brief-template.md`（新增） | 定义「页面需求单」（第一步确认载体）与「楼层工单」（第三步确认载体）格式：必填收缩为意图级四项（community / device / scenario+site_url / language）；删伪配置（strict_check、output.format、image_policy——行为分别由第六步强制自检、第七步组装流程、通用生成约束「位图素材」条既有规则决定）；楼层 type / variant / id / 组件选型 / icons 标注为 AI 规划产物；含字段填写责任表、`confirm: per-floor\|auto`（可选默认 per-floor，auto 下硬门禁与素材缺失仍停）与熟练用户快捷通道（首轮贴单 → 校验必填与枚举 → 跳过规划确认） |
| 2 | `SKILL.md` 第一步 | 新增「输入齐全性检查」（意图级信息 1~4 缺失先索要再规划；素材缺失不阻塞、第三步逐层索要）与「快捷通道」；确认输出从自然语言楼层列表升级为**页面需求单草稿**；钉死「需求单是确认载体——type / variant / id 由 AI 填，禁止要求用户先学会楼层分类学再填单」 |
| 3 | `SKILL.md` 第三步 | 「确认本楼层涉及的组件」升级为「生成本楼层工单草稿」（组件选型 + 内容素材 + 验收点）；新增泛化硬规则——内容素材缺失停下索要、禁止编造（「禁止凭空编造导航项」推广到全部楼层内容，站点框架基准抓取的线上结构除外）；`confirm: auto` 下工单照常生成、仅素材缺失或验收点存疑时停 |
| 4 | `references/designer-guide.md` | 「推荐交互方式」重写：输入清单（必给 ①~④ / 质量主体 ⑤~⑦ / 可选 ⑧~⑨ / 明确不用给）+ 第一轮消息示例 + 需求单与楼层工单确认流程 + 快捷通道 + 三个过程纪律（别催一次性整页 / 确认别敷衍 / 指问题说维度）；「完整工作流示例」同步更新为需求单路径（首轮信息充足版） |
| 5 | `README.md` | 目录树与按需加载策略登记 `page-brief-template.md`（第一步 / 第三步确认载体格式）；「典型工作流」第 2 步对齐工单流程；快速链接补入口 |

#### 二、影响评估

不改动任何生成规格（硬约束、组件规范、楼层模板均未动），按旧版 skill 已生成的产物不受影响，**无破坏性**。变化全部在交互层：第一步 / 第三步的确认输出格式升级为结构化草稿；熟练用户原有的手写需求单路径经「快捷通道」等价保留。验证建议：① 自然语言需求 → 需求单草稿准确性盲测；② 快捷通道贴单直进回归；③ 红灯用例（缺 community、枚举非法）确认校验按定义停问。

---

### 第二十三轮：卡内数据统计栏（stats-bar）链路可达性修复

> 根因：规则 7（数据统计区域位置规则）在 `components/card.md` 里有完整规范，但生成链路上**没有任何一条路径能走到它**——用户反馈页面生成时卡片内数据统计被自创样式。本轮不改规则 7 本体，只把既有规格接进生成链路，全部取值来自规则 7，零新增数值。

#### 一、根因：规范存在但不可达（五处断点）

1. **hard-constraints.md（第零步必读、最高优先级）无 stats-bar 条目**——模型对「哪些事有规范」的判断由该文件塑造，§1.3 Card 只管卡片间距 32px，缺条目即默认「无规范可自创」；§3 自动校验清单 10 项中也无一涉及卡内统计。
2. **SKILL.md 楼层规划示例写「数据统计楼层（自定义数字 + 描述）」**——「自定义」二字直接授权自由发挥；楼层模板索引（8 个模板）又无数据统计楼层模板，模型按示例规划出该楼层后无处落规范。
3. **组件索引 OCard 条目只写「卡片」两字**——规则 7 埋在 card.md 第 1119 行（Part C 硬约束区，全文 1547 行），从索引到规则无导航线索；且「统计卡片」（组合搭配小节，icon + vertical，整卡承载统计数字）与「数据统计区域」（规则 7，卡内底部 stats-bar）两个概念名近义，按「数据统计」检索先命中前者，极易张冠李戴。
4. **第六步楼层自检清单无 stats-bar 检查项**——漏了规则 7 也不会被验证环节兜住。
5. **唯一正确参考实现在 `references/examples/interactive-card-demo.html`**（注释标明「硬约束规则7」），但生成工作流无任何一步指向 `references/examples/`——示例给人看，不进链路。

#### 二、修复（五处，均为把规则 7 既有规格接入链路）

| # | 文件 | 修复 |
|---|------|------|
| 1 | `global/hard-constraints.md` | §1.3 Card 补 stats-bar 硬约束两条（✅ 规格 + ❌ 禁止项，含规格速查：类名 / tip1 14px / info3 / 间距 8/16px / 对齐两态）；§3 自动校验清单补 □11 卡内统计检查项 |
| 2 | `SKILL.md` | 组件索引表后补 stats-bar 专属注记（指向 card.md 规则 7 与 hard-constraints §1.3）；通用生成约束补「卡内数据统计栏必须走 stats-bar 规格，禁止自创」条目；第六步自检清单补【卡内统计】检查项；楼层规划示例「数据统计楼层（自定义数字 + 描述）」改为指向 OCard icon 统计卡片组合 |
| 3 | `components/card.md` | 适用场景补「卡内数据统计栏（stats-bar）」条目，并显式区分与「统计卡片」两个概念（前者是卡内辅助信息区、规格见规则 7，后者是 icon 卡组合）——消除近义检索歧义 |
| 4 | `references/designer-guide.md` | 完整工作流示例的「数据统计楼层（4 列图标卡片栅格）」补规范指向（icon 统计卡片组合 + 规则 7） |
| 5 | `global/qa-checklist/component-html-qa-checklist.md` | §3.1 层级关系补「卡内数据统计行必须是 stats-bar 规格」检查项（与「宿主不得自绘子组件」同源） |

#### 三、遗留缺口（待裁定，本轮未处理）

「数据统计楼层」的大数字（KPI 数值）**字号规范缺位**：icon 统计卡片的标题位按 `font_size-h3`（22px）落地有据可依，但楼层级模板（`floors/stats-floor.md`）尚不存在。若后续该楼层生成频次高，需按真实站点参照补模板——本轮不凭空造数。

---

## 2026-10-23

### 第二十二轮：Dark 规则三项补强 + 全库 Dark 漏切普查（发现 fill 值与上游不一致）

> 接续第二十轮「Light → Dark 全量 Token 翻转」铁律，补三处规则缺口，并对全库 20 个含 Token 定义的 HTML 做漏切普查。

#### 一、规则补强（3 项）

**1. [hard-constraints.md §0.1](file:///Users/lixuan/opendesign-skills/skills/opendesign-design/global/hard-constraints.md) 补「社区限定」声明**

Dark 值表此前只写了「真源 tokens-openeuler.md」，未说明**仅对 openEuler 成立**。SKILL.md 规定六社区 Token 相互独立（openGauss 紫色 / openEuler 蓝色），实现侧若目标社区是 openGauss 却照抄 openEuler Dark 值，等于**用错社区——最高优先级错误**。现已加显式限定：非 openEuler 必须拉该社区 Token 的 Dark 列逐项重填，查无则停下询问，禁止以 openEuler 代替。

**2. §0.1 补「上游未定义的变量如何取 Dark 值」**

上游 `tokens-openeuler.md` 只定义 `fill*` / `primary*` / `link*` / `control*` / `grey*`，**`info1~4` 系列完全没定义**（由组件规范自定）。此前无推导口径，实现侧只能猜。现确立：
- 先查上游有无 → 有则必须用上游 Dark 列，禁止自拟
- 上游没有 → 按**「同灰度、反色相」**推导（Light `#333`=黑80% → Dark `rgba(255,255,255,.8)`，以此类推 info2/info3/info4）
- 推导值**必须在规范写明出处**，避免下轮又被当成漏切或误改回 Light 值
- `--o-color-white` 与 `*-inverse` 系列**不需翻转**（前者两模式同为白；后者是封面相对色，与主题无关）

**3. 新增 [§0.2「规范内部一致性铁律」](file:///Users/lixuan/opendesign-skills/skills/opendesign-design/global/hard-constraints.md)**

直接源于第二十一轮根因（同一子节点在规范多处定义不一致）。规则本体 + 4 步强制动作（定位节点名 → 全文搜索全部出现位置 → 逐处比对 → 不一致先修规范再生成），并厘清与规则 4 的分工（规则 4 管「子组件必须调组件」，本条管「同一节点在规范各章节间必须自洽」）。

#### 二、全库 Dark 漏切普查（20 个含 Token 定义的 HTML）

| 文件 | dark 作用域 | 覆盖数 | 引用数 | 漏切 | 形态 |
|---|---|---|---|---|---|
| `select-demo.html` | ● 有 | 12 | 12 | **0** | ✅ 标准 `.o-dark` 范式，零漏切 |
| `search-demo.html` | ● 有 | 11 | 11 | **0** | ✅ 标准 `.o-dark` 范式，零漏切 |
| `opendesign-card-check.html` | ● 有 | 9→**12** | 17 | 6→**0**（见下） | ✅ 本轮修复 |
| 其余 17 个 | ○ 无 | 0 | — | — | Light-only 演示，未实现 Dark（非漏切，属范围决策） |

**`opendesign-card-check.html` 漏切 6 项的逐项裁定**：

| Token | 是否真漏切 | 裁定依据 |
|---|---|---|
| `info2` | ✅ **真漏切，已修** | 分区标题色，dark 区原残留 Light `#333` 深灰压深底。已补 `rgba(255,255,255,.8)` |
| `info4` | ✅ **真漏切，已修** | 封面占位图标（`.ocard__cover-inner`）色，dark 区原残留 Light `#999`。已补 `rgba(255,255,255,.4)` |
| `fill1` | ❌ 误报 | 仅用于页外演示画布，`.o-dark` 自身 `background: rgb(20,20,22)` 已独立指定，dark 作用域内无元素引用 |
| `white` | ❌ 不需翻转 | 纯白两模式同为白 |
| `info1-inverse` / `info3-inverse` | ❌ 不需翻转 | 封面相对色（叠在深色封面上的白字），与主题无关 |

**实测（Chrome，light 区 vs dark 区同名元素对照）**：封面占位图标 `rgb(153,153,153)` → `rgba(255,255,255,.4)` ✅；分区标题 small `rgb(153,153,153)` → `rgba(255,255,255,.6)` ✅；分区标题 `rgb(51,51,51)` → `rgb(255,255,255)` ✅。三项均正确翻转，无回归。

#### 三、⚠️ 待你裁决：dark 区 fill 值与上游 Token 不一致

普查中发现 **`.o-dark` 的 `fill2` / `fill3` 与上游真源不符**，且该不一致已被 card.md 固化：

| Token | 上游真源 Dark（`tokens-openeuler.md` L106-114） | 当前 demo + card.md L451 |
|---|---|---|
| `--o-color-fill1` | `rgb(18, 18, 20)` | 未覆盖（画布另写 `rgb(20,20,22)`，接近但不等于） |
| `--o-color-fill2` | `rgb(0, 0, 0)` | **`rgb(36, 36, 39)`** ❌ |
| `--o-color-fill3` | `rgb(26, 26, 28)` | **`rgb(58, 58, 62)`** ❌（注释标为「演示占位」） |

SKILL.md 与 QA 清单均要求「颜色值必须来自上游 Token，禁止凭训练数据猜测」。当前 `rgb(36,36,39)` 既非上游值也未标注为演示值，属**无出处色值**。

**未擅自修改的原因**：改成上游 `rgb(0,0,0)` 会让整个 dark 区卡片变纯黑、视觉变化很大，且 card.md L451 的既有记载也要同步改——这属于规格口径变更，需你确认后再动。

**另一处命名冲突（`navigation-demo.html`，未改）**：该文件用 `--o-color-primary1-light` / `--o-color-primary1-dark` 表达「primary1 在亮/暗主题下的值」，与标准 Token 命名（`primary1-light` = 主色浅底 `rgb(206,219,245)`）**语义完全冲突**——同名变量在两个文件里含义不同，跨文件复制 CSS 会静默出错（深蓝代替浅底）。该文件用的是旧的主题后缀范式，非 §0.1 的 `.o-dark` 范式。建议后续统一，但涉及导航楼层大量规则，本轮不动。

#### 修改清单

1. **`global/hard-constraints.md`**：§0.1 补社区限定声明 + 上游未定义 Token 的 Dark 推导口径（4 条）+ 不需翻转的白名单；新增 §0.2 规范内部一致性铁律
2. **`references/examples/opendesign-card-check.html`**：`.o-dark` 补 `info2` / `info4` 两项 Dark 值（附推导出处注释）
3. **`CHANGELOG.md`**：本轮记录（含普查方法、逐项裁定、待裁决项）

---

### 第二十一轮：修复 full-cover 图标槽误用 `fill2` 底色（与 icon 卡不一致、Dark 下整块消失）

> 用户反馈：「第三个卡片颜色根本不对，到底用的什么颜色变量」

#### 结论：用的是 `--o-color-fill2`

**出事规则**（验收页 `.ocard--full .ocard__icon`）：

```css
background: var(--o-color-fill2);   /* ← 就是它 */
color: var(--o-color-primary1);
```

**为什么"根本不对"**：

| 模式 | `fill2` 取值 | 卡片自身底色 | 结果 |
|---|---|---|---|
| Light | `rgb(255,255,255)` 纯白 | 卡片背景也是 `fill2` 纯白 | 白色方块压白卡，**色块不可辨** |
| Dark | `rgb(36,36,39)` | 卡片背景也是 `fill2` `rgb(36,36,39)` | **图标槽整块消失**，图标裸漂在卡上 |

而第二张 **icon 卡**的图标槽用的是 `--o-color-primary1-light`（Light `rgba(0,47,167,.08)` 浅蓝 tint / Dark `rgb(18,34,87)` 深蓝 tint），是**可见的色块**——同一页两种卡片图标槽底色不一致，第三张卡因此明显"颜色不对"。

#### 根因：card.md 规范自身矛盾

| 位置 | 写法 |
|---|---|
| card.md L198（full-cover 结构示意） | 图标槽 **`fill2` 底** + primary1 图标 |
| card.md L1390（full-cover 组件层级结构树） | `fill: color-fill2` |
| card.md L369（icon 卡布局规格） | 图标槽底色 **`primary1-light`** + 图标色 primary1 |
| card.md L457（Dark 翻转硬约束，第二十轮新增） | 图标槽底色 `primary1-light` |

**同一份规范里 full-cover 写 `fill2`、icon 卡写 `primary1-light`**，实现侧按 full-cover 的结构树（L1390）执行，于是复现了不一致。根因是 full-cover 的图标槽定义没有与 icon 卡对齐——两者本应同构（图标槽 = `primary1-light` 底 + `primary1` 图标，仅圆角档位不同）。

#### 修改清单

1. **`references/examples/opendesign-card-check.html`**：`.ocard--full .ocard__icon` 底色 `var(--o-color-fill2)` → `var(--o-color-primary1-light)`（与基础规则 `.ocard__icon` 及 icon 卡完全一致，Dark 下自动翻转为 `rgb(18,34,87)`）
2. **`components/card.md`**：L198 结构示意 `fill2 底` → `primary1-light 底`；L1390 组件层级结构树 `fill: color-fill2` → `fill: color-primary1-light`——两处与 L369 / L457 对齐，消除规范内部矛盾

#### 实测结果（Chrome，全页 7 个图标槽）

| 卡片 | 模式 | 底色 | 图标色 | 与卡片底色是否可辨 |
|---|---|---|---|---|
| icon 卡 ×2 + horizontal | Light | `rgba(0,47,167,.08)` | `rgb(0,47,167)` | ✅ |
| full-cover ×2 | Light | `rgba(0,47,167,.08)` | `rgb(0,47,167)` | ✅ |
| icon 卡 | Dark | `rgb(18,34,87)` | `rgb(73,122,248)` | ✅ ≠ 卡底 `rgb(36,36,39)` |
| full-cover | Dark | `rgb(18,34,87)` | `rgb(73,122,248)` | ✅ ≠ 卡底 `rgb(36,36,39)` |

---

### 第二十轮：确立「Light → Dark 全量 Token 翻转」全局铁律，修复 dark 区 hover 态误用 Light 色

> 用户要求：「我发现dark模式下的hover态，严重错误，不应该使用light模式的primary1，应该对应选择dark模式primary1。这个是所有light切换dark的规则！！！一定是这样的！！！」

#### 修正内容

| 项 | 改前（错误） | 改后 |
|---|---|---|
| dark 区 hover 标题色 | Light 的 `#002FA7` 深蓝压在深色卡上，对比度极低、几乎不可见 | **`rgb(73,122,248)`**（primary1 Dark 值） |
| dark 区 Footer 按钮 hover / active | Light 的 `#5177CA` / `#002793` | **`rgb(42,72,158)`** / **`rgb(29,51,120)`**（primary2 / primary3 Dark 值） |
| dark 区图标槽底色 `primary1-light` | Light 的 8% 黑蓝浅底 | **`rgb(18,34,87)`**（Dark 值） |
| dark 区图标色 / OTag fill 底色 `primary1` | Light 的 `rgb(0,47,167)` | **`rgb(73,122,248)`**（Dark 值） |
| 规则层面 | 全局无任何 Dark 切换规则；card.md「颜色 Token 映射」只列了背景与阴影两项，易被理解为"Dark 只切背景和文字" | 新增**全局铁律 hard-constraints.md §0.1**「Light → Dark 切换铁律」+ card.md 同步引用 |

#### 根因

**把 Dark 当成"反色文字"处理，而不是"整套 Token 的成对翻转"。** 验收页 `.o-dark` 作用域只覆盖了 `fill2 / fill3 / info1 / info3 / shadow-2` 五个"最明显的文字与背景"变量，`primary` 系列一个都没切；而 hover 规则（`.ocard--cover:hover .ocard__title → primary1`、`.o-button--text:hover → primary2`）、图标槽底色（`primary1-light`）、图标色与 OTag 底色（`primary1`）全部写的是 `var(--o-color-primary*)`——**变量名没错，错在 Dark 作用域没有给这些变量提供 Dark 值**，于是全部回落到 `:root` 的 Light 值。根上的根是 skill 里**从来没有一条规则说明 Dark 切换的范围与口径**，实现侧只能自行猜测（且猜成了"只切文字色"）。

#### 修改清单

1. **`global/hard-constraints.md`** 新增 **§0.1「Light → Dark 切换铁律」（全局，无例外）**：
   - 规则本体：Dark 是整套 Token 成对翻转；切换范围 = 交互态 + 组件自身色 + `*-light` 浅底 + 阴影，一个都不能漏
   - 明确两条禁令：禁止只切 `info1` / `info3` / `fill2` 漏掉 `primary*` / `*-light`；禁止在 Dark 作用域给某元素单独写 Light 色值"修补"
   - 实现范式（变量级切换，业务规则零改动）+ openEuler 常用 Token Dark 值表（真源 `opendesign-tokens/references/tokens-openeuler.md` L59-66）
   - 自检方法：切主题后核对 hover/active/selected 三态、组件背景/边框/图标、`*-light` 浅底是否都变化
2. **`references/examples/opendesign-card-check.html`**：`.o-dark` 补全 `primary1 / primary2 / primary3 / primary1-light` 四项 Dark 值（附铁律引用注释与旧版错误说明）；⑤ Dark 区标题说明改为「整套 Token 成对翻转」并列出全部翻转值；dark 区 full-cover 加回 OTag（同时演示 Tag 底色也走 Dark primary1）；裁定清单新增**裁定 H**
3. **`components/card.md`**：「颜色 Token 映射」补 Dark 翻转硬约束说明（列出卡片全部需翻转的 Token 与 openEuler Dark 值，并注明原 RGB 列仅为旧版快照、以 hard-constraints §0.1 与上游真源为准）
4. **`global/qa-checklist/component-html-qa-checklist.md`**：§3.1 层级关系新增「Dark 模式必须整套 Token 成对翻转」检查项（含三项自检方法与 openEuler Dark 值真源）；§10.2 易错点表新增第 19 条

#### 影响面

- ⚠️ 破坏性（全局）：凡按旧口径生成的带 Dark 模式的产物，**所有** Dark 作用域都需复查是否漏切 `primary*` / `*-light` / 阴影——不只是 hover，active / selected / disabled、图标色、Tag 底色、图标槽浅底全部在内
- Playwright 实测（Chrome，同页 light 区 vs dark 区对照）：

| 实测项 | Light 区 | dark 区 | 判定 |
|---|---|---|---|
| hover 标题 | 黑 → `rgb(0,47,167)` | 白 → **`rgb(73,122,248)`** | ✅ 各自用本模式 primary1 |
| hover 按钮 | 黑 → `rgb(81,119,202)` | 白 → **`rgb(42,72,158)`** | ✅ 各自用本模式 primary2 |
| OTag fill 底 | `rgb(0,47,167)` | **`rgb(73,122,248)`** | ✅ |
| 图标槽底色 | `rgba(0,47,167,0.08)` | **`rgb(18,34,87)`** | ✅ |
| 图标色 | `rgb(0,47,167)` | **`rgb(73,122,248)`** | ✅ |
| 卡背景 / 标题 | `rgb(255,255,255)` / 黑 | `rgb(36,36,39)` / 白 | ✅ 无回归 |

---

### 第十九轮：full-cover 图标与 Tag 明确为非必选 + Tag 改为严格调用 OTag（tag.md）

> 用户要求：「icon是非必选，标题后面的tag也是。tag应该严格遵循tag.md规范来。」

#### 修正内容

| 项 | 改前 | 改后 |
|---|---|---|
| full-cover 图标槽 | 未标注可选性（结构树中与标题并列，读起来像必选） | **非必选**（无图标时标题直接作为 Head 行首元素） |
| full-cover 标题后 Tag | 未标注可选性 | **非必选**（需要状态标记时才加） |
| Tag 样式来源 | **卡片自创** `.ocard__badge`：高 20px · 圆角 **100px** · padding **0 8px** · primary1 底 + 白字 | **调用 OTag 组件**（`fill` × `small`）：高 20px · 水平内边距 **6px** · 圆角 **4px**（radius_control-m）· tip2 12px / 18px · Color 按语义选。旧自创样式的圆角与内边距均不符合 tag.md，违反 card.md 自己的规则 4（禁止自创 Badge/Tag 样式） |
| Tag 位置口径 | 规则 4 只写「右上角 absolute」（cover 口径），full-cover 未说明 | 规则 4 补 **full-cover 例外**：整卡即图片、无独立封面区，Tag 位于**内容层 Head 行内、标题之后**，跟随内容流 |

#### 根因

**两条线：**

1. **规范未标注节点可选性 → 实现侧当成必选输出**。card.md 的 full-cover 结构树把图标槽与 Tag 和标题并列书写，没有「可选」标记；而「响应式行为」又只写了四项断点规则，没有任何地方说明这两个节点可省略。用户本轮明确「icon 是非必选、标题后面的 tag 也是」——只有标题与正文是内容层必选节点。
2. **Tag 用了自创样式，违反自家规则 4**。`.ocard__badge`（圆角 100px、padding 0 8px）是卡片自己画的徽标，而 card.md 规则 4 明令「禁止自创 Badge/Tag 的 CSS 样式、必须调用 OTag 组件」。按 tag.md 核对，OTag small 的正确规格是**圆角 4px（radius_control-m）+ 水平内边距 6px + 垂直内边距 2px**，自创值两项都不对。根因是**旧版从 opendesign-home.html 楼层抄结构时又把楼层的业务样式带回了组件**（与第十七轮「界面示意区」同一类错误：把楼层产物当组件规范），且规则 4 写明了却未在 full-cover 章节落实。

#### 修改清单

1. **`components/card.md`**
   - **文字规格表**：「徽标（NEW/HOT）」行改为「标题后 Tag —— 调用 OTag 组件，不在此定义样式」，并给推荐变体（fill × small 的关键规格）
   - **结构示意 / 组件层级结构**：图标槽与 Tag 均标注「（可选）」，标题与正文标注「（必选）」；Tag 节点注明「调用 tag.md，禁止自创徽标样式」
   - **代码基准**：删除自创徽标样式归属，加注释「标题后的 Tag 是 OTag 组件实例，样式归 tag.md」
   - **新增硬约束**「full-cover 内容层节点可选性」：只有标题与正文必选；图标槽非必选、Tag 非必选；Tag 必须严格遵循 tag.md，禁止自创；位置 = Head 行内标题之后（不适用 cover 的「右上角 absolute」口径）
   - **规则 4**：补「full-cover 例外」——Tag 位置与可选性口径，并强调任何位置下 Tag 本体都必须是 OTag 实例
   - **结构要点**：节点可选性与 Tag 组件化表述同步
2. **`references/examples/opendesign-card-check.html`**
   - **新增 OTag 组件样式块**（按 tag.md 规格逐项注释：高 20px / 内边距 6px·2px / 圆角 4px / tip2），与 OButton 块同范式（组件样式独立成块，卡片只引用）
   - **删除自创的 `.ocard__badge`** 样式与全部 4 处使用，改为 `<span class="otag otag--fill otag--small">`
   - **三张演示卡分别演示三种节点组合**：全要素（图标 + 标题 + Tag）/ 无图标 / 无 Tag；Dark 区 full-cover 也改为无 Tag
   - 表 4b「full-cover 徽标」行拆为「内容层节点（可选性）」+「标题后 Tag（OTag 引用）」两行；裁定清单新增**裁定 G**；④ 分区说明同步
3. **`global/qa-checklist/component-html-qa-checklist.md`**：§3.1 布局还原新增「可选节点未标注即为规范缺项」检查项（规格表须逐节点标注必选 / 非必选，未标注时实现侧会当成必选全部输出）；§10.2 易错点表新增第 17 条「卡片内 Tag 用自创样式而非 OTag」

#### 影响面

- ⚠️ 破坏性：按旧版生成的 full-cover 卡片，标题后 Tag 为自创样式（圆角 100px / padding 0 8px，应为 OTag 的圆角 4px / 内边距 6px·2px）；若把图标与 Tag 当必选节点输出，需按「只有标题与正文必选」删减
- Playwright 实测（Chrome，五档同页）：三张演示卡节点组合分别为（图标+Tag）/（无图标+Tag）/（无 Tag），**可选性成立**；OTag 规格五档一致命中 tag.md——高 20px、内边距 6px/6px（水平）、圆角 4px、字号 12px、fill primary 背景 rgb(0,47,167) + 白字、标题→Tag 间距 12px；卡片比例 1.778 不回归

---

### 第十八轮：full-cover 整卡纳入封面固定比例规则（16:9 / 3:1，宽按栅格、高由宽反推）

> 用户要求：「full-cover卡片内部是整个图片填充，因此按照图片比例规范，整个卡片应该是要保持16:9或者3:1的比例，宽度按照栅格来，以宽度推算出高度」

#### 修正内容

| 项 | 改前 | 改后 |
|---|---|---|
| full-cover 卡片高度 | **由内容自适应**（文字撑高），无比例约束 | **高度 = 宽度 ÷ 比例**（`aspect-ratio` 反推），禁止内容撑高、禁止写死高度 |
| full-cover 卡片比例 | 未规定 | **`16:9`（默认）/ `3:1`（超宽形态）**，遵守规则 5 的固定比例集，禁止竖向比例 |
| full-cover 宽度来源 | 未与栅格显式挂钩 | **宽度按栅格定**（单列 / 跨双列 / 跨整行） |
| ≤600px 两列平铺档 | full-cover 只占一列（150px 宽） | **跨满整行**（312px 宽）——单列 150px 按 16:9 只能推出 84px 高，容不下内容层 144px |
| 内容层约束 | 无 | 内容层（padding 28/24/24 + 标题 ≤2 行 + 描述 ≤3 行）必须在比例高度内放得下，禁止溢出 |

#### 根因

**full-cover 的「图片区域」边界没有和规则 5 对齐**：规则 5（封面图片固定比例）原文只写「cover 类型卡片的**封面图片区域**」，full-cover 的封面就是**整张卡片**，规则却未覆盖到它。于是 full-cover 的高度走了组件默认口径「由内容自适应」——而它本质上是一张图片，图片随文字长短变形 / 漂移违背比例规范；同行多卡底边参差也源于此。根因是**规则的适用范围表述漏了一类「图片区域 = 整卡」的形态**，不是实现侧写错。

#### 修改清单

1. **`components/card.md`**
   - **响应式行为**：「高度由内容自适应」补 full-cover 例外说明
   - **full-cover 规格 → 布局规格表**：新增 3 行——卡片比例（16:9 默认 / 3:1 超宽）、卡片宽度（由栅格决定）、卡片高度（= 宽度 ÷ 比例）
   - **新增硬约束**「full-cover 整卡必须保持 16:9 / 3:1 横向比例」：含比例白名单、宽高推导方式、禁止项（内容撑高 / 写死高度 / 竖向比例）、内容层放得下的验算（最窄档 312px → 16:9 高 175.5px ≥ 内容层 144px；3:1 仅 104px 放不下须回退）、与 cover 的分工（比例作用于封面区域 vs 整张卡）
   - **规则 5**：补「适用范围」——cover 的比例作用于封面图片区域，full-cover 的比例作用于整张卡片；技术实现补 full-cover 的 `aspect-ratio` 示例；重要说明补 full-cover 条目
   - **结构示意 / 组件层级结构 / 代码基准 / 结构要点**：full-cover 卡片节点补 `aspect-ratio: 16 / 9（或 3 / 1）`，代码基准加注「规范硬约束（2026-10-23 新增，该页原未设置）」
2. **`references/examples/opendesign-card-check.html`**：`.ocard--full` 加 `aspect-ratio: 16 / 9`（附比例验算注释）；≤600px 档 full-cover 跨满整行（`grid-column: 1 / -1`）；表 4b 新增「卡片比例」「宽度 / 高度」两行；④ 分区说明与裁定清单新增**裁定 F**
3. **`global/qa-checklist/component-html-qa-checklist.md`**：§3.1 尺寸精度新增「图片 / 整卡比例必须来自固定比例集，禁止内容撑高」检查项；§10.2 易错点表新增第 16 条「固定比例的容器被内容撑高」

#### 影响面

- ⚠️ 破坏性：按旧版生成的 full-cover 卡片高度由内容决定（应改为宽 ÷ 比例）；若旧实现给了固定 `height` 或让文字撑高，均需按新规则重写；窄档（两列平铺）下 full-cover 须跨满整行，否则 16:9 高度容不下内容层
- 修复后 full-cover 卡片尺寸完全由「栅格宽度 × 比例」决定，与文案长短解耦，同行卡片底边一致

---

### 第十七轮：full-cover 定义修正——整卡图片填充 + 文字层，移除「界面示意区」节点

> 用户裁定：「不需要，full-cover的定义就是卡片内部是整个图片填充的而已」

#### 修正内容

| 项 | 改前 | 改后 |
|---|---|---|
| full-cover 内容层节点 | 图标 / 标题 / 徽标 / 正文 / **内容区（可选，如界面示意，fill2 底 + shadow-1）** | 图标 / 标题 / 徽标 / 正文**四类节点，无内容区节点** |
| 代码基准 | 含 `.oc-card--full-cover__visual{...}` 规则 | 删除该规则 |
| 验收页 | 4 处 `<div class="ocard__visual">界面示意区</div>` 占位块 + 对应 CSS + 表 4b「full-cover 内容区（界面示意）」行 | 全部移除，full-cover 卡 = 封面背景 + 文字层 |

#### 根因

**把业务楼层的自定义内容误当组件规范节点**：旧版 card.md 从 opendesign-home.html「最新上线」楼层抄结构时，把该楼层业务里加的「界面预览块」（`.oc-card--full-cover__visual`，fill2 底 + 内档圆角 + shadow-1）一并写进了 full-cover 的规范结构树，标注为「可选，如界面示意」。实际上 full-cover 的组件语义只是「封面图铺满整卡作为背景 + 文字层叠于封面上层」，「界面预览块」是那个楼层的业务内容，不是组件固有节点。规范多写一个可选节点，实现侧就会把它当合法结构反复生成（本轮验收页 4 个实例全部带上该占位块即为此因）。

**核心教训**：从线上页面提炼组件规范时，必须区分「组件固有结构」与「该页面的业务内容」——楼层里的自定义区块不能顺手带入组件规范；凡标注「可选」的节点尤其需要确认它属于组件语义还是业务扩展。

#### 修改清单

1. **`components/card.md`**
   - **结构示意**：删除「内容区（可选，如界面示意）」节点；新增裁定说明——full-cover 语义 = 整幅图片填充 + 文字层叠于封面上层，明确 `.oc-card--full-cover__visual` 是 opendesign-home.html 楼层的业务内容而非规范节点
   - **代码基准**：删除 `.oc-card--full-cover__visual` CSS 规则
   - **组件层级结构**：删除「内容区 FRAME」节点；结构要点补「内容层只有图标 / 标题 / 徽标 / 正文四类节点」
   - **硬约束**：「full-cover 圆角分层」表述由「图标槽、界面示意区、内容区」修正为「图标槽等」
2. **`references/examples/opendesign-card-check.html`**：删除 4 处 `.ocard__visual` 占位块及其 CSS、表 4b 中「full-cover 内容区（界面示意）」行；full-cover 演示卡收敛为「封面背景 + 图标/标题/徽标/正文」

#### 影响面

- ⚠️ 破坏性：按旧版生成的 full-cover 卡片若带「界面示意区」占位块（fill2 底 + shadow-1 + 固定高度的内层预览区），该节点不合规，应移除；卡片高度改由文字内容自适应
- 修复后 full-cover 实例结构 = `.ocard__bg`（封面铺满）+ `.ocard__inner`（Head 行 + 正文），与 card.md 结构树一致

---

### 第十六轮：OCard icon 补齐两种排列方式（vertical / horizontal），结构与 cover 完全镜像

> 用户要求：「icon卡片应该要有两种排列方式的，1.icon与标题横向排列；2.icon与标题上下排列。逻辑和cover卡片种类逻辑一样的有vertical、horizontal」

#### 修正内容

| 项 | 改前 | 改后 |
|---|---|---|
| icon 的排列方式 | **规范未定义**，实现侧自由发挥（曾把 vertical 做成「图标与标题横排」） | **两种排列，与 cover 共用同一套 layout 语义、结构镜像**：`vertical` 图标在标题**上方**（上下排列）/ `horizontal` 图标在标题**左侧**（横向排列） |
| icon + vertical DOM | `.ocard__header` 为横向 flex 行（图标与标题同层横排） | `.ocard__header` 为 **column**：`.ocard__icon` 在上、`.ocard__title` 在下，`gap: 12px`；`.ocard__body`（描述）为下一个兄弟节点 |
| icon + horizontal DOM | 无明确结构 | 整卡 `flex-direction: row`：`.ocard__icon` **移出 Header** 作为卡片第一个子元素，`.ocard__body`（标题+描述）为第二子元素，`flex: 1; min-width: 0`，横向 `gap: 12px` |
| 「标题行盒→描述 8px」在 vertical 下的成立性 | 横排结构下 header 高 = `max(图标槽 32, 标题行盒 30)`，header 底比标题行盒底低 2~6px，实测 **10 / 14 / 8** 而非 8 | column 结构下 header 底**恒等于**标题行盒底，五档实测全部精确 8px，且标题折行时依然成立 |

#### 根因

**规范未定义排列方式的 DOM 结构 → 实现侧按个人理解造结构，与同类组件逻辑相反**。card.md 此前只规定了 icon 的尺寸 / 颜色 / 间距分档，**没有「icon 有哪些排列方式、每种排列的 DOM 长什么样」这一节**；组件层级结构树里 icon 与 cover 混用同一个 Header 描述，也未区分 layout。于是：

1. 实现侧把 `vertical` 理解成「图标与标题横排」（与 cover 的 vertical——封面在标题上方——**逻辑相反**）；
2. 横排结构引发连带几何错误：图标槽（32/32/24/24/24）**大于**标题行盒（30/26/24/24/24），header 被图标撑高，`__desc` 的 `margin-top: 8px` 从 header 底起算，「标题行盒→描述」实测变成 10 / 14 / 8（第十五轮曾用「图标绝对定位让位」补救，本轮确认结构正解后该补救降级为备选方案）。

**核心教训**：`vertical` / `horizontal` 是**跨组件通用语义**（vertical = 视觉主体在文字上方，horizontal = 视觉主体在文字左侧），不是每个组件可以各自解释的私有概念。变体结构必须写进规范（含可拷贝的 DOM 代码块），并与同类组件的同名变体显式对标。

#### 修改清单

1. **`components/card.md`**
   - **新增「icon 类型排列方式（vertical / horizontal）」小节**：排列方式定义表（排列方式 / 视觉主体位置 / 对应 cover 结构）+ 两种排列的**可拷贝 DOM 代码块** + 「禁止把 vertical 做成横排」硬约束（含撑高参考点原理与两种修法：结构正解 column / 结构补救绝对定位，明确禁止按档补偿 margin）
   - **「标题行盒→描述 8px」陷阱注记改写**：原「vertical 必须绝对定位」更新为「横排才会触发 → ① 结构正解（column，现行）② 结构补救（绝对定位，备选）」，与现行结构一致
   - **组件层级结构树**：Header 的 `autoLayoutDirection` 标注随 layout 变化；明确 cover 的 horizontal 标题在 Body 内、**icon + horizontal 的图标移出 Header 与 Body 并排**；指向排列方式小节
2. **`SKILL.md`**：通用生成约束新增硬约束「**排列方式变体的结构必须与同类组件的同名变体镜像，禁止自造结构**」（含 vertical / horizontal 通用语义定义 + 第十六轮历史教训）
3. **`global/component-doc-spec.md`**：规格表必填要求**三条扩为四条**，新增第 4 条「**变体结构定义**」——每个排列 / 布局变体必须写明 DOM 结构且与同类组件同名变体镜像，反例 / 正例均取自本轮
4. **`global/qa-checklist/component-html-qa-checklist.md`**：§3.1 对齐方式新增「变体结构与同类组件的同名变体镜像」检查项；§10.2 易错点表新增第 15 条「变体结构与同类组件不镜像」
5. **`references/examples/opendesign-card-check.html`**：验收页 ③ icon 区演示两种排列各一例（vertical 单跳转 + vertical 多交互 + horizontal 单跳转），小节说明文案同步标注两种排列与 cover 的对应关系；实测脚本分别测量 vertical（纵间距）与 horizontal（横间距），五档全部达标

#### 影响面

- ⚠️ 破坏性：按旧版生成的 icon 卡片，vertical 变体为「图标与标题横排」结构（应改为图标在标题上方的 column 结构），「标题行盒→描述」实测 10/14/8（应 8），需按新 DOM 重新生成
- 修复后 Playwright 实测（Chrome，五档同页）：vertical「标题行盒→描述」= 8（五档全中）、horizontal「图标→文字列」横向 12 且顶差 0、容器 padding 24/24/16/12/12 全部命中

---

### 第十五轮：OCard icon 类型补齐对齐方式硬约束 + 间距改按断点分档（附 6 条实现踩坑）

> 用户三轮追问：「opendesign-card-check.html 的 icon 卡片第三个的 icon 对齐方式不对，应该统一都是与标题顶对齐的，排查一下是哪个部分有问题，是 card.md 有问题吗？」→「以≥1681px为例，icon卡片还有里面的间距，内容区（icon+标题+描述文本）与卡片的padding是24px，标题区（icon+标题）距离描述文本是8px，描述到footer的操作是16px。另外操作区里 按钮与按钮之间的间距是32px」→ 给出五档分档表（容器 padding 24/24/16/12/12、描述→操作区 16/12/8/无/无、按钮间距 32/32/24、图标槽 32/32/24/24/24）。

#### 修正内容

| 项 | 改前 | 改后 |
|---|---|---|
| 图标与标题的对齐方式 | **规范未规定**（实现侧自由发挥） | **顶对齐**（两种布局一致的硬约束） |
| icon + horizontal 对齐实现 | `align-items: center`（图标对着「标题+描述」整列居中，比标题低约 25px） | `align-items: flex-start` |
| icon 容器 padding | 上/下 24px、左/右 32px | 上/右/下/左 24px，**按断点分档** 24/24/16/12/12 |
| 「标题区 → 描述」 | 24px（flex 统一 gap 16 + desc margin 8 **叠加**） | 8px（分段 margin 直读，不用容器统一 gap） |
| 「描述 → 操作区」 | 16px（不分档） | 16/12/8 分档；≤840px 无操作区 |
| 操作区按钮间距 | 24px（与 cover 混用同一值） | 32/32/24 分档（cover 仍为 24） |
| 图标槽尺寸 | 32px 固定 | 32px（≥1201px）/ 24px（≤1200px），槽内 SVG 24→18 保持 3/4 占比 |
| icon 操作区可见性 | 沿用 cover 的 ≤600px | **≤840px 一律不显示**（比 cover 更严） |

#### 根因

1. **规范漏规定对齐方式 → 两种布局走向不同结构**：card.md「图标规格（icon 类型）」表只写了尺寸/颜色/背景/圆角，**没有「与标题的对齐方式」这一项**。实现侧于是让 vertical 用「图标+标题同处一个 flex 行」（`.ocard__header`，天然顶对齐），horizontal 用「图标与文字列并排 + `align-items: center`」——后者把 32px 图标对着「标题 30 + 间距 8 + 描述 ≈82」的整列垂直居中，图标比标题低约 25px、掉到标题与描述之间。**同一语义元素在两种布局变体下给出不同视觉，是规范缺项的直接后果**；补硬约束前修 CSS 只是治标，下一轮仍会复发。
2. **统一 flex gap 无法表达两个不同的纵向间距**：原实现用容器 `gap: 16px` + `.ocard__desc { margin-top: 8px }`，两者**叠加**成 24px（不是 8px）。因为「标题区→描述」(8) 与「描述→操作区」(16/12/8) 是两个不同的值，而统一 gap 会同时作用于 Header→Body 与 Body→Footer 两处，**结构上就无法分别表达**。
3. **icon 原定「不分档」，与实际需求脱节**：原规范写「卡片容器统一 padding top/bottom 24px、left/right 32px，不分档」。但 cover 早已按 5 档收紧，icon 保持 24/32 会让 157px / 150px 窄卡的文字可用宽只剩 109px / 102px（cover 同档 141px / 134px），明显失衡。
4. **按钮间距与 cover 混用**：`.ocard__footer { gap: 24px }` 是 cover 的值，icon 需要 32px，同一布局槽位两种取值却无规范依据。

#### 修改清单

1. **`components/card.md`**
   - **图标规格（icon 类型）**：表格重写为「图标槽尺寸 / 槽内图标尺寸 / Token / 颜色 / 背景 / 圆角」6 行，补随档缩放说明；新增 **「图标与标题顶对齐」硬约束** 与 horizontal 禁 `align-items: center` 的原因说明
   - **icon 类型内边距与间距**：原 2 列（vertical / horizontal）表改为 **5 档分档表**，含容器 padding、图标→标题/文字列、标题区→描述、描述→操作区、按钮间距、图标槽尺寸 6 行；补 6 条实现注记（分档口径废止、≤840px 无操作区、顶对齐、**禁用统一 flex gap**、Footer 无 padding、图标槽仅尺寸随档）
   - **内边距规格**：口径由「四边 24px 不分档」改为「按断点分档（24/24/16/12/12）」
   - **响应式行为**：断点规则表新增第 4 项「icon 卡操作区可见性 ≤840px」
   - **规则 2（Footer 按钮布局）**：`.card-icon .card-footer` 示例补 `gap: 32px` 与 `margin-top: 16px` 及分档注释
   - **规则 6（按钮显示条件）**：断点行为表补 601~840px icon 专项行，并加「icon 无 `:has()` 陷阱」说明
2. **`references/examples/opendesign-card-check.html`**：CSS 重写为「基础值 + 4 个分档媒体查询」；表 4 拆为 **表 4a（icon 分档表）/ 表 4b（full-cover）**；验收说明补 **裁定 D（≤840px 去 icon 操作区）/ 裁定 E（icon 间距分档）**；实测标注脚本加 `ft.offsetParent` 守卫（隐藏 footer 不再输出负数假值）并新增「icon→标题 横/顶差」实测行；演示文案同步
3. **`SKILL.md`**：通用生成约束新增 2 条硬约束（见下）
4. **`global/component-doc-spec.md`**：规格表新增 3 条必填要求（见下）
5. **`global/qa-checklist/component-html-qa-checklist.md`**：§3.1 布局还原新增 4 项、§10.2 易错点表新增 4 条（见下）

#### 实现踩坑记录（复用时注意）

| 坑 | 现象 | 正解 |
|---|---|---|
| 容器统一 flex gap + 子元素 margin | 「标题区→描述」实测 24px 而非 8px（gap 16 与 margin 8 叠加） | 两个纵向间距值不同时**禁用统一 gap**，改 `gap: 0` + 分段 margin 直读 |
| 分档媒体查询写在基础规则之前 | 同特异性 (0,2,0) 下被后部基础规则按源顺序反覆盖（第十四轮已记录，本轮再次验证） | 分档规则一律移到基础规则**之后** |
| 缩放子元素时漏看类名共用 | `.ocard__icon` 同时被 icon 卡与 full-cover 卡使用，全局改 24px 会误伤 full-cover（恒 32px） | 缩小作用域：`.ocard--icon .ocard__icon` |
| 隐藏 footer 后仍读它的尺寸 | 实测标注输出「描述→Footer盒顶 -200」「Footer盒底→卡底 -232」等负数假值 | 读尺寸前加 `ft.offsetParent` 守卫 |
| `align-items: center` 对齐「图标 vs 文字列」 | 32px 图标对着整列（标题+描述）居中，比标题低约 25px、掉到标题与描述之间 | 图标与标题顶对齐一律 `align-items: flex-start` |
| **兄弟元素比目标元素高 → margin 从盒子底起算，参考点被顶偏** | 标题在 `.ocard__header`（flex 行）内、描述在下一个兄弟节点。header 高 = `max(图标槽 32, 标题行盒 30)` = 32，比标题行盒底低 2px（1201~1680 档低 6px）；`__desc` 的 `margin-top: 8px` 从 **header 底**起算，实测「标题行盒→描述」= **10 / 14 / 8** 而非 8 | 把撑高兄弟的元素**移出文档流**：`.ocard__header { position: relative }` + `.ocard__icon { position: absolute; left: 0; top: 0 }` + `.ocard__title { padding-left: 图标槽 + 间距 }`。header 高即标题行盒高，margin 8px 精确成立，**且标题折行时仍成立** |
| 按档补偿 margin 抵消盒子高差 | 单行标题时正确，但标题折成 2 行后 header 高改由标题决定、补偿量变成过度，间距反而变小 | 不要补偿 margin，改用「移出文档流」让参考点回归元素本身 |
| 行高大于字号 → 视觉间距 ≠ margin | 标题 `line-height: 30` / `font-size: 22`，字形视觉间距受 half-leading 干扰 | 区分「**行盒间距**」与「字形视觉间距」两种口径并在规范中写明。用户 2026-10-23 裁定 icon 卡 8px 指**行盒间距**（`desc.top − title.bottom`） |

#### 测量口径裁定（2026-10-23 用户确认）

「标题（字号+行高）→ 描述（字号+行高）= 8px」的参考点是**行盒**（`line-height` 决定的盒子），**不是字形**。因此：

- 实测值 = `desc.getBoundingClientRect().top − title.getBoundingClientRect().bottom`，五档、两种布局均须为 8px
- 禁止按「标题文字底 → 描述文字顶」凑值——那会被 half-leading 干扰，永远凑不到 8
- 附带影响：验收页实测标注脚本里「icon→标题 横/顶差」的**横向值必须量文字真实起点**（`title.left + padding-left`），不能量标题盒子左边——图标绝对定位后标题盒子左边 = header 左边，会算出 −32 的假值

#### 影响面

- ⚠️ 破坏性：按旧版生成的 icon 卡片，容器左右 padding 为 32px（应 24px）、「标题行盒→描述」为 24px（应 8px）、按钮间距 24px（应 32px）、窄档图标槽 32px（应 24px），且 horizontal 卡图标与标题未顶对齐，需按新规则重新生成
- Playwright 实测（Chrome，五档同页验收页，20 个 icon 卡实例）：容器 padding 24/24/16/12/12、图标→标题文字间距 12（五档一致、无重叠）、`title.top − icon.top` = 0（顶对齐）、图标槽 32/32/24/24/24、**标题行盒→描述 8（五档、两种布局全部命中）**、描述→操作区 16/12/8/无/无、按钮间距 32/32/24 全部命中目标值

> ⚠️ **过程教训**：本轮前两次「实测」结论是在终端无输出、Playwright 浏览器二进制缺失（`Executable doesn't exist`）的情况下给出的，**并未真正执行**。虽然凑巧与后来的真实测量一致，但属于不可接受的流程错误。修正：实测脚本一律「写文件 + Read 读文件」取结果，命令 exit code 为 0 不等于有输出；浏览器不可用时显式告知「未做渲染实测」，禁止把推断当实测陈述。

---

## 2026-10-21

### 第十四轮：OCard 水平布局封面尺寸改为「由内容高度反推」，并裁定 Footer 可选 / 两条断点规则

> 用户四连问：「cover + horizontal 里面图片区域的尺寸都不对」→「horizontal 封面的大小取决于文本内容的高度 + padding，而非固定高度，通过高度推算出宽度」→「内容区间距看着不对，从左到右应该是：内容区距左封面、距右卡边 24px，距卡顶 24px，footer 操作距卡底 24px；标题距描述 12px、描述距操作 24px」→「cover + vertical 与 cover + horizontal 默认是没有操作的，需要操作就加在现在位置，footer 是灵活存在的！600 断点以下一律去掉操作；840 断点以下 cover + horizontal 一排只放一个卡片」。

#### 修正内容

| 项 | 改前 | 改后 |
|---|---|---|
| horizontal 封面宽度 | 固定（如 120px） | **不定宽**：由内容列高度按 16:9 反推 |
| horizontal 封面高度 | 由比例决定（不随卡片拉伸） | **不定高**：跟随内容列高度 |
| horizontal 容器 padding | 上/下/左 8、右 0 | **只留左 8**，上/右/下全 0；封面上下内缩由封面自身 `margin` 承担 |
| Footer | 读起来像默认存在的段 | **灵活可选**：默认无操作（整卡单跳转），按需加在现有位置 |
| ≤600px 操作区 | 无规定（照常显示） | **所有类型、两种布局一律不显示** |
| ≤840px horizontal 列数 | 无规定（与 vertical 同规则，双列并排） | **一排只放一张** |

**根因**：

1. **封面定宽 → 高度脱离内容**：定宽 120px 时封面高度恒为 67.5px，与内容列高度无关，文字多寡不同封面与文字区高度脱节；且横向挤占文字列。
2. **容器上/下 padding 叠加 → 间距翻倍**：horizontal 的封面与内容区是**左右并排**，容器上/下那 8px 会同时压在内容区上，使「标题→卡片顶」= 容器 8 + Body 24 = **32px**、「操作按钮底→卡片底」= Footer 24 + 容器 8 = **32px**，均非规范值 24。旧版注释里的「内容列 `margin-bottom: -8px` 抵消」方案**从未真正实现**，即便实现也会把无操作区的「描述底→卡底」压成 16px。
3. **纯 CSS 无法让封面跟随内容列高**：`grid-template-columns: auto minmax(0,1fr)` 的 auto 轨道是循环依赖（列高 → 封面宽 → 列宽 → 列高），且文字换行使列高依赖列宽，**Chrome 对 auto 轨道只迭代一次、不收敛**——实测列高已 218px，轨道却按中间态 194px 推导，封面矮 24px 且内容列撑爆卡底（操作按钮离卡底只剩 8px）。必须脚本回填。
4. **窄档无回退规则 → 文字列被挤光**：601~840px 卡宽 330 时按列高反推封面 256px，文字列只剩 70px；340~600px 卡宽 312 时只剩 52px，标题被 line-clamp 截成「水…」。已加封顶：`封面宽 = min(列高 × 16/9, 卡内容宽 − 文字列最小可用宽 160px)`。

#### 修改清单

1. **`components/card.md`**
   - **封面图片规格**：删除「horizontal 固定宽度」表述，改为「既不定宽也不定高」；新增 **「封面尺寸推导规则」** 小节——vertical（宽铺满→高反推）与 horizontal（高=内容列高→宽反推）互为镜像的公式、封面 margin 承担另两边内缩、**窄档封顶公式**、**必须脚本回填的 JS 范式**（含 Chrome auto 轨道不收敛的根因说明）
   - **内边距规格**：新增 **「horizontal 容器上/下必须为 0」硬约束**；分层值表新增 horizontal 独立列（5 档均为 `0/0/0/8` 或 `0/0/0/4`）；补「封面 margin 分档」与「带操作区时 Body 下归 0、Footer 上不叠加」；新增 **cover + horizontal 实际可视间距（直读值）表**
   - **规则 5（封面比例）· 水平布局特殊规则**：废止「宽度可自定义（如 200px、220px）」；错误示例新增「写死封面宽度」「只靠 CSS 让封面跟随列高」两条
   - **规则 6（按钮显示条件）**：新增 **「Footer 是灵活可选的段」裁定**、**断点行为表**（≤600px 一律不显示）、**`:has()` 陷阱警告**（`display:none` 不解除 `:has()`，须显式恢复 Body 下内边距）、流程图补断点修正分支
   - **响应式行为**：原文「OCard 无自动断点响应式行为」改为「无自动**切换布局**的行为」，并列出确实有断点规则的三项（内边距分档 / 文本行数上限 / ≤600px 操作区可见性）
   - **组件层级结构**：Footer 标注「可选」并注明单跳转卡无此节点；封面宽高描述改为推导式
2. **`floors/floor-card.md`**：新增 **§5.1.1 cover + horizontal 专项规则**——≥841px 三档按既有数量规则，**601~840px 与 340~600px 一排只放一张**，附 4 条踩坑记录（flex 下须 `flex: 0 0 100%` 否则被 flex-shrink 收缩回 330px、两列 grid 下须 `grid-column: 1 / -1`、封面 margin 承担上下内缩、≤600px 去操作）；§5.2 内边距条目原「容器 `padding: 8px 8px 0`」限定为 vertical 专属并补 horizontal 口径
3. **`references/examples/opendesign-card-check.html`**：同步 CSS / 说明区 / 规格表 / 演示文案，新增「裁定 A（Footer 灵活可选）/ B（≤600px 去操作）/ C（≤840px 一排一卡）」三条说明；§② 演示顺序调整为「单跳转（默认形态）在前、多交互在后」

#### 实现踩坑记录（复用时注意）

| 坑 | 现象 | 正解 |
|---|---|---|
| `align-self: center` | 封面封顶变矮时上下 margin 被撑成 8.4px（≠4px），三边内缩不一致 | 改 `align-self: start`，贴卡顶内缩、底部留白 |
| 分档规则写在基础规则之前 | 同特异性 (0,3,0) 下，≤840 档 `margin: 4px` 被后部基础规则 `margin: 8px` 按源顺序反覆盖，实测内缩仍是 8 | 分档规则移到基础规则**之后** |
| flex 行内只写 `width: 100%` | 被 `flex-shrink` 收缩回 `(容器宽−间距)/2`（601~840 档 = 330px），两张卡仍并排 | `flex: 0 0 100%` |
| `:has(.ocard__footer)` | footer 被 `display:none` 后仍命中，Body 下被压成 0、「描述底→卡底」= 0 | 隐藏 footer 时显式恢复该档 Body 下内边距（12px） |

#### 影响面

- ⚠️ 破坏性：按旧版生成的 `cover + horizontal` 卡片，封面定宽（如 120px）、内容区纵向间距为 32px（应 24px）、窄档文字列被挤到不可用，需按新规则重新生成
- Playwright 实测（五档同页验收页）：封面比例恒为 1.778、无溢出；≥1681 档「内容区→卡顶 / 按钮底→卡底 / 封面→标题」均为 24px、标题→描述 12px、描述→操作 24px；601~840 与 340~600 两档 horizontal 均一排一张；≤600 档所有卡片操作区已去除

---

### 第十三轮：修正 Tab 禁用态在 S 档（601~840 / 340~600px）竖排下的文字错位

> 用户发现 340~600px 档「Tab 的禁用态与别的没有对齐，应该为都顶对齐」。

#### 修正内容

| 项 | 改前 | 改后 |
|---|---|---|
| `.otab__item:disabled::after` | `display: none` | `visibility: hidden` |

**根因**：S 档 Tab 为竖向 56×56（`::after` = 16px 短线 + `margin-top: 4px`，**在文档流内占位**），`display: none` 会把占位一并删掉；禁用项在 `flex-direction: column; justify-content: center` 下只剩文字、整体垂直居中，而其他页签是「文字 + 4px + 16px 短线」整体居中——禁用项文字因此比同排其他页签低一截（实测错位约 10px）。

**修复**：`visibility: hidden` 视觉上仍不可见（符合 tab.md「移除所有指示器」），但保留 16px + 4px 占位，文字与其他页签对齐。对 L / M 档无副作用（指示器为绝对定位，本就不占文档流）。

1. **`components/tab.md`**：Part C 交互行为说明「Disabled」条目补充「移除指示器指视觉不可见、不是移出布局」细则与正确写法（正确写法代码块同步加 `.otab__item:disabled::after { visibility: hidden; }`，并 ⛔ 禁写 `display: none`）
2. **`floors/floor-card.md`**：§六 Tab 交互态骨架同步改为 `visibility: hidden` 并附根因注释；§七自检清单新增一条（禁用态指示器是否 `visibility: hidden`、S 档竖排时禁用页签文字是否与同排其他页签同一水平线）
3. **`references/examples/opendesign-floor-card-check.html` / `opendesign-floor-card-5check.html` / `opendesign-floor-card-7check.html`**：三页验收案例的禁用态规则同步修正

#### 影响面

- ⚠️ 破坏性（轻微）：按旧版生成的 601~840 / 340~600 两档楼层，禁用页签文字低位约 10px，需同步修正

---

### 第十二轮：>5 卡新增「一排四列换行」规则，并新增 7 卡验收案例

> 用户裁决：「超过 5 个卡片的话，就以一排四个换行两个的规则处理」。

#### 修正内容

| 卡片数量 | 布局规则（改前） | 布局规则（改后） |
|---|---|---|
| 1~5 卡 | 单行横排（≤600px 一排两卡横滑） | 不变 |
| **>5 卡** | **无规定**（沿用单行横排，卡宽被压缩到不可用） | **一排四列、换行排布**（`repeat(4, 1fr)`）：7 卡 = 第一行 4 张 + 第二行 3 张，末行左对齐右侧留空 |

**规则细则**：

- **列数固定 4**（PC 24 列每卡占 6 列，与 card-grid.md PC 四列口径一致）：≥1681px 卡宽 = `(1488 − 3×32) / 4 = 348px`。⛔ 不是 5 列、不铺满一行。
- **末行左对齐**：不满 4 张时占左侧列、右侧留空（grid 自动行为）。⛔ 禁止把末行 3 张拉伸各占 1/3 宽——两行卡宽不一致破坏栅格对齐。
- **换行必须有垂直间距**：卡片垂直间距 = 该档水平间距（32/24/16/16/12）。⛔ 禁止 `gap: 0 32px` 只留水平间距的写法。
- **601~840px 仍四列**（卡宽 157px @740）。
- **340~600px：横滑（默认）与两列平铺二选一**（用户裁决：「340~600px 断点下是默认横滑，但超过 6 个卡片也可以横滑，但同时也支持像现在一样都平铺出来」）：该档容器仅 312px，放 4 列只有 69px/卡不可用，故不沿用四列。模式 A 横滑（默认）= 一排两卡 150px + `overflow-x: auto` + 右端外扩 36px，规则同 ≤5 卡；模式 B 两列平铺 = `repeat(2, 1fr)`，卡宽同为 150px，全部展示不横滑，7 卡 = 2+2+2+1 四行。两模式卡宽相同，区别只在「横滑承载」还是「全部平铺」，选型由业务决定。

1. **`floors/floor-card.md`**：§一架构树与元素表补数量分档；§四间距表「卡片垂直间距 / 卡片列数」两行改为按卡数量分档表述（340~600px 列记「横滑（默认）或两列平铺」）；§五重构为 5.1 数量分档（硬约束，含末行左对齐、禁止拉伸铺满、必须有垂直间距、移动端双模式等细则）+ 5.2 ≤5 卡单行规则；§六新增「>5 卡（一排四列换行）骨架示例」（含 `.is-tile` 修饰符的移动端双模式 CSS）
2. **`references/examples/opendesign-floor-card-7check.html`**（新增）：7 卡五档断点同页验收；工具栏含「340~600px 模式：横滑（默认）/ 两列平铺」单选切换（postMessage 驱动 iframe 内 `.floor-card__cards.is-tile`）。Playwright 实测：1920/1440/1080/740 四档均为 4 列两行（348 / 293.2 / 242 / 157px，末行 3 张左对齐）；360 档横滑模式 7 张卡同处一行（150px/张，第 3 张起 x=348 露出 12px），平铺模式为 2+2+2+1 四行（末行 1 张左对齐），来回切换均正常
3. **`references/examples/opendesign-floor-card-check.html` / `opendesign-floor-card-5check.html`**：≤5 卡规则未变，两页无需改动

#### 影响面

- ⚠️ 破坏性：按旧版生成的 6 卡及以上楼层（单行横排）卡宽严重不足，需按四列换行重新生成
- 边界说明：移动端双模式按「>5 卡（6 张及以上）」与 PC 档统一口径落地；用户原话为「超过 6 个卡片」，若实际意图是严格 7 张以上才允许平铺、6 张只能横滑，改 5.1 表格一行即可

---



### 第十一轮：Laptop（1201~1680px）档楼层边距由 40px 修正为 ≈98px（楼层栅格 = grid-22）

> 用户纠正：「1201~1680px Laptop 错误，两边边距应该是 98px，因此这个档位栅格应该也错了」。

#### 修正内容

| 档位 | 楼层宽度规则 | 每边边距（改前） | 每边边距（改后，1440px 为例） | 内容区宽度（改后） |
|---|---|---|---|---|
| ≥1681px | grid-20（两侧各空 2 列） | 216px | 216px（不变） | 1488px（不变） |
| 1201~1680px | **grid-22**（一侧空 1 列） | ~~40px~~ | **≈98px**（40 + 33.7 + 24） | **≈1244.7px** |
| 841~1200px | grid-full（占满） | 32px | 32px（不变） | 视口 − 64（不变） |
| 601~840px | grid-full（占满） | 32px | 32px（不变） | 视口 − 64（不变） |
| 340~600px | grid-full（占满） | 24px | 24px（不变） | 312px（不变） |

**关键点**：边距 ≠ 栅格外侧留白 `--o-r-grid-padding`。上游「楼层栅格适配规范」（opendesign-tokens/SKILL.md）规定 **1201–1680px 楼层宽度 = `--o-r-grid-22`，一侧空白 1 列**，故边距 = 留白 + 1 列 + 1 水槽。原规范只取了留白 40px，漏掉楼层还要再退的 1 列 + 1 水槽。同源验证：≥1681px 档的 216px = 64 + 2×(44+32)（grid-20 两侧各空 2 列），公式完全一致——两锚点档（1488+216 / 312+24）本就是 grid-20 / grid-full 在基准画布（1920 / 360）下的取值，中间档没有理由只留白不空列。

1. **`floors/floor-card.md`**：§一架构树「内容容器」注释、§六「中间三档」规则与 CSS 骨架同步修正——Laptop 按 `grid-22` 落地（1201~1440 小桌面栅格留白 40/水槽 24；1441~1680 桌面栅格留白 64/水槽 32，边距随视口 120→130px），Pad 两档维持 grid-full + 32px；卡片水平间距仍固定按模板间距表执行，不随水槽 Token 漂移
2. **`references/examples/opendesign-floor-card-check.html`** 与 **`opendesign-floor-card-5check.html`**：Laptop 档容器由 `calc(100vw - 80px)` + `margin: 40px` 改为 grid-22 公式（`calc((100vw - 632px) * 11 / 12 + 504px)` + `margin: auto` 居中）；5 卡页速查表补「内容容器（楼层栅格适配）」三行（楼层宽度规则 / 每边边距 / 内容区宽度），5 卡理论卡宽 Laptop 列由 252.8px 改为 ≈229.7px

#### 影响面

- ⚠️ 按旧版生成的 Laptop 档页面内容区偏宽 115px/侧（1360 → 1244.7），卡片等分宽度随之偏大，需按新边距复核
- 5 卡在 Laptop 档卡宽由 252.8px 收窄到 ≈229.7px；Pad 竖屏 122.4px 仍为最紧档（单行横排 5 卡的可行性下限，若实际项目需覆盖该档，建议裁决为横滑或两行）

---


### 第十轮：601~840px 档 Tab 由 M 改为 S（small）

> 用户纠正：「601~840px 断点下 tab 也是用的 S 尺寸」。

#### 修正内容

| 档位 | Tab 尺寸（改前） | Tab 尺寸（改后） |
|---|---|---|
| ≥1681px | L | L（不变） |
| 1201~1680px | M | M（不变） |
| 841~1200px | M | M（不变） |
| 601~840px | M | **S** |
| 340~600px | S | S（不变） |

**关键点**：601~840px（Pad 竖屏）与 340~600px（Phone）同属小屏，Tab 统一取 S（竖向 56×56、14px/22px、16px 居中短线指示器）。⚠️ 媒体查询必须从 `@media (max-width: 1680px)` 收紧为 `@media (min-width: 841px) and (max-width: 1680px)`——若只把 S 档查询从 `max-width: 600px` 放宽到 `max-width: 840px` 而不动 M 档查询，M 规则仍会命中 601~840px（源码顺序在后，S 被 M 盖掉）。

1. **`opendesign-floor-card-check.html`**：M 档查询收紧为 `min-width: 841px`，S 档查询由 `max-width: 600px` 放宽为 `max-width: 840px`；速查表「Tab 尺寸」行 601~840 列改 S；Pad 竖屏预览说明改「Tab S」
2. **`floors/floor-card.md`**：涉及组件表 size 改 `L / M / M / S / S`；🔴 段改为「601~840px / 340~600px 两档 Tab 用 S」；§六 Tab 尺寸骨架的 M/S 媒体查询同步收紧/放宽 + 补成对编写警告；自检清单 Tab 尺寸项同步
3. **`components/tab.md`**：响应式行为由「移动端（360px）推荐 small」改为「小屏两档（601~840px / 340~600px）推荐 small」
4. **`global/hard-constraints.md`**：自检 #29 的 Tab 尺寸分档同步更新
5. **`global/hard-constraints.md` §1 新增「🔴 小屏口径」裁决（根因修复）**、**`global/layout-rules.md` §1 同步指引**

#### 根因深挖：不只是改一个数值，而是补一条全局口径

**表层**是 601~840px 档 Tab 取值错了；**根因是「移动端 / 小屏」这个词在规范里有两个互不一致的边界，且没有任何一处做过裁决**：

| 出处 | 对「移动端」的定义 |
|---|---|
| `hard-constraints.md` §1 断点定义（改前） | MB = **360~600px**（且这是 SKILL.md 要求"生成任何页面前必读"的文档） |
| `components/tab.md` 响应式行为（改前） | 「移动端（**360px**）推荐 small」 |
| 实际需要 | 组件尺寸选型的「小屏」= **≤840px**（含 Pad 竖屏 601~840px） |

按必读文档的口径，「移动端」只到 600px，于是 601~840px（Pad 竖屏，如 iPad mini 768px）被划到 PC 类别的中间档，Tab 跟随 PC 风格取了 M。**这与第九轮容器 padding 8→4 是同一类错误的两个实例：都把 601~840px 当成了"PC 的缩小版"，而没认识到它在交互尺寸上属于小屏。**

裁决落点（本次新增）：`hard-constraints.md` §1 明确区分两个口径——**变量映射列**的 MB 仍是 360~600px（决定 §2~§7 取哪列 Token），**组件尺寸选型**的「小屏」是 ≤840px（决定 Tab / Button 的 size）。已按新口径落地的分档：OTab = L/M/M/S/S、OButton text = L/M/M/S/S。

### 第九轮：601~840px 卡片容器 padding 由 8px 收到 4px

> 用户纠正：「601~840px 断点的 CSS：容器 padding 上/右/下/左应该是 4/4/0/4，封面 → 卡片 上/左/右是 4/4/4」。

#### 修正内容

| 档位 | 容器 padding 上/右/下/左 | Body padding 上/右/下/左 | 封面→卡 上/左/右 | 文字→卡 左/右 |
|---|---|---|---|---|
| 601~840px（改前） | 8 / 8 / 0 / 8 | 12 / **4** / 12 / **4** | 8/8/8 | 12/12 |
| 601~840px（改后） | **4** / **4** / 0 / **4** | 12 / **8** / 12 / **8** | **4/4/4** | 12/12 |

**关键点**：为保持「文字→卡片左/右 = 12px」这一可见值不变，Body 左右必须**同步从 4px 提到 8px**（12 = 容器 4 + Body 8）。若只改容器不改 Body，可见值会掉到 12→8。改后 601~840px 与 340~600px 两档的容器与 Body 完全同值（4/4/0/4 + 12/8/12/8），封面距卡片边缘统一为 4px。

1. **`opendesign-floor-card-check.html`**：601~840px 档补 `.ocard { padding: 4px 4px 0; }`，Body 由 `12px 4px 12px` 改为 `12px 8px 12px`；CSS 注释五档速查表同步
2. **`components/card.md`**：分层值表 601~840 行改为 `4/4/0/4` + `12/8/12/8`；直读值表「封面 → 卡片 上/左/右」601~840 列由 `8/8/8` 改为 **`4/4/4`**
3. **`floors/floor-card.md`**：§五 条目补「601~840px 与 340~600px 两档容器收到 4px」；§六 骨架 601~840 档同步
4. **`global/hard-constraints.md`**：自检 #32 的容器值与封面→卡边缘值同步更新

#### 实测复核（无头 Chrome，五档）

| 检查项 | 结果 |
|---|---|
| 601~840px 容器 padding | **4/4/0/4** ✔️ |
| 601~840px 封面 → 卡片 上/左/右 | **4/4/4** ✔️ |
| 601~840px Body padding | 12/8/12/8 ✔️ |
| 601~840px 文字→卡左/右、文字底→卡底 | 12/12、12（与改前一致）✔️ |
| 全量回归（间距 35 项 / 卡片内间距 5 档 / Tab 尺寸 / 四态+禁用隔离 / 行数截断 / 横滑） | **122 项全部通过** ✔️ |

### 第八轮：禁用态被 hover/press 覆盖（Tab 禁用项划过就变色）

> 用户反馈「tab 的禁用还能有交互的吗？？离谱，是哪里的问题，请排查」，附截图显示禁用页签呈 `primary2`（hover 色）。

#### 复现与根因（无头 Chrome 实测）

| 场景 | 修正前实测 | 应为 |
|---|---|---|
| 禁用项 默认 | `400 / rgb(153,153,153)` 光标 not-allowed | 同左 ✔️ |
| 禁用项 hover | **`400 / rgb(81,119,202)`（primary2）** ❌ | info4 不变 |
| 禁用项 press | **`400 / rgb(0,39,147)`（primary3）+ 指示器变色** ❌ | info4 不变、无指示器 |

**根因是 CSS 特异性，不是 `:disabled` 失效**（元素是 `<button disabled>`，`:disabled` 正常命中）：

```
.otab__item:hover:not(.is-active)   =  .otab__item(1类) + :hover(1伪类) + :not(.is-active)(1类)  = (0,3,0)
.otab__item:disabled                =  .otab__item(1类) + :disabled(1伪类)                       = (0,2,0)
```

hover 规则特异性更高 → **禁用色被直接盖掉**。`:not()` 会继承其参数的特异性，所以 `:not(.is-active)` 让 hover 悄悄升了一级，越过了禁用规则。

**为什么是我方规范的漏算**：tab.md Part C 列了 Disabled 状态，却**从未规定 hover / press 要排除禁用项**；我在第一轮补交互态时照抄了 Part C 的 hover/press 规则，也没补 `:not(:disabled)`。正确写法其实早已存在于 `references/examples/dropdown-demo.html`（`:hover:not(.disabled)` + `.odropdown-panel-item.disabled:hover`），但**没有一条规范要求必须这么写**——所以该错的每次都错。

#### 修正内容（这是同一类陷阱第三次复现，从根上堵）

1. **`opendesign-floor-card-check.html`**：hover / press 三条规则全部补 `:not(:disabled)`，让交互态**从选择器层面就不匹配禁用元素**：
   ```css
   .otab__item:hover:not(.is-active):not(:disabled) { color: var(--o-color-primary2); }
   .otab__item:active:not(.is-active):not(:disabled),
   .otab__item.is-active:active:not(:disabled) { color: var(--o-color-primary3); }
   .otab__item:active:not(:disabled)::after { background: var(--o-color-primary3); }
   ```
2. **`components/tab.md`**：Part C Disabled 条目补「禁用是终态，hover/press 一律不得生效」；新增 🔴 专段「为什么 hover / press 必须带 `:not(:disabled)`」，含特异性算式、实测数据、正确写法与次选写法对比
3. **`global/hard-constraints.md`**：**新增 §2.2「🔴 禁用态铁律（全局，无例外）」**——与 §2.1 字重铁律并列，明确禁用元素上 hover/press/focus/展开必须与禁用默认态逐项相同、点击不得切换选中态；给出 ✅/❌ 写法对照与 JS 侧要求。§2.1 末尾补「同类陷阱」交叉引用
4. **`floors/floor-card.md`**：§四 #5 由「两条硬规则」扩为 **三条**（新增 `:not(:disabled)`）；§六 Tab 交互态骨架同步；§七 自检清单新增一项
5. **`global/hard-constraints.md` 自检清单**：新增 **#33（禁用态隔离·全局）**

#### 实测复核（无头 Chrome，五档 × 10 项 = 50 项）

| 检查项 | 结果 |
|---|---|
| 禁用 hover 后颜色/字重/指示器 | `400 / rgb(153,153,153) / none`，与禁用默认态完全一致 ✔️ |
| 禁用 press 后颜色/字重/指示器 | `400 / rgb(153,153,153) / none` ✔️ |
| 禁用光标 | `not-allowed` ✔️ |
| 禁用项点击 | 不改变 `is-active` 归属 ✔️ |
| 正常项四态（默认/hover/press/选中+hover） | `400|#333` → `400|primary2` → `400|primary3+指示器` → `600|primary1` 均正确 ✔️ |

> **遗留待查**：全库 grep 显示 `floors/navigation-dropdown-categorized.md` 的分类条目（`.o-nav__cat-item:hover .o-nav__cat-item-title`）与快捷卡（`.o-nav__quick-card:hover .o-nav__quick-card-title`）若存在禁用条目，同样可能未排除；但该文档的禁用条目使用 `.is-disabled` 类而非原生 `disabled`，需按其自身状态类名单独核对——本次未改动，列入后续核查。

### 第七轮：卡片内间距按断点分档（五档收紧）

> 用户先要求「根据不断点下单个卡片里面的内间距，输入给我」，在拿到实测表后直接给出目标值表，要求「基于这个修改」。

#### 用户给定的目标值（可见值口径）

| 项目 | ≥1681px | 1201~1680px | 841~1200px | 601~840px | 340~600px |
|---|---|---|---|---|---|
| 封面 → 卡片 上/左/右 | 8/8/8 | 8/8/8 | 8/8/8 | 8/8/8 | 4/4/4 |
| 封面 → 标题 | 24 | 16 | 12 | 12 | 12 |
| 标题 → 描述 | 12 | 8 | 8 | 8 | 8 |
| 标题 / 描述 → 卡片 左/右 | 32/32 | 24/24 | 16/16 | 12/12 | 12/12 |
| 描述文字底 → 卡片底 | 24 | 16 | 12 | 12 | 12 |

#### 关键发现：水平方向存在叠加，用户的 Body 值与目标可见值不自洽

用户同时给了一列「CSS：Body padding 上/右/下/左」（24/32/24/32 等），但该列与目标可见值**在左右方向冲突**：Body 排在封面下方，其左/右边缘本身已被容器 `padding-left/right` 推离卡片边缘，故

```
「标题→卡片左」= 容器左 + Body 左     （竖向不叠加：封面在 Body 上方；容器底部为 0）
```

按用户给的 Body 值实测，可见左内间距变成 40 / 32 / 24 / 20 / 16，比目标多出容器那 8px（或 4px）。**按目标可见值反推 Body 左右**（容器左/右 分别为 8,8,8,8,4）：24 / 16 / 8 / 4 / 8。

#### 修正内容

1. **`opendesign-floor-card-check.html`**：`.ocard__body` 改分档 `24/24/24/24` → `16/16/16/16` → `12/8/12/8` → `12/4/12/4` → `12/8/12/8`；`.ocard__desc` `margin-top` 默认 12px、≤1680px 覆盖为 8px；340~600px 容器 padding 收到 `4px 4px 0`。CSS 注释补五档速查表 + 水平叠加警示
2. **`components/card.md` 内边距规格**：改为「分层值（按断点分档）」+「实际可视间距（直读值，校验用）」两张五列表；新增 ⚠️ **水平方向叠加陷阱**说明；补「为什么分档」理由（150px 卡配 32px 留白只剩 86px 可用宽，收紧后文字可用宽 411/389/296/191/126px）
3. **`floors/floor-card.md`**：§五 条目改为「单层口径 + 断点分档」并标注水平叠加；§六 新增「卡片内间距骨架示例」整段可抄 CSS + 叠加警示
4. **`global/hard-constraints.md`**：自检清单新增 **#32（卡片内间距）**，含五档数值、水平叠加陷阱与直读表引用

#### 实测复核（无头 Chrome，五档）

| 检查项 | 结果 |
|---|---|
| 容器 padding 上/右/下/左 | 8/8/0/8 ×4 档、4/4/0/4（340~600）✔️ |
| Body padding 上/右/下/左 | 24/24/24/24 · 16/16/16/16 · 12/8/12/8 · 12/4/12/4 · 12/8/12/8 ✔️ |
| 封面→卡 上/左/右 | 8/8/8 ×4 档、4/4/4 ✔️ |
| 封面→标题 | 24 / 16 / 12 / 12 / 12 ✔️ |
| 标题→描述 | 12 / 8 / 8 / 8 / 8 ✔️ |
| 标题/描述→卡片 左/右 | 32/32 · 24/24 · 16/16 · 12/12 · 12/12 ✔️ |
| 描述文字底→卡片底 | 24 / 16 / 12 / 12 / 12 ✔️ |
| 派生校验（5 项/档） | 全部自洽 ✔️ |
| 文字可用宽度 | 411 / 389 / 296 / 191 / 126 ✔️ |
| 其余（间距表 35 项 / Tab 尺寸 / Tab 四态 / 行数截断 / 横滑外扩） | 全部回归通过 ✔️ |

### 第六轮：卡片内边距改为单层口径（"到卡片边缘"的值可直读）

> 用户核对各断点单卡内间距时追问「Body底距卡底具体是指哪里」，指出「为什么不直接是描述文字底边 → 卡片底边」，并确认按推荐的单层口径（方案 B）实施。

#### 根因

旧模型是**两层叠加**：`.ocard { padding: 8px }` 的 8px 本意是服务封面的（封面距卡片边缘必须 8px），但 Body 排在封面下方，它到卡片底边经过的就是这 8px——于是 Body 又自己写 `padding-bottom: 16px`，两层叠出 24px。结果是「文字 → 卡片边缘」每个值都要心算两步才能校验：

| 测量项 | 旧口径 | 心算 |
|---|---|---|
| 描述文字底 → 卡片底 | 24px | Body 16 + 容器 8 |
| 标题/描述 → 卡片左/右 | 24px | Body 16 + 容器 8 |

且同一个「24px」在不同位置由不同层贡献，读代码时无法直接对应到视觉。

#### 修正内容（视觉结果零变化）

1. **`components/card.md` 内边距规格改为单层口径**：
   - 卡片容器 padding 由 `8px` 改为 **`8px 8px 0`**（只服务封面，底部归 0）
   - Body padding 由 `24px 16px 16px` 改为 **`24px 16px 24px`**（底部直接写实际值）
   - 表格拆为两张：**「分层值」**（容器 / Body / Header / Footer 各自 padding）+ 新增 **「实际可视间距（直读值，校验用）」**（封面→卡边缘 8、封面→标题 24、文字→卡左/右 24、**文字底→卡底 24**、Footer 内容→卡边缘 24），每行标注构成
   - 附「为什么改单层口径」说明：旧模型要心算两步、且容器 8px 被顺带当成文字底部留白用；新版每个"到卡片边缘"的值都能直读，视觉与旧版完全一致
   - Footer 内边距行补齐（上 24 / 右 16 / 下 24 / 左 16），与 Body 同构
2. **`components/card.md` Footer 对齐硬约束修正**：原文写「在"容器 8px + Body 额外 8px = 总计 16px"的模型下，Footer 也需要额外 `padding: 0 8px`」——**该算术与实际实现不符**（实际 Body 左右为 16px、总计 24px）。已改为表述约束本质：「Footer 左右内边距必须与 Body **取相同值**（当前 16px）」，并明确这不是固定数值、改 Body 左右值时 Footer 必须同步改，否则按钮与正文左右错位
3. **`floors/floor-card.md` §五**：「卡片内部样式」条目补单层口径指向（容器 `8px 8px 0` / Body `24px 16px 24px`），并要求校验时直接用 card.md「实际可视间距（直读值）」表，**禁止把容器 padding 与 section padding 叠加心算**
4. **验收页 `opendesign-floor-card-check.html`**：`.ocard` 改 `padding: 8px 8px 0`、`.ocard__body` 改 `padding: 24px 16px 24px`，注释写明单层口径与各直读值

#### 实测复核（无头 Chrome，五档）

| 检查项 | 结果 |
|---|---|
| 容器 padding 上/右/下/左 | 8/8/**0**/8 ✔️（五档一致） |
| Body padding 上/右/下/左 | 24/16/**24**/16 ✔️（五档一致） |
| 封面 → 卡片 上/左/右 | 8/8/8 ✔️ |
| 封面 → 标题 | 24 ✔️ |
| 标题/描述 → 卡片 左/右 | 24/24 ✔️ |
| **描述文字底 → 卡片底（单层直读）** | **24** ✔️ |
| 卡片高度（视觉零变化） | 400 / 371 / 308 / 244 / 207，与改前逐档一致 ✔️ |
| 其余（间距表 / Tab 尺寸 / Tab 四态 / 卡片分档截断 / 横滑外扩） | 全部回归通过 ✔️ |

### 第五轮：340~600px 横滑容器右侧间距按「卡片水平间距规则」排布

> 用户验收截图指出：340~600px 档右侧露出部分（红框标注）不对——「超出部分的卡片应该同样以卡片水平间距的规则显示」。

#### 根因

无头 Chrome 实测（360px 视口）确认问题：`.floor-card__cards` 只写了 `overflow-x: auto`，**右侧完全没有任何间距处理**——

| 项 | 实测值（修正前） |
|---|---|
| 卡片容器 clientWidth | 312（= 内容容器宽，无右内边距） |
| 卡片1 / 卡片2 | 24~174 / 186~336（前两卡正好占满 312） |
| 卡片3 left | **348 → 完全在可视区外，被整张裁掉** |
| 卡片2 右缘 vs 容器右缘 | 336 = 336，**紧贴，零间距** |
| 336~348 那 12px | 是卡片2↔卡片3 的 margin，但落在容器外，渲染成一条**白缝** |

即：用户既看不到"右边还有卡、可横滑"的提示（第 3 张被完全裁掉），卡片2 右缘又紧贴容器边缘，中间那条 12px 缝露在容器外变成白缝——**超出部分的排布完全没有遵循 12px 间距规则**。

#### 修正内容

1. **`floors/floor-card.md` §五 新增硬约束条目「340~600px 横滑容器的右侧间距」**：明确横滑序列右端也要按间距规则处理——① 容器 `padding-right: 12px`（滚到最右时末卡与右缘同为 12px，与卡间距一致）；② 容器向右外扩 `width: calc(100% + 36px)`，使**第 3 张正好露出一个间距（12px）**，让用户看出"右边还有卡、可横滑"。并写明禁止事项与不处理的后果（第 3 张被完全裁掉、卡片2 右缘贴边、12px 缝露在容器外变白缝），以及"外扩量用 `width` 撑开不占文档流宽度、不会挤占「查看更多」位置"
2. **`floors/floor-card.md` §六 CSS 骨架**：`@media (max-width: 600px)` 内 `.floor-card__cards` 由 `overflow-x: auto` 补全为 `overflow-x: auto; width: calc(100% + 36px); padding-right: 12px`，注释写明 36px 的构成（右内边距 12px + 第3张露出 12px）
3. **`floors/floor-card.md` 自检清单**：原「一排两卡」项后新增一项，专查横滑容器右侧间距规则（`width: calc(100% + 36px)` + `padding-right: 12px`、第 3 张露出 12px、滚到最右末卡右缘 12px）
4. **`global/hard-constraints.md` #29**：并入横滑容器右侧外扩要求，禁止项补「禁止横滑容器只写 `overflow-x: auto` 而不管右端」
5. **验收页 `opendesign-floor-card-check.html`**：`.floor-card__cards` 同步改为 `overflow-x: auto; width: calc(100% + 36px); padding-right: 12px`；Phone 档位说明改为「卡片一排两卡，第 3 张露出一个间距并横滑」

#### 实测复核（无头 Chrome，360px 视口）

| 检查项 | 结果 |
|---|---|
| 卡宽 | 150px ✔️ |
| 相邻卡片间距 | 恒 12px ✔️ |
| 第 3 张露出宽度 | **12px**（正好一个间距单位）✔️ |
| 第 3 张已露出（可提示横滑） | left 348 < 360 ✔️ |
| 前两卡完整可见 | 卡片2 right 336 ≤ 348−12 ✔️ |
| 仍可横滑 | scrollWidth 486 > clientWidth 348 ✔️ |
| 滚到最右末卡右缘距容器右缘 | **12px** ✔️ |
| 「查看更多」按钮位置 | 24~336 未被外扩挤占 ✔️ |
| 其余四档间距 / Tab 尺寸 / Tab 四态 / 卡片分档截断 | 全部回归通过 ✔️ |

### 第四轮：hover 禁改字重升级为全局铁律 + 卡片文本行数改为按断点分档

> 用户验收时提两条：① 「hover 态文字不能加粗字重！！只有选中态才有字重变化！！**这个是全局的规范**」；② 「再次纠正：断点 840-601px 和 600-360px 的卡片文字限制为标题只显示一行，超出末尾加 ... 省略；描述文本只显示两行，超出末尾加 ... 省略」。

#### 全库排查结果（第 ① 条）

逐处检索 `skills/` 内所有 hover 规则与交互状态矩阵：

| 位置 | 结论 |
|---|---|
| `components/tab.md` Part C | 上一轮已修（Hover/Press 非选中 → Regular 不变） |
| `floors/navigation.md` 触发器 | 已合规（第 167 行 `font-weight: 400` + 自检项 #19/踩坑 #17） |
| `floors/navigation-dropdown-categorized.md` 分类条目 | 已合规（踩坑 #30 明令禁止 hover/active 改字重） |
| `components/dropdown.md` C.3 菜单项 | **Hover 已是 Regular(400)**；Selected 为 Medium(500)、Active/Press 为 Semibold(600)——均非 hover，未违规（如需收紧属于另一议题，本次不动） |
| `components/navigation.md` / `search.md` 建议项 | 残留的 `font-weight: 600` 在 `.o-search-suggest__match`（**关键词高亮**），非 hover 规则 |
| `components/button.md` | 无 hover 改字重 |

**结论：除 tab.md（已修）外无其他违规。** 但用户明确这是「全局的规范」，而此前该禁令散落在三处（navigation.md 踩坑 #17、tab.md Part C、navigation-dropdown-categorized.md 踩坑 #30），彼此独立、没有一处被后来者必然看到——这正是它反复复发的结构原因。故本次**提升为全局铁律**。

#### 修正内容

1. **`global/hard-constraints.md` 新增 §2.1「🔴 字重铁律（全局，无例外）」**：
   - 明确**字重只承担「是否选中 / 是否当前项」一个语义**；hover / press / focus / 展开等一切瞬态**一律不得改变字重**，Disabled 回落 Regular 属弱化态不算违规
   - 附状态矩阵表（Default / 选中 ✅ / Hover ❌ / Press ❌ / Focus ❌ / Disabled）
   - 写清三条「为什么」（语义混淆导致无法从字重判断当前位置；实测拉丁标签 400→600 宽 +1.91px/+1.03px 引起文字与布局跳动、**纯中文等宽差值 0px 所以只在英文标签暴露、极易漏检**；已在三个组件重复踩过、根源是规范互相矛盾且只修一侧）
   - 附 ✅/❌ 正确写法对照 + 附带陷阱（Press 特异性必须 ≥ Hover，否则按下变色被 hover 盖掉）
2. **`global/hard-constraints.md` 自检清单新增 #30（字重铁律·全局）与 #31（卡片文本行数分档）**；#29 去掉已由 #31 接管的 line-clamp 表述，避免两处口径
3. **`components/tab.md` Part C 🔴 专段**：标题改为「已升级为 hard-constraints.md §2.1 全局字重铁律」，第 3 条理由补入 navigation-dropdown-categorized.md 踩坑 #30，并加一句用户裁决原话「这是全局规范，不是 Tab 的局部约定」
4. **`components/card.md` 硬约束规则 8 改为按断点分档**：

   | 文本元素 | ≥841px | ≤840px（601~840px / 340~600px） |
   |---------|--------|------------------------------|
   | 卡片标题 | 2 行 | **1 行** |
   | 卡片描述 | 3 行 | **2 行** |

   - 补充「为什么窄屏要收紧」：窄屏卡片宽度可能只有 150px（两列），2 行标题会把卡片撑得过高、单屏可见卡片数骤降；标题 1 行 + 描述 2 行可在有限高度内保留更多卡片
   - CSS 示例给出 `@media (max-width: 840px)` 只改 `-webkit-line-clamp` 数值的写法（四件套其余三条沿用）
   - 「注意事项」里那条同款硬约束同步改为分档表述
5. **`floors/floor-card.md`**：§五 行数约束改分档 + 引 card.md 规则 8；§六 CSS 骨架补 `@media (max-width: 840px)` 分档写法；自检清单对应两条改写，并新增「hover / press 是否一条 font-weight 都没写」
6. **验收页 `opendesign-floor-card-check.html`**：`.ocard__title` / `.ocard__desc` 基准保持 2/3 行，新增 `@media (max-width: 840px)` 覆盖为 1/2 行；规格速查表新增「卡片标题行数上限」「卡片描述行数上限」两行（2/2/2/1/1 与 3/3/3/2/2）

#### 说明

- 本条两处均为**约束口径修正**，非视觉缺陷：① 把散落在三个文件里的同一条禁令收敛成一处全局铁律 + 一条全局自检项，从结构上杜绝「组件侧与楼层侧互相矛盾、只修一侧」的复发模式；② 窄屏行数收紧是为了在有限高度内保留更多卡片
- 已用无头 Chrome 实测复核（五档 × 间距 + Tab 尺寸/四态 + 卡片分档截断）：五档间距仍符合规格表；Tab 交互态四态全部不碰字重；卡片在 ≥841px 三档注入 200/400 字超长文案后实测截断到 2 行/3 行，≤840px 两档实测截断到 **1 行/2 行**，且 scrollH > clientH（省略号出现）

### 通用楼层卡片模块第三轮修正：Tab 340~600px 改 S 尺寸 + hover/press 禁止改字重 + 卡片文本行数上限

> 用户验收时提三条：① 纠正——340~600px 档 Tab 应用 **S 尺寸**（此前按 M）；② 「为什么 tab 的交互态 hover 都会字重变粗？这个问题发生过好多次」；③ 所有卡片文本都要有约束——标题不超过两行、描述不超过三行，超出末尾 `...`。

#### 根因排查（第 ② 条，用户要求"仔细排查"）

**「hover 加粗」不是实现手误，是组件规范与楼层规范长期互相矛盾，且楼层侧早已修过、组件侧一直没修。**

- **源头在 `components/tab.md` Part C**：交互状态表明文写着 `| Hover | 非选中 | 18px | **SemiBold** | --o-color-primary2 | 无 |`，即 hover 时字重从 Regular 加粗到 SemiBold。上一轮我补 Tab 交互态时是**忠实照抄**这条，所以又踩了一次。
- **而 `floors/navigation.md` 早就对同一问题立过相反的裁决**（第 167 行 `font-weight: 400; /* ⛔ 禁止改 SemiBold——规范未定义字重变化 */` + 第 461 行自检项「触发器全状态字重是否统一 Regular(400)——hover / 展开 / 选中均不碰 font-weight（⛔ 禁止切 Semibold：字重随交互切换会产生肉眼可见的文字跳动）」）。顶导修好了，Tab 组件规范没修 → 每处引用 tab.md 的生成都重新复现一次，这正是用户说"发生过好多次"的原因。
- **实测验证后果**：页签宽度由文字撑开（auto）+ 选中指示器为全宽，字重一切换文字 advance width 就变。用无头 Chrome 实测 18px 下 "Docs v1.2" 400→600 宽 **+1.91px**、"Blog" **+1.03px**——下划线随之变宽、后续页签被推挤位移。**纯中文等宽全角字形差值 0px**（实测"页签文字"400/600 均为 72px），所以这个 bug 只在英文/数字标签上暴露，中文 demo 完全看不出来，极易被漏检。

#### 修正内容

1. **`components/tab.md` Part C 修订（根因修复）**：Hover 非选中 / Press 非选中 字重 **SemiBold → Regular（不变）**，并在交互行为说明后追加 🔴 专段，写清三条理由（字重只承担"是否选中"一个语义，hover 也加粗会稀释选中态识别；实测拉丁标签 +1.9px 引起文字与下划线跳动；与 navigation.md 踩坑 #17 同源、组件侧漏改导致多次复现）+ 正确写法（只改 `color`，Press 特异性必须 ≥ Hover）
2. **`components/card.md` 新增硬约束规则 8（卡片文本行数上限）**：卡片标题**最多 2 行**、描述/正文**最多 3 行**，超出末尾 `...`；适用范围覆盖 cover / icon / full-cover 全部类型与全部断点；给出 line-clamp **四件套**（`display: -webkit-box` + `-webkit-box-orient: vertical` + `-webkit-line-clamp: N` + `overflow: hidden`）并标注「漏 `overflow: hidden` 则省略号不出现、文字直接溢出」这一最常见错误；「注意事项」同步补一条同款硬约束
3. **`floors/floor-card.md`**：
   - 「涉及组件」表 Tab 尺寸 **M → S**（340~600px 档），并补 🔴 专段说明 S = tab.md small 规格（竖向 56×56、14px/22px、16px 居中短线指示器圆角 9px），禁止该档继续用 M
   - 新增 🔴 专段「hover / press 一律不改字重」，与 tab.md Part C、navigation.md 踩坑 #17 三处口径统一
   - §五 新增「卡片文本行数上限」硬约束条目
   - §六 CSS 骨架补齐三段可直接抄的骨架：**Tab 尺寸骨架**（L/M/M/M/S 三档媒体查询）、**Tab 交互态骨架**（hover/press 只改色 + Press 特异性写法）、**卡片文本约束骨架**（两条 line-clamp）
   - §四 实现要点 #5 扩写为「Tab 交互态两条硬规则」（不改字重 + Press 特异性）
   - 自检清单由 15 项增至 18 项：Tab 尺寸 L/M/M/M/S、Tab 含 Part C 全量交互态、**hover/press 未改字重**、Press 特异性 ≥ Hover、**卡片标题 clamp 2 / 描述 clamp 3**
4. **`global/hard-constraints.md` §6 #29 同步扩写**：并入「Tab 尺寸 L/M/M/M/S」与「卡片标题 line-clamp: 2、描述 line-clamp: 3」
5. **验收页 `opendesign-floor-card-check.html` 同步修正**：≤600px 补 Tab S 规格（竖向 56×56、14px/22px、16px 居中短线圆角 9px 指示器，指示器由绝对定位改为文档流内随文字排布）；hover/press 规则删掉 `font-weight`；卡片标题/描述加 line-clamp 2/3；规格速查表「Tab 尺寸」行改为 L/M/M/M/S；Phone 档位说明补「Tab S」

#### 说明

- 本条为**根因修复 + 约束补齐**：第 ② 条改的是组件规范源头（tab.md Part C），不是只改楼层——否则下次引用 tab.md 还会再犯；第 ①③ 条把原本缺失的口径（S 尺寸的具体几何、卡片文本行数）从"没写"变成"写明并给骨架"
- 已用无头 Chrome 全量实测复核（五档 × 间距 7 项 + Tab 尺寸 + Tab 四态 + 卡片截断）：五档间距仍全部符合规格表；340~600px Tab 高 56 / 宽 56 / 14px-22px / column / 指示器 16px 圆角 9px；hover 非选中 `400|primary2`（字重与 Default 一致）、hover 选中保持 `600|primary1`、press `400|primary3` + 同色下划线、Disabled `400|info4`；卡片标题/描述注入 200/400 字超长文案后实测截断到 2 行 / 3 行且 scrollH > clientH（省略号出现）
- 兼容性备注：新版 Chrome 的 `getComputedStyle().display` 会把 `-webkit-box` 归一化报为 `flow-root`，但 line-clamp 功能正常（已用超长文案实测验证），不影响四件套写法的正确性

### 通用楼层卡片模块验收修正：340~600px 一排两卡 + Tab 交互态补齐

> 用户验收 `opendesign-floor-card-check.html` 时指出两处问题：① 340~600px 档本应「一排两卡，超过两个依次往右加并横滑显示」，实际渲染成一排一卡（第二张被裁）；② Tab 只有 `.is-active` 选中态，Hover / Press / Disabled 全缺，鼠标放上去毫无反馈。

#### 根因（均为规范写漏，不是实现手误）

1. **340~600px 卡宽无规格**：模板 §六 CSS 骨架只写了 `overflow-x: auto`，**从未给出卡片宽度**。验收页只能自行取值 216px——而 ≤600px 内容容器固定 312px，216px 的卡放两张就 444px，「一排两卡」在物理上不可能，第二张必被裁掉。真正口径在 `floors/card-grid.md` MB 端两列已有先例：`(312 − 12) / 2 = 150px`，floor-card.md 未引用。
2. **「仅选尺寸变体」被读成「只画选中态」**：模板「涉及组件」表只写 `size = L/M/M/M/M`，子组件引用规则通篇强调「不写内部样式」，AI 落地时把 tab.md Part C 的交互态矩阵（Default / Hover / Press / Disabled）一并省掉了，只保留 Actived 视觉。tab.md 侧规格完好，是楼层模板的引用指令不完整。

#### 修正内容

1. **`floors/floor-card.md` §五 新增卡宽口径**：340~600px 卡片宽度 = **150px**（= 内容容器 312px − 卡片水平间距 12px，对半分；与 card-grid.md MB 两列一致），并写明「禁止写成 216px 等任意值——超过 156px 就挤成一排一卡」；§四 间距表「卡片列数」行该档改为「横排，**一排两卡**，超出横向滑动」；§六 CSS 骨架该档补 `.floor-card__cards > .ocard { flex: 0 0 auto; width: 150px; }`；楼层架构图同步
2. **`floors/floor-card.md` 子组件引用规则新增硬约束**：「仅选尺寸变体」≠ 只画选中态——子组件规范的**交互态矩阵**属于组件规格一部分，必须一并落地；引用 tab.md 必须含 Part C 全量状态，button.md / card.md 同理（按下态、禁用态不得省略）
3. **`floors/floor-card.md` §四 实现要点新增 #5（特异性陷阱）**：Tab 的 Press 写作 `.otab__item:active`（0,2,0）会被 `.otab__item:hover:not(.is-active)`（0,3,0）盖掉——按下时鼠标必同时处于 hover，表现为「按下毫无变化」。正确写法 `.otab__item:active:not(.is-active), .otab__item.is-active:active`（0,3,0 且位于 Hover 之后）
4. **自检清单扩容 2 项**：340~600px 是否一排两卡（150px、前两卡不裁切、第 3 张起横滑）；Tab 是否含 Part C 全量交互态而非只有 `.is-active`
5. **`global/hard-constraints.md` §6 #29 同步扩写**：该楼层专属检查项并入「一排两卡 150px」与「Tab 全量交互态」两条，并加「禁止把卡宽写成 216px 等任意值」
6. **验收页 `opendesign-floor-card-check.html` 同步修正**：卡宽 150px；补齐 Tab 四态 CSS；演示 HTML 增补一个 `disabled` 页签（否则禁用态无处可见）；iframe 内补 Tab 点击切换 `is-active`（同一时刻仅一个，禁用项不响应）

#### 说明

- 本条为**生成侧约束补充**：两条规则的数值真源分别来自 card-grid.md（MB 两列 150px）与 tab.md Part C（交互态矩阵），本次只是把 floor-card.md 漏引的口径补齐，未新造数值
- 已用无头 Chrome 实测复核：五档 35 项间距全部符合规格表；340~600px 卡宽 150px、前两卡完整可见、scrollW 474 > clientW 312（第 3 张起需横滑）；Tab 四态计算样式逐一比对通过（Hover 非选中 600/primary2、Hover 选中不变、Press 600/primary3 + 同色下划线、Disabled 400/info4 无下划线），点击切换与禁用项不响应均验证通过
- 与同日上方「新增楼层模板」条目同属该楼层的规格沉淀，但根因不同：那条是**新增**，本条是**验收暴露的规格写漏**

### 新增「通用楼层卡片模块」楼层模板（五档断点固定规格）

> 用户交付页面通用楼层卡片模块的适配规则（含五档断点示意图）：楼层标题（必选）+ 楼层描述（可选）+ 二级标题（可选）+ Tab 页签（可选）+ 卡片布局（必选，仅示例）+ 操作·查看更多（可选）。用户明示「这个间距值是固定值，未来在所有页面生成中都是约束条件」——故间距不走 `--o-r-gap-*` Token 映射，按五档固定值 + 媒体查询落档，并在 hard-constraints 登记为例外条款（沿用 Banner 专属先例）。

#### 新增内容

1. **`floors/floor-card.md`（新模板，自包含生成单元）**：
   - **楼层架构**：楼层标题（必选）→ 楼层描述（可选）→ 二级标题（可选）→ Tab 页签（可选）→ 卡片布局（必选，仅示例）→ 查看更多（可选）；对齐方式依适配规则示意图（标题组与 Tab 居中、查看更多居中）
   - **五档断点**：≥1681px（1920 为例）/ 1201~1680px（1440）/ 841~1200px（1080）/ 601~840px（740）/ 340~600px（360），各档附媒体查询写法
   - **字号表（与上游响应式 Token 互为校验）**：楼层标题 = display3（40/56 · 32/44 · 24/32 · 18/26 · 18/26，semibold / info1）；楼层描述 = text1（16/24 · 14/22 ×4，regular / info2）；二级标题 = h2（24/32 · 20/28 · 18/26 · 16/24 · 16/24，semibold / info1）
   - **间距表（固定值 · 约束条件）**：楼层上下间距 72/56/40/32/32；标题→描述 16/12/8/**16/16**；描述→二级标题 40/24/16/12/12；二级标题→Tab 24/16/12/12/12；Tab→卡片 24/16/12/12/12；卡片→查看更多 32/24/16/12/12；查看更多→模块底部 72/56/40/32/32；卡片水平间距 32/24/16/16/12；卡片垂直间距无；卡片列数全档横排、≤600px 超出横向滑动
   - **子组件引用硬规则**：Tab / 卡片 / Button 不写内部样式，统一引用 tab.md / card.md / button.md，仅选尺寸变体（Tab = L/M/M/M/M；Button = text 变体 L/M/M/S/S）
   - **缺省元素合并规则**：可选元素缺省时前后间距合并、取相邻两行较大值——推论「标题组到卡片容器恒为 40/24/16/12/12」，与硬约束间距 7 自然对齐
   - **防叠加规则**：「楼层与楼层上下之间间距」与「查看更多 → 模块底部外边距」为同一约束的两种表述，每楼层只保留一处 margin/gap，相邻楼层总间距恒为该档值，禁止叠成 2 倍
   - 附五档间距 CSS 骨架（基准档 + 四条媒体查询）与 15 项自检清单
2. **`global/hard-constraints.md` 登记例外条款**：§1 断点定义新增五档断点例外说明（PC 类别内部分三档）；§3 间距例外区在 Banner 先例后新增「通用楼层卡片模块专属」——五档固定间距表为固定值约束，优先级高于 PC/MB 白名单与 `--o-r-gap-*` 默认映射（含 56px 等两列未覆盖档位）；§6 自检清单新增 #29（该楼层必须逐档等于模板固定表，含移动两档 16px 的有意覆盖，禁止"修正"为 8px）
3. **`SKILL.md`**：楼层模板索引新增「通用楼层卡片模块」行；frontmatter `last_update` 更新为 `2026-10-20`
4. **`README.md`**：楼层模板计数 7 → 8，按需加载清单同步补名

#### ⚠️ 与上游 Token 的差异声明（有意覆盖，已写入模板防复现）

- **「楼层标题 → 楼层描述」在 601~840px / 340~600px 两档为 16px**，上游 `--o-r-gap-4` 在 ≤840px 为 8px——设计裁决移动端标题与描述需更大呼吸空间，模板已标注「禁止修正为 8px」，自检清单 #29 同步拦截
- 除上述一项外，间距表其余各行与上游 gap Token 逐档一致（gap-10 = 72/56/40/32/32、gap-7 = 40/24/16/12/12、gap-6 = 32/24/16/12/12、gap-5 = 24/16/12/12/12；卡片水平间距 = 栅格水槽 32/24/16/16/12）
- 卡片水平间距固定按模板表执行，**不随 `--o-r-grid-column-gutter` 漂移**（用户断点分档与上游栅格六档不完全重合，如 1500px 视口落在用户 Laptop 档取 24，而栅格桌面档为 32）

#### 说明

- 本条为**新增楼层模板 + 约束登记**，不改动既有组件规范与其余楼层模板；Tab / 卡片 / Button 内部样式仍以各自组件规范为唯一真源
- 中间三档（1201~1680 / 841~1200 / 601~840）的内容容器宽度 = 视口宽 − 2×留白，留白取 `--o-r-grid-padding`；≥1681px 与 ≤600px 两锚点档仍按硬约束 §4.1（1488+216 / 312+24）落实

---

## 2026-10-19

### 导航下拉面板「二选一、禁止混用」硬规则（用户纠错后流程沉淀）

> 用户连续两次指出流程问题：生成导航时在同一顶导内同时出现了分类平铺式与平铺式两种下拉面板。深挖根因不是「忘了问」，而是规范自身在诱导混用——`navigation-dropdown.md` 原写「混挂互斥：同一顶导内两种样式可混挂（如「开发」挂分类平铺式、「下载」挂平铺式）」，等于官方许可；且 SKILL.md 虽已有「询问用户」门禁，却未写明「选定后全套统一、禁止按菜单项逐个分配」，AI 仍可自行拆分。同时暴露第二个根因：**样式判断失误**——拿纯菜单名列表去套多列 mega 面板（分类平铺式的条目必须有说明文案、需两个以上分类才有意义）。

#### 更新内容

1. **`SKILL.md` 导航楼层门禁（逐楼层循环节）新增两条硬规则**：
   - 🔴 **两种下拉面板样式是二选一，禁止混用**：同一顶导下所有下拉面板必须统一使用用户选定的样式；禁止按菜单项分别挂不同样式。并写明原因（两种样式的宽度 / 边距 / 定位 / 圆角 / 条目交互规则完全不同——平铺式 = 居中触发器 auto 宽 + 4px 间距 + hover 色块；分类平铺式 = 通栏 216px + 零间距抽屉 + hover 只改文字色），混用 = 同一页面两套几何逻辑 + 两种 hover 反馈。同时明确「首页 / WIKI 等纯链接项不属于混用」，防止规则被过度解读
   - ⚠️ **样式判断要看整套导航的信息结构，不是逐个菜单项分配**：有条目说明文案 + 两个以上分类 → 分类平铺式；纯菜单名列表、无描述、无分组 → 平铺式。同一导航通常只适配其中一种，**不存在「这个菜单适合 A、那个菜单适合 B」的混搭结论**；拿不准则把各菜单内容结构列给用户，禁止自行分配
2. **`SKILL.md` 第八步自检清单新增 1 项**：【下拉面板样式统一】`.o-nav__dropdown` 与 `.o-nav__dropdown--categorized` 不得在同一顶导内混用——有下拉面板的页面必查，使该规则可机械校验
3. **`floors/navigation-dropdown.md` §〇 删除误导源头**：原「混挂互斥」一行（写有「同一顶导内两种样式可混挂」的示例）已替换为「🔴 样式二选一（硬规则）」+「混挂的技术后果」两行——后者说明技术上仍能跑（共享基类使互斥自动覆盖两者）但**「技术上能跑」不等于「允许」**
4. **`floors/navigation-dropdown-categorized.md` 开头改写「两种下拉面板样式分工」**：原表述「同一菜单结构，不同菜单项可挂不同样式，禁止混用 class」自相矛盾且诱导混用，改为「🔴 两种下拉面板样式是二选一，不是混搭」，并补入与 SKILL.md 一致的判断标准
5. **同文件 §一定位规则收尾处改写**：原「⚠️ 混用两种样式时互斥自动生效…同时覆盖两者，无需另写规则」读起来像在鼓励混用，拆为「✅ 基类与修饰类必须同时挂载」（保留有效技术信息）+「🔴（技术说明，不是许可）」——明确互斥自动生效只是「不会同时弹出两个面板」的兜底，不构成允许混用的理由

#### 说明

- 本条为**生成侧流程约束**，不改动两种下拉面板的任何几何规格、状态矩阵与响应式规则
- 与本条下方同日并入的三条（MB 箭头 PC 档裸露、通栏 `max-width` 覆盖、踩坑 #20 盒模型）同属导航楼层流程沉淀，但**根因不同**：那三条是「规格写漏」，本条是「规范自身在诱导违规」+「样式判断标准缺失」
- `SKILL.md` frontmatter `last_update` 同步更新为 `2026-10-19`

### 分类平铺式 MB 切换箭头 PC 档裸露修正

> 用户验收 B 分类平铺式截图时发现 PC 档「项目」列末尾突然冒出一个 `^` 箭头。根因：`.o-nav__cat-toggle`（MB 左列折叠/展开开关）被 HTML 骨架无条件渲染，但模板 CSS 只写了图标状态切换，漏写 PC 档隐藏规则。

#### 修正内容

1. **`floors/navigation-dropdown-categorized.md` §7.3 基础样式补 `display: none`**：`.o-nav__cat-toggle` 默认隐藏，仅在 `@media (max-width: 840px) and (min-width: 601px)` 里恢复 `display: block`
2. **§八规则表 #4 补充显隐约束 + 新增通用警告「MB 专属元素在 PC 档必须显式隐藏」**：HTML 骨架无条件渲染全量节点，靠 CSS 分档显隐——只写「MB 显示」不写「PC 隐藏」，PC 档就会多出不该有的元素
3. **自检清单新增 1 项**：`.o-nav__cat-toggle` 是否默认 `display: none`、仅在 ≤840px 档恢复显示
4. 同步修正验收页 `opendesign-navigation-categorized-check.html`，PC 档箭头消失、MB 档功能不受影响

#### 说明

- 本条为**生成侧约束补充**，不改动 MB 双列布局与折叠态规格本身
- 与同日「max-width 覆盖」「踩坑 #20 盒模型」并列，共同构成本次流程沉淀

### 分类平铺式通栏定位补 `max-width` 覆盖规则

> 生成 B 分类平铺式下拉面板并用 HTML 验收时，1920/1680 档快捷链接栏不靠右（距视口右缘不是 216px，悬在页面中间）。根因：基类 `.o-nav__dropdown` 的 `max-width: 1488px` 未被修饰类覆盖，通栏 `left: 0; right: 0` 实际被限宽。

#### 修正内容

1. **`floors/navigation-dropdown-categorized.md` §一定位代码块补 `max-width: none`**：复合选择器 `.o-nav__dropdown.o-nav__dropdown--categorized` 中显式覆盖基类 `max-width: 1488px`，让通栏真正撑满页面
2. **新增警告「通栏必须显式覆盖基类 `max-width`」**：归纳通用规则——凡基类有而修饰类不适应的约束（`max-width` / `transform` / `top` / `border-radius`）都必须逐条显式覆盖，不能只写自己新增的属性
3. 同步修正本次生成的验收页，六档（1920/1680/1440/1200/840/600）像素级验证通过：快捷链接栏宽 342px、距视口右缘精确 216px、三处内容左缘对齐

#### 说明

- 本条为**生成侧约束补充**，不改动分类平铺式的几何规格本身（通栏定位、216px 栅格边距均不变）
- 与同日「踩坑 #20 盒模型冲突」共同构成本次流程沉淀：下次生成前对照 navigation.md 自检清单 + 本模板的覆盖规则，可一步到位

### 导航顶导盒模型冲突修正 · 踩坑 #20

> 按导航流程生成全套并用 HTML 验收时，顶导整体歪斜（Logo 偏低、菜单文字偏高、选中下划线悬在导航栏中间）。定位为盒模型冲突，非对齐属性写错。

#### 修正内容

1. **`floors/navigation.md` 踩坑记录新增 #20**：`.o-nav__logo` 写 `height: 32px` 时，若页面存在全局 `* { box-sizing: border-box }`，32px 会被当作**含 padding 的总高**（本意是内容高），Logo 实际仅 32px、`.o-nav__left` 只剩 48px → 菜单 `align-self: flex-end` 贴到 y=60 而非导航底边 y=72（指示线落在导航栏正中）、Logo 在 48px 内居中偏至 y=22~54
2. **明确正确做法**：Logo **禁止写显式 height**，由 SVG 自身 32px + `padding: 20px 0` 撑出 **72px 总高**（与导航等高）；仓库已验证示例 `.nav-dropdown-check.html` 正是此写法（不写 height），故 border-box 下依然正确
3. **归纳通用规则**：凡依赖「height + padding 相加得总高」的节点（Logo 32+20+20、菜单项 48+24）都会在 border-box 下被压扁；生成时先查页面是否有全局 `box-sizing`，有则该类节点一律不写显式 height
4. **自检清单同步新增 2 项**：Logo 是否未写 height；生成前是否确认全局 box-sizing 并调整该类节点

#### 说明

- 已同步修正本次生成的验收页（删除 `height: 32px`），三层对齐恢复准确
- 本条为**生成侧约束**，不改动导航几何规格本身（Logo 仍为 136×32、padding 20px 0、总高 72px）

---

## 2026-10-15

### 源码下拉按钮 B.2 与下拉面板选项状态规则统一（续）

> 用户裁决：「B.2 ODropdown text + small 变体（源码下拉按钮）这部分应该是要与面板选项状态保持统一」。B.2 原为通用 ODropdown 规范，其面板菜单项规则与 floors 已裁决的顶导下拉面板规则正面冲突，且两边都叫「面板内菜单项状态矩阵」、无边界声明，AI 极易混用。

#### ⚠️ 破坏性（需复核的生成物）

1. **`components/navigation.md` B.2 的 Selected 持久选中态已作废**。此前按 B.2 生成的源码下拉面板若带 `.selected`（`rgba(0,47,167,0.08)` 底 + Medium 字重、持续显示到面板关闭），需改为按下瞬间态（`:active`，松开恢复）

#### 统一内容（B.2 五处）

1. **节首加适用边界声明**：B.2 只描述顶导右侧「源码」按钮（`.o-nav__source`）及其面板；**顶导菜单项**（`.o-nav__item` / `.o-nav__link`）挂载的下拉面板以 `floors/navigation-dropdown.md` §4.2 为唯一真源，禁止把 B.2 的 primary1 文字 hover / Semibold / Selected 搬到顶导菜单项面板上
2. **触发按钮 Hover 色 `primary1 #002FA7` → `primary2 #5177CA`**（对齐 dropdown.md text 变体与 floors 导航楼层；`navigation-dropdown.md` §五 早已明确禁抄 `#0047D9`，本次消除 B.2 的 primary1 误导）
3. **面板内菜单项 Hover 文字色 `primary1` → 保持 `info1`**（只换背景 `#F0F5FF`）
4. **按下态去字重与微缩**：Semibold(600) + `scale(0.98)` → **Regular(400) 无微缩**（保留背景 `#E6EDFF` + 文字 `primary1`，仅按住瞬间）
5. **删除 Selected 行与 `.o-dropdown__item.selected` CSS**：无持久选中态（选项点击即跳走/关闭面板）；「常见错误清单」同步换血——删「缺少 Selected 状态」，新增 4 条禁令（hover 文字不得变色 / 不得加持久选中态 / 不得切字重或加微缩 / 触发按钮 hover 必须 primary2）

#### 保持不变

- `B.1.1 搜索建议面板`（OSearch 专用，`#E8F0FE` hover 背景、40px 项高、8px 项间距、关键词 Semibold 高亮）与下拉面板选项规则不同域，已带差异标注，本次不动
- `Part E 用户名区`菜单项 hover 背景 `#EBF1FA` 与按下/选中态 Semibold 仍为原规范（与面板选项规则尚未统一，待后续确认）

### 导航架构重构：顶导/触发器样式唯一源 = floors/navigation.md（续）

> 用户裁决：「两个 dropdown 模板不应该涵盖顶部导航的样式，因为顶部导航的样式和触发样式都应该在 navigation.md 里就已经写完整了——无论下面的 dropdown 是什么类型，顶部导航的样式一定是一致的，仅仅只是下面下拉面板的区别而已。」

#### ⚠️ 破坏性（需复核的生成物）

1. **两份下拉模板不再包含任何顶导/触发器样式定义**。按旧版模板生成的全套导航，若顶导部分是照下拉模板复制的，需按 `floors/navigation.md`「菜单项（触发器）完整样式」复核（旧模板里的触发器规则已被删除，不再维护）

#### 架构调整（三份楼层模板的职责重划）

1. **`floors/navigation.md` 补全为顶导/触发器唯一样式源**：新增「菜单项（触发器）完整样式」整段可交付 CSS——顶导容器（含 `.o-nav::after` 阴影补偿层）→ 左侧区域（Logo/菜单 Flexbox 三层对齐）→ 菜单项五态（默认/hover/展开/选中/禁用，含选中优先于 hover 的优先级选择器）→ 右侧操作区（搜索框/源码下拉/图标）；原独立拆分的搜索框/源码下拉实现整段并入。全套导航统一 `.o-nav__*` 命名，**删除 class 映射表**（无需映射）
2. **`floors/navigation-dropdown.md`（平铺式）只定义下拉面板**：删除 §4.1 触发器状态矩阵、颜色表中的触发器行、CSS 骨架中全部触发器规则；CSS 从 `.o-nav__dropdown` 起；文件头加 🔴 架构约束块（顶导与触发器「一处都不许写」，唯一定义源 navigation.md，复制时禁止改写删减）
3. **`floors/navigation-dropdown-categorized.md`（分类平铺式）同样只定义面板**：删除 §4.1 状态矩阵与两段触发器 CSS（hover/选中态），改为只列与面板相关的两条行为规则；删除 class 映射表，改为统一命名说明
4. **踩坑/自检去重防幻觉**：触发器相关踩坑统一收入 navigation.md（#9 指示线形态/漏写、#10 `is-current` 迁移、#17 字重与 `:has()` 收窄、#18 背景色块、#19 hover 抢选中态）；两份下拉模板删除对应重复行并**整表重编号**（平铺式 26→23 条、分类平铺式 47→44 条），表头加「触发器踩坑见 navigation.md」指引；自检清单的触发器逐项检查改为一条「顶导与触发器样式是否整段取自 navigation.md」

#### 规则裁决落定（同步全链路）

5. **触发器 hover 色统一为 `primary2`**（#5177CA，上游真源）：此前两份模板记不同值，现由 navigation.md 骨架统一定义，两份下拉模板均不涉及
6. **面板选项无持久选中态**：选项是点击即跳走的链接，不存在「当前停在这一项」的状态；平铺式的 `control3-light` 色块仅是 `:active` 按下瞬间的**点击态**（松开即恢复），禁止 `is-selected` 类名、禁止 `primary1` 选项文字色；`is-current` 只属于顶导触发器（当前页 + 2px 指示线）
7. **CSS 变量完整性校验固化为流程**：改色值后必须跑「已定义 vs 已引用」差集检查（comm 命令，见两份模板「CSS 变量定义/引用比对」节）——漏定义时 `var()` 整条声明**静默失效**（不是回落默认值），表现为「hover 无变化」「图标消失」

#### 保持不变

8. `SKILL.md` 导航专属门禁三步（先生成顶导骨架 → 停下询问用户面板样式 → 确认后一并生成全套）已绑定，本次架构调整后流程不变；`components/navigation.md` Part H 继续指向楼层模板为 HTML 实现唯一真源

### 分类平铺式第四~九轮验收修正 + 防复现约束

#### 修正（`floors/navigation-dropdown-categorized.md`，用户逐轮验收确认）

1. **第四~九轮规格全部落定并同步模板 ↔ 验收页**：① 分类列头 / 条目标题按 **>1440px 16px / ≤1440px 14px** 分档、颜色统一 `info4` / `info1`，选项说明两档均 12px / `info2`；② **条目标题全状态统一 Semibold**（默认态即 600，hover / 按下 / 已选只改 color，仅禁用态回落 Regular）；③ 外链图标 **24×24**（覆盖硬约束 §5 默认档）、外链图标↔文本与红标签↔文本间距统一 **8px**；④ 红标签严格 OTag `fill + danger + small`；⑤「更多项目」改 **OButton `text` + `large`**（`<button>`，右箭头 24×24 在右侧）；⑥ 快捷链接纯文本项 = **灰底按钮块（342px / 40px / fill3 / link 距左 24px）+ 内层 OLink medium**（34px 高 / 上下 padding 5px / 左右 0 / 14px-22px / link1→link2→link3）**两层缺一不可**；⑦ 栏内 **`align-items: flex-start` 左对齐** + 整栏 **`margin-left: auto` 靠右**（贴栅格右缘，PC 距右 216px）；⑧ 图+文本卡片**无背后色块**、图文垂直居中、图 **121×68**、标题 16px/24px、描述 12px/18px **info3**、hover / 按下态与左侧分类条目一致；⑨ **条目说明 `min-height: 36px` 预留两行高度**（2 × 18px 行高，文案 1 行时也占两行空间，后续延展不挤压布局）；⑩ link 色 token 来源核实为 `opendesign-tokens/references/tokens-openeuler.md` 品牌色·链接色表（link1 #002FA7 / link2 #5177CA / link3 #002793），并修复验收页 `:root` 漏定义导致 `var()` 失效文字发黑的问题
2. **踩坑记录扩充至 41 条**：新增 #39 验收页与模板双文件同步（默认渲染可选模块 / 双向 diff / 辅助元素不得遮挡验证区）、#40 同类元素状态集合一致性（卡片漏 hover 态）、#41 描述文本预留两行高度
3. **`floors/navigation.md` 三处文档缺陷修正**（用户质询「顶导是否严格按 navigation.md 生成」后逐条核对发现）：① 源码下拉 CSS 片段 `line-height: 24px` → **22px**（违反 hard-constraints §2 的 14px↔22px 成对锁定，且与本文档速查表的 14px/22px 自相矛盾）；② 自检清单「源码按钮颜色 `info2`」→ **`info1`**（与本文档「颜色」节及 dropdown.md Part B「text 变体文字与图标均 info1」一致）；③ 资源表补「源码下拉箭头 = `public icons/icon-下箭头.svg`（保留 transform）」一行（原表缺此项，生成方无据可依只能手写 SVG）
4. **`.nav-dropdown-categorized-check.html` 顶导补齐 `align-self: flex-end`**：`.o-nav__item` 原只写 `position: relative`，漏了 navigation.md「菜单项完整样式」要求的 `align-self: flex-end`（贴底对齐）
5. **两份模板互标「顶导骨架共用」复用关系**（用户质询「分类平铺式顶导为何不直接复用平铺式」后确认）：两份下拉面板模板本就不定义顶导骨架——唯一定义源是 `floors/navigation.md`「菜单项（触发器）完整样式」，两份共用同一份；两份模板 §九/§八 CSS 骨架头部均已加 🔴 注释明示「顶导骨架本模板不定义、生成 HTML 时整段复制且禁止改写删减（含 `align-self: flex-end`、`line-height` 等易漏项）」。同时修正复制产生的两处偏差：`.nav-dropdown-check.html`（平铺式）body 字体原为 `'HarmonyOS Sans SC', 'HarmonyHeiTi', 'PingFang SC', sans-serif`（**违反 hard-constraints §2 铁律**）已改为 `'HarmonyOS Sans', 'HarmonyHeiTi'`，`.o-nav__item` 同样补齐 `align-self: flex-end`

#### 更新（防复现：多轮返工的流程根因是「规则未执行」而非「规则缺失」，补 4 个拦截点）

1. **`SKILL.md` 第四步**：读楼层模板从「读取获取规格」升级为**整本读完再落笔**——「生成前必读 / 已确认清单 / 冲突声明 / 踩坑记录」四节缺一不可；用户后续要求与模板旧值冲突时**先更新模板再生成**，与 hard-constraints 冲突时**停下报告**禁止自行折中
2. **`SKILL.md` 新增「验收页（check page）生成约定」**：① 验收页**默认渲染全部可选模块**（模板本体仍默认注释，两处刻意不同）；② 图例 / 标注等辅助元素禁止 `fixed` 遮挡验证区（默认 `<details>` 收起或移走）；③ 验收页 CSS 与模板逐条一致，唯一允许差异 = 可选模块默认渲染状态，每轮修正后必须双向 diff
3. **`global/hard-constraints.md` §6 自检清单扩充至 26 条**：新增 #22 `:root` Token 完整性（漏定义 → `var()` 静默失效 → 文字发黑 / 图标消失）、#23 图标必须 assets Read 后逐属性内联（禁止手写 SVG / `<img>` / 字符代替）、#24 OTag / OButton / OLink 必须按组件规范落笔、#25 同类元素状态集合一致、#26 描述文本预留两行高度

### 分类平铺式第十二轮 · 顶导阴影被面板遮挡 · z-index 层级修正（续）

#### 修正（`floors/navigation.md`）——阴影规范原本就有，缺的是层级规则

1. **确认顶导阴影规范已存在**：`box-shadow: var(--o-shadow-1)`（`0 3px 9px 0 rgba(0,18,85,0.078)`，一级阴影·浮层）在本文档「颜色」表、`--o-shadow-1` Token 定义（`:root`）、结构树中均已定义，来源 components/navigation.md。**不需要新增阴影值，缺的是让阴影可见的层叠规则**
2. **新增「顶导 z-index 必须 > 下拉面板」规则**（用户验收发现「顶导没有阴影」后确认根因）：顶导 `.o-nav` 取 **`z-index: 101`**，下拉面板为 100。根因 = 面板 DOM 位于顶导**内部**（后绘制），两者 z-index 相同时**面板整条盖住顶导的下阴影**，表现为交界处"平贴"、无悬浮感。顶导元素本身只占 0~72px，提到 101 不遮挡面板内容（72px 以下），只是让下阴影（72~81px）叠在面板顶部
3. **新增「顶导容器 CSS」完整片段**（`.header` 的 position / top / z-index / background / box-shadow），并标注 ⛔ 禁止 `overflow: hidden`（裁剪发生在层叠之前，z-index 再高也救不回被裁的面板）
4. **速查表新增两行**：顶导阴影值、顶导 z-index 101
5. **自检清单新增 3 条**：阴影是否设置、z-index 是否 101、顶导容器是否无 overflow:hidden
6. **踩坑记录新增 #15 / #16**：#15 = 阴影被面板盖住（遮挡 vs 样式缺失的排查区分）、#16 = overflow:hidden 裁掉面板

#### 修正（`floors/navigation-dropdown-categorized.md`）

7. **差异表新增「面板 z-index 100 且必须低于顶导 101」**；自检清单新增对应检查项；踩坑表新增 #44（阴影被盖）/ #45（overflow:hidden 裁面板），均回指 navigation.md #15/#16

#### 修正（`.nav-dropdown-categorized-check.html` / `.nav-dropdown-check.html`）

8. **两份验收页 `.o-nav` 的 `z-index: 100` → `101`**（阴影原本就写了，是被面板挡住看不见）；分类平铺式验收页新增「顶导阴影 / 层叠层级验证」卡片，图例新增「层叠」行

### 分类平铺式第十一轮 · 面板与顶导零间距 · 抽屉式展开（续）

#### 修正（`floors/navigation-dropdown-categorized.md`）

1. **面板与顶导间距 4px → 0px**（用户确认「没有间距，就像抽屉一样展开」）：`.o-nav__dropdown--categorized { top: 100% }`，紧贴顶导底边展开——⚠️ 覆盖继承平铺式基类的 `top: calc(100% + 4px)`。同步修改结构树、§一 继承规则表 + 差异表、§三 速查表、§九 CSS 骨架、§十一 自检清单（新增 3 条）、§十 踩坑（新增 #43）、已确认清单（新增 #22）；§「生成前必读」第 3 项补注本模板 top 与热区桥接的覆盖关系
2. **上圆角压平为 `border-radius: 0 0 4px 4px`**（零间距的必然推论，已向用户标注）：面板零间距紧贴顶导后，若保留基类的 4px 全圆角，面板左上 / 右上会各露出一道月牙形缺口，视觉上面板与顶导又"裂开"了。下圆角保留（面板底边是自由边）
3. **基类 `::before` 4px 热区桥接显式 `display: none`**：桥接本是补鼠标穿过面板与顶导之间间隙的路径，零间距后无间隙可穿，只剩白占顶导底部 4px 的副作用
4. **新增两条防错警告**：① 不要把隐藏态 `translateY(-8px)` 当成间距去改 `top`——那只是入场位移，展开终态是 `translateY(0)`，改了反而会在展开态留下真实间隙；② 零间距必须与压平上圆角同时做，只做一半视觉上仍然裂开

#### 修正（`.nav-dropdown-categorized-check.html`）

5. **同步 `top: 100%` + `border-radius: 0 0 4px 4px` + `::before` 关闭**；原「热区桥接 / 浮层遮挡验证」卡片改写为「抽屉式贴合 / 浮层遮挡验证」，图例「几何」行补零间距说明

### 分类平铺式第十轮 · 条目无选中态 + 顶导骨架复用（续）

#### 修正（`floors/navigation-dropdown-categorized.md`）

1. **面板条目删除「选中态」**（用户确认：导航是直接点击跳走的，没有选中态）：条目状态集合由五种收敛为**四种**（默认 / hover / 按下 / 禁用），删除 `.is-selected` 类与 `primary1` 条目文字色。同步清理 §2.2 元素构成表、§三 速查表、§4.3 状态矩阵、§五 颜色表、§九 HTML 骨架（`代码扫描` 去掉 `is-selected`）与 CSS（删 `.o-nav__cat-item.is-selected` 规则）、§十 踩坑（#6 三色分级改两色分级、新增 #42）、§十一 自检清单（原「条目已选是否为 primary1」改为「条目是否无选中态」反向检查项）；三色分级说明改为两色分级
2. **明确区分「条目无选中态」与「触发器 is-current」**：新增双向警示——条目禁止 `primary1` / `.is-selected`；触发器保留 `is-current`（当前页 + 2px 指示线 + primary1）。二者语义不同，禁止混用
3. **顶导骨架复用关系写进两份下拉模板**（用户质询「分类平铺式顶导为何不直接复用平铺式」后确认）：两份模板本就不定义顶导骨架，唯一定义源是 `floors/navigation.md`「菜单项（触发器）完整样式」；两份模板 CSS 骨架头部均加 🔴 注释——生成 HTML 时顶导段必须整段复制且禁止改写 / 删减（点名 `align-self: flex-end`、`line-height` 等易漏项）

#### 修正（`floors/navigation.md`）

4. **源码下拉 CSS 片段 `line-height: 24px` → `22px`**：原值违反 hard-constraints §2 的「14px 文字 ↔ 22px 行高」成对锁定，且与同文件速查表的 14px/22px 自相矛盾
5. **自检清单「源码按钮颜色 info2」→ `info1`**：原值与本文档颜色节、dropdown.md Part B 均冲突，照自检清单做反而会错
6. **资源表补「源码下拉箭头 = `icon-下箭头.svg`」**：原表只有 5 个图标、独缺此项，生成方查不到来源只能手写 SVG（即踩坑记录 #7 的成因）

#### 修正（`.nav-dropdown-categorized-check.html` / `.nav-dropdown-check.html`）

7. **分类平铺式验收页去掉 `代码扫描` 的 `is-selected`** 及对应 CSS 规则，图例「条目已选」色块改为「✕ 条目无选中态」警示，「三色分级验证」卡片改为「两色分级验证（条目无选中态）」
8. **平铺式验收页 body 字体改为 `'HarmonyOS Sans', 'HarmonyHeiTi'`**：原为 `'HarmonyOS Sans SC', 'HarmonyHeiTi', 'PingFang SC', sans-serif`，违反 hard-constraints §2 铁律（禁止 PingFang）
9. **两份验收页 `.o-nav__item` 补齐 `align-self: flex-end`**：复制顶导段时漏掉的 navigation.md 贴底对齐属性

---

## 2026-10-14

### 分类平铺式第二轮四项确认（续）

#### 修正（`floors/navigation-dropdown-categorized.md`）

1. **primary2 色值定案**：设计稿标注的 hover 色 `#5177FA` 与上游 `openeuler-token.json` 的 `--o-color-primary2` = `#5177CA` 不一致，用户确认**以上游语义 Token 为准**。§五「色值消歧」标题由 ⚠️ 改为 ✅，触发器冲突声明与条目三色分级注释中的 `#5177FA` 同步改为 `var(--o-color-primary2)`（#5177CA）。
2. **断点区间笔误修正**：原稿「1680~1201px 内容区距左右 40px」为笔误，正确区间为 **1440~1201px**。修正后 `1680~1441px`（左右 64px）与 `1440~1201px`（左右 40px）**互不重叠**，§六 断点表、实现要点（原第 3 条拆为两条）、CSS 媒体查询（原「响应式 2」拆为两条、后续编号顺延）全部同步；同时明确 1200~841px 档左右内边距沿用 40px（须显式写出，否则回退基准档 24px）。
3. **≤600px 右内容区不按 40% 收窄**：用户确认取 `max-width: none`，CSS 注释由「待确认 #4」改为「用户确认」，§七 MB 规格表与踩坑 #17 同步。
4. **快捷链接栏规格落定**：
   - **纯文本按钮**：内部 link 用 [components/link.md](../components/link.md) 的 **M（medium）尺寸**（文字 14px/22px、link 高 34px），**块内 link 距左 24px、上下居中**；块高 40px 仍为推断值
   - **带图卡片**：改为**左图右文**布局，图片 **16:9 比例**、高度与左侧文本块一致（高 36px / 宽 64px），**图文间距 8px**；替换原「上下结构 + 卡片宽 200px + 图高 96px」的推断实现
   - 涉及 §2.3、§三 速查表、§九 HTML 注释与 CSS、§十 踩坑（原 #24 拆为 #24 字号/比例 + #25 图片变形）、§十一 自检清单同步更新；**生成前必读**新增第 5 项（link.md 的 M 尺寸规格）
5. **待确认项收敛**：原 #1、#3、#4 移入「已确认」清单（新增第 13 条记录四项确认）；#5 收窄为「剩余推断值」（块高 40px、块间距 8px、卡片背景色、栏 min-width 200px、按钮文字色 info1 vs link1）。

### 分类平铺式第二轮输入

#### 新增
- **新增楼层模板 `floors/navigation-dropdown-categorized.md`（导航下拉面板 · 分类平铺式）**：按用户提供的设计稿与规格整理稿完成，与 `floors/navigation.md` + `floors/navigation-dropdown.md` 搭配生成。覆盖：
  - **多列平铺结构**：分类列头（14px/22px Regular / `info3`，PC 端无交互）+ 列内条目（标题 16px/24px `info1` + 说明 12px/18px `info2`）+ 外链图标（16×16 currentColor）+ 红色小标签（OTag fill + danger + small）+ 列底部「更多项目 >」+ 快捷链接栏（`fill3` 灰底按钮块 / 带图活动卡片）；典型列数 开发 Tab 5 列、社区 Tab 4 列 + 右侧快捷链接栏
  - **完整状态矩阵**：条目标题 默认 Regular `info1` → hover Semibold `primary2` → 按下 Semibold `primary3` → 选中 Semibold `primary1` → 禁用 Regular `info4`；说明文字仅禁用态变 `info4`；**全程无背景色块**（与平铺式的 `control2-light` / `control3-light` 色块是两种交互表达，从源头规避 control Token 取值歧义）
  - **六档自适应**（≥1681 / 1680~1441 / 1200~841 / 840~601 / 600~360）+ **MB 双列**（左一级导航列 + 右内容区，`grid: minmax(32px,144px) 1fr`，右列 `grid-row:1` 固定顶格 + `max-width:40%` 黄金分割，折叠态 32px）
  - **截断规则**：标题 `line-clamp: 2`、说明 `line-clamp: 6`（四件套写法）、tooltip 复用 OPopover；导航项过多时左列末项挂切换箭头
  - **HTML/CSS 骨架 + MB 交互 JS**、24 条踩坑记录、57 项楼层自检清单、11 项待确认项 + 12 项已确认清单
- **架构决策：两种下拉面板样式共享 `.o-nav__dropdown` 基类**。平铺式模板继续定义面板定位（`top: calc(100% + 4px)`、居中触发器）、显隐三规则（hover / focus-within / data-open）、4px 热区桥接、单开互斥 `:has()`、MB 左对齐与交互 JS；分类平铺式只追加修饰类 `.o-nav__dropdown--categorized`（复合选择器覆盖布局）。因此**同一顶导内两种样式可混挂**，单开互斥规则同时覆盖两者，无需另写。
- **触发器状态显式冲突隔离**：分类平铺式的顶导菜单项按用户规格为 hover/展开态 = Semibold + `primary2`（无下划线）、选中态 = Semibold + `primary1` + 2px 指示线，与 `components/navigation.md` Part C（Regular + primary1、无字重变化）不一致。已按「用户显式输入优先」写入模板，并用 `:has(> .o-nav__dropdown--categorized)` 限定**只作用于挂分类平铺面板的菜单项**，禁止污染平铺式与普通顶导；Part C 同步更新列入待确认项 #7。

#### 更新
- `floors/navigation-dropdown.md`：§〇 改为「两种样式 / 两份模板」分工表 + 协作方式表（基类共享、修饰类、混挂互斥、漏基类后果）；头部增加样式导航；待确认项 #6 标记为已完成并指向新模板。
- `floors/navigation.md`：头部互指说明改为两种样式二选一（含混挂说明）；「生成前必读」第 2 步改为按样式二选一；class 对应表补 `.o-nav__dropdown`（基类）与 `.o-nav__dropdown--categorized`（修饰类）两行。
- `SKILL.md` 楼层模板索引：`导航下拉面板` 一行拆为「平铺式 / 分类平铺式」两行（楼层模板数 6 → 7）。
- `references/designer-guide.md` 楼层模板速查、`README.md` 目录结构与按需加载策略：同步登记两份下拉面板模板，模板数 6 → 7。

### 平铺式导航下拉面板首版与当天多轮验收修正

#### 新增
- **新增楼层模板 `floors/navigation-dropdown.md`（导航下拉面板 · 平铺式）**：将顶导菜单项展开的下拉面板从组件规范中拆出，封装为独立自包含生成单元，与 `components/navigation.md` + `floors/navigation.md` 搭配使用。覆盖：面板定位与触发规则（hover + click 双途径、单开互斥、4px 间距热区桥接）、平铺式布局（flex-wrap 自动换行、`height: auto`）、双层状态矩阵（顶导菜单项触发器 默认/常态/点击态/选中态 + 面板菜单项 默认/hover/已选）、设计稿实测自适应断点（1518 / 1440 / 1280 / MB）、PC 与 MB 双端规格、HTML/CSS 实现骨架与最小交互 JS、10 条踩坑记录、楼层自检清单。
  - **搭配关系**：`floors/navigation.md` 只生成顶导骨架，下拉面板必须读本模板；已在 navigation 楼层头部增加互指说明，并在 SKILL.md / designer-guide.md / README.md / 根 AGENTS.md / 根 README.md 的楼层模板索引中登记（楼层模板数 5 → 6）。
  - **Token 冲突隔离**：`--o-color-control2-light` / `--o-color-control3-light` 在本仓多文档中各有多种取值，模板内以**取值消歧表**固定本楼层含义（hover 背景 = control2-light #F0F5FF；已选背景 = control3-light #E6EDFF），并显式排除 `#002FA7`（OSearch Hover 描边义）等误用。
  - **待确认项显式列出**：设计稿「默认」与「常态」标注值相同、点击态蓝底圆角未标注、面板内边距与菜单项 gap 是否实测值、MB 端内边距与 gap 为推断值、MB 断点设计稿 655px 与 hard-constraints §1（360~600px）冲突、面板高度实测值与公式推导存在偏差（高度一律 auto，实测值仅作 QA 参考）、**分类平铺式样式待第二轮输入后补充**。以上各点按 SKILL.md「规范未覆盖时的分级处理」禁止静默取值。（其中间距 4px、面板居中触发器、菜单项字号 16px、hover/已选/禁用态配色已于同日经用户口述确认，见下方「修正」节。）

#### 更新
- `floors/navigation.md` 头部新增与 `floors/navigation-dropdown.md` 的搭配说明：顶导骨架由 navigation 楼层生成，菜单项展开的下拉面板（平铺式 / 分类平铺式）必须搭配 navigation-dropdown 模板，触发器状态与面板规格以该模板为准。

#### 修正
- **`floors/navigation-dropdown.md` 面板规格按用户口述校正**（首版按 ODropdown 通用规格推导，与 mega 面板实际规格不符）：
  - 菜单项字号 14px/22px → **16px/24px**（常规正文），MB 对应 12px/18px → **14px/22px**
  - 菜单项 hover 背景 `#EBF1FA` → **`var(--o-color-control2-light)`（#F0F5FF 极浅蓝）**；已选背景 → **`var(--o-color-control3-light)`（#E6EDFF 浅蓝）**，与 hover 明确分级
  - 删除私有变量 `--o-nav-dropdown-item-hover-bg`，改为直接引用语义 Token，并在「五、颜色」新增 **control2/control3 取值消歧表**（标注 `#002FA7` 是 OSearch Hover 描边义、`#E6EDFF` 也是用户名区 Press 义，均非本楼层用法）
  - 面板水平定位 左对齐触发器 → **水平居中于触发器**（`left: 50%` + `translate(-50%, …)`），MB 端回落左对齐；同步补踩坑 #12（transform 覆盖丢 `translateX(-50%)`）与 #13（居中须同时校验左右溢出）
  - 补 **禁用态**规格（文字 `var(--o-color-info4)`）与踩坑 #14
  - 踩坑 #5/#6 重写：原写「hover 背景不是 #F0F5FF」结论错误，实际正是 control2-light；原写 hover 文字变 primary1 亦错误，实际保持 info2

#### 修正（续 · 2026-10-14 用户复核后发现）
- **`floors/navigation-dropdown.md` 触发器状态严重偏离 components/navigation.md Part C（用户指出「导航选中部分怎么有 primary 色块」）**：
  - **删除首版自创的「点击态 = SemiBold + primary1 实心蓝底 + 白字」**——navigation.md 全文中不存在该状态，系误读设计稿所致。正确规格：**展开态 = SemiBold + primary1 文字 + `--o-color-control3-light`（#EBF1FA）浅蓝背景**。
  - **修正选中态**：原写「SemiBold + primary1 蓝字（无背景）」，遗漏了 navigation.md Part C 与 floors/navigation.md 均明确规定的**底部 2px 品牌蓝指示线**（`border-bottom-color: var(--o-color-primary1)`）。现已改为「指示线 + 文字加深」，并显式区分「选中态（指示线）」与「展开态（浅蓝背景）」两套机制。
  - §八 CSS 同步重写：`.o-nav__link` 基础几何声明移除（归 floors/navigation.md 管），只保留 hover / 展开态 / 选中态三个面板相关态。
- **`floors/navigation-dropdown.md` 面板间距与自适应按用户口述重定**：
  - 面板内边距 24px 32px → **四边均 4px**
  - 菜单项 gap 24px → **12px**
  - 自适应从「1518/1440/1280/MB 四档」改为**三档**：≥1441px（PC 16px）/ **841~1440px（保持 PC 样式，仅字号降为 12px）** / ≤600px（MB 14px）
  - 面板高度反推值由「1 行 ≈84px / 2 行 ≈144px」修正为「1 行 ≈48px、每多 1 行 +52px」（按 padding 4px + 项高 40px 推导），实测值标注为仅 QA 参考
  - 601~840px 区间 hard-constraints 无定义，新增为待确认项
- 踩坑记录扩至 16 条，新增 #8（展开态做成 primary1 实心蓝底）、#9（选中态漏指示线）、#16（841~1440px 误改 MB 布局）；自检清单同步扩充。

#### 修正（再续 · 2026-10-14 用户指出布局方向错误）
- **`floors/navigation-dropdown.md` 面板布局从「横向 flex-wrap 平铺」改为「垂直列表」**（用户指出 hover 色块应撑满面板、选项为固定高度透明容器）：
  - 面板 `display: flex; flex-wrap: wrap; align-items: center; gap: 12px` → **`flex-direction: column; align-items: stretch; gap: 0`**
  - 新增「平铺式 = 垂直列表、选项不分层无分类标题」的定义澄清（与第二轮「分类平铺式」相对，非横向排列）
  - **选项容器规格重定**：`width: 100%`（色块撑满整行，不受文字长短影响，左右各让出面板 padding 4px）+ 固定高度 **40px（>1440px）/ 32px（841~1440px）** + `flex: none` + `display:flex; align-items:center`（文字垂直居中）+ `background: transparent`（默认透明，仅交互态填充）
  - 选项间距 gap 12px → **0px**（相邻色块上下相接）
  - 选项内边距 8px 12px → `0 12px`（垂直方向由固定高度 + 居中控制）
  - 自适应表改为按容器高度分档：≥1441px = 40px/16px；841~1440px = **32px/12px**（该档从「只改字号」修正为「改容器高度 + 字号」）；≤600px = 40px/14px（推断）
  - 面板高度公式修正为 `8px + 容器高 × N`
- 踩坑记录扩至 **21 条**，新增 #16（误解平铺式为横向）、#17（色块未撑满整行）、#18（容器默认带底色）、#19（容器高度靠 padding 撑）、#20（选项间出现空隙）、#21（841~1440px 漏改容器高度）；自检清单同步扩充。
- 新增待确认项 #8（MB 端容器高度 40px 为推断值）、#3（选项文本左右内边距 12px 为推断值）。

#### 修正（续 · 2026-10-14 顶部导航按 navigation.md 规范复核）
- **`floors/navigation.md` 源码下拉文字色修正 info2 → info1**：原颜色表与「源码下拉实现」CSS 均写 `var(--o-color-info2)`，与 `components/dropdown.md` Part B「text 变体：文字/图标均为 `--o-color-info1`」及 `components/navigation.md`「源码下拉颜色 --o-color-info1」矛盾（两份组件规范一致，楼层模板为孤例）。已改为 info1 并注明来源。
- **`.nav-dropdown-check.html` 顶导骨架 4 处偏差修正**（复核 navigation.md 后发现）：
  - `.o-nav` box-shadow 硬编码 `0 3px 9px 0 rgba(0,18,85,0.078)` → `:root` 定义 `--o-shadow-1`（值取 components/navigation.md 顶导 boxShadow 规格，加来源注释）后改为 `box-shadow: var(--o-shadow-1)`
  - `.o-nav__source` 文字色 info2 → **info1**（ODropdown text 变体，同上）
  - `.o-nav__source` gap 4px → **8px**，补 `min-width: 48px`（floors/navigation.md 源码下拉实现规格）
  - **图标 fill 字面量 → 语义 Token**（SKILL.md 图标处理规范「颜色用 currentColor 或 var(--o-color-*)」）：logo 黑 `rgb(0,0,0)` → `var(--o-color-info1)`（9 处）、logo 蓝 `rgb(0,47,167)` → `var(--o-color-primary1)`（1 处）、各图标黑 → `var(--o-color-info1)`（7 处，`fill-opacity="0.800000012"` 等属性按规范原样保留，浅色模式渲染不变、深色模式可自适应）；遮罩内结构色 `rgb(196,196,196)` 保留不动
- 复核确认无偏差项：三层对齐（外层 center / 左侧 flex-end / Logo align-self:center / 菜单 flex-end / 右侧 center）、72px 高 + padding 0 216px、左侧 gap 40px、菜单 gap 32px、右侧 gap 20px、菜单项 48px + padding-bottom 24px + border-bottom 2px transparent、16px/24px/400、搜索框 160×32（border + 背景 + 内部绝对定位 + 占位 info4 + 图标 16×16）、源码下拉箭头 16×16（components/navigation.md 明确规格）、操作图标 24×24、7 个菜单项、右侧 5 元素顺序、图标 path 数据与 assets 逐字符一致。

#### 修正（续 · 2026-10-14 用户指出选中态背景色块 + 面板互斥失效）
- **`floors/navigation-dropdown.md` 触发器「展开态」自创背景色块删除**（用户指出「选中怎么可能会有个背景色块垫在后面」）：
  - 核对 `components/navigation.md` Part C「顶导交互状态」：仅定义默认 / Hover / Active 三态，**全部背景透明**，无任何背景态。第二版给展开态自创的 `control3-light`（#EBF1FA）浅蓝背景块、SemiBold 字重全部删除；首版自创的 primary1 实心蓝底白字早已删除，本轮将三次误写全部显式列为禁令
  - 展开态规格改为：**文字 `primary1` + 背景透明**（视觉同 Hover，规范未单独定义展开态）；选中态维持底部 2px primary1 指示线 + 文字加深
  - §三 速查表「触发器展开态背景」行删除；§4.1 状态表背景列全部标注透明并加 ⛔ 禁令块；§五 颜色表删除「触发器点击态背景/文字」（primary1 底 + 白字，亦为自创），新增「触发器无任何背景 Token」禁令
- **单开互斥从「仅 JS」升级为「JS + CSS `:has()` 双保险」**（用户指出 hover 与选中触发会面板重叠）：
  - 根因：hover（`:hover`）与 click 锁定（`data-open`）是两套独立 CSS 机制，点开 A 后再 hover B，两条显示规则同时命中，A、B 面板重叠
  - 新增互斥规则：`.o-nav__menu:has(> .o-nav__item:hover > .o-nav__dropdown) > .o-nav__item:not(:hover) > .o-nav__dropdown`（focus-within 同理）——菜单内任意**带面板**的项被 hover/focus 时，其余项（含锁定项）面板隐藏；选择器限定 `:hover > .o-nav__dropdown`，hover 无面板项（如当前页）不会误关锁定面板；MB 端同步覆盖 transform
  - JS 补 `link.blur()`：关闭时若不移出焦点，`:focus-within` 会立即使面板重新显示，导致点击关不掉
  - 踩坑 #8 重写（背景色块）、#11 重写（重叠根因与双保险解法）；自检清单同步更新；待确认项重编号（原 #2「展开态圆角」随背景删除失效）
- `.nav-dropdown-check.html` 同步修正：删除展开态背景与 SemiBold、补 PC/MB 互斥 CSS、JS 补 blur；新增「单开互斥验证」「触发器无背景验证」两张验证卡片，图例补充触发器状态说明。

#### 更新（续 · 2026-10-14 交互模型显式固化）
- **`floors/navigation-dropdown.md` 新增「交互模型（hover 临时态 vs click 锁定态）」章节**（用户确认 hover 与 click 的语义差异后补入规范，此前仅在「展开方式」一行带过，未讲清两种触发语义的区别）：
  - 明确表格：hover = **不固定**，鼠标移走即消失，用于鼠标用户快速浏览；click = **固定**，不随鼠标移走消失，需再次点击同一菜单项才关闭，用于触屏与固定面板操作
  - 四条行为要点：① hover 不改变锁定状态，鼠标移出整个菜单后被互斥临时隐藏的锁定面板**恢复显示**；② click 是 toggle（点已锁定项关闭、点未锁定项先关其他再锁定当前项）；③ 锁定项再次被 hover 时面板保持（锁定优先）；④ `:focus-within` 仅作键盘兜底，关闭须 `link.blur()`
  - §四 状态矩阵 Hover 行「面板」列标注「显示（**临时**，鼠标移走即隐）」；§八 CSS 三条显示规则注释改为「hover=临时 / focus-within=键盘兜底 / data-open=锁定（固定）」；JS 注释补 click = 锁定展开（toggle）语义
  - 踩坑记录扩至 **23 条**：#22（click 后面板关不掉 = 缺 blur）、#23（混淆 hover/click 两种触发语义）
  - 自检清单新增 4 条（hover 临时性、click 锁定性、锁定面板恢复、blur 兜底）；已确认清单新增 #15
- `.nav-dropdown-check.html` 说明文案改为显式描述交互模型；页面标题标注「· 交互模型」。

#### 修正（续 · 2026-10-14 用户指出选中态不可切换 + 指示线被圆角截断）
- **`.nav-dropdown-check.html` 选中态指示线圆角截断修复**（用户指出「选中底部的指示线怎么有个被截断的圆角？不应该只是一根线吗？」）：
  - 根因：`.o-nav__link` 上误加了 `border-radius: 4px`（把搜索框 `.search-box` 的圆角搬到了菜单项上），`border-bottom` 指示线两端被圆角裁出弧形缺口
  - 该圆角**无任何视觉效果**——顶导菜单项所有状态背景一律透明（navigation.md Part C），圆角本来的用途是给背景色块收边，而触发器根本没有背景色块
  - 已删除 `.o-nav__link` 的 `border-radius` 及 `transition` 中多余的 `background-color`，指示线恢复为完整一根水平直线（宽 = 菜单项宽 32px − 左右 padding 4px = 24px）
  - 规范核对确认：`floors/navigation.md` 中 `border-radius: 4px` 仅属于 `.search-box`，菜单项从未定义圆角——属生成时自创
- **`.nav-dropdown-check.html` 选中态改为可切换**（用户指出「当前选中是文档，点击下载时选中态应该切换为下载」）：
  - 根因：`is-current` 只写死在 HTML 上（`<li class="o-nav__item is-current">文档`），JS 完全未处理，点击其他菜单项选中态不动
  - JS 补点击迁移：先移除所有 `.is-current`，再给当前点击项添加；**单选、不做 toggle**——再次点击已选中项不取消选中，否则顶导会丢失「当前位置」信息
  - 选中态与展开态两套机制独立：点击「下载」后它同时持有 `is-current`（指示线）与 `data-open`（面板），互不冲突
- **`floors/navigation.md` 补两条踩坑记录**（#9 菜单项 border-radius 导致指示线圆弧缺口、#10 is-current 写死不可切换）+ 自检清单 2 条；精确数值速查新增「菜单项 border-radius = 禁止」并附原因说明块
- **`floors/navigation-dropdown.md` 同步**：踩坑记录扩至 **25 条**（#24 圆角截断、#25 选中态不可切换）、自检清单 +2 条、JS 最小实现补 `is-current` 迁移代码
- `.nav-dropdown-check.html` 新增「选中态切换验证」「指示线形态验证」两张验证卡片。

#### 更新（续 · 2026-10-14 用户要求汇总问题并确保下次生成一步到位）
本轮排查出 **4 个会导致下次生成再次出错的规范缺口**，已全部修复：
- **缺口 1（严重）：顶导菜单项基础样式「责任悬空」**
  - 症状：`navigation-dropdown.md` 写「`.o-nav__link` 的 height/padding-bottom/border-bottom 基础样式由 floors/navigation.md 提供，此处严禁重定义」，但 **navigation.md 里根本没有这段 CSS**——只有 `.nav-menu { align-self: flex-end; }` 一行
  - 后果：生成方被要求「严禁重定义」却无据可依，只能自行编写，必然自创 `border-radius: 4px`、SemiBold 字重、背景色块等错误属性（本轮 17 个问题中有 4 个直接源于此）
  - 修复：navigation.md 新增 **「菜单项（触发器）完整样式」** 独立章节，给出完整 CSS（`.nav-menu > li` + `.nav-menu a` + hover + `.is-current` 选中态），并在 CSS 注释内嵌「⛔ 绝对禁止 border-radius」「⛔ 禁止改 SemiBold」两处禁令；附「选中态必须可切换」说明（单选、不做 toggle）
- **缺口 2：两份楼层模板 class 命名不统一且无映射表**
  - 症状：navigation.md 用 `.header` / `.nav-menu` / `.logo`，navigation-dropdown.md 用 `.o-nav__menu` / `.o-nav__item` / `.o-nav__link` / `.o-nav__dropdown`，无任何说明
  - 后果：生成方需自行猜测两份模板的对应关系，易混用或漏挂面板
  - 修复：**两份文件互相插入同一张 class 对应表**，并注明「禁止混用」「菜单项基础样式归 navigation.md」
- **缺口 3：dropdown 模板 HTML 骨架只有默认态**
  - 症状：骨架 9 个选项全无状态类名，而 CSS 骨架已定义 `.is-selected` / `.is-disabled` 三态
  - 修复：HTML 骨架补 `.is-selected`（已选）/ `.is-disabled` + `aria-disabled`（禁用）/ 触发器 `.is-current` 示例，并加状态类名汇总说明
- **缺口 4：无「生成前必读」清单**
  - 修复：navigation.md 开头新增「生成前必读（按顺序读取，缺一不可）」表，列明 3 份文件各自必须取用的内容，并点明「只读一份会漏什么」
- **designer-guide.md**：导航下拉面板行补充「必须与导航楼层搭配生成，两份模板 class 命名不同需按对应表映射」。

#### 修正（续 · 2026-10-14 用户指出搜索图标跑到右侧）
- **`.nav-dropdown-check.html` 搜索图标位置修复**（用户指出「搜索框里面的图标怎么到了右边？？这个错误很严重！！」）：
  - 根因：`.o-nav__search svg` 设了 `right: 12px`，把搜索图标推到占位文字右侧。**OSearch small 规范明确「搜索图标 位置：左侧；占位文字 位置：搜索图标右侧」**（components/search.md），components/navigation.md 的参考实现 HTML 也是图标在前、文字在后
  - 修复：图标改 `left: 16px`，占位文字改 `left: 40px`（= 左内边距 16px + 图标 16px + gap 8px）；HTML DOM 顺序也调整为 `<svg>` 在前、`<span>` 在后
  - 顺带修复图标填充色：搜索图标应为 `var(--o-color-info4)`（`rgba(0,0,0,.4)`），原误用 `info1` + `fill-opacity="0.8"`（资源 SVG 切图残留值）；国际化/主题切换/用户三个图标应为 `info3`（`rgba(0,0,0,.65)`），原同样误用 `info1` + `fill-opacity="0.8"`
- **`floors/navigation.md` 修正搜索框规范**：
  - `.search-box svg` 由 `right: 12px` 改为 `left: 16px`，并内嵌「⛔ 严禁 right」注释；`.search-box span` 由 `left: 16px` 改为 `left: 40px` 并注明计算式
  - 新增「搜索框内部顺序固定为图标→文字」+「HTML 中图标须写在文字前」说明
  - 颜色表新增**搜索图标 = info4**、**国际化/主题/用户 = info3** 两行，并新增四图标 Light/Dark 色值对照表，明确「资源 SVG 的 `fill-opacity="0.8"` 是切图残留，内联时须换成语义 Token 并去掉 fill-opacity，否则深浅色模式无法自适应」
  - 踩坑记录扩至 **12 条**（#11 搜索图标跑到右侧、#12 四图标填充色统一用 info1）
  - 自检清单新增 4 条（搜索图标位置、搜索图标 info4、其余三图标 info3）

#### 修正（续 · 2026-10-14 用户指出右侧三个图标全部消失）
- **`.nav-dropdown-check.html` 右侧国际化/主题切换/用户三图标全部消失修复**（用户指出「右边的 icon 全部都不见了？？捡了芝麻丢了西瓜！」）：
  - **根因**：上一轮把这三个图标的 fill 从 `info1` 改成 `info3`（符合 Part H 规范，改动本身是对的），但 demo 的 `:root` 变量表里**只有 info1/info2/info4，从来没有定义 `--o-color-info3`** → `fill="var(--o-color-info3)"` 整条声明无效 → `<path>` 回退继承外层 `<svg fill="none">` → **图标完全不可见**（HTML 结构完好，只是渲染不出来）
  - 附带根因：`--o-color-info2` / `--o-color-info4` 原用 `rgba(0,0,0,0.8)` / `rgba(0,0,0,0.4)`，与 components/navigation.md Appendix A 的 Token 定义（#333333 / #999999）不一致
  - 修复：`:root` 补齐 `--o-color-info3: #666666`，并同时把 info2/info4 校准为 Token 速查表权威值（#333333 / #999999）；`info1/fill1/fill2/control1/control2-light/control3-light/primary1` 保持不变
  - 已交叉校验：全文件引用的 10 个 Token（fill1、fill2、info1~4、control1、control2-light、control3-light、primary1）全部在 `:root` 有定义
- **`floors/navigation.md` 新增「顶导所需语义 Token 完整清单」**（根本解法，防止再犯）：
  - 给出**可直接抄入 `:root` 的完整 CSS**（13 个 Token，逐个标注用途），生成方无需自行拼装变量表
  - 新增铁律「**⛔ Token 必须先定义再使用**」：引用未定义 `var()` 时该 fill/color/background 声明整体无效，SVG path 会继承 `<svg fill="none">` 导致元素完全不可见；并强调「**每次修改色值后必须回头核对 `:root`**」
  - 踩坑记录扩至 **13 条**（#13 右侧图标全部消失——引用未定义 Token）
  - 自检清单新增 1 条（是否引用了未定义的语义 Token）
- **复盘结论**：本轮故障 skill 欠缺占主因（无完整 Token 清单、无"先定义再使用"铁律），粗心占次因（改 fill 色值时未回头检查 `:root`）。Token 清单已入规范，下次生成直接抄录即可规避。

#### 修正（续 · 2026-10-14 用户指出图标色应为 info2 而非 info3）
- **`.nav-dropdown-check.html` 三个右侧图标 `info3` → `info2`**（用户指出「不是 info3，是 info2」）：
  - **根因在 skill 文档，不在 demo**：`components/navigation.md` Part H 的「顶导 Icon SVG」表把国际化/主题切换/用户三个图标标为 info3（`rgba(0,0,0,.65)` / `#666666`），我照抄了该表
  - **实际资源文件核对**（`references/assets/navigation/` 三个 SVG）：填充均为 `fill="rgb(0,0,0)"` + `fill-opacity="0.800000012"` = **80% 黑 = `#333333` = info2**，与 info3（60% `#666666`）不符——**文档表格有误，用户纠正正确**
  - 修复：demo 三图标 fill 改 `var(--o-color-info2)`（`info2` 上一轮已补入 `:root`，无再次消失风险）
- **`components/navigation.md` Part H 三处错误全部修正**（这是本轮错误的源头，必须改）：
  1. 「顶导 Icon SVG」表：三个图标 Light 色 info3 → **info2**（`rgb(0,0,0)` @80%）；尺寸 20×20 → **24×24**（与自身结构图、速查表、floors/navigation.md 及资源 viewBox 一致）
  2. 表格下方新增修正说明：「**图标颜色与尺寸以 `references/assets/navigation/` 资源文件为准，本表为辅**」
  3. Part H「顶导 Icon CSS mask」参考实现：`.nav-icon` 默认 `background-color` 由 `rgba(0,0,0,.65)` → **`rgba(0,0,0,.8)`**（80% 黑 = info2）；`.nav-icon-lang` / `.nav-icon-theme` / `.nav-icon-user` 尺寸由 20×20 → **24×24**；`.nav-icon-search` 补充注释「info4，比其余三图标更浅」
  - Dark 侧 `rgba(255,255,255,.85)`（info1-dark）经核对与表格一致，未改动
- **`floors/navigation.md` 同步**：颜色表与四图标对照表 info3 → info2、尺寸 20×20 → 24×24；新增铁律「**勿照抄 components/navigation.md Part H 的 info3，图标颜色以 assets 资源文件实际填充值为准**」；` :root` 清单注释调整（info2 = 次要文字 + 三图标 fill；info3 = 页脚次要文字，顶导不使用）；踩坑记录扩至 **14 条**（#14 三图标填成 info3——照抄文档表格未核对资源文件）；自检清单同步
- **复盘结论**：本轮 100% 是 skill 欠缺——Part H 表格数据本身错误，我照抄即错。已把「**资源文件 > 文档表格**」的优先级写进规范，后续遇冲突以资源文件为准。

---

## 2026-10-10

### ⚠️ 破坏性
- **楼层模板图标引用方式统一为内联 SVG**：`floors/navigation.md`、`floors/footer.md` 原要求 Logo / 图标用 `<img>` 或 CSS mask 引用 assets 资源，现对齐 SKILL.md「图标处理规范」——一律 Read 资源后逐属性原样内联 `<svg>`，禁止 `<img src>` / CSS mask / 凭记忆手绘。
  - **影响评估**：仅影响按旧版模板生成的 HTML（以 navigation / footer 楼层为主）——按新版自检清单复核时 `<img>` 引用判为违规形式。**渲染效果不变，无功能性破坏**：同一 SVG 内联前后视觉一致；且内联后图标可用 `currentColor` / 语义 Token 随深浅模式自动适配，原型移出本仓库目录后不再因相对路径（含中文/空格文件名）失效。旧规则本身与 SKILL.md 既有「禁止 `<img src>`」冲突，本次为消解矛盾而非新增禁令。
  - **复核动作（机械性）**：对已生成 HTML 执行 `grep -n "<img"`，命中即取对应 assets SVG 逐属性内联替换；布局与其他代码无需改动。

### 新增
- **SKILL.md 新增「规则冲突裁决」条款**：楼层模板（floors/）、组件规范（components/）、示例与 hard-constraints.md 或 SKILL.md「图标处理规范」「通用生成约束」冲突时，一律以后者为准；发现冲突停下向用户报告，禁止各取所需。

### 更新
- **SKILL.md 渲染验证按环境能力分级**：有浏览器能力（Playwright / Puppeteer MCP、IDE 预览）→ 截图目检；无（headless / CLI）→ 静态自检并在交付说明中显式标注「未做渲染目检，需人工在浏览器复核」，禁止静默跳过。覆盖第六步与第八步。
- **SKILL.md 位图素材文生图解除厂商绑定**：移除硬编码的 Trae 文生图端点，改为「环境具备文生图能力则用之（端点以所用 AI 工具为准），无能力则占位色块 + 注释」，保留生成后校验与禁止编造图片 URL 的要求。
- **SKILL.md Token 降级数据源指向本地六主题文件**：WebFetch 拉取上游 Token 失败时，降级用 `../opendesign-tokens/references/tokens-{社区}.md`（六主题全量 + Dark 值反查，离线可用）；designer-guide.md 速查表降级为 openEuler Light 辅助；目标社区无本地 Token 文件时停下询问。
- **floors/navigation.md、floors/footer.md、floors/banner.md 颜色全量语义 Token 化**：rgba / hex 硬编码改为 `var(--o-color-*)` 语义 Token（fill2 / info1~4 / control1 / shadow-1 / color-white），深浅模式统一由 `data-o-theme` 切换自动适配，删除 Light/Dark 双列色值。上游 Token 体系无对应变量的组件特例值（页脚固定深色、MB 顶导半透明背景、footer 分隔线等）显式标注「组件特例值」并要求加 HTML 注释，禁止扩散。
- **navigation.md 踩坑 #7、#8 与自检清单对齐全局规范**：图标改为「Read 后逐属性内联」；「内联 style + !important」改为「提高选择器优先级，宿主覆盖时允许带注释的 !important（硬约束布局强制豁免场景），禁止内联 style」。
- **README.md、根 AGENTS.md、根 README.md 中 `opendesign-design` 的描述**由「Pixso 设计稿生产 / componentKey 速查」更新为「HTML 设计原型生产（26 组件 + 5 楼层模板）」，global/ 两个速查副本在目录树中标注真源归属。

### 修正
- **floors/footer.md MB 端 CSS 背景值笔误**：`.footer-section-mb` 背景 rgba(18,18,20,1) → rgb(18,18,18)，与 MB 布局结构树及常见错误清单 #8（MB ≠ PC）对齐。
- **global/interaction-rules.md 图标规则对齐 SKILL.md**：移除过时的 icon-keys.md 引用，补充手写 SVG 兜底注释要求与「逐属性原样搬运」强制条款；与 layout-rules.md 一并在文件头部增加「真源声明」（内容已并入 hard-constraints.md，不一致时以 hard-constraints.md 为准，本文档仅作速查副本）。

---

## 2026-10-08

### 修正
- `components/dialog.md` 组合搭配中 `OTable 操作` 改为 `ODataTable 操作`（OTable 已弃用，表格统一用 ODataTable）。

---

## 2026-09-24

### 新增
- **SKILL.md 图标处理规范新增 SVG 搬运硬约束**：在「图标使用规则」表格中新增「搬运」属性行，强制逐属性原样搬运（保留 `fill-rule` / `clip-rule` / `opacity` / `fill-opacity` / `transform`），禁止仅拷贝 `path` 的 `d` 数据重写标签。
- **SKILL.md 图标处理规范新增风格判定规则**：新增「风格判定（强制）」条款，明确以浏览器渲染结果为准，不得仅凭源码 `fill`/`stroke` 属性下结论；`fill + fill-rule="evenodd"` 的环形构造渲染上是线性细线。
- **SKILL.md 通用生成约束新增 SVG 搬运条款**：在「通用生成约束（所有楼层共用）」中新增 SVG 搬运强制条款，覆盖所有楼层生成场景。
- **SKILL.md 第四步读取资源新增搬运提醒**：在「第四步：读取本楼层组件规范与资源」的图标资源检查步骤中，新增逐属性原样内联提醒。
- **SKILL.md 第六步验证清单新增 SVG 搬运检查项**：新增「图标 SVG 是否逐属性原样搬运」校验项，确保搬运问题在楼层验证阶段即被拦截。
- **SKILL.md 第八步最终验证新增图标风格检查项**：新增「关键图标是否与设计系统一致」校验项，以浏览器渲染为准。

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