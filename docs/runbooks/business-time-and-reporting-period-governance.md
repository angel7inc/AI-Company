# AI Company 事業基準タイムゾーン・報告期間境界管理Runbook

## 位置づけ
本Runbookは、KPI・Finance・投稿実績・作業ログ・障害記録・レポート・集計において、日時と期間の解釈が担当者・サービス・ファイルごとに食い違わないよう、事業上の基準タイムゾーン・外部サービスの表示タイムゾーン・保存時のタイムゾーン表現・日次/週次/月次/年次集計の境界・期間開始終了の包含規則・夏時間の扱い・タイムゾーン変換前後の記録・基準変更時の版管理を統一管理するための**ルール・空テンプレート・変更管理方針の設計**である。

**今回のBatchはルール・空テンプレート・状態定義・変更管理方針の設計だけを対象とする。** 実KPI値・実Finance値・実投稿実績・実ログ・実日時記録の作成、外部サービスの数値取得は行っていない。

## 0. 既存構造の調査結果とケース判定

### 0.1 調査結果
- `kpi-data-entry-and-validation.md`・`kpi-definition-governance.md`・`kpi-definition-observation-traceability.md`はいずれも、事業上の基準タイムゾーンを「`not_decided`」と明記し、独断決定を避けて将来課題としている。
- `ai-company-local-operations.md`は、現PCの検出値として「UTC+0900(JST)」を記載しているが、**これは現PCのローカル設定値であり、事業の正式な基準として宣言されたものではない**(同ファイル内でも「現PCの設定値」と明記済み)。
- `.env.example`に`AI_COMPANY_TIMEZONE=`という未使用の環境変数プレースホルダーが存在するが、値は空欄であり、実行コードも存在しないため、正式なポリシーとしては機能していない。
- `calendar.md`・第1投稿ドラフト等は、日付を「YYYY-MM-DD」形式で記載するのみで、タイムゾーンやオフセットの明示はない。
- `_company/kpi/schemas/kpi-definition-schema.yaml`・`kpi-definition-template.yaml`には`business_timezone`・`source_timezone_policy`・`week_start_day`・`month_boundary_rule`という**項目**は既に存在するが、これらは個々のKPI定義が参照する値であり、**事業全体で一元管理する正式な基準・期間境界ポリシーそのものではない**。
- `_company/governance/`または同等の正式な保存場所は、本Batch開始時点で存在しなかった。
- 既存文書間で矛盾する記載(例: あるファイルはJSTを事業基準と断定し、別のファイルはUTCを断定している等)は確認されなかった。すべての既存参照は一貫して「未確定」の立場を取っている。

### 0.2 ケース判定: ケースA
タイムゾーン項目・個別記載(KPI定義スキーマの`business_timezone`欄等)は存在するが、事業全体の基準と期間境界を統一管理する正式な管理基盤(ポリシー・状態管理・版管理・外部サービス対応表)は存在しないため、**ケースAとして実装を続行する。**

## 1. 事業基準タイムゾーンの確定状況

**【2026-07-16追記】CEO決定により、事業基準タイムゾーンは`Asia/Tokyo`として承認・`active`化された。** 承認済み正式ポリシーは[`_company/governance/policies/business-time-policy__v1.yaml`](../../_company/governance/policies/business-time-policy__v1.yaml)、承認済み期間定義(日次/週次/月次/四半期/年次)は[`_company/governance/reporting-periods/`](../../_company/governance/reporting-periods/)配下に保存されている。

**承認内容の要旨:**
- `business_timezone`: `Asia/Tokyo`(`business_timezone_status: active`)
- 正規化timestampはRFC 3339形式のUTCで保存する
- 標準表示・標準報告タイムゾーンは`Asia/Tokyo`
- 期間境界は開始を含み終了を含まない(`start_inclusive: true`/`end_inclusive: false`)
- 会計年度は`not_decided`のまま(`fiscal_year_start_month: null`)。暦年と会計年度を同一視しない

**承認前の経緯(記録として保持):** 本Batch起案時点では、外部サービス(Instagram・LINE・Google系等)の実タイムゾーン設定が未確認であることを理由に確定を見送っていた。**この状況は今回の承認によっても変わっていない。外部サービス別タイムゾーンは依然として未確認のままであり、Instagram・LINE・Google等がAsia/Tokyoであると推測してはならない。** 外部サービス時刻対応表(`source-timezone-mapping-template.yaml`)への実登録は、実ログイン・実設定確認を伴う別Batchで行う。

以下、旧版に記載した候補整理は経緯として残す。CEOの主な運用環境(検出PC)がJST(UTC+0900)であったこと、対象ブランド(Tomo_Angel7)のコンテンツ・想定読者が日本語圏であったこと、Finance・KPI・投稿運用に関する既存文書がいずれもJST運用を前提とすることと矛盾しなかったことが、承認の背景事実として存在した。

## 2. 事業基準タイムゾーン(business_timezone)の管理項目
`business_timezone`・`business_timezone_status`・`timezone_authority`・`approved_at`・`approved_by`・`effective_from`・`effective_to`・`previous_timezone`・`change_reason`・`reprocessing_required`を管理する。

`business_timezone_status`の状態: `not_decided`/`proposed`/`review_required`/`approved`/`active`/`deprecated`/`blocked`。

- `approved`と`active`を区別する(承認済みでも、他の利用開始条件を満たすまで`active`ではない)
- `active`な基準タイムゾーンは、同一期間に1つだけとする
- `active`化前にhuman reviewとCEO reviewの両方が必要
- タイムゾーン名は原則としてIANA形式(例: `Area/City`)を使用する
- UTCオフセットだけ(例: `+09:00`)を恒久的な識別子として使用しない(夏時間等でオフセットが変動する地域があるため)
- `JST`等の略称だけで保存しない
- タイムゾーン不明時にローカルPC時刻を推測で採用しない
- OS設定だけを事業基準の正本としない
- 基準タイムゾーン変更時は過去データへの影響を評価する

### 2.1 レビュー状態値(human_review_status・ceo_review)
**【2026-07-16追記、CEO決定】** 事業時刻ポリシー・期間定義の`human_review_status`・`ceo_review`について、以下を正式な状態値とする。

`human_review_status`の許容値: `not_started`/`review_required`/`changes_requested`/`approved`/`blocked`。

`ceo_review`の許容値: `not_requested`/`review_required`/`approved`/`rejected`/`blocked`。

- `human_review_status: approved`は、人間による内容確認が完了し承認された状態を意味する
- `ceo_review: approved`は、CEOによる内容承認が完了した状態を意味する
- `human_review_status`または`ceo_review`が`null`・未承認・`changes_requested`・`rejected`・`blocked`の場合は`active`にしない
- `active`化後にレビュー状態が無効になった場合(訂正・撤回等)は、利用停止または再レビューを必要とする
- これらはKPI実績値や投稿の外部公開承認(`external_reporting_approved`・`public_release_approved`)とは別概念である

**本追記は今回の事業時刻ポリシー・期間定義に限定した状態値である。** 他の既存ファイル(KPI定義・アセット管理等)や全社共通の状態語彙への展開は今回行わず、将来課題として記録する。

## 3. 日時の保存形式
- timestampはタイムゾーンまたはUTCオフセットを明示する
- 日付のみの値(date)は日時値(datetime)と区別する
- ローカル時刻だけを保存しない
- UTC保存とローカル表示を区別する
- `original_source_timestamp`・`original_source_timezone`・`normalized_timestamp`・`normalized_timezone`・`conversion_performed`・`conversion_rule_version`を管理する

- 元サービスの表示時刻を消さずに保持する(`original_source_timestamp`)
- 正規化後の時刻だけで元時刻を上書きしない
- タイムゾーン不明の日時を推測変換しない
- オフセットなし日時を確定値として扱わない
- 夏時間対象地域ではIANAタイムゾーンに基づき変換する(固定オフセットでは夏時間をまたぐ変換を誤る)
- 手動変換時は操作者と変換日時を記録する
- **本Batchでは、自動変換処理を実装していない**

## 4. 期間境界
`period_type`・`period_start`・`period_end`・`start_inclusive`・`end_inclusive`・`boundary_timezone`・`week_start_day`・`month_start_rule`・`year_start_rule`・`cumulative_value_allowed`・`partial_period_allowed`・`late_arriving_data_rule`を管理する。

`period_type`候補: `daily`/`weekly`/`monthly`/`quarterly`/`yearly`/`custom`/`cumulative`/`point_in_time`。

- 日次の開始・終了境界を明記する
- 週の開始曜日を明記する
- 月次の開始日と終了日を明記する
- 四半期の定義を明記する
- 暦年と会計年度を区別する
- 期間開始・終了が包含か排他かを明記する
- 累計値と期間値を混同しない
- `point_in_time`は期間集計と区別する
- 部分期間を完全期間として扱わない
- 遅延到着データの扱いを定義する(`late_arriving_data_rule`)
- **境界が未決定の期間を`validated`・`locked`・外部報告可能にしない**

## 5. 週次境界
- `week_start_day`を必須とする
- `week_end_day`を計算上明確にする(`week_start_day`から自動導出)
- ISO week(`iso_week_used`)を使用するか否かを明記する
- 外部サービスの週定義(例: 一部サービスは日曜始まり)と事業週定義を区別する
- 週番号だけを期間識別子として使用しない(年またぎで曖昧になるため)
- 年をまたぐ週の扱いを明記する
- 部分週の扱いを明記する
- **週開始曜日が不明な場合は週次値を確定しない。独断で月曜日または日曜日にしない**

## 6. 月次・四半期・年次境界
- 暦月と請求月を区別する
- 月末締め・任意締め日を区別する
- 四半期区分を明示する
- 暦年と会計年度を区別する
- `fiscal_year_start_month`が未決定なら未決定として扱う
- 締め日変更時の移行期間を定義する
- 同一データを旧境界・新境界で二重計上しない
- 年度変更時に過去期間を無条件で再分類しない

## 7. 外部サービス別タイムゾーン
`source_system`・`account_reference`・`source_timezone`・`timezone_source`・`display_timezone`・`export_timezone`・`api_timezone`・`configurable`・`configuration_verified`・`verified_at`・`verified_by`・`conversion_required`・`notes`を管理する。

- 画面表示・CSV・APIでタイムゾーンが異なる可能性がある
- サービス名だけでタイムゾーンを推測しない
- アカウント設定を確認していない場合は未確認とする
- 外部サービスの仕様変更時に再確認する
- `account_reference`へ実アカウント名を空テンプレートでは入れない
- **本Batchでは、実ログイン・実設定確認を行っていない**

## 8. KPIとの関係
- KPI定義は`business_timezone`または`source_timezone_policy`を参照する
- 観測値は`source_timezone`と`reporting_timezone`を区別する
- `definition_version`が同じでも、期間境界が違う値を無条件に比較しない
- タイムゾーン変換前後の期間を追跡する
- 定義版が要求するタイムゾーンと観測値が一致しない場合は`blocked`
- 期間境界の変更は`definition_version`更新対象となる可能性がある
- 基準時刻未決定のKPIは、期間解釈に影響する場合`active`にしない
- **本Batchでは、実KPIテンプレート(`kpi-observation-template.yaml`等)への項目追加は行っていない。** 不足項目がある場合は「15. 現在の状態」の将来課題として記録する

## 9. Financeとの関係
- Financeの金額P&L正本は`_company/finance/actuals/`に維持する(本Batchで変更していない)
- 取引日・計上日・入金日・請求日を区別する
- Finance期間とKPI報告期間を無条件に同一視しない
- タイムゾーン差による日付ずれを確認する
- 月末付近の取引を二重計上しない
- Financeの締め境界が未確認の場合、月次利益等の外部報告を確定しない
- **本Batchでは、Financeファイルを変更していない。実金額・実取引日時も作成していない**

## 10. 投稿・コンテンツとの関係
`scheduled_at`・`published_at`・`source_platform_timestamp`・`business_date`・`reporting_period`を区別する。

- 予約時刻と実公開時刻を混同しない
- 外部プラットフォーム表示日と事業上の日付が異なる可能性がある
- **【2026-07-16追記、CEO決定】投稿実績の集計基準は原則`published_at`(実公開時刻)とする。** `scheduled_at`(予約操作の時刻)は投稿実績の代替ではない
- `source_platform_timestamp`を保持する。`published_at`のsource timezoneを確認する
- `published_at`が不明またはタイムゾーン未確認の場合、日次・週次・月次集計へ確定反映しない
- 投稿先サービス側の表示時刻を`Asia/Tokyo`と推測しない
- タイムゾーン未確認の投稿を日次・週次集計へ無条件に含めない
- **本Batchでは、第1投稿(`tomo-angel7-pilot-01`)の日時・公開状態を変更していない**

## 11. 基準変更と版管理
`policy_version`・`effective_from`・`effective_to`・`supersedes_policy_version`・`superseded_by_policy_version`・`backward_compatible`・`reprocessing_required`・`change_summary`・`change_reason`を管理する。

- `active`な時刻ポリシーを同一versionのまま意味変更しない
- 旧版を削除しない
- 基準タイムゾーン・週開始曜日・締め境界変更は新version対象
- 過去データを新基準で再集計するかを明記する
- 再集計時に元データを上書きしない
- 過去レポートへの影響を確認する
- 同一期間に複数の`active`ポリシーを存在させない

**`effective_from`と`approved_at`の役割の違い(2026-07-16追記):** `effective_from`はポリシー・期間定義が**適用され始める時点**(業務上の発効日時)であり、`approved_at`はCEOが**実際に承認操作を行った時刻**である。両者は別概念であり、同一値になるとは限らない。`created_at`・`updated_at`もそれぞれ、ファイルを実際に作成した日時・最終更新した日時であり、`effective_from`とは別の意味を持つ。

## 12. フェイルクローズ条件
以下のいずれかに該当する場合、日時正規化・期間確定・`validated`化・`locked`化・外部報告使用を停止する。

1. business_timezoneが未決定
2. source_timezoneが不明
3. reporting_timezoneが不明
4. 日時にオフセットがない
5. 元時刻と変換後時刻の対応が不明
6. 変換規則が不明
7. 期間開始が不明
8. 期間終了が不明
9. 包含・排他規則が不明
10. week_start_dayが不明
11. 月次締め境界が不明
12. 会計年度境界が不明
13. 累計値と期間値を区別できない
14. 部分期間か完全期間か不明
15. 外部サービスの表示・CSV・API時刻が競合
16. 夏時間の影響を確認できない
17. KPI定義の要求タイムゾーンと観測値が不一致
18. Finance締め境界と報告期間が競合
19. 同一データが複数期間へ重複する可能性がある
20. 基準変更前後の有効ポリシーが不明
21. 同一期間に複数のactiveポリシーが存在
22. human reviewが未完了
23. CEO reviewが未完了
24. 元データの時刻証跡がない

**停止時に、PCの現在時刻・推定JST・UTC・サービス既定値等を独断で補完しない。**

## 13. Git管理区分

**Git管理対象:** 時刻・期間境界ルール、空テンプレート、状態定義、承認済み時刻ポリシー(将来)、承認済み期間境界定義(将来)、外部サービス時刻対応定義(将来)、版管理情報、機密情報を含まない設定。

**Git管理外:** 実KPI値、実Finance値、実投稿実績、実ログ、実外部サービス設定画面、スクリーンショット、CSV、APIレスポンス、非公開URL、実アカウント名、個人情報、秘密情報、実変換記録、実照合記録。

**本Batchでは`.gitignore`を変更していない。**

## 14. 構成物一覧
- [`_company/governance/templates/business-time-policy-template.yaml`](../../_company/governance/templates/business-time-policy-template.yaml)
- [`_company/governance/templates/source-timezone-mapping-template.yaml`](../../_company/governance/templates/source-timezone-mapping-template.yaml)
- [`_company/governance/templates/reporting-period-definition-template.yaml`](../../_company/governance/templates/reporting-period-definition-template.yaml)

いずれも`sample_only: true`であり、実在する情報を含まない。承認済みポリシー・定義の正式な保存場所は、CEOが`business_timezone`を含む基準を決定した段階で別途確定する。

## 15. 現在の状態(正直な申告)
- **【2026-07-16更新】business_timezoneは`Asia/Tokyo`としてCEO承認済み・`active`。** 正式ポリシー([`business-time-policy__v1.yaml`](../../_company/governance/policies/business-time-policy__v1.yaml))・日次/週次/月次/四半期/年次の期間定義([`_company/governance/reporting-periods/`](../../_company/governance/reporting-periods/))を初回登録済み
- **【2026-07-16更新】** `human_review_status`・`ceo_review`の正式な許容値をCEO決定として「2.1」に追記し、事業時刻ポリシー・5期間定義とも`human_review_status: approved`・`ceo_review: approved`で確定した。この状態値は今回のBatchに限定した定義であり、他の既存ファイル・全社共通語彙への展開は将来課題のまま
- 実外部サービスのタイムゾーン確認は未実施(Instagram・LINE・Google系等がAsia/Tokyoであるとは推測していない)
- 外部サービス時刻対応表(`source-timezone-mapping-template.yaml`)への実登録は別Batchで行う(今回未実施)
- 会計年度は`not_decided`のまま(`fiscal_year_start_month: null`)
- 投稿実績は`published_at`基準とすることをCEO決定済み。ただし実投稿への適用(第1投稿等)は今回行っていない
- 過去データへの影響評価・過去期間の再分類は未実施
- 自動変換は未実装
- 自動期間計算は未実装
- 自動照合は未実装
- migration scriptは未実装
- 外部サービス接続は行っていない
- 完全な期間整合性は未検証
- 実KPIテンプレートへの項目追加(`source_timezone`等の追加検証)は本Batchの範囲外の将来課題

## 16. 変更履歴
| 版 | 日付 | 変更内容 | 変更者 |
|---|---|---|---|
| 1 | 2026-07-14 | 初版作成(環境構築Gate2、事業基準タイムゾーン・報告期間境界管理基盤Batch) | COO |
| 2 | 2026-07-16 | CEO決定によりAsia/Tokyoを事業基準タイムゾーンとして承認・active化。正式ポリシー・日次/週次/月次/四半期/年次の期間定義を初回登録。human_review_status・ceo_reviewの正式な許容値をCEO決定として追記し、5期間定義+ポリシーをapproved/active状態で確定(承認済み事業時刻ポリシー・報告期間定義の初回登録Batch) | COO |
