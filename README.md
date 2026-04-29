# 👨‍⚖️ tyc-vibe-lawyering

> **律师工作流 AI SKILL 集** — 天眼查 OpenAPI + MCP 协议驱动的 3 个律师向 SKILL

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)
[![MCP](https://img.shields.io/badge/MCP-TYC%20天眼查-orange.svg)](https://agent.tianyancha.com)

---

## 1. 项目简介

`tyc-vibe-lawyering` 为律师事务所、法律尽调团队、合同审查律师、法律研究员提供 AI Agent SKILL 集，覆盖：

- 法律尽职调查（10 章底稿 + 报告）
- 合同审查（批注 docx + 六维交易对手核查）
- 法律研究（关联企业案例检索 + 法律意见书）

---

## 2. 核心价值

```
+---------------------------------------------------------------+
|   律师专业产出 · 天眼查数据驱动                               |
|                                                               |
|   /tyc-legal-dd          → 10 章法律尽调底稿 + docx 报告       |
|   /tyc-contract-review   → 合同审查 docx + 批注 + 业务流程图   |
|   /tyc-legal-research    → 法律研究 + 关联案例检索             |
|                                                               |
|   零幻觉：所有企业信息来自天眼查官方接口                       |
|   可追溯：每条证据引用自动生成索引                             |
|                                                               |
+---------------------------------------------------------------+
```

---

## 3. 快速开始（2 种装法选一）

### 方式 A · bash 一键脚本（推荐 · 30 秒搞定）

```bash
bash <(curl -sL https://raw.githubusercontent.com/tyc-opensource/tyc-vibe-lawyering/main/install_tyc_mcp.sh)
```

脚本会：① 提示输入 `TYC_MCP_API_KEY`（[申请](https://agent.tianyancha.com)）→ ② 自动写入 `~/.claude/.mcp.json` → ③ 复制 SKILL 到 `~/.claude/skills/tyc-*` → ④ 复制命令到 `~/.claude/commands/`。完成后**重启 Claude Code** 即可使用。

### 方式 B · 本地 plugin-dir（开发 / 调试）

```bash
git clone https://github.com/tyc-opensource/tyc-vibe-lawyering.git
cd tyc-vibe-lawyering
export TYC_MCP_API_KEY="your_api_key_here"
claude --plugin-dir .
```

启动后项目级 `.mcp.json` 自动加载。

---

### 试用

```
/tyc-legal-dd 某拟 IPO 公司
/tyc-contract-review 合同.docx --party 交易对方名
/tyc-legal-research 某涉诉企业 --mode case
```

---

## 4. SKILL / Command 列表（3 个）

| # | 命令 | 名称 |
|---|------|------|
| 1 | `/tyc-legal-dd` | 法律尽职调查（10 章底稿）|
| 2 | `/tyc-contract-review` | 合同审查（批注 docx）|
| 3 | `/tyc-legal-research` | 法律研究（关联案例检索）|

### 目录结构

```
tyc-vibe-lawyering/
├── commands/
│   ├── tyc-legal-dd.md
│   ├── tyc-contract-review.md
│   └── tyc-legal-research.md
├── skills/
│   ├── legal-dd/
│   │   ├── SKILL.md
│   │   ├── references/       # 10 章法律底稿模板
│   │   └── scripts/          # docx 输出
│   ├── contract-review/
│   │   ├── SKILL.md
│   │   ├── references/       # docx 批注模板 + 风险词库
│   │   └── scripts/          # docx 读写 / Mermaid 生成
│   └── legal-research/
│       ├── SKILL.md
│       ├── references/
│       └── scripts/
├── .mcp.json
├── LICENSE                   # MIT
└── README.md
```

---

## 5. 典型场景

### 场景 A: 拟 IPO 法律尽调 10 章底稿

```
/tyc-legal-dd 某拟 IPO 公司
  ↓ 10 板块并发调用：
  ↓ 主体 / 股权 / 治理 / 资产 / 经营 / 财税 / 劳动 / 债权 / 诉讼 / 其他
Claude: 输出 10 章底稿（draft 模式）
       → 可继续执行 /tyc-legal-dd {公司} --phase report 生成客户交付报告
```

### 场景 B: 合同审查带批注

```
用户：/tyc-contract-review my-contract.docx --party "乙方公司名"
  ↓ Layer 0-3：主体 / 文本 / 商务 / 法律 四层审查
  ↓ 合并 6 维对手风险
Claude: 生成 3 份交付物
  - my-contract_审核版.docx（原文 + 批注，reviewer 按 🔴🟡🔵 标注）
  - 合同概要.docx
  - 综合审核意见.docx
  - 业务流程图.mmd（Mermaid）
```

### 场景 C: 涉诉企业法律研究

```
/tyc-legal-research 某涉诉企业 --mode case
  ↓ 关联企业案例检索（同一实控人下兄弟公司）
Claude: 输出判例分析报告 + 证据索引
```

---

## 6. 天眼查 MCP 集成

参考 [docs/MCP_CONFIGURATION.md](docs/MCP_CONFIGURATION.md)。

主要使用工具：

| 用途 | 工具 |
|------|------|
| 主体识别 | `get_company_registration_info` |
| 10 章尽调 | company + risk + ipr + operation + executive + history（全维度）|
| 关联企业案例 | `get_actual_controller` → `get_controlled_companies` → 兄弟公司案件聚合 |

---

## 7. 与 [`tyc-legal-assistant`](../tyc-legal-assistant/) 边界

| 维度 | `tyc-vibe-lawyering` | `tyc-legal-assistant` |
|------|---------------------|----------------------|
| 定位 | 律师专业产出 | 法务 / 合规自助 |
| 合同审查 | `/tyc-contract-review` 带批注 docx | `/tyc-contract-party` 只做主体核验 |
| 产出形式 | docx + references + scripts | 纯 Markdown 报告 |
| 耗时 | 3-5 分钟 | 30 秒 |

---

## 8-12. 配置 / FAQ / 贡献 / License / 致谢 / 联系

MIT License · [LICENSE](LICENSE) · contact@tianyancha.com
