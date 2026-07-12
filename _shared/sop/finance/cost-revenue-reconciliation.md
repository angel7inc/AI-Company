---
id: cost-revenue-reconciliation
title: 売上・コスト突合手順
category: finance
tags: [reconciliation]
owner: finance-controller
status: draft
priority: medium
risk_level: medium
version: 1
frequency: monthly
estimated_time: "30分"
requires_ceo_approval: false
automation_possible: true
automation_status: not-automated
related_sop: [monthly-closing]
related_knowledge: [finance, analytics]
sensitivity: internal
last_execution: null
success_rate: null
created_at: "2026-07-12"
updated_at: "2026-07-12"
review_due: "2027-01-12"
---

# 売上・コスト突合手順

## Purpose(目的)
`revenue-analytics`の売上データと`management-resource`のコストデータの間に、計上漏れ・二重計上がないかを突合する。

## Scope(適用範囲)
月次決算の前段階として実施する、データ整合性の確認。

## Trigger(実行のきっかけ)
月次決算(`monthly-closing.md`)の実行前。

## Inputs(必要な入力)
- `revenue-analytics`の売上データ
- `management-resource`のコストデータ

## Outputs(成果物)
- 突合結果(不整合があれば差し戻し)

## Tools(使用ツール)
Claude Code

## Preconditions(前提条件)
なし

## Procedure(手順)
1. `revenue-analytics`の売上データを確認する
2. `management-resource`のコストデータを確認する
3. 計上漏れ・二重計上がないか突合する
4. 不整合があれば、該当エージェントへ差し戻して確認する
5. 整合が取れたら`monthly-closing.md`の手順へ進む

## Checklist(実行時チェックリスト)
- [ ] 売上・コストのデータ期間が一致しているか
- [ ] 計上漏れ・二重計上がないか

## Quality Standard(品質基準)
月次決算の前に、データの整合性が確認済みであること。

## Failure Pattern(失敗パターン)
突合をせずに決算を進めてしまい、後から数値の誤りが発覚する。

## Handoff(引き継ぎ)
`monthly-closing.md`

## Revision History(変更履歴)
| 版 | 日付 | 変更内容 | 変更者 |
|---|---|---|---|
| 1 | 2026-07-12 | 初版作成 | COO |
