# Skill 路由表

---

## 流程 Skills — 阶段编排器（Inception ↔ Operation）

> 负责流程推进，不直接出图。Construction 页面 skill 通常由 Inception 路由调起。

| 阶段 | 触发词 | Skill |
|------|--------|-------|
| ① Inception 构思 | 帮我规划 / 出方案 / 先想想 / 信息架构 / 不确定做什么 / 有定制需求 / `/inception` | `.claude/skills/inception/SKILL.md` |
| ③ Operation 运维 | 验收完成 / 复盘 / retro / 总结这次 / 沉淀经验 / `/retro` | `.claude/skills/operations/retro/SKILL.md` |

---

## Construction Page Skills — 仅限阶段 ② 调用

> 入口有两种：(a) Inception 确认 DESIGN.md 后路由过来；(b) 用户明确说出页面类型，直接出图。
> 不接受模糊需求——遇到"帮我想想"先走 Inception。

| 页面类型 | 触发词 | Skill |
|---------|--------|-------|
| 登录 / 认证 | 登录 / 注册 / sign in / sign up / 忘记密码 / MFA / OAuth | `.claude/skills/construction/login/SKILL.md` |
| Dashboard | dashboard / 首页 / 仪表盘 / 控制台首屏 / 资源概览 | `.claude/skills/construction/dashboard/SKILL.md` |
| 列表页 | 列表页 / 资源列表 / list page / 实例列表 / 带 tab 的列表 | `.claude/skills/construction/list-page/SKILL.md` |
| 创建页 | 创建页 / 新建页 / 购买页 / 下单页 / 配置页 | `.claude/skills/construction/create-page/SKILL.md` |
| 详情页 | 详情页 / 资源详情 / 实例详情 / detail page | `.claude/skills/construction/detail-page/SKILL.md` |
| 订单详情 | 订单详情 / order detail / 预付费 / 后付费 / 账单详情 | `.claude/skills/construction/order-detail/SKILL.md` |

---

## 路由规则

- 需求模糊 / 有定制要求 → 先走 **Inception**，DESIGN.md 确认后再进 Construction
- 明确说了具体页面类型 → 直接走对应 **Construction** skill（skill 内部会问变体）
- 完全不知道 → 展示此表让用户选
