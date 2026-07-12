---
id: monthly-closing
title: 月次決算手順
category: finance
tags: [accounting, profit]
owner: finance-controller
status: draft
priority: high
risk_level: medium
version: 1
frequency: monthly
estimated_time: "1時間"
requires_ceo_approval: false
automation_possible: true
automation_status: not-automated
related_sop: [cost-revenue-reconciliation, management]
related_knowledge: [finance, analytics]
sensitivity: internal
last_execution: null
success_rate: null
created_at: "2026-07-12"
updated_at: "2026-07-12"
review_due: "2027-01-12"
---

# 月次決算手順

## Purpose(目的)
月次で利益(売上−AI/APIコスト)を算出し、会社全体の財務状況を可視化する。

## Scope(適用範囲)
全ブランド合算の月次利益算出。`_company/finance/actuals/`に記録済みの原始入力値を用いる。`sample_only: true`または`do_not_import: true`が設定されたファイルは対象外とする。

## Trigger(実行のきっかけ)
毎月1回、月初。

## Inputs(必要な入力)
- `_company/finance/actuals/{brand_id}/{当月}.yaml`の原始入力値(gross_revenue_jpy等)
- `management-resource`の当月AI/APIコストデータ(actualsへの転記元)

## Outputs(成果物)
- 月次利益レポート(`_company/kpi/`。**`net_revenue_jpy`・`operating_profit_jpyはactualsへは書き戻さない`**)

## Tools(使用ツール)
Claude Code

## Preconditions(前提条件)
当月の`actuals/{brand_id}/{YYYY-MM}.yaml`に原始入力値が記録済み(`status: confirmed`)であること。

## Procedure(手順)
1. `_company/finance/actuals/`から当月分のファイルを取得する(`sample_only`/`do_not_import`ファイルは除外)
2. 各レコードについて、以下を算出する(actualsへの書き戻しはしない)
   - `net_revenue_jpy = gross_revenue_jpy - platform_fee_jpy - refund_jpy`
   - `operating_profit_jpy = net_revenue_jpy - ad_spend_jpy - ai_api_cost_jpy - outsourcing_fixed_cost_jpy - other_expenses_jpy`
   - `cash_received_jpy`・`cash_paid_out_jpy`はこの計算式に使用しない
3. 全ブランド・全レコードを合算し、会社全体の月次利益を算出する
4. `_company/kpi/`にレポートとして記録する
5. `sop/management/review-cycle.md`の月次レビューへ提供する

## Checklist(実行時チェックリスト)
- [ ] `sample_only`/`do_not_import`ファイルを除外したか
- [ ] 原始入力値のみを使用し、actualsへ導出値を書き戻していないか
- [ ] `_company/kpi/`に記録したか

## Quality Standard(品質基準)
売上・コストの計算元が明確で、二重計算がないこと。

## Failure Pattern(失敗パターン)
`revenue-analytics`・`management-resource`のデータを待たずに概算で決算してしまう。`sample_only`ファイルを誤って実績として集計してしまう。導出値をactualsへ書き戻してしまう。

## Handoff(引き継ぎ)
`sop/management/review-cycle.md`の月次レビュー

## Revision History(変更履歴)
| 版 | 日付 | 変更内容 | 変更者 |
|---|---|---|---|
| 1 | 2026-07-12 | 初版作成 | COO |
| 2 | 2026-07-12 | actuals保存先を`_company/finance/actuals/`に確定。導出値(net_revenue_jpy/operating_profit_jpy)をactualsへ書き戻さない方針に修正 | COO |
