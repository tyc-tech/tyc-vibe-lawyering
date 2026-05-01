---
name: tyc-contract-review
description: 合同审查 · 带批注 docx + 六维交易对手核查
argument-hint: "<合同 docx 路径> [--party 交易对方名]"
---

# /tyc-contract-review · 合同审查（批注 docx）

本命令由 SKILL 文件定义，详见：[`../skills/contract-review/SKILL.md`](../skills/contract-review/SKILL.md)

## 快速使用

```
/tyc-contract-review <searchKey>
```

替换 `<searchKey>` 为目标企业名称或统一社会信用代码。

## 参数与示例

参数与产出格式见 SKILL.md。典型产出：Markdown 报告（可直接上屏或归档）。

## 底层 MCP 调用

通过 `tyc` MCP server（`https://mcp.tianyancha.com/v1`）调用 T1.1 业务语义聚合层工具。

## 相关命令

参见 [`../README.md`](../README.md) SKILL 清单章节。
