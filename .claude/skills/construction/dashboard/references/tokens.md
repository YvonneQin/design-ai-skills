# Dashboard 设计 Tokens

从 Dashboard 根节点 `7230:48098` 通过 `get_variable_defs` 提取。复制到 DESIGN.md 时**原样保留斜杠命名**。

> 与 Login 的 token 集**基本一致**，但 Dashboard 额外引入了 `Padding/M`、`Padding/S`、`Radius/M`、`Caption/C 12_h16_B`、`Headline/H3 34_h36_B`、`Elevation/2F`、`Elevation/5F`。

## 颜色 — 品牌

| Token | 值 | 用途 |
|-------|----|----|
| `Primary/High` | `#1863ff` | 主 CTA 背景、强调元素 |
| `Primary/Med` | `#1863ff94` | 主色悬浮 / 点击状态 |
| `Primary/Low` | `#1863ff61` | 主色弱化背景 |
| `Secondary/High` | `#01bab2` | 次品牌强调色 |

## 颜色 — 语义

| Token | 值 | 用途 |
|-------|----|----|
| `Meaning/Warning/High` | `#f6994d` | 警告图标 / 文字 |

## 颜色 — 表面

| Token | 值 | 用途 |
|-------|----|----|
| `var(--zen-design-background-bg)` | `#0a0a0f` | 页面背景 |
| `Elevation/1F` | `#0a0a0a` | 表面 elevation 1 层 |
| `Elevation/2F` | `#0e0f13` | Section 卡背景 |
| `Elevation/5F` | `#131928` | 高层级浮层 |
| `Special Surface/Down Layer` | `#0a0a0a80` | 输入框 / 下拉框边框 |
| `Special Surface/Uppon Layer` | `#596d8f47` | 浮层表面 |

## 颜色 — 文字与图标

| Token | 值 | 用途 |
|-------|----|----|
| `On Surface/Font High` | `#eaeef7` | 主文字（标题、数值） |
| `On Surface/Med` | `#eaeef7e0` | 次级文字 |
| `On Surface/Low` | `#eaeef761` | 占位 / 辅助文字 |
| `On Surface/Icon High` | `#ffffff` | 深色表面图标 |
| `On Color/Font High` | `#eaeef7` | 主色背景上的文字 |
| `On Color/Icon High` | `#ffffff` | 主色背景上的图标 |

## 颜色 — 描边

| Token | 值 | 用途 |
|-------|----|----|
| `Outline/Enabled` | `#fafcff29` | Section / Button 边框 |

## 间距 — Gap

| Token | 值 | 用途 |
|-------|----|----|
| `Gap/None` | `0` | 无间距 |
| `Gap/S` | `4` | 紧凑内联（图标 ↔ 标签、徽章内部） |
| `Gap/M` | `8` | 组件内边距 |
| `Gap/L` | `16` | Section 内部堆叠 |
| `Gap/XL` | `24` | Section 之间垂直间距 |
| `Gap/XXL` | `32` | 页面外层 padding |

## 间距 — Padding

| Token | 值 | 用途 |
|-------|----|----|
| `Padding/S` | `4` | 紧凑按钮 / 徽章内边距 |
| `Padding/M` | `8` | Tag / 中等按钮内边距 |
| `Padding/L` | `12` | 输入框 / Section 内边距 |

## 圆角

| Token | 值 | 用途 |
|-------|----|----|
| `Radius/Square` | `0` | 无圆角 |
| `Radius-N` | `0` | Square 别名 |
| `Radius/S` | `2` | 极小圆角 |
| `Radius/M` | `4` | Tag、小按钮、徽章 |
| `Radius/L` | `16` | Section 卡圆角（实际可能为 8） |

## 字体

| Token | 规格 | 典型用法 |
|-------|------|---------|
| `Headline/H1 60_h72_B` | Proxima Nova Bold 60 / 行高 72 / 字距 -0.5 | "Components" 文档标题 |
| `Headline/H3 34_h36_B` | Proxima Nova Bold 34 / 行高 36 / 字距 0 | "Hi, Zeno" / Welcome 主标题 |
| `Headline/H4 20_h24_B` | Proxima Nova Bold 20 / 行高 24 / 字距 0.18 | Section 标题（Resource Overview、Resource Map） |
| `Headline/H5 18_h20_B` | Proxima Nova Bold 18 / 行高 20 / 字距 0.15 | 子区域标题 |
| `Subtitle/S1-1 16_h20_B` | Proxima Nova Bold 16 / 行高 20 / 字距 0.15 | 卡片标题 |
| `Subtitle/S2 14_h18_B` | Proxima Nova Bold 14 / 行高 18 / 字距 0.10 | 次级标题 |
| `Body/B1 16_h20_Re` | Proxima Nova Regular 16 / 行高 20 / 字距 0.5 | 描述文字 |
| `Body/B2-1 14_h20_Re` | Proxima Nova Regular 14 / 行高 20 / 字距 0 | 列表项文字 |
| `Button/BT1 16_h20_B` | Proxima Nova Bold 16 / 行高 20 / 字距 1 | 主按钮文字 |
| `Button/BT2 14_h16_B` | Proxima Nova Bold 14 / 行高 16 / 字距 1 | 次按钮文字、导航文字 |
| `Caption/C 12_h16_Re` | Proxima Nova Regular 12 / 行高 16 / 字距 0 | 辅助说明、Instances 计数 |
| `Caption/C 12_h16_B` | Proxima Nova Bold 12 / 行高 16 / 字距 0.4 | Tag 文字、徽章数字 |

## 备注

- 字体家族：**Proxima Nova**（全状态统一）
- 主题：**仅深色**模式
- 与 Login 相比 Dashboard 多了 `Elevation/2F`（Section 卡背景）和 `Elevation/5F`（浮层）—— 写 DESIGN.md 时要点明这两个 elevation 的语义差异
- `ΩStickersheet/Note: #cf96ff` 是设计师内部 stickersheet 颜色，**不要**写进 DESIGN.md
