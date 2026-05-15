---
name: dashboard
description: 根据用户一句话需求，在 Zenlayer zenConsole 的 Figma 文件里出一张 Dashboard 页面图。触发词：「做一个 dashboard」、「生成首页」、「Figma 出一个仪表盘」、「画一个资源概览页」、「我要一个紧凑版 dashboard」、「生成详细版控制台」、「create dashboard in figma」、「generate console home」、「/dashboard」。本 skill 是调度器，会先把那句话匹配到 2 个 Dashboard 变体之一（Type2 详细版 / Basic 紧凑版），再调用 figma-generate-design + figma-use 写入 Figma。常作为 /login 流程的下游被串联调用。
---

# Dashboard 出图调度器

输入一句话，输出一张写在 Figma 里的 Dashboard 页面 frame。本 skill 本身不画图——它负责**理解意图、匹配变体、调度 figma-generate-design**。也是 `/login` 流程模式的下游环节。

## Spec 驱动入口（Construction Phase）

若本次调用来自 `/inception` 流程（用户已确认 DESIGN.md），先读取 `designs/{feature}/DESIGN.md`：
- 第 3 节「推荐执行方案」→ 直接确定变体，跳过阶段 0 的 AskUserQuestion
- 第 4 节「定制清单」→ 若有定制项，自动切换到改写模式
- 第 5 节「验收标准」→ 阶段 6 校验时对照

若无 DESIGN.md（用户直接一句话出图），走阶段 0 意图解析流程。

---

## 数据源

- **组件库文件（只读）**: `VjnKgyA71h6uJxJCSTB6NS`
- **目标写入文件**: 用户当前打开的 Figma 文件（不一定是库文件）
- **Dashboard 根节点**: `7230:48098`（仅在库文件内有效，用于 `get_design_context`）
- **2 个变体的 nodeId + componentKey 索引**: [references/states.md](references/states.md)
- **一句话 → 变体的映射表**: [references/intent-map.md](references/intent-map.md)
- **设计 Token 参考**: [references/tokens.md](references/tokens.md)

> **跨文件说明**：`componentKey` 是稳定的 UUID，只要组件库已发布到 Team Library 且目标文件已引用该库，就可以在任意文件里用 `importComponentByKeyAsync(key)` 拉取实例，无需 nodeId。

## 何时触发

用户那句话提到 dashboard / 首页 / 仪表盘 / 控制台首屏 / 资源概览 / 主页 等概念，且想"做一个 / 生成 / 画"时触发。

**作为 /login 的下游被调用**：当 `/login` 流程模式处理完登录变体后，应自动调起本 skill 把用户带到 Dashboard。

**不要触发**：
- 用户只想要 Login → `/login`
- 用户想根据 Figma 写代码 → `figma-implement-design`
- 用户想看现有 Dashboard 长什么样 → 直接 `get_screenshot`

## 工作流程

### 阶段 0 — 解析用户意图

读取 [references/intent-map.md](references/intent-map.md)，把那句话匹配到 Type2 或 Basic。

匹配策略：
- 关键词 "详细 / 展开 / 多资源 / 老用户" → **Type2**（`7230:48104`）
- 关键词 "紧凑 / 概览 / 新用户 / 简版 / 促销" → **Basic**（`7230:48694`）
- 关键词 "登录后第一屏 / 默认首页" → 询问用户当前账户场景。**默认 Basic**（新用户视角更通用）
- 用户没指定 → `AskUserQuestion` 问 Type2 还是 Basic（带预览描述）

### 阶段 1 — 拉取目标变体的设计上下文

```
mcp__claude_ai_Figma__get_design_context(
  nodeId="<7230:48104 或 7230:48694>",
  fileKey="VjnKgyA71h6uJxJCSTB6NS",
  clientFrameworks="figma-plugin-api",
  clientLanguages="javascript",
  excludeScreenshot=false
)
```

**⚠️ 响应必然超大**（Type2 ~345K 字符、Basic ~182K）。**必须**派 `general-purpose` subagent 用 `jq`/`python` 解析临时文件，subagent prompt 参考 [上一版 SKILL.md](../../.claude/skills/dashboard/SKILL.md) 的阶段 2 模板。

### 阶段 2 — 拉取共享变量

```
mcp__claude_ai_Figma__get_variable_defs(nodeId="7230:48098", fileKey="VjnKgyA71h6uJxJCSTB6NS")
```

缓存结果。Dashboard 比 Login 多了 `Elevation/2F`、`Elevation/5F`、`Padding/M/S`、`Radius/M`、`Caption/C 12_h16_B`、`Headline/H3 34_h36_B`，把这些重点传给 figma-generate-design。

### 阶段 3 — 选定生成模式

| 用户那句话 | 模式 | 工具 |
|------------|------|------|
| "做一个 Type2 dashboard" / "出一个紧凑版"（纯粹要变体） | **复刻模式** | `figma-use`：克隆变体 |
| "dashboard 但只显示 Compute 资源" / "去掉地图区" | **改写模式** | `figma-generate-design` + `figma-use` |
| 上游来自 `/login` 流程 | **复刻模式 + 连线** | `figma-use`：克隆 + 画箭头从 Login 终点过来 |

**默认复刻**。改写需要用户明确说"改/去掉/换/加"。

### 阶段 4 — 落点位置

落点由**动态扫描**决定，不再硬编码库文件坐标：
- 扫描目标 page 上所有已有节点，找到最右侧边界 `rightmostX`
- 新 frame 放在 `x = rightmostX + 100`，`y = 300`
- 若 page 为空，放在 `x = 200, y = 300`

**流程模式下**：`/login` 完成后把它的 `newNodeId` 和 `x + width` 传过来，Dashboard 落在 Login frame 右侧 100px，无需手动计算绝对坐标。

### 阶段 5 — 出图

**复刻模式**（首选）：

调用 `figma-use` skill。脚本：

```javascript
// ① 从 references/states.md 查到对应变体的 componentKey，填入下方
const COMPONENT_KEY = "<变体 componentKey>";  // Type2 或 Basic

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
instance.name = "<语义化名字，如 "Dashboard - Basic (login flow output)">";
return { newNodeId: instance.id, x: instance.x, y: instance.y };
```

**改写模式**：

切换到 `figma-generate-design`。Brief 模板：

```
TARGET: 在 fileKey VjnKgyA71h6uJxJCSTB6NS 文件的坐标 (<x>, <y>) 新建一个
        1440×<高度> 的 Dashboard frame。
        - Type2 高度 1736
        - Basic 高度 1104

BASE VARIANT: <匹配到的变体 nodeId>
  -- get_design_context 解析后的结构摘要见 <subagent 返回>

USER REQUEST: <用户原句>

DESIGN TOKENS（Dashboard 专属重点）:
  - 背景:        var(--zen-design-background-bg) #0a0a0f
  - Section 卡:  Elevation/2F #0e0f13, Radius/L 16, Padding/L 12
  - 浮层:        Elevation/5F #131928
  - 文字:        On Surface/Font High #eaeef7, Body/B1
  - 标题:        Headline/H3（页面顶 "Hi, Zeno"）, Headline/H4（Section 标题）
  - 主色:        Primary/High #1863ff
  - 间距:        Gap/XL 24（Section 之间）, Gap/L 16（Section 内部）
  - Tag/徽章:    Padding/M 8, Radius/M 4

CONSTRAINTS:
  - 深色模式（无浅色版本）
  - 字体 Proxima Nova
  - 复用共享组件实例（TopNavigation 525:17195、Button 192:3022、
    DropDown 320:4296、Section、CopyRight 90:1433、DarkModeMap 1）
  - 区域顺序固定: Welcome → Resource Overview → Resource Map → Get Started
  - 无 Sidebar（全宽单列）
```

**流程模式补充**：

完成复刻后，用 `figma-use` 在 Login 终点 frame 和 Dashboard 新 frame 之间画箭头：

```javascript
const arrow = figma.createConnector();
arrow.connectorStart = { endpointNodeId: "<Login 新 frame ID>", magnet: "RIGHT" };
arrow.connectorEnd   = { endpointNodeId: "<Dashboard 新 frame ID>", magnet: "LEFT" };
arrow.text.characters = "Login Success";
return { arrowId: arrow.id };
```

### 阶段 6 — 校验与汇报

```
mcp__claude_ai_Figma__get_screenshot(nodeId="<新 frame ID>", fileKey="...")
```

返回给用户：
- Dashboard 新 frame 的 nodeId + 直链
- 用了 Type2 还是 Basic、为什么
- 如果是流程模式，附带 Login frame 与连线的 ID
- 任何未执行的用户要求

## 关键规则

1. **调度器，不写绘图脚本** —— 写入靠 `figma-use`，构建靠 `figma-generate-design`。
2. **默认复刻** —— 95% 情况都是复刻。
3. **响应超大 → 必派 subagent** —— Type2 单次必爆 token，不要尝试直接读临时文件。
4. **`Elevation/2F` 是 Section 卡背景** —— Login 没用到，Dashboard 重度依赖，告诉 figma-generate-design 时务必强调。
5. **区域顺序不可换** —— Welcome → Resource Overview → Resource Map → Get Started。
6. **无 Sidebar** —— 这个文件 Dashboard 是全宽单列，不要画侧栏。
7. **流程模式下必须画箭头** —— 如果是 `/login` 调过来的，没有箭头连线 = 流程没完成。
8. **深色专属** —— 不要生成浅色版本。

## 反模式

- ❌ 不派 subagent 直接读临时文件（必爆 context）
- ❌ 给 Dashboard 加 Sidebar（这个文件没有）
- ❌ 用户说"做个 dashboard"就默认 Type2（应该问，或默认 Basic）
- ❌ 流程模式下不画箭头
- ❌ 写 200 行 `use_figma` 脚本而不先加载 `figma-use` skill
- ❌ 把 Section 卡背景写成透明或 `Elevation/1F`（应该是 `Elevation/2F`）

## 文档模式（兼容旧用法）

若用户明确说 "生成 dashboard 的 DESIGN.md" / "提取仪表盘文档"，走原 DESIGN.md 模式：
- 模板：[references/DESIGN.template.md](references/DESIGN.template.md)
- 流程：subagent 解析两个变体 → 填充模板 → Write `./DESIGN.md`
- 这是次要用途，本 skill 优先服务于"一句话出图"。

## 输出位置

- **Figma 写入：** `VjnKgyA71h6uJxJCSTB6NS` 文件，原 Dashboard frame 右侧（流程模式下放 Login 新 frame 右侧）
- **文档模式：** 当前工作目录的 `./DESIGN.md`
- **绝不要**写入 `.claude/skills/dashboard/` 内部
