---
name: "adversarial-reviewer"
description: "Adversarial review for design drafts: independently reviews prototype_*.html against a 7-dimension checklist (requirement traceability, principles compliance, etc.), producing evidence-based Review_Design_* report. Invoke after design draft generation and before delivery, or when user asks for 设计稿评审/对抗评审."
---

# 对抗评审器（设计稿交付前的最后一道关卡）

承接 design-draft-generator 的产出，以**独立评审者**身份对其设计稿做对抗评审。核心价值来自**独立视角**：评审者与生成者上下文隔离（subagent 派发），不存在"自己检查自己"的确认偏误。

**业务背景**：鲲鹏、昇腾等计算生态产品，以及 openEuler（欧拉）、openGauss（高斯）、openUBMC 等开源项目。

## 安装方式

本 skill 遵循 Agent Skills 开放标准（一个目录 + 一个 SKILL.md），支持 Claude Code、OpenCode、Codex 主流 AI 编程平台。`<SKILL_SOURCE>` 指本 skill 目录（含 SKILL.md 的 `adversarial-reviewer/` 文件夹）。

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
| Claude Code | `~/.claude/skills/adversarial-reviewer/SKILL.md` |
| OpenCode | `~/.config/opencode/skills/adversarial-reviewer/SKILL.md`（兼容 `~/.claude/skills/`、`~/.agents/skills/`） |
| Codex | `~/.codex/skills/adversarial-reviewer/SKILL.md`（新版亦支持 `~/.agents/skills/`） |

**注意**：目录名必须与 frontmatter `name` 一致（`adversarial-reviewer`）；修改后未生效时重启对应 CLI。

## 评审者立场

1. **假设有偏**：心态是"这份 demo 一定有问题，我的任务是找出来"；通过项也要有证据支撑，不允许"看起来没问题"式通过
2. **证据先行**：每个判定（✅/❌）必须附证据——❌ 项定位到具体位置/代码行，禁止笼统结论
3. **物理隔离**：本 skill 的执行者不是生成者（独立上下文/独立会话派发），天然持有外部视角——禁止因"我了解生成过程"而放松检查

## 输入契约

| 材料 | 期望位置 | 缺失处理 |
|------|---------|---------|
| 需求基准 | `docs/PRD_[主题]_[YYYYMMDD].md` 或设计简报（`design-draft-generator/templates/new-community-brief.md` 填写件） | 两者皆无则评审阻塞，在报告中声明缺失并终止 |
| 被评审物 | `design/prototype_*.html` 清单（多方案时全部提供） | 缺失即终止 |
| 方案数量 N | 单方案 / 多方案 | 由被评审物文件名推断（`方案A/B/C` 后缀），无法推断时按单方案处理并在报告声明 |
| 路由类型 | A（设计系统看护）/ B（自由生成） | 未指明时按 B 处理并在报告声明 |
| 评审基准规范 | `design-draft-generator/SKILL.md` 的"设计纲领"与"质量标准"两节 | 必读——第 2/3/4/5 项检查的判定依据来源 |

**subagent 模式**（由 design-workflow 派发时）：以上输入全部来自派发任务描述中的路径，**禁止向用户提问**；信息缺失时在报告中声明缺失项并终止，禁止猜测。交互模式（用户直接调用）下材料缺失可向用户索取。

## 工作流程

### 第 1 步：读取评审基准

1. 读需求基准（PRD 第 3/4 章，或设计简报），提取功能编号索引（PRD-001…）与验收标准
2. 读 design-draft-generator/SKILL.md 的"设计纲领"（三大理念 + 五大原则 + CRAP 四原则）与"质量标准"（尤其第 8 条视觉细节对齐）
3. 读全部被评审物（HTML 源码逐个通读，含注释中的交互说明）

### 第 2 步：逐项对照检查表

按下表逐项检查，每项给出 ✅/❌ 及证据（❌ 项须定位到具体位置/代码行，禁止笼统结论）：

| # | 检查维度 | 检查要点 |
|---|---------|---------|
| 1 | 需求可溯 | 每个模块能对应 PRD-XXX 或简报条目；抽查 3 个模块反向追溯 |
| 2 | 纲领合规 | 三大理念逐条过：有无营销导向元素？有无保姆式隐藏复杂度？核心功能从发现到操作是否闭环？ |
| 3 | 五大原则 | 简洁/准确/连贯/高效/闭环各过一遍；重点抓：关键功能是否 Hover 才可见、状态是否仅靠颜色传义 |
| 4 | CRAP 四原则 | 同级元素规格是否一致（重复）？层级 ≤ 4 级（对比）？组内间距 < 组间间距（亲密）？对齐线是否贯通（对齐）？ |
| 5 | 视觉细节 | 质量标准第 8 条：导航 Logo/文字垂直居中；深色背景上无黑色图标；**布局堆叠/溢出**——所有固定 `width`/`height` 的容器若承载文本，逐个核对文案实际宽度 vs 容器宽度（估算公式：CJK 字符×字号 + 拉丁字符×约0.55×字号 + 空格），超出即折行；固定 `height` + 可折行长文案 = 堆叠高发区（步骤条节点、卡片标题、按钮、标签），必须逐个排查并在报告中列出核对记录 |
| 6 | 多方案横向对比（N > 1 时） | 信息架构是否严格一致？布局/交互是否可辨差异？视觉是否均在开发者调性内（无商务/营销风）？ |
| 7 | 真实可评审 | 占位文案是否真实中文（无 lorem ipsum）？声明了的交互是否真的可点？ |

**路由 A 场景**：对抗评审聚焦需求可溯性与楼层规划的执行情况；Token/组件合规由 opendesign-design 的硬约束自检清单负责，本环节不重复。

### 第 3 步：输出评审报告

输出到 `reports/` 目录（与 `docs/`、`design/` 并列），命名 `Review_Design_[主题]_[YYYYMMDD].md`，结构：

1. **概览**：被评审物清单、路由类型、方案数量、❌ 项统计（按检查维度分列）
2. **检查表**：7 项 → 判定（✅/❌）→ 证据（❌ 项定位到位置/代码行）
3. **修复建议**：每个 ❌ 项给出改什么、怎么改
4. **复检记录**：初始为空，随修复进度更新（见第 4 步）
5. **风险与声明**：无法修复项（如素材缺失导致的占位）显式声明原因与风险

**返回编排层的结构化摘要**（subagent 模式）：报告路径 + 结论（通过 / ❌ 项数量与所属维度）。

### 第 4 步：复检模式

派发任务附上次 ❌ 清单时，进入复检模式：**只复检修改影响范围**（❌ 项所在维度 + 直接相邻结构），不全文重审。复检结果更新进报告"复检记录"，全部通过才宣告评审完成。

**每次复检必须同步更新终态**：报告概览区的 ❌ 项统计与"评审通过/不通过"结论、头部"评审模式"元信息须反映最新一轮复检状态——终态是 design-workflow 编排层判断"可否进入交付流程"（PRD 回填、阶段 4 派发）的读取依据，仅追加复检记录而不更新终态会导致编排层误判。

修复动作本身不在本 skill 内执行（修复由生成者在主对话完成），本 skill 只负责评审与复检。

## 质量标准（评审前自查 + 报告评审基准）

1. **基准完备**：需求基准与评审规范均已读取；缺失项已在报告声明
2. **证据完备**：所有 ❌ 判定附位置/代码行证据；✅ 项有检查记录可查
3. **立场独立**：不因了解生成过程而放松检查；禁止"整体还行"式模糊结论
4. **闭环完整**：❌ 项全部关闭或有显式风险声明
5. **高频问题优先**：深色背景图标颜色、导航垂直居中、**固定尺寸容器文本溢出堆叠**三类历史高频问题必查

## 与其他 skill 的关系

- **design-draft-generator**：上游，其产出是被评审物；其"设计纲领"与"质量标准"是本 skill 的评审规范来源
- **design-workflow**：编排器，本 skill 是其阶段 3（对抗评审）的执行者；也可独立使用（用户已有设计稿只要评审时），不强制经过该编排
- **visual-reviewer / functional-reviewer**：同属评审类 skill，分别负责阶段 4 的视觉与功能检视（对象是测试环境实现，本 skill 对象是设计稿本身）
