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

### status変更の責任者(2026-07-14 Batch B追加)
| 担当 | 権限範囲 |
|---|---|
| `ig-content-planner` | 新規成果物を`draft`として作成。`qa_failed`から`draft`へ戻った後、修正して再提出 |
| `shared-brand-quality-checker` | QA結果を報告する。本文を直接編集しない。合格時は`qa_passed`への変更条件を満たしたことを報告、不合格時は`qa_failed`への変更条件を満たしたことを報告(status変更の実行自体は`core-coo`が行う) |
| `core-coo` | status遷移条件の確認、成果物とcalendarの同期、CEO承認結果の記録支援、遷移条件が不足している場合の停止 |
| CEO | `ceo_review`から`approved`への内容承認、`approved`から`published`へ進むための公開承認、差し戻し判断 |
| 人間の投稿担当者 | 有効な公開承認を確認した後の手動投稿、投稿完了証拠の記録。**自分の判断だけでstatusを`approved`または`published`へ変更しない** |
| `ig-analytics` | 実KPI記録後、`published`から`measured`への遷移条件を報告 |
| `hr-workforce-manager`・その他未記載のAgent | **投稿成果物のstatus変更権限なし** |

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

### 投稿成果物のstatus(draft/qa_failed/qa_passed/ceo_review/approved/published/measured/archivedの8段階)【2026-07-14 Batch B: 遷移・同期ルールを追加】
既存の`agents.yaml`(エージェントのライフサイクル)・SOPの`status`(draft/active/deprecated/archived)とは別軸の、**投稿成果物1件ごとの状態**を表す語彙である(既存語彙と衝突しないことを確認済み)。8段階の名称・数は変更・削除・追加していない。

**重要な設計原則:** `businesses/instagram/content/calendar.md`は投稿本文・承認記録の正本にしない。calendarの役割は「投稿枠の一覧・投稿順序・現在状態の要約・正本成果物への参照」のみに限定する。**成果物側(`pilot-drafts/`または`pilot-approved/`配下の企画書ファイル)のstatusが常に正本であり、calendarはそれを反映するミラー情報に過ぎない。calendarだけを先に変更してはならず、成果物側を変更した同じ作業単位でcalendarを同期する。両者に不一致を発見した場合は、必ず成果物側の正本を基準にcalendarを修正する。calendarには投稿本文・画像・承認内容そのものを複製しない。**

#### status遷移図(基本ルート)
```
draft ──┬─→ qa_failed ─→ draft(差し戻し)
        └─→ qa_passed ─→ ceo_review ──┬─→ draft(CEOによる原稿差し戻し)
                                        └─→ approved ──┬─→ ceo_review(承認後に内容変更があった場合)
                                                         └─→ published ─→ measured ─→ archived
```

#### 各statusの定義

| status | 意味 | 正本保存場所 | 変更できる担当者 | 遷移元 | 遷移先 | 必要な前提条件 | 必要な記録 | calendarへの反映方法 | 差し戻し時の戻り先 |
|---|---|---|---|---|---|---|---|---|---|
| `draft` | 制作中(企画〜セルフチェック前) | `pilot-drafts/` | `ig-content-planner` | (新規)/`qa_failed`/`ceo_review`/`approved`(差し戻し時) | `qa_failed`または`qa_passed` | 新規作成、または差し戻し理由への対応完了 | セルフチェック結果 | calendarのstatus列を`draft`へ同期 | ― |
| `qa_failed` | Brand QA不合格 | `pilot-drafts/` | `shared-brand-quality-checker`(判定報告のみ、本文は編集しない) | `draft` | `draft`(修正のため) | Brand QA実施済み | 問題箇所・不合格理由・修正要件 | calendarのstatus列を`qa_failed`へ同期 | `draft` |
| `qa_passed` | Brand QA合格 | `pilot-drafts/` | `shared-brand-quality-checker`(判定報告のみ) | `draft` | `ceo_review` | Brand QA全項目合格 | QA合格結果の記録 | calendarのstatus列を`qa_passed`へ同期 | (合格のため通常は戻らない) |
| `ceo_review` | CEO内容確認前 | `pilot-drafts/` | `core-coo`(遷移条件確認・同期のみ) | `qa_passed`/`approved`(内容変更時)/`published`(内容変更時、要再承認) | `draft`(差し戻し)または`approved`(内容承認) | CEO承認用パッケージ作成済み | CEO確認用サマリー | calendarのstatus列を`ceo_review`へ同期 | `draft` |
| `approved` | **投稿内容および完成画像がCEOによる内容確認(content approval)を通過した状態。外部公開の実行許可(public release approval)を意味しない** | `pilot-approved/`(`pilot-drafts/`から移動) | CEO(承認)、`core-coo`(正本移動の実務) | `ceo_review` | `ceo_review`(内容変更時)または`published`(下記「publishedへの必須条件」を全て満たした場合のみ) | CEOによる内容承認 | 承認日時・承認者 | calendarのstatus列を`approved`へ同期 | `ceo_review` |
| `published` | 実際に投稿が公開された状態 | `pilot-approved/` | 人間の投稿担当者(有効な公開承認確認後のみ)。CEOまたは許可された人間以外は変更不可 | `approved` | `measured` | 下記「`published`への必須条件」を**全て**満たすこと | 投稿完了証拠、Outbound Action Approval記録 | calendarのstatus列を`published`へ同期 | (公開後は`published`を過去に戻して履歴を書き換えない。下記「公開後の差し戻し」参照) |
| `measured` | 投稿後KPIを記録した状態 | `pilot-approved/` | `ig-analytics`(実KPI記録後、遷移条件を報告) | `published` | `archived` | 実KPIが`_company/kpi/actuals/`へ記録済み | KPI記録日 | calendarのstatus列を`measured`へ同期 | ― |
| `archived` | サイクル完了 | `pilot-approved/` | `ig-analytics`(改善提案完了後) | `measured` | (終端) | 改善提案が次サイクルへ反映済み | 改善レポート参照 | calendarのstatus列を`archived`へ同期 | ― |

**`hr-workforce-manager`および上記に明記されていないその他のAgentには、投稿成果物のstatus変更権限を与えない。** 既存Agent定義自体は変更していない(役割はいずれも既存定義の範囲内)。

#### `published`への必須条件(手動運用上のフェイルクローズ規則。**技術的な自動強制機構はまだ存在せず、今回は人間が確認する運用ルールとして明文化するのみ**)
以下を**全て**満たさない限り、`approved`から`published`へは変更禁止とする。
1. 最終画像が完成している
2. 全ページの人間確認が完了している
3. `human_review_required`が`false`になっている
4. 固定誘導画像の`asset_reference`が確定している
5. `public_release_approved`が`true`である
6. 有効なOutbound Action Approval記録(`_company/charter/outbound-action-policy.md`準拠)が存在する
7. 承認対象と実際の投稿内容の`content_hash`が一致する
8. 投稿先・CTA・公開範囲・投稿日時が承認内容と一致する

一つでも満たさない場合、`published`への変更は禁止する。

#### content approval と public release approval の区別
- **content approval(内容承認、`status: approved`):** 投稿内容・完成画像がCEOの内容確認を通過したことのみを意味する
- **public release approval(公開承認、`public_release_approved: true`+有効なOutbound Action Approval記録):** 外部公開(execute)そのものを許可する、別の独立した承認
- `approved`であっても`public_release_approved`が`false`のままであれば、公開してはならない

#### draft/approved領域の正本切替ルール
- 作成中(status: `draft`〜`ceo_review`)は`businesses/instagram/content/pilot-drafts/`が正本
- CEOによる内容承認後(status: `approved`以降)は`businesses/instagram/content/pilot-approved/`が正本
- `pilot-approved/`へ移動した後、`pilot-drafts/`側の元ファイルはstatus: `archived`とするか、`pilot-approved/`側への参照のみを残す(同一成果物が複数の正本にならないようにする)
- **`tomo-angel7-pilot-01`は6枚目画像未特定・公開承認前のため、現時点では`pilot-drafts/`側を正本として維持する(`pilot-approved/`へは移動しない)**

#### 差し戻しルール
| 状況 | 遷移 | 追加処理 |
|---|---|---|
| QA不合格 | `qa_failed` → `draft` | `shared-brand-quality-checker`の指摘に基づき`ig-content-planner`が修正 |
| CEOによる原稿差し戻し | `ceo_review` → `draft` | 差し戻し理由を記録 |
| CEO内容承認後に本文・画像・キャプション・CTA等が変更された場合 | `approved` → `ceo_review` | 再度CEO内容確認が必要 |
| 公開承認後に承認対象内容が変更された場合 | `published`未達の`approved`/`ceo_review`へ戻す | `public_release_approved`を`false`へ戻し、既存の公開承認記録を無効扱いにし、再度CEO内容確認と公開承認を受ける |
| 公開後の誤記等が判明した場合 | `published`は変更しない | `published`を過去のstatusへ戻して履歴を書き換えない。修正投稿・削除・再公開等は別の新規外部executeとして扱い、改めてCEO承認を要する。発生事実と対応は監査記録へ残す |

#### 誤った状態変更の復旧方法
成果物側のstatusとcalendarのミラー情報が食い違った場合、または誤ったstatus変更が行われた場合は、常に**成果物側ファイル(企画書)を正本として基準にし、calendar側を修正する**。成果物側のstatus自体を誤って変更した場合は、上記「差し戻しルール」の該当行に従って正しい遷移元へ戻す。

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
| 2 | 2026-07-14 | 環境構築Gate2 Batch B: 投稿成果物status(8段階)の遷移図・各status定義表(正本保存場所/変更担当者/遷移元先/前提条件/必要記録/calendar反映方法/差し戻し先)・`published`への必須条件(手動フェイルクローズ規則、技術的自動強制は未実装と明記)・content approval/public release approvalの区別・status変更責任者・差し戻しルール・誤った状態変更の復旧方法・calendarはミラー情報であり正本にしない、という設計原則を追加。既存8段階のstatus名称・数は変更していない | COO |
