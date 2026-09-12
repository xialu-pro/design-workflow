---
name: "design-workflow"
description: "End-to-end workflow orchestrator: PRD generation (requirement-doc-generator) → design draft generation (design-draft-generator, route A/B) → adversarial review → PRD backfill → delivery review (visual-reviewer first, then functional-reviewer in serial). Invoke when user asks to run the full 需求到交付 flow or mentions 设计工作流/端到端."
---

# 设计工作流（需求 → 设计稿 → 交付检视 端到端编排）

串联 requirement-doc-generator、design-draft-generator、adversarial-reviewer、visual-reviewer、functional-reviewer 五个 skill，把"想法 → PRD → 设计稿 → 闭环回填 → 开发交付检视"跑成一条流水线。本 skill **只负责编排**（阶段流转、确认门、交接物管理），不重复各执行 skill 的内部规范——执行时严格按各 skill 自身的工作流走。

**执行模式（阶段 1-2 与阶段 3-4 不同）**：

| 阶段 | 执行方式 | 原因 |
|------|---------|------|
| 1 PRD 生成、2 设计稿生成 | 主对话直接切换到对应 skill 执行 | 需与用户多轮交互（提问、确认门） |
| 3 对抗评审、4 交付检视 | **Task 派发独立评审 subagent**（类型 `general_purpose_task`，无预设人设，完全按 SKILL.md 指令执行） | 评审需要独立视角：生成者与评审者上下文物理隔离，消除确认偏误；三个评审均为非交互、基于证据产报告的任务 |

派发规范见"评审 subagent 派发规范"一节。

**业务背景**：鲲鹏、昇腾等计算生态产品，以及 openEuler（欧拉）、openGauss（高斯）、openUBMC 等开源项目。

## 安装方式

本 skill 遵循 Agent Skills 开放标准（一个目录 + 一个 SKILL.md），支持 Claude Code、OpenCode、Codex 主流 AI 编程平台。`<SKILL_SOURCE>` 指本 skill 目录（含 SKILL.md 的 `design-workflow/` 文件夹）。

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
| Claude Code | `~/.claude/skills/design-workflow/SKILL.md` |
| OpenCode | `~/.config/opencode/skills/design-workflow/SKILL.md`（兼容 `~/.claude/skills/`、`~/.agents/skills/`） |
| Codex | `~/.codex/skills/design-workflow/SKILL.md`（新版亦支持 `~/.agents/skills/`） |

**依赖**：本 skill 编排以下 skill，需一并安装（通常位于同一 skills 目录）：
- `requirement-doc-generator`（阶段 1 执行者）
- `design-draft-generator`（阶段 2 执行者；路由 A 另需 `opendesign-design`，从 atomgit 获取）
- `adversarial-reviewer`（阶段 3 · 对抗评审 subagent）
- `visual-reviewer`（阶段 4 · 视觉一致性测试员 subagent）
- `functional-reviewer`（阶段 4 · 功能测试员 subagent）

**注意**：目录名必须与 frontmatter `name` 一致（`design-workflow`）；修改后未生效时重启对应 CLI。

## 工作流总览

```mermaid
flowchart TD
    S[用户需求输入] --> E{阶段 0：输入判定}
    E -- 已有 PRD --> G2
    E -- 新建社区·有简报素材 --> G2
    E -- 只有想法/简述 --> G1

    subgate G1[阶段 1：生成 PRD<br/>requirement-doc-generator]
    G1 --> D1{确认门 1<br/>PRD 评审通过？}
    D1 -- 需修改 --> G1
    D1 -- 通过 --> G2

    subgate G2[阶段 2：生成设计稿<br/>design-draft-generator]
    G2 --> R{路由判定：询问用户<br/>是否使用 OpenDesign 设计系统}
    R -- 使用 --> A[路由 A：opendesign-design<br/>楼层规划 + 组件清单]
    R -- 不使用 --> B[路由 B：HTML 交互原型<br/>简报 + 信息架构 + N 方案]
    A --> G3
    B --> G3

    subgate G3[阶段 3：对抗评审<br/>adversarial-reviewer subagent]
    G3 --> D2{全部 ✅？}
    D2 -- 有 ❌ --> F[主对话修复<br/>+ subagent 复检]
    F --> D2
    D2 -- 通过 --> H[回填 PRD 第 4 章]

    H --> DEV[开发实现 · 部署测试环境<br/>（流水线外部环节）]
    DEV --> G4
    subgate G4[阶段 4：交付检视<br/>visual-reviewer subagent → functional-reviewer subagent]
    G4 --> D3{视觉 P0/P1 关闭？<br/>→ 功能 ❌ 关闭？}
    D3 -- 修复后复检 --> G4
    D3 -- 通过 --> DONE[交付上线<br/>工作流完成]
```

| 阶段 | 执行 skill | 核心动作 | 通过条件 |
|------|-----------|---------|---------|
| 0 输入判定 | 本 skill | 判定从哪进入，避免重复劳动 | 入口确定 |
| 1 PRD 生成 | requirement-doc-generator | 提问 → 生成 `docs/PRD_[主题]_[YYYYMMDD].md` | 用户确认通过 |
| 2 设计稿生成 | design-draft-generator | 路由判定（用户确认是否使用设计系统） → 预消化/简报 → 生成 `design/prototype_*.html` | 设计稿产出 |
| 3 对抗评审 + 闭环 | adversarial-reviewer（subagent）| 7 项检查表 → 修复闭环（修复在主对话，复检再派发）→ 回填 PRD | 全部 ✅ + 回填完成 |
| 4 交付检视 | visual-reviewer → functional-reviewer | 先视觉还原度比对（P0/P1/P2），P0/P1 关闭后再做设计师清单主基准的关键流程走查 | 视觉 P0/P1 先关闭，功能 ❌ 随后关闭（或有显式风险声明） |

## 阶段 0：输入判定（必先执行）

| 用户带来的输入 | 进入点 | 说明 |
|--------------|--------|------|
| 完整 PRD（`docs/PRD_*.md`） | 直接阶段 2 | 验证 PRD 含功能编号与验收标准，缺失则回到阶段 1 补齐 |
| 新社区 + 简报/素材 | 阶段 2（默认建议路由 B，仍需用户确认是否使用设计系统） | 用设计简报替代 PRD；简报不完整时按 3B-0 代填 |
| 想法 / 一句话描述 / 痛点 | 从阶段 1 开始 | 走完整流水线 |
| PRD + 设计稿都有，只要评审 | 直接阶段 3 | 只跑对抗评审 |
| 开发已完成，测试环境 + DEMO 都有 | 直接阶段 4 | 交付检视按串行执行：先视觉还原度检视，P0/P1 关闭后再功能检视；用户只要其中一项时同样遵守前置（功能检视需视觉 P0/P1 已关闭） |

无法判断时 AskUserQuestion 询问，禁止臆测入口。

## 阶段 1：PRD 生成

**执行**：切换到 requirement-doc-generator 的工作流（交互提问 ≤3 轮 → 生成 7 章 PRD），本 skill 不修改其流程。

**编排职责**：
1. 向用户预告阶段目标："本阶段产出 PRD，评审通过后才进入设计稿生成"
2. 生成完成后，将 PRD 路径与功能编号索引（PRD-001…）登记进交接物清单
3. **确认门 1**：引导用户评审，重点提示检查——
   - 每条需求验收标准可测试（无"优化体验"类模糊表述）
   - "本期不做"边界清晰
   - P1 功能有详述或明确延后
4. 用户提修改意见 → 修改 PRD → 再次过确认门 1；通过后才进入阶段 2，**禁止 PRD 未确认就生成设计稿**

## 阶段 2：设计稿生成

**执行**：切换到 design-draft-generator 的工作流（读 PRD → 路由判定 → 生成），本 skill 不修改其流程。

**编排职责**：
1. 传递阶段 1 的上下文：PRD 路径、功能编号索引、用户已确认的范围边界
2. 路由判定必须由用户确认（design-draft-generator 第 2 步会询问"是否使用 OpenDesign 设计系统"，用 → A、不用 → B），判定结果与依据（含用户答复）登记进交接物清单；路由 A 需先确认 `opendesign-design` 已安装
3. 路由 A：等待用户确认"楼层规划 + 组件清单"两份预消化材料后再继续（沿用其 skill 内置确认步骤）
4. 路由 B：N > 1 时确认方案数量与信息架构（沿用其 skill 内置确认步骤）
5. 产出后登记设计稿路径进交接物清单

## 阶段 3：对抗评审 + 闭环

**执行**：Task 派发 adversarial-reviewer subagent（生成者与评审者物理隔离，消除确认偏误）。

**编排职责**：
1. 按派发规范（见"评审 subagent 派发规范"）派发 adversarial-reviewer，任务描述含：PRD/简报路径、被评审物清单、方案数量 N、路由类型（A/B）
2. subagent 返回后**验证 `reports/Review_Design_*.md` 已落盘**并读取内容——只信磁盘产物，不信任口头返回；报告缺失或检查表不完整（缺判定/证据）时重新派发
3. ❌ 项修复在主对话执行（design-draft-generator，生成者上下文仍在）→ 修复后再派发 adversarial-reviewer，任务附上次 ❌ 清单（复检模式，只复检影响范围）
4. 全部通过后执行**闭环回填**：设计稿路径写入 PRD 第 4 章"交互与原型说明"，注明覆盖的验收标准
5. 输出阶段交付摘要：PRD 路径 + 版本、设计稿路径清单、评审报告结论、回填位置——登记进交接物清单
6. **阶段 3 完成即设计侧交付**；是否继续阶段 4 由用户决定（开发实现属流水线外部环节，时长不可控）——用户宣布开发完成、测试环境就绪时再进入阶段 4

## 阶段 4：交付检视（开发完成、测试环境就绪后）

**执行**：依次 Task 派发 visual-reviewer（视觉一致性测试员）与 functional-reviewer（功能测试员）subagent，本 skill 不修改其流程。两 subagent 互不可见——functional-reviewer 不预读视觉报告，避免"顺杆爬"。

**编排职责**：
1. 前置核对：DEMO 交付物路径（阶段 2 交接物）、测试环境访问方式；确认设计师功能检视清单是否已填写（`docs/FuncChecklist_*.md`），未填写时引导先补清单（主对话交互，subagent 无法向用户索取）
2. **串行硬阻断**：派发 functional-reviewer 前，编排层**直接读取 `reports/Review_Visual_*.md`** 验证 P0/P1 全部关闭（或用户明确跳过 + 风险声明）——不依赖 skill 自觉前置检查；未关闭时不派发，先走视觉修复闭环
3. 派发 visual-reviewer：检视计划中需用户确认的环节（检视页面清单），由编排层在主对话完成确认后写入派发任务
4. subagent 返回后**验证报告已落盘**（`reports/Review_Visual_*` / `Review_Func_*`）并读取内容——只信磁盘产物；报告缺失或判定缺证据时重新派发
5. 两报告独立归档、独立闭环；P0/P1 与 ❌ 项修复后监督复检（修复在主对话，复检再派发，只复检影响范围），全部关闭才宣告检视完成
6. functional-reviewer 回传的 PRD 冲突/模糊项，引导用户修订 PRD 后登记变更
7. 输出最终交付摘要：两份检视报告路径、问题终态、遗留风险清单——即交接物清单的最终态

## 评审 subagent 派发规范

三个评审环节（阶段 3 对抗评审、阶段 4 视觉/功能检视）统一按本规范派发。**subagent 是无状态的**：看不到用户消息与之前对话，所有输入必须自包含于任务描述；且无法向用户提问（无 AskUserQuestion），需用户决策的事项一律前置到主对话完成。

**统一规则**：

1. **任务描述三要素**：① 指令"读取 [skill 路径]/SKILL.md 并严格执行其完整流程"（subagent 类型 `general_purpose_task`）② 输入材料的完整路径清单 ③ 输出契约（报告路径 + 命名 + 返回格式）
2. **只信磁盘产物**：subagent 返回后必须验证报告已落盘并读取内容；口头返回的结论仅作参考，以磁盘报告为准
3. **缺失即终止**：subagent 遇输入缺失时会在报告中声明并终止——编排层读到"缺失终止"报告时，回主对话补齐材料后重新派发，禁止让 subagent 猜测
4. **信息隔离**：派发 functional-reviewer 的任务描述中不附视觉报告内容；派发评审 subagent 不附生成过程对话——保持独立视角
5. **复检模式**：任务附上次 ❌/P0/P1 清单时，subagent 只复检影响范围

**三个环节的任务模板**：

> skill 路径按实际安装目录书写（`.trae/skills/` / `.claude/skills/` / `.opencode/skills/` / `.agents/skills/` 等，本 skill 与各评审 skill 位于同一 skills 目录）；下表以"skills 目录"指代。

| 环节 | 任务描述要点 |
|------|------------|
| 阶段 3 对抗评审 | 读 skills 目录下 `adversarial-reviewer/SKILL.md` 并严格执行；输入：PRD/简报路径、`design/prototype_*.html` 清单、方案数 N、路由 A/B、评审基准规范（skills 目录下 `design-draft-generator/SKILL.md`，供其读取"设计纲领"+"质量标准"两节）；输出：`reports/Review_Design_[主题]_[YYYYMMDD].md`；返回：报告路径 + 结论摘要 |
| 阶段 4 视觉检视 | 读 skills 目录下 `visual-reviewer/SKILL.md` 并严格执行；输入：DEMO 路径、测试环境访问方式、已确认的检视页面清单、PRD 路径（适配范围核对时参考）；输出：`reports/Review_Visual_[主题]_[YYYYMMDD].md`；返回：报告路径 + P0/P1/P2 统计 |
| 阶段 4 功能检视 | 读 skills 目录下 `functional-reviewer/SKILL.md` 并严格执行；输入：`docs/FuncChecklist_*.md` 路径、DEMO 路径、PRD 路径、测试环境访问方式、已确认的用例清单；输出：`reports/Review_Func_[主题]_[YYYYMMDD].md`；返回：报告路径 + 通过率 + ❌ 统计 |

## 交接物清单（全程维护）

从阶段 1 开始在对话中维护，每阶段结束更新，交付摘要即其最终态：

| 交接物 | 登记时机 | 内容 |
|--------|---------|------|
| PRD 路径 + 版本 | 阶段 1 完成 | `docs/PRD_[主题]_[YYYYMMDD].md`，vX.Y |
| 功能编号索引 | 阶段 1 完成 | PRD-001 名称 / PRD-002 名称…（阶段 2-3 的可溯基准） |
| 路由判定记录 | 阶段 2 判定后 | 路由 A/B + 判定依据（含用户对"是否使用设计系统"的答复） |
| 预消化材料 / 简报 | 阶段 2 确认后 | 楼层规划 + 组件清单（A）；设计简报（B） |
| 设计稿路径 | 阶段 2 完成 | `design/prototype_*.html` 清单 |
| 评审报告 | 阶段 3 完成 | `reports/Review_Design_[主题]_[YYYYMMDD].md` 路径 + 7 项检查结果 + 修复记录 |
| 回填位置 | 阶段 3 完成 | PRD 章节 + 回填内容摘要 |
| 测试环境信息 | 阶段 4 开始 | 访问方式（URL 实测 / 材料包）+ 覆盖范围 |
| 检视报告 | 阶段 4 完成 | `reports/Review_Visual_*` + `Review_Func_*` 路径、问题终态、遗留风险 |
| PRD 修订记录 | 阶段 4 收尾 | 功能检视回传的冲突/模糊项修订情况 |

**上下文压缩安全网**：交接物清单是工作流的单一事实来源。会话中断或上下文压缩后，凭清单 + 磁盘文件即可恢复现场，继续未完成的阶段。

## 中断恢复

会话恢复时按序检查，从第一个未完成处继续（评审进度一律凭磁盘报告判断，subagent 的口头返回不作依据）：
1. `docs/` 是否有对应 PRD 且用户已确认？（无 → 阶段 1）
2. `design/` 是否有对应设计稿？（无 → 阶段 2）
3. `reports/Review_Design_*` 是否存在且 ❌ 全部关闭？PRD 是否已回填？（否 → 阶段 3；报告存在但有 ❌ → 主对话修复后派发复检）
4. 用户是否宣布开发完成、测试环境就绪？`reports/Review_Visual_*` 是否存在且视觉 P0/P1 全部关闭？（否 → 阶段 4，从视觉检视开始）视觉已通过后，`Review_Func_*` 是否存在且 ❌ 全部关闭？（否 → 继续阶段 4 的功能检视）

## 与其他 skill 的关系

- **requirement-doc-generator**：阶段 1 执行者（主对话），产出 PRD；也可独立使用（用户只要 PRD 时）
- **design-draft-generator**：阶段 2 执行者（主对话），消费 PRD 并产出设计稿；也可独立使用（用户已有 PRD 时）
- **adversarial-reviewer**：阶段 3 执行者（subagent 派发），对设计稿做 7 项对抗评审；也可独立使用（用户已有设计稿只要评审时）
- **visual-reviewer**：阶段 4 · 视觉一致性测试员（subagent 派发，测试环境 vs DEMO 还原度）；也可独立使用
- **functional-reviewer**：阶段 4 · 功能测试员（subagent 派发，设计师清单主基准的关键流程走查）；也可独立使用
- **opendesign-design**：路由 A 的最终执行者，由 design-draft-generator 委托，本 skill 不直接交互

五个 skill 可独立调用，也可由本 skill 编排为端到端流水线（阶段 1-2 主对话执行、阶段 3-4 subagent 派发）；独立使用时不强制经过本工作流。
