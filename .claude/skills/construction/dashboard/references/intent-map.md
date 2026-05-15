# Dashboard 意图映射表

把用户那句话映射到 Type2 还是 Basic 变体。fileKey: `VjnKgyA71h6uJxJCSTB6NS`。

## 中文 → 变体

| 关键词 / 用户句子 | 变体 | nodeId | 画布高 |
|------------------|------|--------|--------|
| 详细版 / 展开版 / 完整 dashboard / 多资源用户 / 全资源 / 老用户首页 | Type=Type2 | `7230:48104` | 1736 |
| 紧凑版 / 概览版 / 简版 / 新用户首页 / 资源稀少 / 促销首页 / 极简 dashboard | Type=Basic | `7230:48694` | 1104 |

## 英文 → 变体

| Keywords | Variant | nodeId |
|----------|---------|--------|
| detailed, expanded, full dashboard, resource-rich, power user home | Type2 | `7230:48104` |
| compact, basic, simple dashboard, overview, new user home, promo banner | Basic | `7230:48694` |

## 模糊场景

| 用户说 | 默认 | 是否必问 |
|--------|------|----------|
| "做一个 dashboard" | Basic（新用户视角更通用） | **必问** |
| "登录后的首页" | Basic | 应问场景（新 vs 老用户） |
| "Zenlayer 主页" | 不明确，必问 | 必问 |
| "出一个资源概览" | Type2（更像"资源概览"语义） | 应问 |
| "极简版" / "简单的" | Basic | 不问 |
| "看资源用的" | Type2 | 不问 |

`AskUserQuestion` 时给两个预览描述：

| 选项 | 描述 |
|------|------|
| **Type2 详细版** | 12 个产品分类卡展开，每卡列出实例数、子产品、跳转链接。适合资源多的账号。画布 1736 高。 |
| **Basic 紧凑版** | 8 个 Product Icon 横排 + 限时促销 Banner。适合新用户/资源稀少。画布 1104 高。 |

## 区域差异速查

两个变体共有的区域：

| 区域 | 内容 |
|------|------|
| Welcome | "Hi, Zeno / Welcome👋" (Headline/H3) |
| Resource Map | DarkModeMap + Global Fabric 4 统计 + Looking glass 4 项 |
| Get Started | Compute / Cloud Networking / Global Accelerator 三栏 |

差异点：

| Resource Overview | Type2 | Basic |
|------------------|-------|-------|
| 布局 | 12 个分类卡展开 | 8 个 Product Icon 横排 |
| 内含资源详情 | ✓（每卡列 Instances + View 链接） | ✗（只有图标 + 数量） |
| 促销位 | ✗ | ✓（限时 Button + Tag） |

## 流程意图（上游来自 /login）

当本 skill 被 `/login` 流程模式调用时，**默认 Basic**（登录后首屏对新用户最友好），除非用户明确指定 Type2。

布局：
- Login 新 frame 在 `x=13394`
- Dashboard 新 frame 应放在 `x=19954`（Login 右侧 +100）、`y=-5043`（同顶部基线）
- 用 `figma-use` 在两个 frame 之间画 `figma.createConnector()`，文字 "Login Success"

完整 4-frame 流程示例：

```
[Sign in / Account Info]  →  [Sign in / Result]  →  [Dashboard / Basic]
   x=13394                     x=20454              x=20454+offset
```

如果 `/login` 走的是 MFA 路径，多一个 frame：

```
[Sign in / Account Info]  →  [Sign in / MFA]  →  [Sign in / Result]  →  [Dashboard]
```
