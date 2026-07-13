---
id: carousel-post-pilot
title: Instagramカルーセル投稿パイロット手順(Tomo_Angel7)
category: instagram
tags: [pilot, carousel, tomo-angel7, life-consultation]
owner: ig-content-planner
status: draft
priority: high
risk_level: medium
version: 1
frequency: as-needed
estimated_time: "投稿1本あたり数時間(QA差し戻しを含む場合はさらに要する)"
requires_ceo_approval: true
automation_possible: false
automation_status: not-automated
related_sop_ids: [consumer-analysis]
related_sop_categories: [publishing, design, research, management]
related_knowledge: [instagram, psychology, copywriting]
sensitivity: internal
last_execution: null
success_rate: null
created_at: "2026-07-13"
updated_at: "2026-07-13"
review_due: "2027-01-13"
---

# Instagramカルーセル投稿パイロット手順(Tomo_Angel7)

## Purpose(目的)
`businesses/instagram/content/pilot-scope.md`で定義した実験範囲内で、Instagramカルーセル投稿を調査から改善提案まで一度通して実行する。新規Agent・Divisionを追加せず、既存資産のみで完結させる。

## Scope(適用範囲)
Tomo_Angel7ブランドのInstagramカルーセル投稿(パイロット期間中の6投稿)。投稿の公開・予約投稿(execute)は本SOPの範囲外であり、`_company/charter/outbound-action-policy.md`に基づきCEO承認済みの人間が別途行う。

## Trigger(実行のきっかけ)
パイロット期間中、`pilot-scope.md`で定められた投稿枠(`businesses/instagram/content/calendar.md`)に着手するとき。

## Inputs(必要な入力)
- `businesses/instagram/content/pilot-scope.md`(実験範囲)
- `_company/brands/tomo-angel7/brand-brief.md`(恒久的なブランド原則)
- `_company/brands/tomo-angel7/pain-point-categories.yaml`
- `_company/brands/tomo-angel7/qa-checklist.md`

## Outputs(成果物)
- `businesses/instagram/content/templates/carousel-pilot-brief-template.md`に基づく投稿企画書1件(`pilot-drafts/`または`pilot-approved/`配下)
- `businesses/instagram/content/templates/carousel-ceo-approval-template.yaml`に基づく承認記録(CEO承認後)

## Tools(使用ツール)
Claude Code、Perplexity(`ig-content-planner`が使用)、Canva(人間が使用)

## Preconditions(前提条件)
`pilot-scope.md`がCEO承認済みであること。

## Procedure(手順)

### 担当Agentの責任範囲(役割定義を超えない)
- **`ig-content-planner`:** テーマ企画、読者の悩みと読後感の定義、5枚構成、文章、キャプション、ハッシュタグ案、作成後のセルフチェックまでを担当する。既存の役割定義(投稿ネタ企画・カレンダー・キャプション下書き)の範囲内で行う。
- **`ig-creative-team`:** 5枚文章を受け取り、既存ブランドテンプレートへ流し込む画像制作指示を作成する(既存の役割定義通り、ゼロからのデザイン作成は行わない)。
- **`shared-brand-quality-checker`:** ブランド整合性・トーン・禁止表現・不安煽り・断定・依存誘導・専門領域への危険な踏み込み・`pilot-scope.md`との整合を**独立して確認する**。**本文の直接編集・書き直しは行わない**(既存の役割定義通り)。不合格時は問題箇所・理由・修正要件を`ig-content-planner`へ返す。
- **`intelligence-consumer`:** テーマ調査・悩みの裏付け調査を担当する(既存の役割定義通り)。
- **`ig-analytics`:** 投稿後データ記録・改善提案を担当する(既存の役割定義通り)。
- **`core-coo`:** Gate1/Gate2・CEO承認用パッケージ作成・CEOとの窓口を担当する(既存の役割定義通り)。
- **今回使用しないエージェント:** `shared-image-quality-checker`(画像最終確認は人間が行う)、`intelligence-insight-synthesizer`(Intelligence入力が`intelligence-consumer`1系統のみのため経由しない)。

### 工程(1投稿あたり)
1. **カテゴリ決定** ― `pilot-scope.md`の配分に従い、今回のカテゴリ(A/B/C)を決める(`ig-content-planner`)
2. **テーマ候補調査** ― `intelligence-consumer`が悩みの裏付けを、`ig-content-planner`が話題性を調査する。**取得できる情報の区分(下記「調査情報の取り扱い」)に従い、根拠のない情報は`assumption: true`と明記する**
3. **テーマ選定** ― 候補から1テーマへ絞る。**【CEO承認ポイント①】** `pilot-scope.md`の禁止事項・ブランド軸との整合をここで確認する
4. **読者の悩みと投稿目的の定義** ― 誰の・どのような悩みに・どのような読後感を与えるかを明確にし、`pain-point-categories.yaml`の該当項目を1つ紐付ける
5. **5枚カルーセルの構成作成** ― 各ページの役割を定義する(1枚目: タイトル、2枚目: 共感、3枚目: 気づき、4枚目: 小さな行動、5枚目: 締め。ただし毎回同じ定型文にしない)
6. **各ページの文章作成** ― 短く、スマートフォンで読みやすい長さにする。説教・長文・専門用語を避ける
7. **キャプション作成** ― カルーセル本文の単純な繰り返しにしない
8. **ハッシュタグ案作成** ― 広すぎるタグ・無関係なタグ・煽り系タグを避ける
9. **画像・レイアウト制作指示** ― `ig-creative-team`が、背景・文字配置・文字量・余白・装飾・強調箇所・雰囲気・使用してよいブランド素材を含めた指示書を作成する
10. **Brand QA・内容QA** ― `shared-brand-quality-checker`が一括して独立確認する。不合格時は`ig-content-planner`へ差し戻す(status: `qa_failed`)
11. **CEO承認用パッケージ作成** ― `core-coo`が、投稿カテゴリ・テーマ・対象読者・投稿目的・5枚の全文・キャプション・画像制作指示・QA結果・リスクまたは注意事項・前回投稿からの改善点をまとめる
12. **CEO最終承認前確認** ― status: `ceo_review`。承認されればstatus: `approved`
13. **人間によるCanva等での仕上げ** ― 画像は自動生成・自動公開せず、人間が仕上げる
14. **画像最終確認** ― 人間が文字切れ・視認性・余白・誤字・ブランド整合を確認する(`shared-image-quality-checker`は経由しない)
15. **CEO最終承認** ― **【CEO承認ポイント②・常時必須】** `businesses/instagram/content/templates/carousel-ceo-approval-template.yaml`に基づく承認記録を紐付ける
16. **手動投稿** ― CEOまたは許可された人間が投稿する(status: `published`)。本SOP・本パイロットではここまでを実行しない(Gate2は構造・テンプレートの整備までとする)
17. **投稿後データ記録** ― `ig-analytics`が実KPIを`_company/kpi/actuals/`へ記録する(status: `measured`)
18. **改善提案** ― `ig-analytics`が投稿結果と制作工程の両方を評価し、次サイクルへ反映する(status: `archived`)

### 投稿成果物のstatus(draft/qa_failed/qa_passed/ceo_review/approved/published/measured/archivedの8段階)
既存の`agents.yaml`(エージェントのライフサイクル)・SOPの`status`(draft/active/deprecated/archived)とは別軸の、**投稿成果物1件ごとの状態**を表す新しい語彙である(既存語彙と衝突しないことを確認済み)。

```
draft → qa_failed(不合格時、draftへ差し戻し) → qa_passed → ceo_review → approved → published → measured → archived
```

### draft/approved領域の正本切替ルール
- 作成中(status: draft〜qa_passed)は`businesses/instagram/content/pilot-drafts/`が正本
- CEO承認後(status: approved以降)は`businesses/instagram/content/pilot-approved/`が正本
- `pilot-approved/`へ移動した後、`pilot-drafts/`側の元ファイルはstatus: `archived`とするか、`pilot-approved/`側への参照のみを残す(同一成果物が複数の正本にならないようにする)

### 調査情報の取り扱い(捏造禁止)
現時点でInstagram API・Meta API・Google Trends・検索API等は接続されていない。テーマ候補ごとに以下を記録する。
- **利用可能な情報源:** 既存Knowledge、既存`pain-point-categories.yaml`、CEOから提供された情報、匿名化・集計済みの過去相談傾向、手動で提供された過去投稿数値、明示的に提供された競合資料
- **現在利用不可な情報源:** 自動取得した最新トレンド、自動取得したInstagram競合データ、自動取得した検索量、自動取得したMetaインサイト
- 利用できない情報を取得済みであるかのように記載しない。テーマ候補ごとに`source_type`・`source_reference`・`source_date`・`freshness`・`evidence_level`・`assumption`を記録する(`carousel-pilot-brief-template.md`のフィールドを使用)。根拠がない場合は`assumption: true`と明記する

### KPI測定ルール(工程17)
生の件数だけでなく、可能な範囲で率も計算する。
- `save_rate` = 保存数 ÷ リーチ数
- `share_rate` = シェア数 ÷ リーチ数
- `profile_visit_rate` = プロフィールアクセス ÷ リーチ数
- `follow_conversion_rate` = 投稿経由フォロー数 ÷ リーチ数
- `line_conversion_rate` = 投稿に帰属可能なLINE登録数 ÷ リーチ数

Meta等から取得できない数値、または投稿との因果関係を特定できない数値は、推測で埋めない。値を取得できない場合は`unavailable`(取得不能)・`not_attributable`(帰属特定不能)・`not_measured`(未計測)のいずれかを明記する。LINE登録・問い合わせを投稿へ帰属させる仕組みがない場合、「投稿による登録」と断定しない。実数値は`_company/kpi/actuals/`(Git管理対象外)へ保存し、生値・率・取得不能理由を区別して記録する。

## Checklist(実行時チェックリスト)
- [ ] `pilot-scope.md`の禁止事項・共通ブランド軸に沿っているか
- [ ] `pain-point-categories.yaml`の具体的な1項目に紐付いているか
- [ ] テーマ調査の根拠区分(`source_type`等)を記録したか
- [ ] `shared-brand-quality-checker`が本文を直接編集せず、差し戻しのみを行ったか
- [ ] CEO承認記録が投稿前に有効化されているか(有効な承認記録が無い場合、投稿禁止)
- [ ] KPIの生値と率を区別し、取得不能値を推測で補完していないか

## Quality Standard(品質基準)
`qa-checklist.md`の共通チェック項目・ブランド固有チェック項目に全て適合すること。

## Failure Pattern(失敗パターン)
テーマ調査で自動取得できない情報を取得済みであるかのように記載してしまう(→`assumption: true`の明記を徹底する)。`shared-brand-quality-checker`が差し戻しではなく本文を直接書き換えてしまう(→役割逸脱のため厳禁)。

## Handoff(引き継ぎ)
パイロット終了後、`_shared/sop/hr/agent-onboarding.md`・`agent-retirement.md`の枠組みで各Agentの評価候補分類を`hr-workforce-manager`へ引き継ぐ(状態変更は別途CEO承認)。

## Revision History(変更履歴)
| 版 | 日付 | 変更内容 | 変更者 |
|---|---|---|---|
| 1 | 2026-07-13 | 初版作成 | COO |
