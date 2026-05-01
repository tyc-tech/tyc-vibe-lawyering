---
name: tyc-legal-dd
description: 法律尽职调查 10 章底稿 + 报告
argument-hint: "<目标企业全称> [--phase draft|report]"
---

# /tyc-legal-dd · 法律尽职调查 10 章底稿

本命令由 SKILL 文件定义，详见：[`../skills/legal-dd/SKILL.md`](../skills/legal-dd/SKILL.md)

## 快速使用

```
/tyc-legal-dd <searchKey>
```

替换 `<searchKey>` 为目标企业名称或统一社会信用代码。

## 参数与示例

参数与产出格式见 SKILL.md。典型产出：Markdown 报告（可直接上屏或归档）。

## 底层 MCP 调用

通过 `tyc` MCP server（`https://mcp.tianyancha.com/v1`）调用 T1.1 业务语义聚合层工具。

## 相关命令

参见 [`../README.md`](../README.md) SKILL 清单章节。
