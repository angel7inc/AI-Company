---
id: consumer-analysis
title: 消費者分析手順
category: research
tags: [consumer, persona, pain-point]
owner: intelligence-consumer
status: draft
priority: high
risk_level: low
version: 1
frequency: weekly
estimated_time: "1時間"
requires_ceo_approval: false
automation_possible: true
automation_status: not-automated
related_sop: [market-analysis, trend-analysis, insight-synthesis]
related_knowledge: [psychology, research-methodology]
sensitivity: internal
last_execution: null
success_rate: null
created_at: "2026-07-12"
updated_at: "2026-07-12"
review_due: "2027-01-12"
---

# 消費者分析手順

## Purpose(目的)
悩み・ペルソナ・行動を継続的に調査し、各チャネルのコンテンツ企画・各ブランドの`pain-point-categories.yaml`の妥当性検証に役立てる。

## Scope(適用範囲)
チャネル横断の消費者理解。Instagram固有のカルーセルフォーマット分析は対象外(→`ig-carousel-intelligence`)。

## Trigger(実行のきっかけ)
週次。

## Inputs(必要な入力)
- 各ブランドの`pain-point-categories.yaml`・`brand-brief.md`のペルソナ

## Outputs(成果物)
- 消費者分析レポート(`intelligence-insight-synthesizer`へ提供)
- `pain-point-categories.yaml`の更新提案(`status: draft`→`validated`への検証)

## Tools(使用ツール)
Claude Code、Perplexity。高難度調査の場合のみGemini Deep Research。

## Preconditions(前提条件)
なし

## Procedure(手順)
1. 対象ブランド・ペルソナを確認する
2. 悩み・行動パターンを調査する
3. 既存`pain-point-categories.yaml`との整合を確認し、妥当性が検証できたカテゴリは`status: validated`への更新を提案する
4. `intelligence-research-quality-checker`のチェックを受ける
5. `intelligence-insight-synthesizer`へ提供する

## Checklist(実行時チェックリスト)
- [ ] 既存ペルソナ・悩みカテゴリと照合したか
- [ ] 品質チェックを受けたか

## Quality Standard(品質基準)
実在する悩みの言語化に基づき、断定的な決めつけをしていないこと。

## Failure Pattern(失敗パターン)
少数の事例から一般化しすぎる。

## Handoff(引き継ぎ)
`intelligence-insight-synthesizer`、対象ブランドの`pain-point-categories.yaml`

## Revision History(変更履歴)
| 版 | 日付 | 変更内容 | 変更者 |
|---|---|---|---|
| 1 | 2026-07-12 | 初版作成 | COO |
