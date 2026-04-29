---
name: tyc-legal-dd
description: 法律尽职调查（10 大板块底稿 + 报告）— 主体 / 股权 / 治理 / 资产 / 经营 / 财税 / 劳动 / 债权 / 诉讼 / 其他
category: legal
version: 1.0
---

# 法律尽职调查

## 触发条件

律师事务所承接法律尽调项目、拟 IPO 法律意见书撰写、并购交易买方法律尽调、合规审查项目启动时触发。

关键词：法律尽调、DD 底稿、尽调报告、法律意见书、LDD

## 输入要求

- **searchKey** (必填): 目标企业名称 或 信用代码
- **phase** (可选): `draft` 底稿模式（律师内部记录） / `report` 报告模式（客户交付），默认 draft

## 执行流程（10 大板块并发调用）

### 板块 1: 主体资格
- `get_company_registration_info` — 工商登记
- `get_actual_controller` — 实控人核验

### 板块 2: 股权结构
- `get_shareholder_info` — 股东构成
- `get_equity_tree` — 多层股权树
- `get_beneficial_owners` — UBO

### 板块 3: 公司治理
- `get_key_personnel` — 董事 / 监事 / 高管
- `get_branches` — 分支机构

### 板块 4: 核心资产
- `get_external_investments` — 对外投资
- `get_patent_info` / `get_trademark_info` / `get_software_copyright_info` — 知产资产
- `get_land_mortgage_info` — 土地不动产

### 板块 5: 业务经营
- `get_products_info` — 主营业务
- `get_qualifications` — 资质
- `get_administrative_license` — 行政许可

### 板块 6: 财税
- `get_financial_data` — 财务
- `get_tax_arrears_notice` — 欠税
- `get_tax_violation` — 税收违法

### 板块 7: 劳动人事
- `get_company_scale` — 参保人数 / 规模
- `get_recruitment_info` — 招聘动态
- `get_disciplinary_list` — 劳动监察

### 板块 8: 债权债务
- `get_equity_pledge_info` — 股权出质
- `get_chattel_mortgage_info` — 动产抵押
- `get_judgment_debtor_info` — 被执行人

### 板块 9: 诉讼仲裁
- `get_judicial_documents` — 裁判文书
- `get_judicial_case` — 司法案件
- `get_lawsuit_detail` — 重大案件详情
- `get_dishonest_info` — 失信
- `get_administrative_penalty` — 行政处罚

### 板块 10: 其他事项
- `get_risk_overview` — 风险总览
- `get_historical_overview` — 历史信息

## 输出格式

```markdown
# 法律尽职调查底稿 — {name}

> 律所: ... · 项目代号: ... · 撰写律师: AI 辅助 · 出具: {ISO8601}
> 数据来源: 天眼查 OpenAPI（公开商业数据）+ 客户提供资料
> 底稿状态: {draft / report}

## 第一章 主体资格
1. 企业名称: {name}
2. 统一社会信用代码: {creditCode}
3. 注册资本 / 实缴资本: {regCapital} / {actualCapital}
4. 法定代表人: {legalPersonName}
5. 实际控制人: {actualController.name}（累计持股 {totalRatio}）
6. 成立日期: {estiblishTime}
7. 存续状态: {regStatus}

**风险提示**:
- 🔴 高: ...
- 🟡 中: ...

## 第二章 股权结构
（TOP 10 股东表 + 股权穿透 Mermaid 图 + UBO 清单）

## 第三章 公司治理
（三会一层 / 董监高清单 / 分支机构 / 授权链条）

## 第四章 核心资产
4.1 知识产权（专利 {n} / 商标 {n} / 软著 {n}）
4.2 不动产与土地
4.3 对外投资（子公司 {n} 家）

## 第五章 业务经营
5.1 主营业务与产品
5.2 资质许可清单（需说明有效期 / 延续要求）
5.3 行政许可

## 第六章 财税
6.1 财务数据（营收 / 利润 / 资产负债）
6.2 欠税 / 税收违法
**风险评级**: 低 / 中 / 高

## 第七章 劳动人事
7.1 员工规模与参保
7.2 劳动监察黑名单

## 第八章 债权债务
8.1 担保与抵押（股权出质 / 动产抵押）
8.2 被执行人记录

## 第九章 诉讼仲裁
9.1 案件总数与类型分布（原 / 被告位）
9.2 重大案件 TOP 5 深度
9.3 失信与限高

## 第十章 其他事项
10.1 历史信息（工商变更 / 曾用名）
10.2 综合风险

---

## 尽调结论
- **法律风险等级**: 低 / 中 / 高 / 重大
- **重大不利事项**: ...
- **投资 / 交易影响**: 可推进 / 需修订交易结构 / 建议终止
- **后续律师关注点**: 现场访谈、合同调阅、工商档案调档、政府走访
```

## 错误处理

- 单板块数据缺失 → 对应章节标注 `[!] 数据暂不可用，建议现场调档`
- `_empty` → 标注"经查无公开记录"（不影响整体结论）
- tyc `error_code: 300000` → 归一化为"经查无结果"（非报错）

## 示例

输入: `searchKey = "某拟 IPO 公司"`

## 与其他 Skill 的关系

- **投资向快速尽调**（30 秒）→ `/tyc-ic-memo` (invest)
- **关联方穿透** → `/tyc-related-party` (invest)
- **法律研究 + 案例检索** → `/tyc-legal-research`
- **合同审查** → `/tyc-contract-review`（vibe 向，带批注 docx）
