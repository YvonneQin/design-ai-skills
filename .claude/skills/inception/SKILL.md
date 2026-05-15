---
name: inception
description: AI-DLC 步骤 ①②③。理解需求 → 生成 DESIGN.md → 支持人类批注修改循环 → 确认后路由到对应 Construction skill。触发词：帮我规划、出方案、先想想、信息架构、/inception。
---

# Inception

## 何时触发

- 用户需求模糊、有定制要求、不知道选哪个变体
- 用户说"先帮我想想"、"做个方案"、"规划一下"

不触发：用户明确说"直接出图" → 走对应 Construction skill。

---

## 工作流

### 步骤 1 — 理解需求，决定是否需要澄清

判断用户意图缺什么信息：
- 目标用户是谁
- 核心任务是什么（查看 / 创建 / 管理）
- 是否需要定制（vs 使用标准变体）

信息够 → 直接写 DESIGN.md。  
有关键缺口 → `AskUserQuestion` 补问，**最多 1 轮，最多 2 个问题**。  
超过这个就先写草稿让用户改，不要一直追问。

### 步骤 2 — 生成 DESIGN.md

确定 feature 名（snake_case，如 `vm_list` / `dashboard_new_user`）。

用 [`references/DESIGN.template.md`](references/DESIGN.template.md) 作为模板，填充后写入：
```
designs/{feature-name}/DESIGN.md
```

写入后告知用户：
> "已生成 `designs/{feature}/DESIGN.md`，请打开第 6 节添加批注。确认后回复「开始出图」。"

### 步骤 3 — 读批注，修改 DESIGN.md

用户说"我批注了"或直接给出修改意见时：
1. Read `designs/{feature}/DESIGN.md`
2. 找第 6 节的批注（`> 批注：...`）
3. 逐条更新第 1–5 节
4. 标记已处理批注（`> ✓ 已处理`）
5. 必要时继续补问

重复步骤 3 直到用户确认。

### 步骤 4 — 确认后路由 Construction

用户说"确认" / "开始出图"时：
1. 读 DESIGN.md 第 3 节「推荐执行方案」
2. 在 `index.md` 里找对应 Construction skill
3. 加载该 SKILL.md，传入上下文：DESIGN.md 路径 + 推荐变体 + 定制清单

---

## 规则

- DESIGN.md 未确认 → 不调用任何 Construction skill
- 每次修改后写回文件，不要只在聊天里改
- DESIGN.md 写在 `designs/`，不要写在 `.claude/skills/` 里
