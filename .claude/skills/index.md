# Skill 路由表

## ① Inception 构思 — 需求理解 + 信息架构 + 设计方向

| 触发词 | Skill |
|--------|-------|
| 帮我规划 / 出方案 / 先想想 / 信息架构 / 不确定做什么 / 有定制需求 / `/inception` | `.claude/skills/inception/SKILL.md` |

## ② Construction 构建 — 在约束内执行，AI 出高保真图

| 触发词 | Skill |
|--------|-------|
| 登录 / 注册 / sign in / sign up / 忘记密码 / MFA / OAuth | `.claude/skills/login/SKILL.md` |
| dashboard / 首页 / 仪表盘 / 控制台首屏 / 资源概览 | `.claude/skills/dashboard/SKILL.md` |
| 列表页 / 资源列表 / list page / 实例列表 / 带 tab 的列表 | `.claude/skills/list-page/SKILL.md` |
| 创建页 / 新建页 / 购买页 / 下单页 / 配置页 | `.claude/skills/create-page/SKILL.md` |
| 详情页 / 资源详情 / 实例详情 / detail page | `.claude/skills/detail-page/SKILL.md` |
| 订单详情 / order detail / 预付费 / 后付费 / 账单详情 | `.claude/skills/order-detail/SKILL.md` |

## ③ Operation 运维 — 验证 + 经验沉淀

| 触发词 | Skill |
|--------|-------|
| 验收完成 / 复盘 / retro / 总结这次 / 沉淀经验 / `/retro` | `.claude/skills/retro/SKILL.md` |

---

## 路由规则

- 需求模糊 / 有定制要求 → 先走 **Inception**，DESIGN.md 确认后再进 Construction
- 明确说了具体页面类型 → 直接走对应 **Construction** skill（skill 内部会问变体）
- 完全不知道 → 展示此表让用户选
