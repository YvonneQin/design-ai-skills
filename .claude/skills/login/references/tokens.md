# Login 设计 Tokens

从 Login 根节点 `532:16740` 通过 `get_variable_defs` 提取。复制到 DESIGN.md 时**原样保留斜杠命名**。

## 颜色 — 品牌

| Token | 值 | 用途 |
|-------|----|----|
| `Primary/High` | `#1863ff` | 主 CTA 背景（如 "Sign up with Email"） |
| `Primary/Med` | `#1863ff94` | 主 CTA 悬浮 / 点击叠加层 |
| `Primary/Low` | `#1863ff61` | 主色弱化背景 |
| `Primary/Text` | `#427eff` | 内联文字链接（如 "Sign up"、"Forgot password?"） |
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
| `Elevation/3F` | `#11131a` | Banner / 卡片背景 |
| `Special Surface/Down Layer` | `#0a0a0a80` | 输入框 / 下拉框边框 |
| `Special Surface/Uppon Layer` | `#596d8f47` | 浮层表面 |

## 颜色 — 文字与图标（On-Surface）

| Token | 值 | 用途 |
|-------|----|----|
| `On Surface/Font High` | `#eaeef7` | 深色表面上的主文字 |
| `On Surface/Med` | `#eaeef7e0` | 次级文字 / 表单输入值 |
| `On Surface/Low` | `#eaeef761` | 禁用 / 占位符 / 页脚文字 |
| `On Surface/Icon High` | `#ffffff` | 深色表面上的图标 |
| `On Color/Font High` | `#eaeef7` | 主色背景上的文字 |
| `On Color/Icon High` | `#ffffff` | 主色背景上的图标 |
| `States/White/High` | `#ffffff` | 纯白态颜色 |

## 颜色 — 描边

| Token | 值 | 用途 |
|-------|----|----|
| `Outline/Enabled` | `#fafcff29` | 默认输入 / 按钮边框 |
| `Outline/Disabled` | `#fafcff14` | 禁用按钮的边框 / 填充 |

## 间距 — Gap

| Token | 值 | 用途 |
|-------|----|----|
| `Gap/None` | `0` | 无间距 |
| `Gap/Number` | `0` | None 别名 |
| `Gap/S` | `4` | 紧凑内联（图标 ↔ 标签） |
| `Gap/M` | `8` | 组件内边距 |
| `Gap/L` | `16` | 表单字段间堆叠间距 |
| `Gap/XL` | `24` | TopNavigation 水平内边距 |
| `Gap/XXL` | `32` | 页脚链接间距 |

## 间距 — Padding

| Token | 值 | 用途 |
|-------|----|----|
| `Padding/L` | `12` | 输入框 / 按钮水平内边距 |

## 圆角

| Token | 值 | 用途 |
|-------|----|----|
| `Radius/Square` | `0` | 无圆角 |
| `Radius-N` | `0` | Square 别名 |
| `Radius/S` | `2` | 小圆角（很少用） |
| `Radius/L` | `16` | 标准控件圆角（注：Figma 变量名为 16，但部分组件实际渲染为 8px —— 逐节点核对） |
| `Radius-L` | `16` | 别名 |

## 字体

| Token | 规格 |
|-------|------|
| `Headline/H1 60_h72_B` | Proxima Nova Bold 60 / 行高 72 / 字距 -0.5 |
| `Headline/H2 48_h56_B` | Proxima Nova Bold 48 / 行高 56 / 字距 0 |
| `Headline/H4 20_h24_B` | Proxima Nova Bold 20 / 行高 24 / 字距 0.18 |
| `Headline/H5 18_h20_B` | Proxima Nova Bold 18 / 行高 20 / 字距 0.15 |
| `Subtitle/S1-1 16_h20_B` | Proxima Nova Bold 16 / 行高 20 / 字距 0.15 |
| `Subtitle/S2 14_h18_B` | Proxima Nova Bold 14 / 行高 18 / 字距 0.10 |
| `Body/B1 16_h20_Re` | Proxima Nova Regular 16 / 行高 20 / 字距 0.5 |
| `Body/B2-1 14_h20_Re` | Proxima Nova Regular 14 / 行高 20 / 字距 0 |
| `Button/BT1 16_h20_B` | Proxima Nova Bold 16 / 行高 20 / 字距 1 |
| `Button/BT2 14_h16_B` | Proxima Nova Bold 14 / 行高 16 / 字距 1 |
| `Caption/C 12_h16_Re` | Proxima Nova Regular 12 / 行高 16 / 字距 0 |

## 备注

- 字体家族：**Proxima Nova**（全状态统一）
- 主题：**仅深色**模式，本 Login 没有浅色版本
- 部分 token 重复（`Radius/L` 与 `Radius-L`）—— DESIGN.md 优先用斜杠形式 `Radius/L`
- `ΩStickersheet/Note: #cf96ff` 是设计师内部 stickersheet 颜色，**不要**写进 DESIGN.md
