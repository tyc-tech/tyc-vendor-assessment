# 9 维度供应商评估模型

## 模型概览

`/tyc-vendor` 采用 9 维度加权评分模型，综合供应商工商基础、财务表现、合规健康度、技术实力与供应链能力。

```
┌─────────────────────────────────────────────────────────────┐
│          9 维度（总权重 100%）                                │
├─────────────────────────────────────────────────────────────┤
│                                                              │
│  基础层 3 维（35%）      合规层 3 维（35%）    实力层 3 维（30%） │
│  ─────────────           ─────────────        ───────────── │
│  ① 工商资质  10%          ④ 司法失信  15%      ⑦ 知识产权  10% │
│  ② 股权实控  10%          ⑤ 经营异常  10%      ⑧ 招投标   10% │
│  ③ 财务能力  15%          ⑥ 税务合规  10%      ⑨ 供应链    10% │
│                                                              │
└─────────────────────────────────────────────────────────────┘
```

## 各维度详解

### ① 基础工商资质 · 10%

| 子项 | T1.1 工具 |
|------|----------|
| 注册资本 / 实缴 | `get_company_registration_info` |
| 存续状态 | `get_company_registration_info` |
| 成立年限 | `get_company_registration_info` |
| 变更频率 | `get_change_records` |
| 分支机构 | `get_branches` |

### ② 股权与实控人 · 10%

| 子项 | T1.1 工具 |
|------|----------|
| 股权分散度 | `get_shareholder_info` |
| 实控人识别 | `get_actual_controller` |
| UBO 穿透层数 | `get_beneficial_owners` |
| 实控稳定性 | `get_historical_shareholders` |

### ③ 财务能力 · 15%

| 子项 | T1.1 工具 |
|------|----------|
| 营收规模 | `get_financial_data` |
| 净利润 / 毛利率 | `get_financial_data` |
| 资产负债率 | `get_financial_data`（上市）/ `get_financial_summary` |
| 参保人数 / 规模 | `get_company_scale` |

### ④ 司法 / 失信风险 · 15%

| 子项 | T1.1 工具 |
|------|----------|
| 失信被执行 | `get_dishonest_info` |
| 被执行记录 | `get_judgment_debtor_info` |
| 限高记录 | `get_high_consumption_restriction` |
| 裁判文书数量 | `get_judicial_documents` |
| 终本案件 | `get_terminated_cases` |

### ⑤ 经营异常 · 10%

| 子项 | T1.1 工具 |
|------|----------|
| 经营异常名录 | `get_business_exception` |
| 严重违法 | `get_serious_violation` |
| 行政处罚 | `get_administrative_penalty` |
| 环保处罚 | `get_environmental_penalty` |

### ⑥ 税务合规 · 10%

| 子项 | T1.1 工具 |
|------|----------|
| 欠税记录 | `get_tax_arrears_notice` |
| 税收违法 | `get_tax_violation` |
| 纳税人等级 | `get_taxpayer_info` |
| 进出口信用 | `get_import_export_credit` |

### ⑦ 知识产权实力 · 10%

| 子项 | T1.1 工具 |
|------|----------|
| 专利总量 / 发明比例 | `get_patent_info` |
| 商标总量 | `get_trademark_info` |
| 软件著作权 | `get_software_copyright_info` |
| 创新力评分 | `get_ipr_score` |

### ⑧ 招投标中标表现 · 10%

| 子项 | T1.1 工具 |
|------|----------|
| 中标次数 / 金额 | `get_bidding_info` |
| 资质 / 许可 | `get_qualifications` / `get_administrative_license` |
| 荣誉 / 榜单 | `get_honor_info` / `get_ranking_list_info` |
| 信用评价 | `get_credit_evaluation` |

### ⑨ 供应链特有 · 10%

| 子项 | T1.1 工具 |
|------|----------|
| 上下游关系集中度 | `get_suppliers_and_customers` |
| 产品线 | `get_products_info` |
| 地域分布 | `get_branches` / `get_company_location` |
| 物流覆盖（海关信用）| `get_import_export_credit` |

## 综合评级

| 分数区间 | 评级 | 建议 |
|---------|------|------|
| 90-100 | AAA | 战略合作、长期绑定 |
| 80-89 | AA | 核心供应商 |
| 70-79 | A | 常规合作 |
| 60-69 | BBB | 加强监控 |
| 50-59 | BB | 限制使用 |
| < 50 | B 或以下 | 谢绝 / 替换 |
