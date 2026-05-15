# Create Page 变体索引

fileKey: `VjnKgyA71h6uJxJCSTB6NS`
根节点：`450:13003`（Create Page，x=13541, y=-5043, 2799×4907）

## 主变体

| 变体名 | nodeId | 尺寸 | 说明 |
|--------|--------|------|------|
| Type=Default | `476:12766` | 1440×2670 | 完整创建页，含配置表单 + Pricing & Summary 区域，适合需要定价选择的产品（如云主机） |
| Type=Solution | `552:23529` | 1440×1260 | 解决方案创建页，中等复杂度，适合 Solution 类产品 |
| Type=Normal | `1075:19793` | 1440×1020 | 轻量创建页，字段少，适合配置项简单的产品 |

## 子区域变体（仅 Default 使用）

| 区域 | 变体 | nodeId | 说明 |
|------|------|--------|------|
| More settings | Fold=Yes | `745:20028` | 折叠状态，只显示标题行 |
| More settings | Fold=No | `745:20027` | 展开状态，显示额外配置项 |
| Pricing & Summary | Pricing model=Pay-as-you-go | `1297:12495` | 按量付费定价模式 |
| Pricing & Summary | Pricing model=Subscription | `1297:12468` | 订阅制定价模式 |

## 区域结构（三个变体共有）

| 区域 | 内容 |
|------|------|
| TopNavigation | 全局导航栏（共享组件 525:17195） |
| Page Header | 页面标题（如 "Create Virtual Machine"） |
| Form | 配置表单（Region、Spec、OS 等字段） |
| Footer / Action Bar | 提交按钮（Create / Cancel） |
| Copyright | 底部版权（共享组件 90:1433） |
