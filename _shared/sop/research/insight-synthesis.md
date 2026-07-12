---
id: insight-synthesis
title: Insight統合・配信手順
category: research
tags: [synthesis, distribution]
owner: intelligence-insight-synthesizer
status: draft
priority: high
risk_level: medium
version: 1
frequency: weekly
estimated_time: "30分"
requires_ceo_approval: false
automation_possible: true
automation_status: not-automated
related_sop: [market-analysis, consumer-analysis, trend-analysis, management]
related_knowledge: []
sensitivity: internal
last_execution: null
success_rate: null
created_at: "2026-07-12"
updated_at: "2026-07-12"
review_due: "2027-01-12"
---

# Insight統合・配信手順

## Purpose(目的)
market/consumer/trend/competitor/traffic/search-researchの出力を統合し、各チャネルのコンテンツ制作へ配信する。**自らリサーチは行わない。**

## Scope(適用範囲)
既存レポートの統合・配信のみ。新規調査の実施は対象外。

## Trigger(実行のきっかけ)
週次(各Intelligenceエージェントのレポート完成後)。

## Inputs(必要な入力)
- `intelligence-market`・`intelligence-consumer`・`intelligence-trend`のレポート
- `shared-competitor-intelligence`・`shared-traffic-intelligence`・`seo-research`の出力

## Outputs(成果物)
- チャネル別配信用インサイト(Instagram/SEO・WordPress/LINE、将来YouTube/Pinterest)
- `management-strategy`への月次インプット

## Tools(使用ツール)
Claude Code

## Preconditions(前提条件)
統合対象のレポートが出揃っていること。

## Procedure(手順)
1. 各Intelligenceエージェントのレポートを収集する
2. 矛盾・重複を確認し、統合する(新規リサーチは行わない)
3. チャネルごとに関連するインサイトを抽出し、`ig-content-planning`・`seo-keyword-strategy`・`revenue-crm-manager`等へ配信する
4. 月次で`management-strategy`へも共有する

## Checklist(実行時チェックリスト)
- [ ] 新規リサーチを行わず、既存レポートの統合に留めたか
- [ ] 各チャネルへ関連インサイトのみを配信したか(無関係な情報を送っていないか)

## Quality Standard(品質基準)
各チャネル担当が「読んですぐ企画に使える」形に整理されていること。

## Failure Pattern(失敗パターン)
複数レポートを単に貼り合わせるだけになり、実質的な統合価値を生んでいない(この場合、`intelligence-market`への統合を再検討する)。

## Handoff(引き継ぎ)
各チャネルの企画担当エージェント、`management-strategy`

## Revision History(変更履歴)
| 版 | 日付 | 変更内容 | 変更者 |
|---|---|---|---|
| 1 | 2026-07-12 | 初版作成 | COO |
