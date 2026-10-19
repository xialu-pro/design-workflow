# opendesign-design

OpenDesign HTML 页面生成 Skill — 组件化设计规范到 HTML 页面的生产指南。

## 目录结构

```
opendesign-design/
├── SKILL.md                          ← 总调度入口（元数据 + 路由表）
├── CHANGELOG.md                      ← 更新日志
├── README.md                         ← 本文件
├── components/                       ← 27 个组件设计规范（每个组件一个 .md）
├── floors/                           ← 7 个楼层模板（每个楼层一个 .md）
├── global/
│   ├── hard-constraints.md           ← 变量映射硬约束 + 组件组合规则（唯一真源）
│   ├── interaction-rules.md          ← 通用交互规范（速查副本，真源 hard-constraints）
│   ├── layout-rules.md               ← 栅格 / 断点 / 间距速查（速查副本，真源 hard-constraints）
│   ├── component-doc-spec.md         ← 组件文档编写规范（Token 匹配性要求）
│   └── qa-checklist/
│       └── component-html-qa-checklist.md  ← HTML 实现质量保证清单
└── references/
    ├── designer-guide.md             ← 设计师使用指南
    ├── examples/                     ← HTML 示例文件
    └── assets/                       ← SVG 图标资源（按组件名分子目录）
```

### 文件命名规范

- 组件规范：`components/{组件英文名小写}.md`，如 `button.md`、`data-table.md`
- 图标资源：`references/assets/{组件英文名小写}/`，如 `checkbox/Selected.svg`
- 公共图标：`references/assets/public icons/icon-{中文名}.svg`

## 按需加载策略

本 Skill 采用**分层加载**策略，避免一次性读取全部内容：

| 层级 | 目录 | 加载时机 | 说明 |
|------|------|----------|------|
| **必加载** | `global/` | 每次调用起手式 | hard-constraints.md 为设计系统"宪法"（唯一真源）；interaction-rules / layout-rules 为其速查副本 |
| **按需加载** | `components/` | 用户确认涉及组件后 | 仅读取本次需要的组件规范文档 |
| **按需加载** | `floors/` | 涉及楼层级复合组件时 | 7 个楼层模板（导航 / 导航下拉面板·平铺式 / 导航下拉面板·分类平铺式 / Banner / 卡片栅格 / 图文特色 / 页脚） |
| **纯参考** | `references/` | 需要时查阅 | SVG 资源、HTML 示例 — 不参与 AI 调度决策 |

### 典型工作流

1. **起手式**：读取 `global/hard-constraints.md`，锁定设备断点和合法取值白名单
2. **确认组件**：向用户列出本次涉及的组件清单，确认后逐一读取 `components/{name}.md`
3. **生成 HTML**：严格按规范文档和硬约束白名单生成，使用 `--o-*` 语义 Token，遵循 CodeGen 工程标准
4. **查阅参考**：需要图标时从 `references/assets/` 读取 SVG 资源

## 与 CodeGen skill 的关系

- **opendesign-design**（本 skill）：生成 HTML 设计原型，遵循设计规范与 CodeGen 工程标准
- **opendesign-codegen**：将设计意图直接转化为 Vue 3 + OpenDesign 生产代码
- 两者共用同一套 Token 体系（`--o-*` 语义变量），确保从 HTML 原型到 Vue SFC 的顺畅转译

## 开源协议与维护信息

- **开源协议**：MulanPSL-2.0（木兰宽松许可证第 2 版）
- **版本**：见 SKILL.md frontmatter `last_update` 字段
- **维护团队**：openEuler OpenDesign SIG
- **联系方式**：opendesign@openeuler.org | [SIG 仓库](https://atomgit.com/openeuler/opendesign-token)

## 后续规划

- [ ] 补充 OTooltip / OPopover / ODrawer 等组件设计规范
- [ ] 深色模式（Dark=on）全组件规范覆盖
- [ ] 图标资源扩展至完整图标库

## 快速链接

- [SKILL.md](SKILL.md) — 总调度入口与完整工作流
- [global/hard-constraints.md](global/hard-constraints.md) — 硬性约束（最高优先级）
- [global/component-doc-spec.md](global/component-doc-spec.md) — 组件文档编写规范
- [CHANGELOG.md](CHANGELOG.md) — 更新日志