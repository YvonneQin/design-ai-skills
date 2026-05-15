# Login 意图映射表

把用户那句话映射到具体变体的 nodeId。匹配时**先关键词命中、再模糊问询**。

fileKey: `VjnKgyA71h6uJxJCSTB6NS`

## 中文 → 变体

| 关键词 / 用户句子 | 变体 | nodeId |
|------------------|------|--------|
| 登录 / 邮箱登录 / Sign in / 账号密码登录 | Sign in / Account Information | `629:9263` |
| MFA / 二次验证 / 多因素验证 / 双重认证 | Sign in / MFA | `4411:28582` |
| 登录结果 / 登录成功 / 登录失败 | Sign in / Result | `4999:30514` |
| 注册 / 注册入口 / Sign up 入口 / 创建账号 | Sign up / Normal | `540:18337` |
| 注册账号信息 / 填写注册信息 | Sign up / Account Information | `540:18492` |
| 邮件验证 / 验证码 / 注册验证邮件 | Sign up / Email Verification | `544:24171` |
| 个人计费 / 个人付款信息 | Sign up / Billing Information - Individual | `629:10176` |
| 企业计费 / 企业付款 / 公司账单 | Sign up / Billing Information - Enterprise | `1179:11688` |
| 合作伙伴注册 / Partner 注册 | Sign up / Partner | `1247:13922` |
| 忘记密码 / 重置密码入口 / Reset password | Reset Password / Normal | `639:11768` |
| 重置密码邮件 / 重置邮件验证 | Reset Password / Email Verification | `639:11958` |
| 新密码 / 设置新密码 | Reset Password / New password | `639:12129` |
| OAuth 已注册 / 第三方已注册提示 | Third party tips / Registered | `635:10836` |
| OAuth 未注册 / 第三方未注册提示 | Third party tips / Unregistered | `639:11571` |
| 注册不可用 / 地区不支持 / Unavailable | Unavailable / Unregistered | `7871:25354` |

## 英文 → 变体

| Keywords | Variant | nodeId |
|----------|---------|--------|
| login, sign in, email login, password login | Sign in / Account Information | `629:9263` |
| MFA, 2FA, multi-factor, two-factor | Sign in / MFA | `4411:28582` |
| login result, login success, login error | Sign in / Result | `4999:30514` |
| sign up, register, signup landing | Sign up / Normal | `540:18337` |
| account info, fill account details | Sign up / Account Information | `540:18492` |
| email verification, verify email, OTP | Sign up / Email Verification | `544:24171` |
| individual billing, personal payment | Sign up / Billing Individual | `629:10176` |
| enterprise billing, company billing | Sign up / Billing Enterprise | `1179:11688` |
| partner signup, channel signup | Sign up / Partner | `1247:13922` |
| forgot password, reset password | Reset Password / Normal | `639:11768` |
| reset email, password reset OTP | Reset Password / Email Verification | `639:11958` |
| new password, set new password | Reset Password / New password | `639:12129` |
| oauth registered, third party registered | Third party tips / Registered | `635:10836` |
| oauth unregistered, third party new user | Third party tips / Unregistered | `639:11571` |
| signup unavailable, region blocked | Unavailable / Unregistered | `7871:25354` |

## 模糊场景（必须 AskUserQuestion）

| 用户说 | 候选选项 |
|--------|---------|
| "做一个登录页" | Sign in / Account Information vs Sign up / Normal vs Reset Password / Normal |
| "做个验证页" | Sign in / MFA vs Sign up / Email Verification vs Reset Password / Email Verification |
| "OAuth 登录" | Third party tips / Registered vs Unregistered |
| "结果页" | Sign in / Result vs Sign up 完成 (没有专门的，建议复用 Sign in / Result) |
| "billing 页" | Individual vs Enterprise |

## 流程意图（用户描述了多步）

| 用户描述 | 处理方式 |
|---------|---------|
| "登录然后到 dashboard" | 本 skill 出 Sign in / Account Information → Sign in / Result → 调用 `/dashboard` |
| "完整注册流程" | Sign up / Normal → Account Information → Email Verification → Billing → 调用 `/dashboard` |
| "忘记密码完整流程" | Reset Password / Normal → Email Verification → New password → 调用 `/login`（回到 Sign in） |

流程模式下，本 skill 每生成一个 frame 后用 `figma-use` 在 frame 之间画箭头连线（FigJam 风格指向下一步）。
