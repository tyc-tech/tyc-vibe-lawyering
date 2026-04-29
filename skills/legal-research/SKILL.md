---
name: tyc-legal-research
description: 法律研究（案例检索 / 法律意见书 / 合规研究）— 关联企业案例自动检索 + 涉诉风险分析
category: legal
version: 1.0
---

# 法律研究

## 触发条件

律师案例检索 / 法律意见书撰写 / 监管合规研究 / 涉诉企业深度分析时触发。

关键词：法律研究、案例检索、法律意见书、判例分析、企业合规研究

## 输入要求

- **target** (必填): 研究对象，可为企业名称 / 信用代码 / 案号 / 法律问题关键词
- **mode** (可选): `case`（企业案例检索）/ `legal-opinion`（法律意见书）/ `compliance`（合规研究），默认 case

## 执行流程

### 模式 A: 企业案例检索（case）

1. 识别主体 → `get_company_registration_info <target>`
2. 涉诉全景 → `get_judicial_documents` / `get_judicial_case`
3. 重大案件 → `get_lawsuit_detail <caseNo>` × TOP 10
4. 执行端 → `get_dishonest_info` / `get_judgment_debtor_info`
5. 案件拓展（同一实控人下关联主体的判例）
   - `get_actual_controller` 获取实控人
   - `get_controlled_companies` 获取兄弟公司
   - 对兄弟公司并发调用 `get_judicial_documents`，识别共性案由
6. 历史趋势 → `get_historical_judicial_docs`

### 模式 B: 法律意见书（legal-opinion）

1. 主体资格核验（同板块 1）
2. 重大合规事项扫描：
   - `get_administrative_penalty` — 行政处罚
   - `get_business_exception` — 经营异常
   - `get_environmental_penalty` — 环保
   - `get_tax_violation` — 税务
3. 资质有效性：`get_qualifications` / `get_administrative_license`
4. 形成意见书章节化模板

### 模式 C: 合规研究（compliance）

1. 行业参照企业（同类资质 / 同品类）：
   - `search_companies_by_industry_region`
   - `search_companies_by_tag`
2. 合规基准：收集同行近 3 年行政处罚、涉诉、经营异常典型样本
3. 对比研究输出

## 输出格式

```markdown
# 法律研究报告 — {target}

> 律所: ... · 研究员: ... · 出具: {ISO8601}
> 研究模式: {case / legal-opinion / compliance}

## 一、研究背景
## 二、法律问题
## 三、证据与数据
### 3.1 主体情况
### 3.2 案件 / 处罚 / 合规事项清单（含证据来源）
### 3.3 关联企业扩展（同一实控人下类似案件）

## 四、判例分析
| 案号 | 案由 | 标的 | 判决要点 | 对本案启示 |
|-----|------|------|---------|-----------|

## 五、法律意见 / 研究结论
- 法律依据: ...
- 论证过程: ...
- 结论与建议: ...
- 风险提示: ...

## 六、证据索引（可追溯来源）
- {证据 1}: 天眼查 - 裁判文书 - {caseNo}
- {证据 2}: 天眼查 - 行政处罚 - {docId}
...
```

## 错误处理

- `_empty` → 对应章节标注"经查未发现相关记录"
- 案件数量 > 100 → 按年度 + 案由做聚合摘要，TOP 20 取详情
- 单案件详情失败 → 标注 `[!]` 跳过，不影响其他

## 示例

- 输入: `target = "某涉诉企业"`, `mode = "case"`
- 输入: `target = "食品经营许可合规性研究"`, `mode = "compliance"`

## 与其他 Skill 的关系

- **法律尽调 10 章底稿** → `/tyc-legal-dd`
- **纯诉讼风险分析**（投资视角）→ `/tyc-litigation-analysis` (invest)
- **关联方穿透** → `/tyc-related-party` (invest)
