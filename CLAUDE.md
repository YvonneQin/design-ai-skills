# Zenlayer Design System

## 每次收到意图，按这三步走

1. 判断用户在三阶段的哪个阶段（见下方）
2. 读 `.claude/skills/index.md` → 找到对应 skill 路径
3. 加载该 SKILL.md → 执行

---

## 三阶段核心流程

| 阶段 | 谁做 | 做什么 | 用哪个 skill |
|------|------|--------|-------------|
| **① Inception 构思** | AI + 人类 | 理解需求 → 信息架构 → 生成 DESIGN.md → 批注确认循环 | `/inception` |
| **② Construction 构建** | AI | 读 DESIGN.md → 在约束内出高保真图 → 写入 Figma | 对应页面 skill |
| **③ Operation 运维** | AI + 人类 | 验收 Figma → 复盘决策 → 经验写回 references/ | `/retro` |

**DESIGN.md 是 Inception 和 Construction 的门控文档，未经人类确认不执行出图。**

---

## 项目背景

- Figma 文件：`VjnKgyA71h6uJxJCSTB6NS`（Zenlayer zenConsole）
- 深色模式专属，字体 Proxima Nova，品牌色 `#1863ff`
- `designs/` 目录：Spec 文档区，存放 DESIGN.md 和 RETRO.md
