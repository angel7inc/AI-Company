---
id: review-cycle
title: レビューサイクル(週次/月次/四半期)とエスカレーション基準
category: management
tags: [review, kpi, escalation]
owner: core-coo
status: active
priority: high
risk_level: medium
version: 1
frequency: weekly
estimated_time: "週次30分/月次1〜2時間"
requires_ceo_approval: false
automation_possible: true
automation_status: not-automated
related_sop_ids: [ceo-approval-flow]
related_sop_categories: []
related_knowledge: [business-strategy, analytics]
sensitivity: internal
last_execution: null
success_rate: null
created_at: "2026-07-12"
updated_at: "2026-07-12"
review_due: "2027-01-12"
---

# レビューサイクル(週次/月次/四半期)とエスカレーション基準

## Purpose(目的)
Management Divisionの「経営ガバナンスループ(Loop2)」を、個別コンテンツの制作・承認ループ(Loop1)と分離しつつ、定期的にCEOへ会社全体の状況を届ける仕組みを作る。

## Scope(適用範囲)
全Division(Instagram・SEO・Creative、および将来のRevenue・Automation・Intelligence・Product・Finance・HR)の週次/月次/四半期の状況確認。

## Trigger(実行のきっかけ)
週次・月次の定期タイミング、または下記「エスカレーション基準」に該当する事象の発生時。

## Inputs(必要な入力)
- `management-kpi`が集計した会社/Division/Agent KPI
- `management-resource`が集計したAPIコスト・稼働状況
- `management-quality`のQAチェッカー合格率・月次較正結果

## Outputs(成果物)
- 週次スナップショット(`_company/reports/`)
- 月次レビュー資料(KPI・品質較正・戦略入力を含む)
- エスカレーションアラート(基準該当時)

## Tools(使用ツール)
Claude Code

## Preconditions(前提条件)
`management-kpi`・`management-resource`・`management-quality`が最低1回以上データを集計していること。

## Procedure(手順)
### 週次レビュー(COO主導)
1. `management-kpi`・`management-resource`から直近1週間のスナップショットを取得する
2. 大きな異常(KPI急落・コスト急増等)がないか確認する
3. `_company/reports/`に週次スナップショットを記録する
4. 異常があれば下記エスカレーション基準に照らして即時対応する

### 月次レビュー(CEO同席)
1. 週次スナップショットを月次で集約する
2. `management-quality`の月次較正結果(チェッカーの合格率・ブランドSSOTとの整合)を確認する
3. `management-strategy`の戦略提案を確認する
4. `core-coo`が優先順位・タスク配分を整理し、CEOへ提示する
5. CEOが方向性を確認・承認する

### 四半期レビュー(月次レビューの拡張)
1. 月次レビューの内容に加え、事業ポートフォリオ全体(新規事業判断・撤退判断)を`management-strategy`が提示する
2. `docs/roadmap.md`のフェーズ進捗を確認し、必要に応じて更新する

## Checklist(実行時チェックリスト)
- [ ] 週次: KPI・コストの急変がないか確認したか
- [ ] 月次: 品質較正・戦略提案を確認したか
- [ ] 四半期: ロードマップの進捗を確認したか

## Quality Standard(品質基準)
CEOが週次/月次/四半期のいずれの粒度でも、会社全体の状況を数分〜数十分で把握できること。

## Failure Pattern(失敗パターン)
個別投稿の承認(Loop1)とこのレビューサイクル(Loop2)を混同し、投稿のたびにManagement層を経由させてしまう(スケールのボトルネックになる)。

## エスカレーション基準(Loop1→Loop2への即時報告)
以下に該当する場合、週次/月次のタイミングを待たず、`core-coo`へ即時報告する。
1. いずれかのQAチェッカー(`shared-brand-quality-checker`・`shared-image-quality-checker`・`shared-video-quality-checker`・`seo-quality-checker`)の合格率が閾値を下回った場合(閾値は運用開始後に設定)
2. `_company/charter/ai-usage-policy.md`基準のAPIコストが予算ラインを超過した場合
3. いずれかのKPIが2期連続で目標未達の場合

## Handoff(引き継ぎ)
月次レビューの結果は`management-strategy`・各Divisionへフィードバックし、次サイクルの優先順位に反映する。

## Revision History(変更履歴)
| 版 | 日付 | 変更内容 | 変更者 |
|---|---|---|---|
| 1 | 2026-07-12 | 初版作成 | COO |
