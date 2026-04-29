---
name: tyc-vendor
description: 供应商 9 维度深度评估 + Kraljic 矩阵分类（战略 / 杠杆 / 瓶颈 / 一般）
argument-hint: "<供应商名称或统一社会信用代码>"
---

# /tyc-vendor · 供应商 9 维深度评估

本命令由 SKILL 文件定义，详见：[`../skills/vendor/SKILL.md`](../skills/vendor/SKILL.md)

## 快速使用

```
/tyc-vendor <供应商名称或统一社会信用代码>
```

## 9 维度模型

详见 [`../docs/NINE_DIMENSIONS.md`](../docs/NINE_DIMENSIONS.md) 与 [`../docs/KRALJIC_MATRIX.md`](../docs/KRALJIC_MATRIX.md)。

## 输出

- Markdown 评估报告
- 9 维雷达图说明
- Kraljic 矩阵象限定位
- 综合评级（AAA/AA/A/BBB/BB/B）+ 策略建议

## 底层 MCP 调用

通过 `tyc` MCP server（`https://ai-mcp.tianyancha.com/mcp`）调用 T1.1 业务语义聚合层共 30+ 个工具。
