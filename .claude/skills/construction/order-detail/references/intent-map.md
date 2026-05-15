# Order Detail Page 意图映射表

fileKey: `VjnKgyA71h6uJxJCSTB6NS`

## 中文 → 变体

| 关键词 / 用户句子 | 变体 | nodeId | 画布高 |
|------------------|------|--------|--------|
| 预付费 / 预付 / prepay / 固定周期 / 包年包月 | Type=prepay | `1095:11516` | 641 |
| 后付费 / 后付 / postpay / 按量 / 按用量 | Type=postpay | `2157:10676` | 693 |

## 英文 → 变体

| Keywords | Variant | nodeId |
|----------|---------|--------|
| prepay, prepaid, fixed term, monthly, annual | Type=prepay | `1095:11516` |
| postpay, postpaid, pay-as-you-go, usage-based | Type=postpay | `2157:10676` |

## 模糊场景

| 用户说 | 默认 | 是否必问 |
|--------|------|----------|
| "做一个订单详情" | — | **必问** |
| "订单页" | — | **必问** |
| "预付费订单" | prepay | 不问 |
| "后付费账单" | postpay | 不问 |

`AskUserQuestion` 时给两个预览描述：

| 选项 | 描述 |
|------|------|
| **预付费（prepay）** | 固定周期计费，显示订单周期、到期时间和固定金额。适合包月/包年产品。画布 641 高。 |
| **后付费（postpay）** | 按用量计费，显示用量统计和账单明细。适合按量付费产品。画布 693 高。 |
