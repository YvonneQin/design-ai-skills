# Login 状态索引

Zenlayer zenConsole Figma 文件里全部 15 个 Login 状态。fileKey: `VjnKgyA71h6uJxJCSTB6NS`。

每一行对应一次 `get_design_context` 调用。

## Sign in（登录，3 个）

| 状态 | nodeId | componentKey | 说明 |
|------|--------|--------------|------|
| Account Information | `629:9263` | `bae5b3a1bbfeba003f2ded7acc4ede1483455ca2` | 邮箱 + 密码表单、社交登录（Google/GitHub/Microsoft）、记住我、忘记密码链接、跳转注册 |
| MFA | `4411:28582` | `1a63ce723ee7de9b5dddb678acfce83059a94633` | 邮箱密码通过后的多因素认证验证码输入 |
| Result | `4999:30514` | `ea0e2580514908a7d458e6547b3c5eaa8aad65a9` | 登录完成后的结果页（成功或终止性错误） |

## Sign up（注册，6 个）

| 状态 | nodeId | componentKey | 说明 |
|------|--------|--------------|------|
| Normal | `540:18337` | `8dc65792edba0fad4fad70452459d8914739d394` | 入口页 — 主 CTA "Sign up with Email"、社交注册、用户协议 |
| Account Information | `540:18492` | `410fd7062907763bedffbbba63ecb7ff6520bb5e` | 账号信息表单（姓名、邮箱、密码等） |
| Email Verification | `544:24171` | `dde15f8252092fdb068e748c53d205d721d9ab61` | 邮箱验证码输入 |
| Billing Information - Individual | `629:10176` | `2a067a4371cfba604534bdd71c9a4d8651b5aea2` | 个人计费信息（个人地址、支付方式） |
| Billing Information - Enterprise | `1179:11688` | `76cbd4044a35b9c962f9eff4122cce5b1bb8d4e9` | 企业计费信息（公司信息、税务、支付方式） |
| Partner | `1247:13922` | `1994464d6155da6efed4ccef06f405b59d6733fa` | 渠道合作伙伴注册路径 |

## Reset Password（重置密码，3 个）

| 状态 | nodeId | componentKey | 说明 |
|------|--------|--------------|------|
| Normal | `639:11768` | `a8e4684edd67adee623bb7075d9288fb1d78c38e` | 输入邮箱开始重置流程 |
| Email Verification | `639:11958` | `57be38ad83db66c3000a881e7468fa9760ee1cb9` | 输入重置邮件里的验证码 |
| New password | `639:12129` | `ef605b57c76d9431a8feb02d56ad9e560cf76d17` | 新密码 + 确认密码 |

## Third party tips（第三方提示，2 个）

| 状态 | nodeId | componentKey | 说明 |
|------|--------|--------------|------|
| Registered | `635:10836` | `bf2e238264548048fd573e887b528912c32aa1a9` | OAuth 返回的邮箱已经注册过 |
| Unregistered | `639:11571` | `b60f7c4afed7639be59a67e17601d859ccc32a2c` | OAuth 返回的邮箱尚未注册 |

## Unavailable（不可用，1 个）

| 状态 | nodeId | componentKey | 说明 |
|------|--------|--------------|------|
| Unregistered | `7871:25354` | `672498e8374df5a0ad4d64ca92c65e320f708fad` | 注册被拦截（地区限制 / 功能开关） |

## Step indicator 子组件

用于 Billing / Partner 多步骤流程。被引用时再按需读取。

| 状态 | nodeId |
|------|--------|
| Completed | `1247:13596` |
| Current | `1247:13595` |
| Unfilled | `1247:13594` |

## 推荐读取顺序

按以下顺序读取，DESIGN.md 里的排版会更贴近用户实际流程：

1. Sign in：Account Information → MFA → Result
2. Sign up：Normal → Account Information → Email Verification → Billing(Individual, Enterprise) → Partner
3. Reset Password：Normal → Email Verification → New password
4. Third party tips：Registered → Unregistered
5. Unavailable：Unregistered

可以按 3–5 个一批并行读取。
