---
name: create-page
description: 根据用户一句话需求，在 Zenlayer zenConsole 的 Figma 文件里出一张资源创建页。触发词：「创建页」、「新建页」、「购买页」、「下单页」、「配置页」、「create page」、「产品创建」、「实例创建」、「解决方案创建」、「简单创建」、「generate create page」、「/create-page」。本 skill 是调度器，把意图匹配到 3 个 Create Page 变体之一（Default / Solution / Normal），再调用 figma-use 写入 Figma。
---

# Create Page 出图调度器

输入一句话，输出一张写在 Figma 里的 Create Page frame。

## Spec 驱动入口（Construction Phase）

若本次调用来自 `/inception` 流程（用户已确认 DESIGN.md），先读取 `designs/{feature}/DESIGN.md`：
- 第 3 节「推荐执行方案」→ 直接确定变体，跳过阶段 0 的 AskUserQuestion
- 第 4 节「定制清单」→ 若有定制项，自动切换到改写模式
- 第 5 节「验收标准」→ 阶段 3 校验时对照

若无 DESIGN.md（用户直接一句话出图），走阶段 0 意图解析流程。

---

## 数据源

- **Figma 文件**：`VjnKgyA71h6uJxJCSTB6NS`
- **Create Page 根节点**：`450:13003`（x=13541, y=-5043, 2799×4907）
- **3 个主变体 nodeId**：[references/states.md](references/states.md)
- **意图映射表**：[references/intent-map.md](references/intent-map.md)

## 工作流程

### 阶段 0 — 解析用户意图

读取 [references/intent-map.md](references/intent-map.md)，匹配变体：

- 关键词 "含定价 / 购买 / 下单 / 完整 / 云主机" → **Default**（`476:12766`）
- 关键词 "solution / 解决方案" → **Solution**（`552:23529`）
- 关键词 "简单 / 快速 / 轻量 / 字段少" → **Normal**（`1075:19793`）
- 未指定 → `AskUserQuestion` 给三个选项（带描述）

### 阶段 1 — 落点位置

新 frame 放到：**Create Page 根 frame 右侧 100px**
- `x = 13541 + 2799 + 100 = 16440`，`y = -5043`
- 若已有生成，向下偏移 3000px（Default 高度大）

### 阶段 2 — 出图（默认复刻模式）

调用 `figma-use` skill，脚本：

```javascript
const source = await figma.getNodeByIdAsync("<变体 nodeId>");
await figma.setCurrentPageAsync(source.parent);
const instance = source.createInstance();
await figma.setCurrentPageAsync(figma.root.children.find(p => p.name === "<目标 page 名>"));
figma.currentPage.appendChild(instance);
instance.x = 16440;
instance.y = -5043;
instance.name = "<语义化名字，如 "Create Page - Default" 或 "Create VM - Normal">";
return { newNodeId: instance.id, x: instance.x, y: instance.y };
```

**改写模式**（用户说"改/加/去掉"时）：切换到 `figma-generate-design`，参考 brief 模板（见下方）。

**Brief 模板（改写模式）**：

```
TARGET: 在 fileKey VjnKgyA71h6uJxJCSTB6NS 坐标 (16440, -5043)
        新建一个 1440×<高度> 的 Create Page frame。
        - Default 高度 2670
        - Solution 高度 1260
        - Normal 高度 1020

BASE VARIANT: <匹配到的变体 nodeId>

USER REQUEST: <用户原句>

DESIGN TOKENS:
  - 背景: #0a0a0f
  - Section 卡: Elevation/2F #0e0f13, Radius/L 16
  - 主色: Primary/High #1863ff
  - 字体: Proxima Nova

CONSTRAINTS:
  - 深色模式
  - 区域顺序: TopNavigation → Page Header → Form → Action Bar → Copyright
  - 复用共享组件（TopNavigation 525:17195、Button 192:3022、CopyRight 90:1433）
```

### 标注后处理（默认执行）

读 [`../common/annotation.md`](../common/annotation.md)，用以下参数执行标注模板：

- `FRAME_ID`：上一阶段返回的新 frame nodeId
- `PAGE_KEYWORDS`：`['Create', 'Submit', 'Order', 'Confirm']`
- `DEST_LABEL`：`'→ Order Detail'`

### 阶段 3 — 校验与汇报

```
get_screenshot(nodeId="<新 frame ID>", fileKey="VjnKgyA71h6uJxJCSTB6NS")
```

返回给用户：
- 新 frame nodeId + 直链
- 用了哪个变体、原因
- 任何未执行的用户要求

## 关键规则

1. **默认复刻** — 用户没说"改"就不走改写模式
2. **3 个变体，必须问清楚** — "做一个创建页"必须 AskUserQuestion
3. **子区域变体（More settings / Pricing）** — 仅在改写模式下按 intent-map 选择
4. **深色专属** — 不要生成浅色版本
5. **位置固定** — x=16440, y=-5043 起步

## 反模式

- ❌ "做一个创建页"直接默认 Default（必须问）
- ❌ 不读 intent-map 直接猜变体
- ❌ 生成位置覆盖原始 frame（应右侧偏移）
- ❌ 忽略 More settings / Pricing 子区域意图
