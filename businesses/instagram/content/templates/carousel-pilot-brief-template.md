---
sample_only: true
do_not_import: true
pilot_post_id: "tomo-angel7-pilot-XX"
pilot_id: tomo-angel7-carousel-pilot-2026-07
category: null
pain_point_id: null
status: draft
created_at: "YYYY-MM-DD"
updated_at: "YYYY-MM-DD"
---

# カルーセル投稿企画書テンプレート(パイロット用)

このテンプレートをコピーし、`../pilot-drafts/`配下に`{pilot_post_id}.md`として1投稿ごとに1ファイル作成する。**本テンプレート自体には実際の投稿内容を記載しない。**

## 1. 投稿カテゴリ
A(恋愛・パートナーシップ) / B(人間関係・家族・仕事) / C(自己肯定感・不安・人生の選択)のいずれか。

## 2. 対応するpain-point-categories.yaml項目
`pain_point_id`(1つのみ)。

## 3. テーマ候補と根拠(調査情報の取り扱いに従う)
| 候補 | source_type | source_reference | source_date | freshness | evidence_level | assumption |
|---|---|---|---|---|---|---|
| | 既存Knowledge/既存pain-point/CEO提供/匿名化集計/手動提供数値/競合資料のいずれか | 参照元の具体名 | 参照元の日付 | 情報の鮮度(例: 2026-07時点等) | high/medium/low | true/false |

**注意:** 自動取得したトレンド・競合データ・検索量・Metaインサイトは現時点で取得不可。取得できない情報を取得済みとして記載しない。根拠がない場合は`assumption: true`とする。

## 4. 選定テーマ
(候補から1つに絞った結果)

## 5. 対象読者・悩み・投稿目的
- 誰の:
- どのような悩みに:
- どのような読後感を与えるか:

## 6. 5枚カルーセル構成
| ページ | 役割 | 文章 |
|---|---|---|
| 1 | 「私のことだ」と思える短いタイトル | |
| 2 | 悩みや苦しさへの共感 | |
| 3 | 少し視点が変わる気づき | |
| 4 | 今日からできる小さな考え方・行動 | |
| 5 | 安心できる締めの言葉と自然な案内 | |

## 7. キャプション

## 8. ハッシュタグ案

## 9. 画像・レイアウト制作指示
- 背景:
- 文字配置:
- 文字量:
- 余白:
- 装飾:
- 強調箇所:
- ページごとの雰囲気:
- 使用してよいブランド素材:

## 10. Brand QA・内容QA結果(`shared-brand-quality-checker`)
- 判定: 合格 / 不合格
- 不合格の場合、問題箇所・理由・修正要件:

## 11. CEO承認用サマリー
投稿カテゴリ・テーマ・対象読者・投稿目的・5枚の全文・キャプション・画像制作指示・QA結果・リスクまたは注意事項・前回投稿からの改善点を、`../templates/carousel-ceo-approval-template.yaml`とあわせて提示する。

## 12. status(投稿成果物のライフサイクル)
`draft` → `qa_failed`(不合格時) → `qa_passed` → `ceo_review` → `approved` → `published` → `measured` → `archived`

## 13. 投稿後KPI(実数値は`_company/kpi/actuals/`へ、本テンプレートには記載しない)
生値: リーチ数・保存数・シェア数・コメント数・プロフィールアクセス・フォロー数・LINE登録・問い合わせ数。
率: `save_rate`(保存数÷リーチ数)・`share_rate`(シェア数÷リーチ数)・`profile_visit_rate`(プロフィールアクセス÷リーチ数)・`follow_conversion_rate`(投稿経由フォロー数÷リーチ数)・`line_conversion_rate`(投稿に帰属可能なLINE登録数÷リーチ数)。
取得できない場合は`unavailable`/`not_attributable`/`not_measured`のいずれかを明記し、推測で埋めない。
