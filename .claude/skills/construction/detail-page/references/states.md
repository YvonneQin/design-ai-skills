# Detail Page 变体索引

fileKey: `VjnKgyA71h6uJxJCSTB6NS`
根节点：`442:5903`（Detail 文档 frame，x=-746, y=-5043, 2852×3172）

## 主变体

| 变体名 | nodeId | 尺寸 | 说明 |
|--------|--------|------|------|
| Detail（主页面） | `474:9320` | 960×2221 | 完整资源详情页，含面包屑 + 标题 + Tab + 图表 + 数据表格 |

## 区域结构

| 区域 | 组件 | 说明 |
|------|------|------|
| TopNavigation | 共享组件 525:17195 | 全局导航栏 |
| Bread crumbs | `474:8081` | 路径导航，如 Products / Virtual Machine / vm-001 |
| Headline | 实例 | 资源名称 + 状态 Badge + 操作按钮 |
| Tab | 实例 | 多 Tab 切换（Overview / Monitoring / Config 等） |
| Chart | 实例 `494:17515` | 监控图表（折线图 + 时间范围选择 + 数据概览） |
| Table | Frame | 关联资源/操作记录表格 |
| Copyright | 共享组件 90:1433 | 底部版权 |

## 子区域组件（改写模式可单独使用）

| 组件名 | nodeId | 尺寸 | 用途 |
|--------|--------|------|------|
| Detail Item/Bread crumbs | `474:8081` | 692×32 | 路径导航 |
| Detail Item/Information | `497:18602` | 912×144 | 基本信息区块（6 个字段） |
| Detail Item/Change Password | `497:19197` | 456×192 | 修改密码区块 |
| Detail Item/Information-Order（预付费-多计费） | `497:18834` | 912×156 | 订单信息：多个计费模型 |
| Detail Item/Information-Order（预付费-单计费） | `497:18987` | 912×156 | 订单信息：单个计费模型 |
| Detail Item/Information-Order（后付费-承诺期） | `497:19016` | 912×156 | 订单信息：后付费承诺期 |
| Detail Item/Information-Order（后付费-无承诺期） | `497:19086` | 896×136 | 订单信息：后付费无承诺期 |
