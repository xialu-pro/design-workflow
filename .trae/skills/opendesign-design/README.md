# opendesign-design

OpenDesign Pixso 设计系统 Skill — 组件化设计稿生产指南。

## 目录结构

```
opendesign-design/
├── SKILL.md                          ← 总调度入口（元数据 + 路由表）
├── CHANGELOG.md                      ← 更新日志
├── README.md                         ← 本文件
├── components/                       ← 26 个组件设计规范（每个组件一个 .md）
├── global/
│   ├── hard-constraints.md           ← 变量映射硬约束 + 组件组合规则（唯一真源）
│   ├── component-doc-spec.md         ← 组件文档编写规范（Token 匹性要求）
│   └── qa-checklist/
│       └── component-html-qa-checklist.md  ← HTML 实现质量保证清单
└── references/
    ├── pixso-mcp-adapter/
    │   ├── component-keys.md         ← 536 个 UI 组件变体 componentKey 索引
    │   └── icon-keys.md              ← 187 个图标 componentKey 索引
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
| **必加载** | `global/` | 每次调用起手式 | 硬约束、栅格规范、交互规则 — 设计系统"宪法" |
| **按需加载** | `components/` | 用户确认涉及组件后 | 仅读取本次需要的组件规范文档 |
| **按需加载** | `floors/` | 涉及楼层级复合组件时 | 暂无内容，预留扩展 |
| **纯参考** | `references/` | 需要时查阅 | 索引、SVG 资源、HTML 示例 — 不参与 AI 调度决策 |

### 典型工作流

1. **起手式**：读取 `global/hard-constraints.md`，锁定设备断点和合法取值白名单
2. **确认组件**：向用户列出本次涉及的组件清单，确认后逐一读取 `components/{name}.md`
3. **生成设计稿**：严格按规范文档和硬约束白名单生成，禁止自定义任何视觉属性
4. **查阅参考**：需要图标时查 `references/pixso-mcp-adapter/icon-keys.md`，需要 SVG 时从 `references/assets/` 读取

## 开源协议与维护信息

- **开源协议**：MulanPSL-2.0（木兰宽松许可证第 2 版）
- **版本**：见 SKILL.md frontmatter `last_update` 字段
- **维护团队**：openEuler OpenDesign SIG
- **联系方式**：opendesign@openeuler.org | [SIG 仓库](https://atomgit.com/openeuler/opendesign-token)

## 后续规划

- [ ] 补充 OTooltip / OPopover / ODrawer 等组件设计规范
- [ ] 深色模式（Dark=on）全组件规范覆盖
- [ ] 落地页模板楼层（floors/）规范文档化
- [ ] 图标资源从 187 扩展至完整图标库
- [ ] Pixso MCP 工具链升级后适配新版 API

## 快速链接

- [SKILL.md](SKILL.md) — 总调度入口与完整工作流
- [global/hard-constraints.md](global/hard-constraints.md) — 硬性约束（最高优先级）
- [global/component-doc-spec.md](global/component-doc-spec.md) — 组件文档编写规范
- [references/pixso-mcp-adapter/component-keys.md](references/pixso-mcp-adapter/component-keys.md) — 组件变体索引
- [references/pixso-mcp-adapter/icon-keys.md](references/pixso-mcp-adapter/icon-keys.md) — 图标索引
- [CHANGELOG.md](CHANGELOG.md) — 更新日志