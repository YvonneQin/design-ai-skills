# Dashboard 变体索引

Zenlayer zenConsole Figma 文件里全部 2 个 Dashboard 变体。fileKey: `VjnKgyA71h6uJxJCSTB6NS`。

每一行对应一次 `get_design_context` 调用（响应必然超大，按 SKILL.md 流程交给 subagent 解析）。

## 变体（2 个）

| 变体 | nodeId | componentKey | 画布 | 定位 |
|------|--------|--------------|------|------|
| Type=Type2 | `7230:48104` | `70551c84495fd189569afc8ca2a583a017e5c930` | 1440 × 1736 | **详细版**：Resource Overview 展开为 12 个产品分类卡，每卡列出 Instances 数量与跳转链接 |
| Type=Basic | `7230:48694` | `9e97cb3fbc49de8f6ccce89a5e05509fadb0c006` | 1440 × 1104 | **紧凑版**：Resource Overview 横排 8 个 Product Icon 卡 + 促销 Banner |

## 区域顺序（两个变体一致）

1. **Welcome** —— "Hi, Zeno / Welcome 👋"
2. **Resource Overview** —— 变体之间差异最大的区域
3. **Resource Map** —— "Global Fabric" 暗色地图 + 4 项统计（290+ / 50+ Tbps / 10000+ / 110+）+ Looking glass 列表
4. **Get Started** —— Compute / Cloud Networking / Global Accelerator 三栏行动卡

## 仅 Type2 出现的组件

| 组件 | 说明 |
|------|------|
| `Link` (`7230:44365`) | 子卡里的"View"跳转链接 |
| `Direction/TriangleLine-Right 右` | 链接箭头 |
| `Direction/Double Down 快速下` | 地图标点 hover 指向 |
| `Cursor` | 地图标点光标 |
| `Resource Overview状态 / 状态-有资源` | Type2 专有容器 |
| `Dashboard`（子模块） | 12 个分类卡的基础模板 |

## 仅 Basic 出现的元素

| 元素 | 说明 |
|------|------|
| 促销 `Button` | "99￠/mo · Hong Kong" |
| 促销 `Tag` | "Limited from: Nov 15, 2023, to Dec 31, 2023" |
| 8 个紧凑 Product Icon 卡 | Bare Metal Cloud / ZCS Virtual Machine / Virtual Machine / Managed Instance / Private Connect L2 / Cloud Router L3 / Elastic IP / Global Accelerator |

## 共享组件（两个变体都有）

| 组件 | nodeId | 用途 |
|------|--------|------|
| `TopNavigation` | `525:17195` | 顶栏（高 56） |
| `zenlayerLogo` | `90:1155` | 品牌 logo |
| `Button` | `192:3022` | 主 / 次按钮 |
| `Divider` | `112:3830` | 分隔线 |
| `DropDown` | `320:4296` | 下拉 / 选择器 |
| `Input` | `196:4613` | 搜索输入框 |
| `Tag` | `160:615` | 状态 / 促销标签 |
| `MenuItem` | `163:1830` | Looking glass / Get Started 列表项 |
| `Product Icon` | —— | 产品图标 |
| `DarkModeMap 1` | —— | Resource Map 暗色地图背景 |
| `Network/ZGA-Testing Tools 测速工具` | `68:131` | Looking glass 区图标 |
| `Section` | —— | 卡片容器 |
| `ListPage` | —— | 整页根容器 |
| `CopyRight` | `90:1433` | 页脚版权 |

## 读取策略

两个变体可以**并行** `get_design_context`，但两者响应都会超大；建议：

1. 一次性发起两个并行调用，预期得到两份临时文件路径
2. 派一个 `general-purpose` subagent 同时解析两个文件
3. 等 subagent 返回结构摘要再合成 DESIGN.md
