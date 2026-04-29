---
name: tyc-contract-review
description: 合同审核与主体核验一体化工具，AI 审合同同步核验双方主体工商状态与 34 类司法风险
category: legal
version: 1.0
---

# 合同核验 (contract-review)

## 触发条件

合同签署前对相对方主体真实性 + 司法风险一并核验，与 AI 合同审阅工具集成。

关键词：合同审核、主体核验、签约前核查、抗 AI 幻觉

## 输入要求

- **searchKey** (必填): 合同对方企业名称 或 信用代码

## 执行流程

### Step 1: 主体核验
- `get_company_registration_info` — 工商登记 + 经营状态
- `verify_company_accuracy` — 三要素核验

### Step 2: 经营异常红线
- `get_business_exception`
- `get_serious_violation`
- `get_administrative_penalty`

### Step 3: 司法红线
- `get_dishonest_info`
- `get_judgment_debtor_info`
- `get_judicial_documents`
- `get_high_consumption_restriction`

### Step 4: 资产风险
- `get_equity_freeze`
- `get_chattel_mortgage_info`
- `get_judicial_auction`

### Step 5: 综合
- `get_risk_overview`

## 输出格式

```markdown
# 合同主体核验批注 — {name}

## 高风险标注
🚨 营业执照已注销 / 吊销
🚨 失信被执行 ({n} 条)
🚨 大额被执行 (累计金额: ...)
🚨 股权被冻结 / 司法拍卖中

## 中风险标注
⚠️ 经营异常 ({n} 条)
⚠️ 行政处罚 ({n} 条)
⚠️ 主要人员变动频繁

## 一致性标注
✓ 三要素核验一致
✓ 工商状态: {regStatus}

## 签约建议
- 是否建议签约: ✓ 是 / ❌ 否 / ⚠️ 加保函
- 风险条款建议: ...
- 履约监控建议: ...
```

## 示例

输入: `searchKey = "合同甲方企业名称"`
