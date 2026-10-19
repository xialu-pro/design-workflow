> ← [SKILL 主索引](../SKILL.md)

# 组件文档规范

生成组件设计规范文档（`skills/opendesign-design/components/{name}.md`）时，**必须**将设计稿中的色值、字号、间距等匹配上游 Token 的变量名（数据从 SKILL.md [#数据资源] 中的远端 URL 拉取）：

## 必须匹配的属性

| 属性类型 | Token 格式 | 示例 |
|---|---|---|
| 颜色 | `--o-color-*` | `--o-color-info3`、`--o-color-primary1`、`--o-color-link1` |
| 字号 | `--o-r-font_size-*` | `--o-r-font_size-tip1` (14px) |
| 行高 | `--o-r-line_height-*` | `--o-r-line_height-tip1` (22px) |
| 字重 | `--o-font_weight-*` | `--o-font_weight-regular` (400)、`--o-font_weight-bold` (600) |
| 间距 | `--o-gap-*` / `--o-r-gap-*` | `--o-gap-1` (4px)、`--o-gap-2` (8px) |
| 圆角 | `--o-radius-*` | `--o-radius-control-m` (4px) |
| 图标尺寸 | `--o-icon_size-*` | `--o-icon_size-m` (24px) |
| 阴影 | `--o-shadow-*` | `--o-shadow-1` |

## 颜色 Token 对照表

| 语义 | Token | 用途 |
|---|---|---|
| 一级文字/标题 | `--o-color-info1` | 强调信息 |
| 二级文字/正文 | `--o-color-info2` | 次强调 |
| 三级文字/辅助 | `--o-color-info3` | 辅助信息、面包屑层级 |
| 禁用文字 | `--o-color-info4` | 禁用状态 |
| 链接文字 | `--o-color-link1` | 链接常规状态 |
| 强调色/主色 | `--o-color-primary1` | 当前页面、激活状态 |
| 成功色 | `--o-color-success1` | 成功提示 |
| 告警色 | `--o-color-warning1` | 告警提示 |
| 危险色 | `--o-color-danger1` | 错误/危险提示 |
| 控件边框 | `--o-color-control1` | 输入框边框等 |

## 文档格式要求

样式规范表格中必须包含 Token 列：

```markdown
### 颜色

| 元素 | Token | Dark=off（浅色） | Dark=on（深色） |
|------|-------|-----------------|-----------------|
| 导航层级文字 | `--o-color-info3` | `rgb(var(--o-grey-14) / 0.6)` | `rgb(var(--o-grey-1) / 0.6)` |
```

> ⚠️ **半透明 Token 写法（强制）**：调色板变量存的是「空格分隔通道值」（如 `--o-grey-14: 0 0 0`），拼接透明度时**只能**用现代斜杠语法 `rgb(var(--o-grey-14) / 0.6)`。
>
> - ✅ 合法：`rgb(var(--o-grey-14) / 0.6)`（空格通道 + 斜杠 alpha，随主题翻转）
> - ✅ 合法：`rgba(0, 0, 0, 0.6)`（写死完整值，逗号分隔）
> - ❌ 非法：`rgba(var(--o-grey-14), 0.6)` —— 展开为 `rgba(0 0 0, 0.6)`，空格通道与逗号 alpha 混用，浏览器整条声明丢弃，会导致描边/阴影/文字色等静默失效。