---
name: list-page
description: 根据用户一句话需求，在 Zenlayer zenConsole 的 Figma 文件里出一张资源列表页。触发词：「列表页」、「资源列表」、「list page」、「资源管理页」、「产品列表」、「实例列表」、「带 tab 的列表」、「分类列表」、「create list page」、「generate resource list」、「/list-page」。本 skill 是调度器，把意图匹配到 2 个 ListPage 变体之一（Basic / With tab），再调用 figma-use 写入 Figma。
---

# ListPage 出图调度器

输入一句话，输出一张写在 Figma 里的 ListPage frame。

## Spec 驱动入口（Construction Phase）

若本次调用来自 `/inception` 流程（用户已确认 DESIGN.md），先读取 `designs/{feature}/DESIGN.md`：
- 第 3 节「推荐执行方案」→ 直接确定变体，跳过阶段 0 的 AskUserQuestion
- 第 4 节「定制清单」→ 若有定制项，自动切换到改写模式
- 第 5 节「验收标准」→ 阶段 3 校验时对照

若无 DESIGN.md（用户直接一句话出图），走阶段 0 意图解析流程。

---

## 数据源

- **组件库文件（只读）**：`VjnKgyA71h6uJxJCSTB6NS`
- **目标写入文件**：用户当前打开的 Figma 文件（不一定是库文件）
- **ListPage 根节点**：`500:19702`（仅在库文件内有效，用于 `get_design_context`）
- **2 个变体 nodeId + componentKey**：[references/states.md](references/states.md)
- **意图映射表**：[references/intent-map.md](references/intent-map.md)

> **跨文件说明**：`componentKey` 是稳定的 UUID，只要组件库已发布到 Team Library 且目标文件已引用该库，就可以在任意文件里用 `importComponentByKeyAsync(key)` 拉取实例，无需 nodeId。

## 工作流程

### 阶段 0 — 解析用户意图

读取 [references/intent-map.md](references/intent-map.md)，把用户那句话匹配到 Basic 或 With tab。

- 关键词 "tab / 分类 / 切换 / 筛选" → **With tab**（`500:20948`）
- 关键词 "基础 / 简单 / 无 tab / 纯列表" → **Basic**（`500:20999`）
- 未指定 → `AskUserQuestion` 询问（带预览描述）

### 阶段 1 — 落点位置

落点由**动态扫描**决定，不再硬编码库文件坐标：
- 扫描目标 page 上所有已有节点，找到最右侧边界 `rightmostX`
- 新 frame 放在 `x = rightmostX + 100`，`y = 300`
- 若 page 为空，放在 `x = 200, y = 300`

### 阶段 2 — 出图（默认复刻模式）

调用 `figma-use` skill，脚本：

```javascript
// ① 从 references/states.md 查到对应变体的 componentKey，填入下方
const COMPONENT_KEY = "<变体 componentKey>";  // Basic 或 With tab

// ② 通过 Team Library 导入组件（跨文件有效）
let comp;
try {
  comp = await figma.importComponentByKeyAsync(COMPONENT_KEY);
} catch (e) {
  // 降级：仅在库文件内有效
  const source = figma.getNodeById("<变体 nodeId>");
  await figma.setCurrentPageAsync(source.parent);
  comp = source;
}
const instance = comp.createInstance ? comp.createInstance() : comp.clone();

// ③ 切换到目标 page，动态计算落点
const targetPage = figma.root.children.find(p => p.name === "<目标 page 名>") || figma.currentPage;
await figma.setCurrentPageAsync(targetPage);
targetPage.appendChild(instance);
const siblings = targetPage.children.filter(n => n.id !== instance.id && 'x' in n && 'width' in n);
const rightmost = siblings.reduce((max, n) => Math.max(max, n.x + n.width), 0);
instance.x = siblings.length > 0 ? rightmost + 100 : 200;
instance.y = 300;
instance.name = "<语义化名字，如 "ListPage - Basic" 或 "ListPage - With Tab">";
return { newNodeId: instance.id, x: instance.x, y: instance.y };
```

**改写模式**（用户说"改/加/去掉"时）：切换到 `figma-generate-design`，参考 dashboard skill 的 brief 模板。

### 阶段 3 — 校验与汇报

```
get_screenshot(nodeId="<新 frame ID>", fileKey="VjnKgyA71h6uJxJCSTB6NS")
```

返回给用户：
- 新 frame nodeId + 直链
- 用了 Basic 还是 With tab、原因
- 任何未执行的用户要求

## 关键规则

1. **默认复刻** — 用户没说"改"就不走改写模式
2. **必须先 `figma.setCurrentPageAsync`** — 每次 use_figma 都要切换到 source 所在 page
3. **深色专属** — 不要生成浅色版本
4. **位置固定** — x=6386, y=-5043 起步，往下叠 1000px

## 反模式

- ❌ 用户没说 tab 就默认 With tab
- ❌ 不读 intent-map 直接猜变体
- ❌ 生成位置覆盖原始 frame（应右侧偏移）
