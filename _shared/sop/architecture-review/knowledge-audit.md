---
id: knowledge-audit
title: Knowledge Layer監査手順
category: architecture-review
tags: [audit, knowledge, duplication]
owner: architecture-review-controller
status: draft
priority: medium
risk_level: low
version: 1
frequency: quarterly
estimated_time: "1〜2時間"
requires_ceo_approval: false
automation_possible: true
automation_status: not-automated
related_sop_ids: [sop-audit, cross-duplication-audit]
related_sop_categories: [management]
related_knowledge: []
sensitivity: internal
last_execution: null
success_rate: null
created_at: "2026-07-13"
updated_at: "2026-07-13"
review_due: "2027-01-13"
---

# Knowledge Layer監査手順

## Purpose(目的)
Knowledge Layerの重複・機密区分の妥当性・相互参照の有効性・命名規則準拠を全件点検する。

## Scope(適用範囲)
`_shared/knowledge/`配下の全カテゴリ・全記事。**検出・報告のみ**。記事の統合・修正の実行は含まない。

## Trigger(実行のきっかけ)
四半期レビュー、または大規模なKnowledge追加(新Division新設等)の後。

## Inputs(必要な入力)
- `_shared/knowledge/`配下の全カテゴリREADME・全記事
- `_shared/knowledge/knowledge-index.md`

## Outputs(成果物)
- 監査レポート(重複候補・sensitivity不整合・無効な相互参照・命名違反の一覧)

## Tools(使用ツール)
Claude Code

## Preconditions(前提条件)
なし

## Procedure(手順)
1. 全カテゴリを走査し、`knowledge-index.md`のカテゴリ表示順と実フォルダの一致を確認する
2. カテゴリ間で重複しているテーマがないか確認する(同じ内容が2カテゴリにまたがっていないか)
3. 各記事(存在する場合)の`sensitivity`区分が内容と整合しているか確認する
4. 各カテゴリREADMEの「関連Knowledge」リンクが実在するカテゴリを指しているか確認する
5. カテゴリ名が小文字ケバブケースであるか確認する(`docs/naming-conventions.md`準拠)
6. 発見事項を監査レポートにまとめ、`sop/management/review-cycle.md`の次回レビューへ提供する

## Checklist(実行時チェックリスト)
- [ ] 全カテゴリを走査したか(サンプリングではなく全件)
- [ ] 重複・sensitivity不整合・無効リンク・命名違反をそれぞれ確認したか

## Quality Standard(品質基準)
発見事項はすべて具体的なファイルパスとともに報告されること。

## Failure Pattern(失敗パターン)
一部のカテゴリのみ抜き打ちで確認し、全件走査を怠ってしまう。

## Handoff(引き継ぎ)
`sop/management/review-cycle.md`の四半期レビュー。是正の実行は各Division・COOが別途二段階承認プロセスで行う。

## Revision History(変更履歴)
| 版 | 日付 | 変更内容 | 変更者 |
|---|---|---|---|
| 1 | 2026-07-13 | 初版作成 | COO |
