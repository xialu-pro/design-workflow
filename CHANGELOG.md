# 修订记录

设计工作流（design-workflow 及各 skill）的版本演进记录。每次更新必须在此登记一条。

格式约定：`[日期] 版本号 | 修改主题`，正文写清改了什么、为什么改、影响哪些 skill。版本号规则：流程结构或判定逻辑变化 → 主版本号 +1；局部规则/模板调整 → 次版本号 +1。当前版本 **v2.3**。

---

## v2.3 · 2026-10-19 · 同步 opendesign-design@2026-10-16

- 上游（atomgit openeuler/opendesign-skills）opendesign-design 由 2026-08-18 更新至 **2026-10-16**，全量同步进仓库副本（rsync --delete，30 个文件变更），上游 CHANGELOG 随副本携带
- 同步后一致性自检通过：SKILL.md `last_update` == 其 CHANGELOG 最新条目（2026-10-16）
- 上游中间版本要点（09-24 / 10-08 / 10-10 / 10-14 / 10-15 / 10-16）：
  - **⚠️ 破坏性（存量稿子需复核）**：① 楼层模板图标引用统一为内联 SVG，`<img src>` / CSS mask 判违规（渲染不变，复核动作机械性：grep `<img` 命中即内联替换，navigation/footer 楼层为主）；② ODropdown 面板菜单项规则与顶导下拉面板规则冲突消解（B.2 收敛为通用规范）
  - **规则强化**：SVG 逐属性原样搬运硬约束（禁止仅拷 path d 重写）、规则冲突裁决条款（floors/components 与 hard-constraints 冲突时以后者为准）、渲染验证按环境能力分级（无浏览器须显式标注"未做渲染目检"）
  - **工程改进**：Token 降级数据源指向本地六主题文件（离线可用）、颜色全量语义 Token 化（深浅模式自动适配）、文生图解除厂商端点绑定
- 影响：.trae/skills/opendesign-design/（30 文件）

## v2.2 · 2026-10-19 · opendesign-design 版本保障机制

**背景**：opendesign-design skill 迭代频繁（上游 atomgit openeuler/opendesign-skills），需保障设计师走路由 A 时调用的是最新版。

**机制：仓库副本唯一真源 + 路由 A 前置版本三查**
- **单一真源**：本仓库 `.trae/skills/opendesign-design/` 是路由 A 唯一执行基准；上游更新由维护者统一同步进仓库（合并 CHANGELOG + 主 CHANGELOG 登记），成员 `git pull` 获得最新；禁止成员自行从 atomgit 另装副本（多副本 = 版本漂移）
- **版本三查**（design-draft-generator 3A 第 1 步，前置必做）：
  1. 一致性——SKILL.md `last_update` 与其 CHANGELOG 最新条目日期比对（防半截同步）
  2. 新鲜度——`git fetch` 后比对远端，落后提示 `git pull`
  3. 上游同步——WebFetch 上游 CHANGELOG 比对；上游更新建议先同步，用户坚持继续须记录风险声明；网络不可达降级注明
- 使用版本（`last_update`）与校验结论登记进交接物清单（design-workflow 路由判定记录）
- README 安装章节新增"opendesign-design 更新机制"（维护者/成员职责）

- 影响：design-draft-generator / design-workflow 的 SKILL.md + README.md

## v2.1 · 2026-10-19 · README 补同步

- 关键机制新增"交付物性质"条目（可演示原型/正式上线页面决定设计基准；素材禁止弃用另编占位）
- 阶段总览表输入来源扩为 ISSUE / DEMO Prompt 等；目录结构补列 opendesign-design/；安装章节路由 A 依赖改为仓库内路径（无需外部获取）
- 影响：README.md

## v2.0 · 2026-10-19 · 路由判定用户确认制 + PRD 双模式

**背景**：两个实际运行问题——① 团队成员执行"openGauss 官网新增 Agent 专区"（属路由 A 场景）被误走路由 B，产出自由 HTML 原型；② 团队无专职产品经理，需求由 ISSUE 触发、逐步明晰，初始阶段写不出详尽 PRD。

**一、路由 A/B 判定重构**
- 判据从"已有社区/新建社区"改为"是否使用 OpenDesign 设计系统"，design-draft-generator 第 2 步必须向用户提问确认，禁止从 PRD 措辞推断
- 新建社区也可选路由 A（无线上站点时框架与用户共同确定，禁止编造）
- 判定结果（含用户答复）登记进交接物清单

**二、PRD 双模式**
- 轻量 PRD（v0.x：用户/场景/功能清单）设计前默认；详细 PRD（v1.0，7 章）在 DEMO 定稿后基于定稿 DEMO 反向生成（补全交互/视觉细节），同文件升级、功能编号不变
- 复杂需求仍可详细先行；ISSUE / DEMO Prompt 均可为输入
- 阶段 3 新增 DEMO 迭代定稿环节（评审通过 ≠ 定稿）；进阶段 4 前核对 PRD 已升级 v1.0

**三、轻量模板定稿（经 HiFloat 官网需求实例校对）**
- 模板新增：交付物性质、功能描述列承载已确认素材、业界参考与调研建议注记
- 下游同步：design-draft-generator 输入表、adversarial-reviewer（演示型交互评审标准、素材如实呈现、轻量 PRD 验收标准暂缺不判 ❌）
- 附 HiFloat 官网轻量 PRD（v0.1）作为新模板首个实例

- 影响：design-workflow / design-draft-generator / requirement-doc-generator / adversarial-reviewer 的 SKILL.md + README.md
- 提交：`1f9ad61`

## v1.4 · 2026-10-19 · 依赖入库与仓库策略

- opendesign-design skill 全量入库（102 文件：设计规范 + 26 组件规范 + componentKey 索引 + SVG 资产）——此前仅存在于本机，成员克隆仓库即可执行路由 A
- SkillHub PRD 回填原型路径（阶段 3 闭环产物补提交）
- .gitignore：试运行期间 design/、reports/ 本地留痕不入库，流程稳定后再挑成套典型样例入库
- 提交：`6e7f9f6`、`7f0f10b`

## v1.3 · 2026-09 上旬 · 评审机制强化

- 生成后强制对抗评审：7 项检查表 + 角色切换（生成者变评审者）+ 不可修复项显式声明
- 质量标准新增：多方案信息架构统一、开发者调性约束（禁营销风）、text_to_image 语义图（禁占位色块）
- 提交：`e7354a4`、`3cfa62e`

## v1.2 · 2026-09 初 · 双检视角色

- 新增 visual-reviewer、functional-reviewer；阶段 4 双路径串行（视觉 P0/P1 关闭 → 功能检视）
- functional-reviewer：清单主基准、⛔ 阻塞态、PRD 验收标准交叉校验
- 提交：`fd36ab3`、`f89b365`、`51f0e48`、`f3fe404`

## v1.1 · 2026-08 下旬 · 流水线串联

- design-workflow 编排器上线，阶段 0-4 串联；新增 design-draft-generator 对抗评审角色、新社区简报模板
- 提交：`1259f8b`、`a326cbc`、`5546655`

## v1.0 · 2026-08 · 初始版本

- requirement-doc-generator（PRD 生成）+ design-draft-generator（设计稿生成）两个基础 skill，附 openEuler SkillHub PRD 样例
- 提交：`0c2a5d6`、`0881e46`
