---
id: agent-registry-audit
title: Agent台帳監査手順
category: architecture-review
tags: [audit, agents, naming, threshold]
owner: architecture-review-controller
status: draft
priority: high
risk_level: medium
version: 1
frequency: quarterly
estimated_time: "1〜2時間"
requires_ceo_approval: false
automation_possible: true
automation_status: not-automated
related_sop: [knowledge-audit, sop-audit, cross-duplication-audit, management]
related_knowledge: []
sensitivity: internal
last_execution: null
success_rate: null
created_at: "2026-07-13"
updated_at: "2026-07-13"
review_due: "2027-01-13"
---

# Agent台帳監査手順

## Purpose(目的)
`_company/org/agents.yaml`の全行を対象に、`division`/`brand`フィールドの整合性・定義ファイルとの対応・命名規則準拠・行数閾値到達を点検する。SEO Division9エージェントのブランド固定化バグ(Phase8で発見)のような、実行時パラメータ化されるべきエージェントが誤って特定ブランドに固定されている状態を再発見できるようにする。

## Scope(適用範囲)
`_company/org/agents.yaml`の全行。**検出・報告のみ**。

## Trigger(実行のきっかけ)
四半期レビュー、または新規Division追加の後。

## Inputs(必要な入力)
- `_company/org/agents.yaml`
- `docs/naming-conventions.md`
- 各エージェントの`definition:`先ファイル

## Outputs(成果物)
- 監査レポート(フィールド不整合・対応漏れ・命名違反・行数閾値到達の一覧)

## Tools(使用ツール)
Claude Code

## Preconditions(前提条件)
なし

## Procedure(手順)
1. `agents.yaml`の全行を走査する(サンプリングではなく全件)
2. `layer: shared`のエージェントで`brand`が特定ブランドに固定されているものがないか確認する(共有Divisionは`brand: null`が原則。Instagram等の意図的なチャネル専属エージェントは例外として許容する)
3. `_company/agents/`配下のエージェントに`division`フィールドが設定されているか確認する(必須項目)
4. 各行の`definition:`パスにファイルが実在するか確認する
5. エージェントIDが命名規則(`shared-{機能}`または`{division名}-{機能}`)に準拠しているか確認する
6. **`agents.yaml`の行数を数え、50行の閾値に到達・超過していないか確認する。到達している場合は`_company/org/divisions.yaml`新設の検討を監査レポートに明記する**
7. 発見事項を監査レポートにまとめ、`sop/management/review-cycle.md`の次回レビューへ提供する

## Checklist(実行時チェックリスト)
- [ ] 全行を走査したか
- [ ] `brand`固定化バグの有無を確認したか
- [ ] 行数閾値(50行)を確認したか

## Quality Standard(品質基準)
発見事項はすべて該当するエージェントIDとともに報告されること。

## Failure Pattern(失敗パターン)
一部のDivisionのみ確認し、全Divisionを横断した走査を怠ってしまう。行数閾値の確認を忘れる。

## Handoff(引き継ぎ)
`sop/management/review-cycle.md`の四半期レビュー。`divisions.yaml`新設の要否はCEO確認事項として提起する。

## Revision History(変更履歴)
| 版 | 日付 | 変更内容 | 変更者 |
|---|---|---|---|
| 1 | 2026-07-13 | 初版作成 | COO |
