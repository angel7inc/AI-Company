---
id: market-analysis
title: 市場分析手順
category: research
tags: [market, positioning]
owner: intelligence-market
status: draft
priority: medium
risk_level: low
version: 1
frequency: monthly
estimated_time: "1〜2時間"
requires_ceo_approval: false
automation_possible: true
automation_status: not-automated
related_sop_ids: [consumer-analysis, trend-analysis, insight-synthesis]
related_sop_categories: []
related_knowledge: [business-strategy, research-methodology]
sensitivity: internal
last_execution: null
success_rate: null
created_at: "2026-07-12"
updated_at: "2026-07-12"
review_due: "2027-01-12"
---

# 市場分析手順

## Purpose(目的)
市場規模・ポジショニング・成長市場を把握し、事業戦略・コンテンツ企画の判断材料を提供する。

## Scope(適用範囲)
チャネル横断の市場全体像の分析。特定チャネルの流入分析(→`_shared/agents/analytics/shared-traffic-intelligence.md`)は対象外。

## Trigger(実行のきっかけ)
月次レビュー、または新規チャネル・新規オファー検討時。

## Inputs(必要な入力)
- 既存の`business-strategy/`Knowledge
- `shared-competitor-intelligence`の監視結果

## Outputs(成果物)
- 市場分析レポート(`intelligence-insight-synthesizer`・`management-strategy`へ提供)

## Tools(使用ツール)
Claude Code。高難度・大量比較・海外事例調査の場合のみGemini Deep Research(`ai-usage-policy.md`参照)。

## Preconditions(前提条件)
なし

## Procedure(手順)
1. 対象市場・カテゴリを定める
2. 既存Knowledge・過去レポートを確認し、重複調査を避ける
3. 必要に応じてGemini Deep Researchで深掘りする(基準は`ai-usage-policy.md`)
4. `intelligence-research-quality-checker`のチェックを受ける
5. `intelligence-insight-synthesizer`へ提供する

## Checklist(実行時チェックリスト)
- [ ] 既存レポートとの重複を確認したか
- [ ] 品質チェックを受けたか

## Quality Standard(品質基準)
一次情報に基づき、出典が明記されていること。

## Failure Pattern(失敗パターン)
二次情報のみで断定的な結論を出してしまう。

## Handoff(引き継ぎ)
`intelligence-insight-synthesizer`・`management-strategy`

## Revision History(変更履歴)
| 版 | 日付 | 変更内容 | 変更者 |
|---|---|---|---|
| 1 | 2026-07-12 | 初版作成 | COO |
