# 🔍 tyc-vendor-assessment

> **单供应商深度评估 AI SKILL** — 天眼查 OpenAPI + MCP 协议驱动，9 维度 + Kraljic 矩阵分类

[![License](https://img.shields.io/badge/License-Apache%202.0-green.svg)](LICENSE)
[![MCP](https://img.shields.io/badge/MCP-TYC%20天眼查-orange.svg)](https://agent.tianyancha.com)

---

## 1. 项目简介

`tyc-vendor-assessment` 是**单 SKILL 专用仓**，为采购品类经理、供应商管理（SQM）、供应商准入评审委员会提供单个供应商的深度 9 维度评估（含 Kraljic 矩阵分类）。

---

## 2. 核心价值

```
+---------------------------------------------------------------+
|   深度评估一个供应商 · 9 维模型 · 3 秒完成                     |
|                                                               |
|   /tyc-vendor 某目标供应商                                     |
|                                                               |
|   9 维度：                                                     |
|   ① 工商资质   ② 股权实控  ③ 财务能力                          |
|   ④ 司法失信   ⑤ 经营异常  ⑥ 税务合规                          |
|   ⑦ 知识产权   ⑧ 招投标表现 ⑨ 供应链特有（产能/产地/物流）     |
|                                                               |
|   + Kraljic 矩阵 4 象限分类：                                  |
|   战略 / 杠杆 / 瓶颈 / 一般 → 对应策略建议                     |
|                                                               |
+---------------------------------------------------------------+
```

---

## 3. 快速开始（2 种装法选一）

### 方式 A · bash 一键脚本（推荐 · 30 秒搞定）

```bash
bash <(curl -sL https://raw.githubusercontent.com/tyc-opensource/tyc-vendor-assessment/main/install_tyc_mcp.sh)
```

脚本会：① 提示输入 `TYC_MCP_API_KEY`（[申请](https://agent.tianyancha.com)）→ ② 自动写入 `~/.claude/.mcp.json` → ③ 复制 SKILL 到 `~/.claude/skills/tyc-*` → ④ 复制命令到 `~/.claude/commands/`。完成后**重启 Claude Code** 即可使用。

### 方式 B · 本地 plugin-dir（开发 / 调试）

```bash
git clone https://github.com/tyc-opensource/tyc-vendor-assessment.git
cd tyc-vendor-assessment
export TYC_MCP_API_KEY="your_api_key_here"
claude --plugin-dir .
```

启动后项目级 `.mcp.json` 自动加载。

---

### 试用

```
/tyc-vendor 某目标供应商
```

---

## 4. SKILL / Command 列表（1 个）

| 命令 | 名称 |
|------|------|
| `/tyc-vendor` | 供应商 9 维深度评估 + Kraljic 矩阵 |

### 目录结构（标准 Claude Code 插件结构）

```
tyc-vendor-assessment/
├── .claude-plugin/
│   └── plugin.json           # Claude Code 插件 manifest
├── .mcp.json                 # MCP 服务配置
├── README.md
├── LICENSE                   # Apache-2.0
├── QUICKSTART.md
├── CHANGELOG.md
├── install_tyc_mcp.sh
├── setup-tyc-env.sh
├── setup-tyc-env.ps1
├── skills/
│   └── vendor/
│       └── SKILL.md          # ← 核心 SKILL
├── commands/
│   └── tyc-vendor.md         # /tyc-vendor 命令入口
├── tyc-mcp-integration/
│   └── README.md             # MCP 集成说明
├── evals/
│   ├── cases.yaml            # ≥ 10 个用例
│   ├── run.py
│   └── trigger_eval_set.json
└── docs/
    ├── NINE_DIMENSIONS.md    # 9 维详细定义
    ├── KRALJIC_MATRIX.md     # Kraljic 矩阵说明
    └── MCP_CONFIGURATION.md
```

---

## 5. 典型场景

### 场景 A: 战略供应商评估

```
/tyc-vendor 某核心供应商
  ↓ 9 维并发扫描
Claude: 输出 9 维雷达图 + Kraljic 矩阵坐标 + 综合评级（A/AA/AAA）+ 策略建议
```

Kraljic 矩阵策略对照：

| 象限 | 特征 | 建议策略 |
|------|------|---------|
| 战略 | 高价值 + 高风险 | 长期合作 + 联合研发 + 深度绑定 |
| 杠杆 | 高价值 + 低风险 | 多源竞标 + 议价 + 招标采购 |
| 瓶颈 | 低价值 + 高风险 | 库存缓冲 + 替代开发 + 合同保障 |
| 一般 | 低价值 + 低风险 | 集中采购 + 标准化 + 电商渠道 |

---

## 6. 9 维度详解

见 [docs/NINE_DIMENSIONS.md](docs/NINE_DIMENSIONS.md)。

| # | 维度 | 权重 | T1.1 工具 |
|---|------|------|-----------|
| 1 | 基础工商资质 | 10% | company |
| 2 | 股权与实控人 | 10% | company + group |
| 3 | 财务能力 | 15% | company 财务分析 |
| 4 | 司法 / 失信风险 | 15% | risk |
| 5 | 经营异常 | 10% | risk |
| 6 | 税务合规 | 10% | risk |
| 7 | 知识产权实力 | 10% | intellectual_property |
| 8 | 招投标中标表现 | 10% | operation |
| 9 | 供应链特有（产能 / 产地 / 物流）| 10% | operation + company |

---

## 7. 与 [`tyc-supply-chain`](../tyc-supply-chain/) 的关系

| 维度 | 本仓（单 SKILL）| `tyc-supply-chain`（6 SKILL）|
|------|---------------|-----------------------------|
| 定位 | 深度评估单个供应商 | 供应商组合管理（准入 / 年审 / 风险 / 支出分析）|
| 适合场景 | 战略供应商评审、单品类深度尽调 | 日常采购运营、季度复盘、年度战略 |
| 使用方式 | 独立装仓 | 独立装仓（与本仓并列安装）|

---

## 8-12. 配置 / FAQ / 贡献 / License / 致谢 / 联系

Apache-2.0 · [LICENSE](LICENSE) · contact@tianyancha.com
