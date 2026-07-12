---
id: trend-analysis
title: トレンド分析手順(コンテンツ企画向け)
category: research
tags: [trend, ideation]
owner: intelligence-trend
status: draft
priority: high
risk_level: low
version: 1
frequency: weekly
estimated_time: "30分〜1時間"
requires_ceo_approval: false
automation_possible: true
automation_status: not-automated
related_sop: [market-analysis, consumer-analysis, insight-synthesis]
related_knowledge: [marketing, analytics]
sensitivity: internal
last_execution: null
success_rate: null
created_at: "2026-07-12"
updated_at: "2026-07-12"
review_due: "2027-01-12"
---

# トレンド分析手順(コンテンツ企画向け)

## Purpose(目的)
「何を作るべきか・なぜ今か」という企画示唆を得るため、Google Trends・SNS・ニュースを横断して兆しを発見する。

## Scope(適用範囲)
コンテンツ企画のための質的なトレンド発見。チャネル別の流入ボリューム・アトリビューション分析(量的分析)は対象外(→`shared-traffic-intelligence`)。

## Trigger(実行のきっかけ)
週次。

## Inputs(必要な入力)
- **`shared-traffic-intelligence`の週次レポート(必須。自らGoogle Trends等へ再照会しない)**
- 業界ニュース

## Outputs(成果物)
- トレンド・企画示唆レポート(`intelligence-insight-synthesizer`へ提供)

## Tools(使用ツール)
Claude Code、Google Trends(traffic-intelligenceの出力で不足する場合のみ直接参照)。

## Preconditions(前提条件)
`shared-traffic-intelligence`の直近レポートが存在すること。

## Procedure(手順)
1. `shared-traffic-intelligence`の週次レポートを確認する(量的データの再取得はしない)
2. 業界ニュース・SNSの話題を確認する
3. 「今週何を作るべきか」の企画示唆にまとめる
4. `intelligence-research-quality-checker`のチェックを受ける
5. `intelligence-insight-synthesizer`へ提供する

## Checklist(実行時チェックリスト)
- [ ] `shared-traffic-intelligence`のデータを再利用したか(重複取得していないか)
- [ ] 品質チェックを受けたか

## Quality Standard(品質基準)
単なる話題の羅列ではなく、「なぜ今、何を作るべきか」の示唆になっていること。

## Failure Pattern(失敗パターン)
`shared-traffic-intelligence`と同じ量的分析を重複して行ってしまう。

## Handoff(引き継ぎ)
`intelligence-insight-synthesizer`

## Revision History(変更履歴)
| 版 | 日付 | 変更内容 | 変更者 |
|---|---|---|---|
| 1 | 2026-07-12 | 初版作成 | COO |
