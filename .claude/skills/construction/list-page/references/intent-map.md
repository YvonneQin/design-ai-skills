# ListPage 意图映射表

fileKey: `VjnKgyA71h6uJxJCSTB6NS`

## 中文 → 变体

| 关键词 / 用户句子 | 变体 | nodeId | 画布高 |
|------------------|------|--------|--------|
| 带 tab / tab 切换 / 分类列表 / 多类型资源 / 筛选 tab | Type=With tab | `500:20948` | 834 |
| 基础列表 / 简单列表 / 无 tab / 纯列表 / 资源列表 / 实例列表 | Type=Basic | `500:20999` | 784 |

## 英文 → 变体

| Keywords | Variant | nodeId |
|----------|---------|--------|
| with tab, tabbed list, categorized list, multi-type | Type=With tab | `500:20948` |
| basic list, simple list, no tab, resource list, instance list | Type=Basic | `500:20999` |

## 模糊场景

| 用户说 | 默认 | 是否必问 |
|--------|------|----------|
| "做一个列表页" | Basic | **必问** |
| "资源管理页" | Basic | 应问（是否需要 Tab） |
| "实例列表" | Basic | 不问 |
| "带分类的列表" | With tab | 不问 |

`AskUserQuestion` 时给两个预览描述：

| 选项 | 描述 |
|------|------|
| **Basic 基础列表** | 顶部标题 + 操作按钮 + 数据表格，无 Tab 切换。适合单一资源类型的管理页。画布 784 高。 |
| **With tab 分类列表** | Tab 栏 + 数据表格，支持多分类资源切换。适合有多种子类型的资源管理页。画布 834 高。 |
