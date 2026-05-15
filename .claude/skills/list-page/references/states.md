# ListPage 变体索引

fileKey: `VjnKgyA71h6uJxJCSTB6NS`  
根节点：`500:19702`（ListPage，x=4686, y=-5043, 1600×2539）

## 变体列表

| 变体名 | nodeId | componentKey | 尺寸 | 说明 |
|--------|--------|--------------|------|------|
| Type=Basic | `500:20999` | `f5e0c38a11916d6a1b3f246999e7e69882b95b29` | 1440×784 | 基础列表页，无 Tab，顶部标题 + 操作按钮 + 数据表格 |
| Type=With tab | `500:20948` | `6bfcb0c93bcdbeb802eb7b980b412e86a68d9af5` | 1440×834 | 带 Tab 切换的列表页，Tab 栏 + 数据表格，适合多分类资源 |

## 区域结构（两个变体共有）

| 区域 | 内容 |
|------|------|
| TopNavigation | 全局导航栏（共享组件 525:17195） |
| Page Header | 页面标题 + 操作按钮（Create/Filter 等） |
| Table | 数据表格（列头 + 数据行 + 分页） |
| Footer | Copyright（共享组件 90:1433） |

## 差异点

| 区域 | Basic | With tab |
|------|-------|----------|
| Tab 栏 | ✗ | ✓（多个分类 Tab） |
| 画布高 | 784 | 834 |
