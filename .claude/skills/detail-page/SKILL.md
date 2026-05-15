---
name: detail-page
description: 根据用户一句话需求，在 Zenlayer zenConsole 的 Figma 文件里出一张资源详情页。触发词：「详情页」、「资源详情」、「实例详情」、「查看详情」、「detail page」、「detail view」、「/detail-page」。本 skill 是调度器，将详情页实例（含面包屑+标题+Tab+图表+表格）写入 Figma。也是 /list-page 流程的下游被串联调用。
---

# Detail Page 出图调度器

输入一句话，输出一张写在 Figma 里的 Detail Page frame。

## Spec 驱动入口（Construction Phase）

若本次调用来自 `/inception` 流程（用户已确认 DESIGN.md），先读取 `designs/{feature}/DESIGN.md`：
- 第 3 节「推荐执行方案」→ 变体已确定，跳过阶段 0
- 第 4 节「定制清单」→ 若有定制项，自动切换到改写模式
- 第 5 节「验收标准」→ 阶段 3 校验时对照

若无 DESIGN.md（用户直接一句话出图），走阶段 0 意图解析流程。

---

## 数据源

- **Figma 文件**：`VjnKgyA71h6uJxJCSTB6NS`
- **Detail 文档根节点**：`442:5903`（x=-746, y=-5043）
- **主变体 nodeId**：`474:9320`（Detail，960×2221）
- **变体 & 子区域索引**：[references/states.md](references/states.md)
- **意图映射表**：[references/intent-map.md](references/intent-map.md)

## 工作流程

### 阶段 0 — 解析用户意图

Detail Page 只有一个主变体，关键词命中"详情/detail"即直接使用 `474:9320`，无需询问。

若用户提到订单计费模型（预付费/后付费/承诺期），记录下来用于改写模式替换子区域。

### 阶段 1 — 落点位置

**独立模式**（直接出详情页）：
- `x = -746 + 2852 + 100 = 2206`，`y = -5043`
- 若已有生成，向下偏移 2500px

**流程模式**（从 /list-page 串联过来）：
- 放在 ListPage 新 frame 的右侧 200px
- 用 `figma-use` 画箭头，文字 "查看详情"

### 阶段 2 — 出图（默认复刻模式）

调用 `figma-use` skill，脚本：

```javascript
const pagePage = figma.root.children.find(p => p.id === "424:5458");
await figma.setCurrentPageAsync(pagePage);
const source = await figma.getNodeByIdAsync("474:9320");
const instance = source.createInstance();
const targetPage = figma.root.children.find(p => p.name === "<目标 page 名>");
await figma.setCurrentPageAsync(targetPage);
targetPage.appendChild(instance);
instance.x = <按阶段 1 计算>;
instance.y = <按阶段 1 计算>;
instance.name = "<语义化名字，如 "Detail - Virtual Machine">";
return { newNodeId: instance.id, x: instance.x, y: instance.y };
```

### 阶段 3 — 校验与汇报

```
get_screenshot(nodeId="<新 frame ID>", fileKey="VjnKgyA71h6uJxJCSTB6NS")
```

返回给用户：
- 新 frame nodeId + 直链
- 是否为流程模式（有无连线）
- 任何未执行的要求

## 关键规则

1. **单变体，不需要问** — 直接用 `474:9320`，不要 AskUserQuestion
2. **默认复刻** — 用 createInstance() 而非 clone()
3. **流程模式必须画箭头** — 从 list-page 串联时，没有连线 = 流程未完成
4. **深色专属** — 不要生成浅色版本
5. **源组件在 "Page 页面组合" 页** — 找节点前先 setCurrentPageAsync 到 id=424:5458

## 反模式

- ❌ 用 clone() 而不是 createInstance()（会生成 Component 副本而非 Instance）
- ❌ 在错误 page 上查找节点（Detail 组件在 "Page 页面组合"，id=424:5458）
- ❌ 流程模式下不画箭头
