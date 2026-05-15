# Create Page 意图映射表

fileKey: `VjnKgyA71h6uJxJCSTB6NS`

## 中文 → 变体

| 关键词 / 用户句子 | 变体 | nodeId | 画布高 |
|------------------|------|--------|--------|
| 完整创建 / 含定价 / 购买页 / 下单页 / 带 Pricing / 云主机创建 / 默认创建 | Type=Default | `476:12766` | 2670 |
| 解决方案创建 / Solution 创建 / solution 页 | Type=Solution | `552:23529` | 1260 |
| 简单创建 / 快速创建 / 轻量创建 / 配置少 / 字段简单 / 普通创建 | Type=Normal | `1075:19793` | 1020 |

## 英文 → 变体

| Keywords | Variant | nodeId |
|----------|---------|--------|
| full create, with pricing, purchase, order, default create, VM create | Type=Default | `476:12766` |
| solution create, solution page | Type=Solution | `552:23529` |
| simple create, quick create, lightweight, minimal fields, normal create | Type=Normal | `1075:19793` |

## 模糊场景

| 用户说 | 默认 | 是否必问 |
|--------|------|----------|
| "做一个创建页" | Default | **必问** |
| "新建资源的页面" | Default | 应问（完整 / 简单） |
| "购买页" / "下单" | Default | 不问 |
| "solution 创建" | Solution | 不问 |
| "简单的创建" | Normal | 不问 |

`AskUserQuestion` 时给三个预览描述：

| 选项 | 描述 |
|------|------|
| **Default 完整创建** | 含完整配置表单 + Pricing & Summary 定价区域。适合云主机、网络等需要选择付费模式的产品。画布 2670 高。 |
| **Solution 解决方案** | 中等复杂度的创建页，适合 Solution 类产品，无独立 Pricing 区域。画布 1260 高。 |
| **Normal 轻量创建** | 字段少、结构简单的创建页，适合配置项少的产品。画布 1020 高。 |

## 子区域意图（仅在改写模式下使用）

| 用户说 | 区域 | 选择变体 |
|--------|------|---------|
| "展开 more settings" / "显示更多配置" | More settings | Fold=No（`745:20027`） |
| "折叠 more settings" | More settings | Fold=Yes（`745:20028`） |
| "按量付费" / "pay-as-you-go" | Pricing | Pricing model=Pay-as-you-go（`1297:12495`） |
| "订阅制" / "subscription" | Pricing | Pricing model=Subscription（`1297:12468`） |
