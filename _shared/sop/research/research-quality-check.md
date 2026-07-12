---
id: research-quality-check
title: リサーチ品質チェック手順
category: research
tags: [quality, fact-check, citation]
owner: intelligence-research-quality-checker
status: draft
priority: high
risk_level: medium
version: 1
frequency: as-needed
estimated_time: "15〜30分"
requires_ceo_approval: false
automation_possible: true
automation_status: not-automated
related_sop: [market-analysis, consumer-analysis, trend-analysis, brand-qa]
related_knowledge: [research-methodology]
sensitivity: internal
last_execution: null
success_rate: null
created_at: "2026-07-12"
updated_at: "2026-07-12"
review_due: "2027-01-12"
---

# リサーチ品質チェック手順

## Purpose(目的)
リサーチ出力の引用漏れ・重複・古い情報・一次情報優先・ファクトチェックを行う。ブランドトーン・AI臭の判定(`shared-brand-quality-checker`の役割)とは独立した、事実正確性の観点のチェック。

## Scope(適用範囲)
`intelligence-market`・`intelligence-consumer`・`intelligence-trend`・`seo-research`等が作成したリサーチレポート。

## Trigger(実行のきっかけ)
各リサーチレポート完成時。

## Inputs(必要な入力)
- チェック対象のリサーチレポート

## Outputs(成果物)
- 合否判定・差し戻し理由

## Tools(使用ツール)
Claude Code

## Preconditions(前提条件)
なし

## Procedure(手順)
1. 引用元が明記されているか確認する
2. 一次情報が優先されているか(二次情報のみで断定していないか)確認する
3. 既存レポートとの重複がないか確認する
4. 情報の鮮度(古くなっていないか)を確認する
5. 事実確認が必要な主張に根拠があるか確認する
6. 不合格の場合、具体的な理由とともに差し戻す

## Checklist(実行時チェックリスト)
- [ ] 引用元が明記されているか
- [ ] 一次情報優先か
- [ ] 重複がないか
- [ ] 情報が古くなっていないか
- [ ] ファクトチェック済みか

## Quality Standard(品質基準)
すべてのチェック項目を満たしていること。

## Failure Pattern(失敗パターン)
二次情報・推測を事実であるかのように記載してしまう。

## Handoff(引き継ぎ)
`intelligence-insight-synthesizer`

## Revision History(変更履歴)
| 版 | 日付 | 変更内容 | 変更者 |
|---|---|---|---|
| 1 | 2026-07-12 | 初版作成 | COO |
