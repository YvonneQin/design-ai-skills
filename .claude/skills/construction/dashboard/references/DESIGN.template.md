# Dashboard — DESIGN.md

> 数据源：[Figma · Zenlayer zenConsole Dashboard](https://www.figma.com/design/VjnKgyA71h6uJxJCSTB6NS/?node-id=7230-48098) · 根节点 `7230:48098` · 由 `.claude/skills/dashboard` 生成
>
> **主题：** 仅深色模式 · **字体：** Proxima Nova · **品牌色：** `#1863ff`

---

## 1. 概览

<!-- 一段话即可。Dashboard 解决什么问题？谁会看？什么时候出现？ -->

**适配端：** Web（默认画布宽 1440）
**变体总数：** 2（Type=Type2 / Type=Basic）
**画布高度：** Type2 1736 · Basic 1104

**布局骨架**（两个变体一致）：

```
┌──────────────────────────────────────────────────────────┐
│ TopNavigation（高 56）       Logo | Docs API EN | Account│
├──────────────────────────────────────────────────────────┤
│                                                          │
│  ┌────────────────────────────────────────────────────┐ │
│  │ Welcome（Hi, Zeno / Welcome👋）                   │ │
│  └────────────────────────────────────────────────────┘ │
│  ┌────────────────────────────────────────────────────┐ │
│  │ Resource Overview（变体差异最大）                  │ │
│  └────────────────────────────────────────────────────┘ │
│  ┌────────────────────────────────────────────────────┐ │
│  │ Resource Map（DarkModeMap + Global Fabric 统计）  │ │
│  └────────────────────────────────────────────────────┘ │
│  ┌────────────────────────────────────────────────────┐ │
│  │ Get Started（Compute / Networking / Accelerator）  │ │
│  └────────────────────────────────────────────────────┘ │
│                                                          │
├──────────────────────────────────────────────────────────┤
│ CopyRight 页脚（高 48）                                  │
└──────────────────────────────────────────────────────────┘
```

**无 Sidebar** —— 全宽单列内容流。Section 之间垂直间距 `Gap/XL`（24），外层 padding `px=32 / py=16`。

---

## 2. 设计 Tokens

<!-- 从 references/tokens.md 原样复制。按 颜色 / 间距 / 圆角 / 字体 分组。Dashboard 特有：Elevation/2F、Elevation/5F、Padding/M、Padding/S、Radius/M、Caption/C 12_h16_B、Headline/H3 34_h36_B。 -->

### 颜色
| Token | 值 |
|-------|----|
| `Primary/High` | `#1863ff` |
| `Elevation/2F` | `#0e0f13` |
| ... | ... |

### 间距
| Token | 值 |
|-------|----|
| `Gap/XL` | 24 |
| `Padding/M` | 8 |
| ... | ... |

### 字体
| Token | 规格 |
|-------|------|
| `Headline/H3 34_h36_B` | Proxima Nova Bold 34 / 行高 36 / 字距 0 |
| ... | ... |

---

## 3. 共享组件

<!-- 一行一个，✓ 表示该变体使用了此组件。 -->

| 组件 | nodeId | 用途 | Type2 | Basic |
|------|--------|------|:-:|:-:|
| `TopNavigation` | `525:17195` | 顶栏 Logo + 导航 | ✓ | ✓ |
| `ZenlayerLogo` | `90:1155` | 品牌 logo | ✓ | ✓ |
| `Button` | `192:3022` | 主 / 次按钮 | ✓ | ✓ |
| `Section` | — | Section 卡容器 | ✓ | ✓ |
| `DropDown` | `320:4296` | 下拉 / 选择器 | ✓ | ✓ |
| `Input` | `196:4613` | 搜索输入 | ✓ | ✓ |
| `Tag` | `160:615` | 状态 / 促销标签 | ✓ | ✓ |
| `Divider` | `112:3830` | 分隔线 | ✓ | ✓ |
| `MenuItem` | `163:1830` | 列表条目 | ✓ | ✓ |
| `Product Icon` | — | 产品图标 | ✓ | ✓ |
| `DarkModeMap 1` | — | 暗色地图背景 | ✓ | ✓ |
| `CopyRight` | `90:1433` | 页脚版权 | ✓ | ✓ |
| `Link` | `7230:44365` | "View" 跳转链接 | ✓ | — |
| `Direction/TriangleLine-Right` | — | 链接箭头 | ✓ | — |
| `Direction/Double Down` | — | 地图 hover 指向 | ✓ | — |
| `Cursor` | — | 地图标点光标 | ✓ | — |

---

## 4. 变体对比

| 维度 | Type=Type2 | Type=Basic |
|------|------------|-----------|
| 节点 | `7230:48104` | `7230:48694` |
| 画布高 | 1736 | 1104 |
| 定位 | 详细展开版 | 概览紧凑版 |
| Resource Overview | 12 个产品分类卡 + Instances 列表 + View 链接 | 8 个 Product Icon 横排 + 促销 Button/Tag |
| 链接跳转 | 每卡有 Link → 分类详情 | 仅整体 CTA |
| 信息密度 | 高（适合资源较多的用户） | 低（适合新用户 / 资源稀少时） |
| Welcome / Map / Get Started | 一致 | 一致 |

---

## 5. 变体目录

### 5.1 Type=Type2（详细展开版）`7230:48104`

**定位：** 适合已有较多资源的账号 —— Resource Overview 按 12 个产品分类展开，每张分类卡内列出 Instances 数量、子产品名称、跳转链接。

**主区域：**

- **Welcome** —— 同 §6.1
- **Resource Overview**（特有展开版）—— 12 张分类卡：
  1. AI Gateway
  2. MCP Gateway
  3. Compute（Bare Metal Cloud / Elastic Compute / Virtual Machine）
  4. Networking（Bandwidth Cluster / Global BGP / Cloud Networking）
  5. Accelerator
  6. Content Delivery
  7. Edge Pulse
  - 每张卡 Header 含 Product Icon + 产品名 + "View" Link
  - Body 列出子产品/资源条目，含 Instances 计数徽章
- **Resource Map** —— 同 §6.2
- **Get Started** —— 同 §6.3

**特有元素：**
- `Link` 组件（"View" 跳转）
- `Direction/TriangleLine-Right` 箭头
- 分类卡内的展开列表

**备注：** 因为信息密度高，Resource Overview 区高度约 1300，占整页大头。

---

### 5.2 Type=Basic（紧凑概览版）`7230:48694`

**定位：** 适合新用户 / 资源稀少的账号 —— Resource Overview 只是一行 8 个产品图标卡 + 一条促销 Banner。

**主区域：**

- **Welcome** —— 同 §6.1
- **Resource Overview**（紧凑横排）—— 8 个 Product Icon 卡：
  1. Bare Metal Cloud — 4 Instances
  2. ZCS Virtual Machine — 4 Instances
  3. Virtual Machine — 4 Instances
  4. Managed Instance — 4 Instances
  5. Private Connect L2 — 4 Instances
  6. Cloud Router L3 — 4 Instances
  7. Elastic IP — 4 Instances
  8. Global Accelerator — 4 Instances
  - 右侧：促销 `Button` "99￠/mo · Hong Kong" + 促销 `Tag` "Limited from: Nov 15, 2023, to Dec 31, 2023"
- **Resource Map** —— 同 §6.2
- **Get Started** —— 同 §6.3

**特有元素：**
- 促销 Button + Tag 组合（限时优惠入口）
- 横排紧凑卡片（无展开列表）

**备注：** 缺少分组与跳转，所以信息层级浅。促销位是这个变体的独有点。

---

## 6. 共有区域细节

### 6.1 Welcome

- 标题：`Hi, Zeno`（`Headline/H3 34_h36_B`）
- 副标题：`Welcome👋`
- 背景：`Elevation/2F` 或透明，外层 padding `Padding/L`
- 无 CTA

### 6.2 Resource Map

- 标题：`Resource Map`（`Headline/H4`）
- 工具栏：右侧筛选 "Only View My Resources"（DropDown）
- 主体：`DarkModeMap 1` 暗色地图背景
- 顶部覆盖："Global Fabric – Leading edge cloud service provider in emerging markets"
- 统计四元组：`290+ PoPs` · `50+ Tbps` · `10000+ Cities` · `110+ Countries`
- 地图覆盖标点：`Cursor` + `Direction/Double Down`（仅 Type2 出现展开 hover）
- 底部 Looking glass 4 条 MenuItem：
  - Last-mile Performance（`General/Address`）
  - Private Backbone Latency（`General/Effect`）
  - Public Backbone Latency（`General/Common-Search`）
  - Global Acceleration Test（`Network/ZGA-Testing Tools`）

### 6.3 Get Started

- 标题：`Get Started`（`Headline/H4`）
- 三栏行动卡：
  - **Compute** —— 引导创建计算资源
  - **Cloud Networking** —— 引导建立网络
  - **Global Accelerator** —— 引导加速服务
- 每张卡：Product Icon + 标题 + 一句话描述 + CTA Button

---

## 7. 行为与可访问性

### 数据加载
<!-- Figma 没有显式 loading/empty 设计，建议确认 -->
- [ ] Resource Overview 加载中的骨架屏？
- [ ] 空账号（0 资源）的回退状态？目前 Basic 已经像是"少资源"版本，但仍展示 4 Instances 假数据

### 错误状态
<!-- 资源加载失败 / 地图加载失败 的设计是否存在 -->

### 焦点与键盘
- Tab 顺序：TopNav → Resource Overview 各卡（含 View 链接）→ Resource Map 筛选 → Looking glass 4 项 → Get Started 三卡 CTA
- 地图区如果非交互（仅装饰），考虑 `tabindex="-1"` 与 ARIA `role="img"`

### 对比度（WCAG）
| 前景 | 背景 | 比值 | 是否通过 |
|------|------|------|:-:|
| `On Surface/Font High` `#eaeef7` | `var(--zen-design-background-bg)` `#0a0a0f` | ~16.4:1 | ✓ AAA |
| `On Surface/Font High` `#eaeef7` | `Elevation/2F` `#0e0f13` | ~15.8:1 | ✓ AAA |
| `Primary/High` `#1863ff` | `#0a0a0f` | ~4.8:1 | ✓ AA |
| `On Surface/Low` `#eaeef761`（38% alpha） | `#0e0f13` | ~6.0:1 | ✓ AA（仅大文本） |

### 待确认问题
- [ ] 切换 Type2 ↔ Basic 的触发条件（账号资源数阈值？feature flag？用户偏好？）
- [ ] Basic 变体的促销位是否有可配置后台
- [ ] Resource Map 是否支持点击标点钻取
- [ ] 移动端断点（当前画布仅 1440）

---

## 8. 实现建议

- 使用 token 名而非硬编码值
- Section 卡：背景 `Elevation/2F`、圆角 `Radius/L`、内边距 `Padding/L`、外间距 `Gap/XL`
- 列表条目（Looking glass、Get Started）使用 `MenuItem` 复用
- 地图考虑懒加载 / 静态 PNG 兜底（避免 SVG 体积影响首屏）
- 促销 Banner（Basic）的展示逻辑应有过期判定 —— Figma 上文案是写死的日期范围

---

*由 Figma 文件 `VjnKgyA71h6uJxJCSTB6NS` 生成 · 通过 `/dashboard` skill 重新生成*
