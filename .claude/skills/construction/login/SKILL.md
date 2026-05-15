---
name: login
description: 根据用户一句话需求，在 Zenlayer zenConsole 的 Figma 文件里出一张 Login 页面图。触发词：「做一个登录页」、「生成 sign in 页」、「Figma 出一个忘记密码页」、「我要一个 MFA 验证页」、「画一个注册页」、「生成 OAuth 提示页」、「create login page in figma」、「generate sign up」、「/login」。本 skill 是调度器，会先把那句话匹配到 15 个 Login 变体之一，再调用 figma-generate-design + figma-use 把变体复制/重生到 Figma 画布上。
---

# Login 出图调度器

输入一句话，输出一张写在 Figma 里的 Login 页面 frame。本 skill 本身不画图——它负责**理解用户意图、匹配变体、调度 figma-generate-design 完成绘制**。

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
- **Login 根节点**: `532:16740`（仅在库文件内有效，用于 `get_design_context`）
- **15 个变体的 nodeId + componentKey 索引**: [references/states.md](references/states.md)
- **一句话 → 变体的映射表**: [references/intent-map.md](references/intent-map.md)
- **设计 Token 参考**: [references/tokens.md](references/tokens.md)

> **跨文件说明**：`componentKey` 是稳定的 UUID，只要组件库已发布到 Team Library 且目标文件已引用该库，就可以在任意文件里用 `importComponentByKeyAsync(key)` 拉取实例，无需 nodeId。

## 何时触发

只要用户那句话能映射到 Login 流程里的某个状态（登录 / 注册 / 重置密码 / OAuth 提示 / 注册不可用），就触发本 skill。如果用户明确想生成 DESIGN.md 文档而非出图，走旧版文档模式（见底部「文档模式」）。

**不要触发**：
- 用户想生成 Dashboard → 用 `/dashboard`
- 用户想根据 Figma 写代码 → 用 `figma-implement-design`
- 用户只想看现有变体长什么样 → 直接 `get_screenshot`，不需要本 skill

## 工作流程

### 阶段 0 — 解析用户意图

读取 [references/intent-map.md](references/intent-map.md)，把用户那句话匹配到最具体的变体。

匹配策略：
- **关键词命中**（如 "MFA"、"忘记密码"、"邮箱验证"）→ 直接确定
- **模糊匹配**（如 "做一个登录页" —— 可能是 Sign in/Account Information 也可能是 Sign up/Normal）→ 用 `AskUserQuestion` 给出 2–3 个最可能选项让用户选
- **完全无法匹配** → 列出 15 个变体让用户重选

**禁止瞎猜**。匹配失败就问，不要默认走 Sign in / Account Information。

### 阶段 1 — 拉取目标变体的设计上下文

```
mcp__claude_ai_Figma__get_design_context(
  nodeId="<匹配到的变体 nodeId>",
  fileKey="VjnKgyA71h6uJxJCSTB6NS",
  clientFrameworks="figma-plugin-api",
  clientLanguages="javascript",
  excludeScreenshot=false   // 截图供 figma-generate-design 校验外观
)
```

**响应可能超大**。若返回 "result exceeds maximum allowed tokens" 并给临时文件路径，派 `general-purpose` subagent 解析（参见 [`/dashboard` skill 的 subagent prompt 模板](../dashboard/SKILL.md)）。

### 阶段 2 — 拉取共享变量（一次即可）

```
mcp__claude_ai_Figma__get_variable_defs(nodeId="532:16740", fileKey="VjnKgyA71h6uJxJCSTB6NS")
```

缓存结果。后续调用 figma-generate-design 时一并传入。

### 阶段 3 — 选定生成模式

根据用户那句话里的修改意图：

| 用户那句话 | 模式 | 工具 |
|------------|------|------|
| "做一个 Sign in 页" / "生成忘记密码页"（**纯粹要变体**） | **复刻模式** | `figma-use`：克隆变体到新位置 |
| "登录页但去掉社交登录" / "把标题改成 Welcome Back" | **改写模式** | `figma-generate-design` + `figma-use` |
| "登录页 + 后接 Dashboard 流程" | **流程模式** | 先本 skill 出 Login，再调 `/dashboard`，最后用 `figma-use` 画箭头 |

**默认是复刻模式**——95% 的"一句话出图"需求都是这个。只有用户明确说"改…"、"去掉…"、"换…"、"加…"时才走改写模式。

### 阶段 4 — 确定落点位置

落点由**动态扫描**决定，不再硬编码库文件坐标：
- 扫描目标 page 上所有已有节点，找到最右侧边界 `rightmostX`
- 新 frame 放在 `x = rightmostX + 100`，`y = 300`（与顶部留出 step label 空间）
- 若 page 为空，放在 `x = 200, y = 300`

### 阶段 5 — 出图

**复刻模式**（首选）：

调用 `figma-use` skill（必读其 SKILL.md 中关于 `use_figma` 的 API 规则），用以下脚本：

```javascript
// ① 从 references/states.md 查到对应变体的 componentKey，填入下方
const COMPONENT_KEY = "<变体 componentKey>";

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
instance.name = "<用户语义化的名字，如 "Sign in - Email">";
return { newNodeId: instance.id, x: instance.x, y: instance.y };
```

**改写模式**：

切换到 `figma-generate-design` skill。把以下 brief 传给它：

```
TARGET: 在 fileKey VjnKgyA71h6uJxJCSTB6NS 文件中，于坐标 (13394, -5043)
        新建一个 1440×900 的 Login frame。

BASE VARIANT: <匹配到的变体 nodeId>
  -- get_design_context 已读取，结构保存于 <临时文件路径 or 上下文>

USER REQUEST: <用户原句>

DESIGN TOKENS（来自 532:16740 的 get_variable_defs）:
  <粘贴 tokens 摘要，重点 Primary/High、On Surface/Font High、Gap/L、Radius/L>

CONSTRAINTS:
  - 主题强制为深色（背景 #0a0a0f）
  - 字体 Proxima Nova
  - 复用现有共享组件实例（TopNavigation 525:17195、Button 192:3022、
    DropDown 320:4296、Divider 112:3830、CopyRight 90:1433、Banner 4987:22388）
  - 所有视觉属性 bind 到上述 token，不要硬编码
```

### 阶段 6 — 校验与汇报

```
mcp__claude_ai_Figma__get_screenshot(nodeId="<新 frame ID>", fileKey="...")
```

返回给用户：
- 新 frame 的 nodeId 和直链 URL（拼装：`https://www.figma.com/design/VjnKgyA71h6uJxJCSTB6NS/?node-id=<id>`）
- 用了哪个 base 变体
- 模式（复刻 / 改写）
- 任何未执行的用户要求（如用户说"加一个 reCAPTCHA"但 Figma 库里没有，应明确告知）

## 关键规则

1. **本 skill 是调度器，不做绘图脚本**。所有 `use_figma` 写入操作都通过 `figma-use` skill；所有"基于描述构建"操作都通过 `figma-generate-design`。
2. **默认复刻** —— 不要因为想"展示能力"而强行走改写模式。用户没说改什么就别改。
3. **匹配失败就问** —— 永远不要凭"登录页最常见是 Sign in"猜默认。
4. **保留斜杠 token 名**（`Primary/High`，不要改 `primary-high`）。
5. **新 frame 位置固定** —— `x=13394, y=-5043` 起步，往下叠 1000px。不要每次随机选位置。
6. **永远先 `figma.setCurrentPageAsync`** —— `use_figma` 每次调用都会重置 page context。
7. **深色专属** —— 这是个 dark mode 文件，不要生成浅色 Login。
8. **流程模式必须串联 /dashboard** —— 用户提到"登录后到 dashboard"时，本 skill 完成 Login frame 后**显式调用** `/dashboard` skill 继续，不要自己画 Dashboard。

## 反模式

- ❌ 模糊匹配时默认走"最常见"变体（必须问）
- ❌ 用户没要求改的情况下，走改写模式（应该复刻）
- ❌ 自己写 200 行 `use_figma` 脚本，不先加载 `figma-use` skill
- ❌ 把生成的新 frame 直接覆盖在原变体位置（应该右侧偏移）
- ❌ 忽略 "深色模式" 约束，输出浅色版本
- ❌ 用户说"登录 + dashboard"时，本 skill 把两个都画了（应该 Login 自己负责，Dashboard 移交给 /dashboard）

## 文档模式（兼容旧用法）

若用户明确说 "生成 login 的 DESIGN.md"、"提取 Login 设计文档"、"export login spec"，走原有的 DESIGN.md 模式：
- 模板：[references/DESIGN.template.md](references/DESIGN.template.md)
- 流程：读取所有 15 个变体的 design context → 填充模板 → Write 到 `./DESIGN.md`
- 这是次要用途，本 skill 优先服务于"一句话出图"。

## 输出位置

- **Figma 写入：** `VjnKgyA71h6uJxJCSTB6NS` 文件，原 Login frame 右侧
- **文档模式：** 当前工作目录的 `./DESIGN.md`
- **绝不要** 写入 `.claude/skills/construction/login/` 内部
