---
name: tyc-vendor
description: 采购准入阶段的供应商深度尽调工具，9 维度评估覆盖 34 类中国特有风险信号
category: supply
version: 1.0
---

# 供应商准入评估

## 触发条件

新供应商首次入库、采购评审委员会决策前的合规审查、关键供应商资格审定。

关键词：供应商准入、采购合规、9 维评估、采购评审

## 输入要求

- **searchKey** (必填): 供应商企业名称 或 信用代码

## 执行流程

### Step 1: 主体合规（维度 1-2）
- `get_company_registration_info` — 工商
- `verify_company_accuracy`

### Step 2: 经营异常（维度 3-4）
- `get_business_exception`
- `get_serious_violation`

### Step 3: 司法风险（维度 5-6）
- `get_dishonest_info`
- `get_judgment_debtor_info`

### Step 4: 财税合规（维度 7）
- `get_administrative_penalty`
- `get_tax_arrears_notice`

### Step 5: 资质能力（维度 8）
- `get_qualifications`
- `get_administrative_license`

### Step 6: 综合风险（维度 9）
- `get_risk_overview`

## 输出格式

```markdown
# 供应商准入评估 — {name}

## 9 维风险评分卡
| 维度 | 评分 (0-100) | 说明 |
|------|-------------|------|
| 1. 主体合规 | ... | ... |
| 2. 三要素一致性 | ... | ... |
| 3. 经营异常 | ... | ... |
| 4. 严重违法 | ... | ... |
| 5. 失信 | ... | ... |
| 6. 被执行 | ... | ... |
| 7. 财税合规 | ... | ... |
| 8. 资质能力 | ... | ... |
| 9. 综合风险 | ... | ... |
| **加权总分** | **xx** | **评级 X** |

## 准入决策
- 准入决策: ✓ 通过 / ⚠️ 加保函 / ❌ 拒绝
- 关键风险点: ...
- 合同条款建议: ...
```

## 示例

输入: `searchKey = "某供应商企业"`
