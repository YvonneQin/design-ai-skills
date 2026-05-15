# Detail Page 意图映射表

fileKey: `VjnKgyA71h6uJxJCSTB6NS`

## 中文 → 变体

| 关键词 / 用户句子 | 变体 | nodeId |
|------------------|------|--------|
| 详情页 / 资源详情 / 实例详情 / 查看详情 / detail page / 详情 / 详情视图 | Detail（主页面） | `474:9320` |

## 英文 → 变体

| Keywords | Variant | nodeId |
|----------|---------|--------|
| detail page, resource detail, instance detail, detail view | Detail | `474:9320` |

## 模糊场景

| 用户说 | 处理 |
|--------|------|
| "做一个详情页" | 直接用 Detail（`474:9320`），无需问 |
| "资源详情" | 直接用 Detail |
| "订单详情" | 优先用 Detail，可在改写模式下替换 Information-Order 子区域 |

> Detail 目前只有一个主变体，不需要 AskUserQuestion。如果用户提到具体订单计费模型（预付费/后付费/承诺期），在改写模式下按 states.md 里的子区域变体组合。

## 流程意图

| 用户描述 | 处理方式 |
|---------|---------|
| "列表页 → 详情页" | 先调 `/list-page`，完成后本 skill 在其右侧放 Detail，画箭头 "查看详情" |
| "登录 → 列表 → 详情" | 依次调 `/login` → `/list-page` → 本 skill，每步画连线箭头 |
