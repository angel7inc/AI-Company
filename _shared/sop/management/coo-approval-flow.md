---
id: coo-approval-flow
title: COO承認フロー(軽微な変更の扱い)
category: management
tags: [approval, governance]
owner: core-coo
status: active
priority: medium
risk_level: low
version: 1
frequency: as-needed
estimated_time: "数分"
requires_ceo_approval: false
automation_possible: true
automation_status: partially-automated
related_sop_ids: [ceo-approval-flow]
related_sop_categories: []
related_knowledge: []
sensitivity: internal
last_execution: null
success_rate: null
created_at: "2026-07-12"
updated_at: "2026-07-12"
review_due: "2027-01-12"
---

# COO承認フロー(軽微な変更の扱い)

## Purpose(目的)
すべての変更をCEO承認の対象にすると、100事業・1000エージェント規模では承認待ちが工数のボトルネックになる。軽微な変更はCOO(Claude Code)の判断で完結させ、CEOの負担を「本当に判断が必要な事項」に集中させる。

## Scope(適用範囲)
既存の承認済みファイルに対する、設計判断を伴わない軽微な変更(誤字脱字修正、既存記述と矛盾しないリンク切れ修正、フォーマットの微調整等)。

## Trigger(実行のきっかけ)
既存ファイルの軽微な不備に気づいたとき。

## Inputs(必要な入力)
- 修正対象ファイル
- 修正内容が「軽微」の基準を満たすことの確認

## Outputs(成果物)
- 修正済みファイル
- `_company/reports/`への軽微修正の記録(月次レビューでまとめて報告)

## Tools(使用ツール)
Claude Code

## Preconditions(前提条件)
以下のいずれにも該当しないこと(該当する場合は`ceo-approval-flow.md`を使う)。
- 新しいアーキテクチャ・Division・エージェントの追加
- 既存の設計判断(カテゴリ構造・エージェントの役割分担・承認ルール等)の変更
- ブランド固有の判断基準(トーン・NG表現・KPI優先順位等)の変更

## Procedure(手順)
1. 修正内容が「軽微な変更」の前提条件を満たすか確認する
2. 満たさない場合は`ceo-approval-flow.md`のGate1から開始する
3. 満たす場合はCOOの判断で修正し、CEOへの個別確認は行わない
4. 修正内容を`_company/reports/`に記録する(週次/月次レビューでまとめて報告)

## Checklist(実行時チェックリスト)
- [ ] 設計判断を伴わない軽微な修正であることを確認したか
- [ ] 修正内容を記録したか

## Quality Standard(品質基準)
CEOが後から月次レビューで「何が軽微修正として処理されたか」を一覧で確認できること。

## Failure Pattern(失敗パターン)
「軽微」の判断を拡大解釈し、実質的な設計変更をCEO承認なしに実施してしまう。

## Handoff(引き継ぎ)
月次レビュー(`review-cycle.md`)で軽微修正の一覧をCEOに共有する。

## Revision History(変更履歴)
| 版 | 日付 | 変更内容 | 変更者 |
|---|---|---|---|
| 1 | 2026-07-12 | 初版作成 | COO |
