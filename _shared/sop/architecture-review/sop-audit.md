---
id: sop-audit
title: SOP Layer監査手順
category: architecture-review
tags: [audit, sop, boundary]
owner: architecture-review-controller
status: draft
priority: medium
risk_level: medium
version: 1
frequency: quarterly
estimated_time: "1〜2時間"
requires_ceo_approval: false
automation_possible: true
automation_status: not-automated
related_sop_ids: [knowledge-audit, cross-duplication-audit]
related_sop_categories: [management]
related_knowledge: []
sensitivity: internal
last_execution: null
success_rate: null
created_at: "2026-07-13"
updated_at: "2026-07-13"
review_due: "2027-01-13"
---

# SOP Layer監査手順

## Purpose(目的)
SOP Layerの「定義」と「実行」の境界遵守・CEO承認ステップの欠落有無・命名規則準拠を全件点検する。

## Scope(適用範囲)
`_shared/sop/`配下の全カテゴリ・全手順ファイル。**検出・報告のみ**。

## Trigger(実行のきっかけ)
四半期レビュー、または新規SOPカテゴリ追加の後。

## Inputs(必要な入力)
- `_shared/sop/`配下の全カテゴリREADME・全手順ファイル
- `_shared/sop/sop-index.md`

## Outputs(成果物)
- 監査レポート(定義/実行の境界違反・CEO承認欠落・命名違反の一覧)

## Tools(使用ツール)
Claude Code

## Preconditions(前提条件)
なし

## Procedure(手順)
1. 各SOPカテゴリのREADMEが「書いてはいけないもの」を明記しているか確認する
2. 支出・公開・アカウント変更等、CEO承認が必要な操作を含む手順に、承認確認ステップが明記されているか確認する(`requires_ceo_approval`フィールドとの整合を含む)
3. 「定義」カテゴリ(例: `product/`・`finance/`)に実行手順が紛れ込んでいないか確認する
4. カテゴリ名・手順ファイル名が命名規則に準拠しているか確認する
5. `sop-index.md`のカテゴリ表と実フォルダの一致を確認する
6. 発見事項を監査レポートにまとめ、`sop/management/review-cycle.md`の次回レビューへ提供する

## Checklist(実行時チェックリスト)
- [ ] 全カテゴリを走査したか
- [ ] 定義/実行の境界違反を確認したか
- [ ] CEO承認ステップの欠落を確認したか

## Quality Standard(品質基準)
発見事項はすべて具体的なファイルパスとともに報告されること。

## Failure Pattern(失敗パターン)
CEO承認が必要な操作(支出・公開等)を見落とし、承認ステップの欠落に気づかない。

## Handoff(引き継ぎ)
`sop/management/review-cycle.md`の四半期レビュー。

## Revision History(変更履歴)
| 版 | 日付 | 変更内容 | 変更者 |
|---|---|---|---|
| 1 | 2026-07-13 | 初版作成 | COO |
