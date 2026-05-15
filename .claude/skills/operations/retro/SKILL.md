---
name: retro
description: AI-DLC Operation 运维阶段。验收完成后，AI 复盘本次设计决策，生成 RETRO.md，并将可复用的经验写回对应 skill 的 references/。触发词：「验收完成」、「复盘」、「retro」、「沉淀经验」、「总结这次设计」、「/retro」。
---

# Retro 复盘调度器

**AI-DLC 步骤 ⑤ 的收尾动作。**

在用户验收 Figma 输出后触发。目标：把这次设计的决策、踩坑、复用模式沉淀成知识，写回 `references/`，让下次出图更快更准。

```
你验收（Figma 确认）→ /retro → RETRO.md + references/ 更新
```

---

## 何时触发

- 用户说"验收完成"/"这版可以了"/"复盘一下"/"总结这次"
- 用户明确触发 `/retro`

**不要触发**：
- 用户还在修改 / 还没看 Figma → 等用户验收后再触发
- 用户只是问"怎么样" → 先汇报出图结果，不跑复盘

---

## 工作流程

### 阶段 0 — 收集上下文

需要以下信息（按优先级读取）：

1. **DESIGN.md**：`designs/{feature}/DESIGN.md`（本次的设计 Spec）
2. **最终 Figma 截图**：`get_screenshot(nodeId="<新 frame ID>", fileKey="VjnKgyA71h6uJxJCSTB6NS")`
3. **对话上下文**：从本次会话中提取关键决策点

如果用户没有说 feature 名称，询问"是哪个 DESIGN.md 对应的功能？"

### 阶段 1 — 生成 RETRO.md

将以下内容写入 `designs/{feature}/RETRO.md`：

```markdown
# {Feature Name} — RETRO.md

> 阶段：Operation | 验收日期：{YYYY-MM-DD} | 对应 Skill：{skill-name}

---

## 1. 本次设计决策

| 决策点 | 选择 | 理由 |
|--------|------|------|
| 变体选择 | {Type2 / Basic / ...} | {理由} |
| 执行模式 | {复刻 / 改写} | {理由} |
| {其他关键决策} | ... | ... |

---

## 2. 与 DESIGN.md 的差异

{实际出图与 Spec 的偏差，以及偏差原因。无偏差则写"与 Spec 一致"}

- {偏差项 1}：{原因}
- {偏差项 2}：{原因}

---

## 3. 复用模式（下次可直接用）

{本次发现的可复用模式或规律}

- {模式 1}
- {模式 2}

---

## 4. 踩坑记录

{遇到的坑、绕过的限制、注意事项}

- {坑 1}
- {坑 2}

---

## 5. references/ 更新建议

{建议写回哪些 skill 的 references/，以及具体更新内容}

- [ ] `{skill-name}/references/tokens.md`：{新增/修改内容}
- [ ] `{skill-name}/references/states.md`：{新增/修改内容}
- [ ] `{skill-name}/references/intent-map.md`：{新增/修改内容}
```

### 阶段 2 — 询问是否写回 references

展示第 5 节的更新建议，用 `AskUserQuestion` 询问：

- "确认更新 references/"（AI 执行写入）
- "仅保留 RETRO.md，暂不更新"（结束）
- "我来手动改"（结束，给出建议内容）

### 阶段 3 — 写回 references/（用户确认后）

按第 5 节逐项更新对应 skill 的 `references/` 文件。每次更新前先 Read 原文件，做追加或修改，不要覆盖已有内容。

更新完成后汇报：
- 更新了哪些文件、改了什么
- RETRO.md 位置

---

## 关键规则

1. **先读 DESIGN.md** — 没有 Spec 对比的复盘是空洞的
2. **写入前先读** — 更新 references/ 前必须先 Read 原文件，避免覆盖
3. **用户确认才写 references/** — 复盘文档可以直接写，但 references 更新需要用户点头
4. **具体 > 抽象** — "Elevation/2F 是 Section 卡背景" 比 "注意颜色" 有用 100 倍

## 反模式

- ❌ 没有 DESIGN.md 就开始写复盘（结论会很空）
- ❌ 直接覆盖 references/（必须先 Read）
- ❌ 不问用户就写 references/
- ❌ 把复盘写成流水账，没有可复用的结论

## 输出位置

- **RETRO.md**：`designs/{feature-name}/RETRO.md`
- **references 更新**：`.claude/skills/{skill-name}/references/` 下对应文件
- **绝不要**写入 `.claude/skills/operations/retro/` 内部
