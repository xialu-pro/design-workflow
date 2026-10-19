> ← [SKILL 主索引](../SKILL.md)

# 组件文档规范

生成组件设计规范文档（`skills/opendesign-design/components/{name}.md`）时，**必须**将设计稿中的色值、字号、间距等匹配上游 Token 的变量名（数据从 SKILL.md [#数据资源] 中的远端 URL 拉取）：

## 必须匹配的属性

| 属性类型 | Token 格式 | 示例 |
|---|---|---|
| 颜色 | `color-*` | `color-info3`、`color-primary1`、`color-link1` |
| 字号 | `font_size-*` | `font_size-tip1` (14px) |
| 行高 | `line_height-*` | `line_height-tip1` (22px) |
| 字重 | `font_weight-*` | `font_weight-regular` (400)、`font_weight-bold` (600) |
| 间距 | `gap-*` | `gap-1` (4px)、`gap-2` (8px) |
| 圆角 | `radius_control-*` | `radius_control-m` (4px) |
| 图标尺寸 | `icon_size-*` / `icon_size_control-*` | `icon_size-m` (24px) |
| 阴影 | `shadow-*` | `shadow-1` |

## 颜色 Token 对照表

| 语义 | Token | 用途 |
|---|---|---|
| 一级文字/标题 | `color-info1` | 强调信息 |
| 二级文字/正文 | `color-info2` | 次强调 |
| 三级文字/辅助 | `color-info3` | 辅助信息、面包屑层级 |
| 禁用文字 | `color-info4` | 禁用状态 |
| 链接文字 | `color-link1` | 链接常规状态 |
| 强调色/主色 | `color-primary1` | 当前页面、激活状态 |
| 成功色 | `color-success1` | 成功提示 |
| 告警色 | `color-warning1` | 告警提示 |
| 危险色 | `color-danger1` | 错误/危险提示 |
| 控件边框 | `color-control1` | 输入框边框等 |

## 文档格式要求

样式规范表格中必须包含 Token 列：

```markdown
### 颜色

| 元素 | Token | Dark=off | Dark=on |
|---|---|---|---|
| 文字 | `color-info3` | `rgba(var(--o-grey-14), 0.6)` | `rgba(var(--o-grey-1), 0.6)` |
```