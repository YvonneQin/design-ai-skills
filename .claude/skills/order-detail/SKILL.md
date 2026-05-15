---
name: order-detail
description: 根据用户一句话需求，在 Zenlayer zenConsole 的 Figma 文件里出一张订单详情页。触发词：「订单详情」、「订单页」、「order detail」、「预付费订单」、「后付费订单」、「账单详情」、「billing detail」、「/order-detail」。本 skill 把意图匹配到 2 个变体之一（prepay 预付费 / postpay 后付费），再调用 figma-use 写入 Figma。
---

# Order Detail Page 出图调度器

输入一句话，输出一张写在 Figma 里的订单详情页 frame。

## Spec 驱动入口（Construction Phase）

若本次调用来自 `/inception` 流程（用户已确认 DESIGN.md），先读取 `designs/{feature}/DESIGN.md`：
- 第 3 节「推荐执行方案」→ 直接确定 prepay / postpay，跳过阶段 0 的 AskUserQuestion
- 第 4 节「定制清单」→ 若有定制项，自动切换到改写模式
- 第 5 节「验收标准」→ 阶段 3 校验时对照

若无 DESIGN.md（用户直接一句话出图），走阶段 0 意图解析流程。

---

## 数据源

- **Figma 文件**：`VjnKgyA71h6uJxJCSTB6NS`
- **文档根节点**：`1088:16452`（x=18368, y=-5043, 1600×1813）
- **COMPONENT_SET**：`2157:10675`（所在 page：Page 页面组合，id=424:5458）
- **2 个变体 nodeId**：[references/states.md](references/states.md)
- **意图映射表**：[references/intent-map.md](references/intent-map.md)

## 工作流程

### 阶段 0 — 解析用户意图

读取 [references/intent-map.md](references/intent-map.md)：

- 关键词 "预付费 / prepay / 包月 / 固定周期" → **Type=prepay**（`1095:11516`）
- 关键词 "后付费 / postpay / 按量 / 用量" → **Type=postpay**（`2157:10676`）
- 未指定 → `AskUserQuestion` 给两个选项（带描述）

### 阶段 1 — 落点位置

- `x = 18368 + 1600 + 100 = 20068`，`y = -5043`
- 若已有生成，向下偏移 800px

### 阶段 2 — 出图（默认复刻模式）

调用 `figma-use` skill，脚本：

```javascript
const pagePage = figma.root.children.find(p => p.id === "424:5458");
await figma.setCurrentPageAsync(pagePage);
const source = await figma.getNodeByIdAsync("<变体 nodeId>");
const instance = source.createInstance();
const targetPage = figma.root.children.find(p => p.name === "<目标 page 名>");
await figma.setCurrentPageAsync(targetPage);
targetPage.appendChild(instance);
instance.x = 20068;
instance.y = -5043;
instance.name = "<语义化名字，如 "Order Detail - Prepay">";
return { newNodeId: instance.id, x: instance.x, y: instance.y };
```

### 阶段 3 — 校验与汇报

```
get_screenshot(nodeId="<新 frame ID>", fileKey="VjnKgyA71h6uJxJCSTB6NS")
```

返回：新 frame nodeId + 直链 + 用了哪个变体。

## 关键规则

1. **"订单详情"必须问** — prepay 和 postpay 差异明显，不能猜
2. **用 createInstance() 而非 clone()** — 保持与原组件的 Instance 引用关系
3. **源组件在 "Page 页面组合"（id=424:5458）** — 找节点前必须先切换 page
4. **深色专属** — 不要生成浅色版本

## 反模式

- ❌ "订单详情"直接默认 prepay（必须问）
- ❌ 用 clone() 而不是 createInstance()
- ❌ 忘记切换到 Page 页面组合再找节点
