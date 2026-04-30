# TYC MCP 集成说明

## 数据源

本仓 `/tyc-vendor` SKILL 通过单一 MCP server `tyc` 调用天眼查 T1.1 业务语义聚合层 163 个工具。

```
MCP endpoint: https://ai-mcp.tianyancha.com/mcp
Auth:         ${TYC_MCP_API_KEY}
```

## 本 SKILL 使用的工具分布

按 9 维度统计：

| 维度 | 主要工具 | Go 包 |
|------|---------|-------|
| ① 工商资质 | `get_company_registration_info` / `get_change_records` / `get_branches` | company |
| ② 股权 | `get_shareholder_info` / `get_actual_controller` / `get_beneficial_owners` | company |
| ③ 财务 | `get_financial_data` / `get_company_scale` | company |
| ④ 司法 | `get_dishonest_info` / `get_judgment_debtor_info` / `get_judicial_documents` | risk |
| ⑤ 经营异常 | `get_business_exception` / `get_administrative_penalty` | risk |
| ⑥ 税务 | `get_tax_arrears_notice` / `get_tax_violation` / `get_taxpayer_info` | risk / operation |
| ⑦ 知识产权 | `get_patent_info` / `get_trademark_info` / `get_software_copyright_info` / `get_ipr_score` | intellectual_property |
| ⑧ 招投标 | `get_bidding_info` / `get_qualifications` / `get_honor_info` | operation |
| ⑨ 供应链 | `get_suppliers_and_customers` / `get_products_info` / `get_import_export_credit` | operation |

## 请求流程

```
用户 → /tyc-vendor 某供应商
  ↓
Claude Agent → MCP 协议 → TYC MCP Server
  ↓
  并发调用 30+ T1.1 工具
  ↓
  多源合并 + 时间戳格式化 + 元数据注入
  ↓
9 维度评分 + Kraljic 落位
  ↓
Markdown 评估报告
```

## 错误码

- `0` 成功
- `300000` 经查无结果 → 该维度得分 0，不影响其他维度
- 其他 → 报告中对应章节标注 `[!] 数据暂不可用`

## 调试

```bash
# 验证 MCP 连通
curl -H "Authorization: $TYC_MCP_API_KEY" \
  https://ai-mcp.tianyancha.com/mcp

# 查看 tyc-cli（可选，命令行调试工具）
npm install -g tyc-cli
tyc company registration-info "某供应商" --verbose
```
