---
id: cross-duplication-audit
title: 横断的重複検出手順
category: architecture-review
tags: [audit, duplication, cross-layer]
owner: architecture-review-controller
status: draft
priority: high
risk_level: medium
version: 1
frequency: quarterly
estimated_time: "2〜3時間"
requires_ceo_approval: false
automation_possible: false
automation_status: not-automated
related_sop_ids: [knowledge-audit, sop-audit, agent-registry-audit]
related_sop_categories: [management]
related_knowledge: []
sensitivity: internal
last_execution: null
success_rate: null
created_at: "2026-07-13"
updated_at: "2026-07-13"
review_due: "2027-01-13"
---

# 横断的重複検出手順

## Purpose(目的)
Agent・Knowledge・SOPを横断して、機能的に重複するもの(同じ仕事をする2つのエージェント、同じ内容を扱う複数のKnowledge記事、同じ手順を重複して定義するSOP)を検出する。これまで各Phase設計時にCOOが手動で行ってきた重複チェックを、正式な監査手順として制度化したもの。

## Scope(適用範囲)
`_company/org/agents.yaml`・`_shared/knowledge/`・`_shared/sop/`全体。**検出・報告のみ**。

## Trigger(実行のきっかけ)
四半期レビュー、または新規Division追加の後。

## Inputs(必要な入力)
- `knowledge-audit.md`・`sop-audit.md`・`agent-registry-audit.md`の結果
- 各エージェントの役割定義

## Outputs(成果物)
- 横断的重複レポート(統合候補・境界の明確化が必要な項目の一覧)

## Tools(使用ツール)
Claude Code。判断が難しい境界事例は、必要に応じてGemini Deep Research等での比較検討を行ってもよい(`ai-usage-policy.md`の基準に該当する場合)。

## Preconditions(前提条件)
`knowledge-audit.md`・`sop-audit.md`・`agent-registry-audit.md`が完了していること。

## Procedure(手順)
1. 各エージェントの役割定義(「役割」欄)を比較し、機能的に重複するものがないか確認する
2. 重複の疑いがある場合、過去の設計書(`docs/phase*-*.md`)で既に検討・解消済みでないか確認する(蒸し返しを避ける)
3. Knowledge記事・SOP手順についても同様に、同じ内容を扱う複数の記事・手順がないか確認する
4. 発見した重複候補について、どちらを正本とし、どちらを統合・退役すべきかの提案を作成する(実行はしない)
5. 監査レポートにまとめ、`sop/management/review-cycle.md`の次回レビューへ提供する

## Checklist(実行時チェックリスト)
- [ ] 過去の設計書で既に解消済みの重複でないか確認したか
- [ ] 統合提案(実行はしない)を含めたか

## Quality Standard(品質基準)
重複候補には、判断根拠(役割定義の類似点)を明記すること。

## Failure Pattern(失敗パターン)
既に過去のPhaseで意図的に分離した機能(例: Carousel IntelligenceとCompetitor Intelligence)を、重複と誤認してしまう。

## Handoff(引き継ぎ)
`sop/management/review-cycle.md`の四半期レビュー。統合・退役の実行判断はCEO確認事項とする。

## Revision History(変更履歴)
| 版 | 日付 | 変更内容 | 変更者 |
|---|---|---|---|
| 1 | 2026-07-13 | 初版作成 | COO |
