# AI Company KPI Definition Registry管理Runbook

## 位置づけ
本Runbookは、KPI実績値([`kpi-data-entry-and-validation.md`](kpi-data-entry-and-validation.md))を入力・検証する前提として、各KPIの意味・単位・計算式・集計方法・対象期間・期間境界・タイムゾーン・対象ブランド/チャネル・使用可能なディメンション・元データ・丸め規則・Financeとの関係・版・有効期間・承認状態・廃止/置換関係を正式に定義・版管理するための**ルール・スキーマ・空テンプレート・保存場所の設計**である。

**今回のBatchはKPI definition registryのルール・スキーマ・空テンプレート・保存場所の設計だけを対象とする。** 実際のKPI定義・実績値・目標値・売上値・広告費・計算結果は登録していない。

## 0. 既存構造の調査結果とケース判定

### 0.1 調査結果
- `_company/kpi/schemas/kpi-actuals-schema.yaml`・`_company/kpi/actuals/` ― 会社/ブランド/事業単位の**集約結果を表示するための構造のみ**(`kpi_name`・`value`・`unit`・`computed_by`・`source_reference`・`updated_at`)。KPIの意味・計算式・期間境界・版管理を扱う定義台帳ではない。
- `_company/finance/` ― 金額P&L明細の正本([`kpi-data-entry-and-validation.md`](kpi-data-entry-and-validation.md)「0.4」参照)。KPI定義台帳ではない。
- `_company/analytics/` ― 非財務KPIの観測値・入力Batch・証跡・訂正記録(Git管理外)+空テンプレート(Git管理対象)。KPI定義台帳ではない。
- `brand-brief.md`22章 ― KPI名称・優先順位のみ。単位・計算式・期間境界・版管理の仕組みは持たない。
- `docs/naming-conventions.md`「KPI項目名」規則 ― KPI項目名は**英語・snake_case**(例: `follower_count`・`revenue_jpy`)と既に定義されている。**本Batchの`kpi_id`はこの既存規則を優先し、新しい命名体系を作らない。**
- 正式なKPI definition registry、定義スキーマ、`_company/kpi/templates/`・`_company/kpi/definitions/`に相当する既存構造は確認されなかった。

### 0.2 ケース判定: ケースA
KPI名称・目標・actuals schema・集約構造は存在するが、完全なKPI定義台帳と版管理スキーマは存在しないため、**ケースAとして実装を続行する。**

## 1. KPI定義の正本
`definition_status`は、複数の値を同時に持つものではなく、常に以下いずれか1つを取る単一のライフサイクル状態として扱う: `draft`/`review_required`/`changes_requested`/`approved`/`active`/`deprecated`/`superseded`/`blocked`(詳細は「3. 定義状態」参照)。

**KPI観測値の新規作成に使用可能な正本定義は、最低限以下をすべて満たすものとする。**
- `definition_status`が`active`
- `human_review_required`に応じたhuman reviewが完了している
- CEO reviewが完了している
- 有効期間(`effective_from`〜`effective_to`)内である
- 同一`kpi_id`・同一期間について、他に`active`版が存在しない
- 「12. フェイルクローズ条件」のいずれにも該当しない(`blocked`ではない)
- 必須定義項目(単位・計算式・対象期間・期間境界等)が確定している

- `brand-brief.md`等はKPI名称・目標・優先順位の管理場所であり、完全なKPI定義の正本とは限らない。
- `_company/kpi/actuals/`はKPI定義の正本ではない(集約・表示結果のみ)。
- `_company/analytics/local/`はKPI定義の正本ではない(観測値・証跡・訂正記録のみ)。
- `_company/finance/actuals/`は金額明細の正本であり、KPI定義の正本とは役割が異なる。
- **同一`kpi_id`について複数の有効な正本定義を作らない。**
- `draft`・`review_required`・`changes_requested`の定義を本番観測値の根拠として使用しない。
- **上記の正本条件を満たす`active`な版だけを新規観測値に使用可能とする。**
- 過去観測値の解釈には、その観測時点で使用された`deprecated`または`superseded`版も必要となるため、**旧版の定義は削除せず保持する。**

## 2. kpi_id規則
- `kpi_id`は一意とし、再利用を禁止する
- KPI名称が変更されても、原則として同じ概念であれば`kpi_id`を維持する
- KPIの意味や計算方法が根本的に異なる場合は、新しい`kpi_id`を検討する
- `kpi_id`に実績値・目標値・個人名・顧客名・メールアドレス・電話番号・注文番号・秘密情報・APIキー・トークン・非公開URLを含めない
- 廃止した`kpi_id`を別KPIへ再割当てしない
- **既存の命名規則([`naming-conventions.md`](../naming-conventions.md)「KPI項目名」= 英語・snake_case)を優先する。** 独自のハイフン区切り・ブランド接頭辞付きの新体系は作らない
- 本Batchでは、実在する`kpi_id`を新規発行していない

## 3. 定義状態(definition_status)

| 状態 | 意味 |
|---|---|
| `draft` | 起案中 |
| `review_required` | レビュー待ち |
| `changes_requested` | 修正指示あり |
| `approved` | 必要なレビューと承認を通過した状態(**ただし新規観測値に使用可能とは限らない**) |
| `active` | 新規KPI観測値の根拠として使用可能なライフサイクル状態 |
| `deprecated` | 新規使用を停止するが、過去データの解釈に必要なため保持 |
| `superseded` | 後継の定義版へ置換された状態 |
| `blocked` | 定義不足・競合・権限不足等により使用不能 |

**`approved`と`active`の関係:**
- `approved`は、human reviewとCEO reviewを含む必要なレビュー・承認を通過した状態を意味する。この時点ではまだ新規観測値に使用可能とは限らない。
- `approved`から`active`へ移行するには、有効期間(`effective_from`)の到来その他の利用開始条件を満たす必要がある。
- `active`は、新規KPI観測値の根拠として実際に使用してよいライフサイクル状態を意味する。
- `active`への移行前に、human reviewとCEO reviewの両方が完了していなければならない。
- `effective_from`が未来の場合、`approved`済みであっても`active`にしない。
- `effective_to`を過ぎた定義を`active`のままにしない(到達後は`deprecated`または`superseded`とする)。
- 同一`kpi_id`について、同一期間に複数の`active`版を存在させない。

`definition_status`(定義の承認・利用可否状態)と`public_release_approved`(投稿等の公開承認)は別概念であり、**KPI定義の承認(`approved`/`active`)は、KPI実績値の外部報告承認(`external_reporting_approved`)を意味しない。**

## 4. 版管理
`definition_version`・`effective_from`・`effective_to`・`supersedes_definition_version`・`superseded_by_definition_version`・`change_summary`・`change_reason`・`backward_compatible`・`reprocessing_required`を記録する。

- 承認済み定義を無条件で上書きしない
- 計算式・単位・期間境界・対象範囲・ディメンションが変更された場合は版更新を検討する
- 過去データへ新しい定義を遡及適用するかを明記する。遡及再計算が必要な場合は`reprocessing_required: true`とする
- 旧定義を削除しない
- どの観測値がどの`definition_version`を使用したか追跡可能にする(詳細は「10. KPI観測値との関係」参照)
- 同一期間に複数の`active`版が重複しないようにする
- 有効期間の境界が不明な場合は`active`にしない

### 4.1 定義版ファイルの保存方式
- **1ファイルには1つの`kpi_id`の1つの`definition_version`だけを保存する。**
- 新しい`definition_version`を作成する際、旧版ファイルを上書きしない。
- 旧版ファイルを削除しない。
- Git履歴だけに旧版の保持を依存しない(`deprecated`・`superseded`版も`_company/kpi/definitions/`配下で直接ファイルとして確認可能な状態を維持する)。
- 同じ内容を複数ファイルへ無断複製しない。
- 同一`kpi_id`・同一`definition_version`の重複ファイルを禁止する。
- **新しい版を`active`にする際は、同一`kpi_id`の他の`active`版と有効期間が重複しないことを確認する。**

**ファイル名規則:** `{kpi_id}__v{definition_version}.yaml`

- `kpi_id`は既存規則([`naming-conventions.md`](../naming-conventions.md)「KPI項目名」)どおり英語・snake_case
- 区切りは二重アンダースコア(`__`)
- `definition_version`の前に`v`を付ける
- ファイル名に空白を含めない
- スラッシュ・バックスラッシュ・コロン等、パス解釈へ影響する文字を含めない
- 個人情報・実績値・目標値・金額・非公開情報を含めない
- 大文字・小文字の扱いを統一する(`kpi_id`側の規則に合わせ小文字を基本とする)
- 同一ファイル名を別の定義へ再利用しない
- `definition_version`にファイル名として不適切な文字(空白・スラッシュ・バックスラッシュ・コロン等)が含まれる場合は、それらの文字を除去またはハイフンに置換したうえで正規化し、正規化前後の対応が追跡できるよう`change_summary`等に記録する

**注意:** 上記は抽象的な命名形式の説明であり、本Batchでは実在する`kpi_id`・`definition_version`を用いたファイル例は作成していない。

## 5. 値・単位・型
`count`・`percentage`・`currency`・`duration`・`ratio`・`score`・`boolean`を扱う。`unit`・`percentage_scale`・`currency_code`・`tax_treatment`・`decimal_precision`・`minimum_value`・`maximum_value`・`negative_value_allowed`・`zero_allowed`・`null_allowed`を定義する。

- `percentage`は0〜1か0〜100かを必ず明示する(`percentage_scale`)
- `currency`はISO通貨コードを明示する(`currency_code`)
- 金額は税込・税抜・対象外等を明示する(`tax_treatment`)
- `0`と`null`を区別する。未取得を`0`で代用しない
- 無限大・NaN・不明な文字列を数値として扱わない
- 単位が未決定の場合は`blocked`
- 別単位の値を同一KPIとして無条件に混在させない

## 6. 期間・タイムゾーン
`reporting_frequency`・`aggregation_level`・`period_boundary_rule`・`business_timezone`・`source_timezone_policy`・`week_start_day`・`month_boundary_rule`・`cumulative_value_allowed`を定義する。

**事業上の基準タイムゾーンは現在`not_decided`([`kpi-data-entry-and-validation.md`](kpi-data-entry-and-validation.md)参照)のため、本Batchで独断決定していない。** `business_timezone`が未決定のKPIは、期間解釈に影響する場合`blocked`とする。source timezoneとreporting timezoneを区別する。日次・週次・月次・累計を区別し、期間開始・終了が含むか含まないかを明記する。週の開始曜日・月末/月初の境界を明記する。累計値と期間値を同じ定義として混同しない。**2026-07-14追加:** 事業基準タイムゾーン・期間境界の全社統一ルールは[`business-time-and-reporting-period-governance.md`](business-time-and-reporting-period-governance.md)を正本として参照する(本Runbookでは重複記載しない)。

## 7. 集計方法・計算式
`raw_or_derived`・`aggregation_method`・`calculation_formula`・`formula_version`・`numerator_kpi_id`・`denominator_kpi_id`・`input_kpi_ids`・`rounding_rule`・`missing_value_rule`・`divide_by_zero_rule`・`duplicate_handling_rule`を定義する。

`aggregation_method`候補: `sum`・`count`・`distinct_count`・`average`・`weighted_average`・`minimum`・`maximum`・`latest`・`ratio`・`custom`。

- 計算式が不明なKPIは`active`にしない
- 分母が0の場合の扱いを明記する(`divide_by_zero_rule`)
- 欠損値を0として扱うか除外するかを明記する(`missing_value_rule`)
- 丸め前と丸め後の値を区別する
- derived KPIは入力元KPI(`input_kpi_ids`)を追跡可能にする
- `custom`計算は説明(`change_summary`等)と版管理を必須とする
- 計算式変更後に過去値との比較可能性が失われる場合、その影響を明記する(`reprocessing_required`等)

## 8. ディメンション
`required_dimensions`・`optional_dimensions`・`prohibited_dimensions`で、`brand_id`・`channel`・`account`・`content_id`・`campaign_id`・`product`・`service`・`region`・`audience_segment`・`device`・`organic_paid`等を必要に応じて許可できる構造とする。

- 許可されていないディメンションで観測値を分割しない
- 必須ディメンションと任意ディメンションを区別する
- ディメンション値の順序違いだけで別定義を作らない
- **顧客個人を特定するディメンションを禁止する。** 顧客氏名・メールアドレス・電話番号・LINEユーザーID等はディメンションとして使用しない(`prohibited_dimensions`)
- ディメンション変更が過去比較へ影響する場合は版管理対象とする

## 9. ソースと証跡
`allowed_source_systems`・`preferred_source_system`・`source_field_reference`・`evidence_required`・`evidence_types`・`source_freshness_rule`・`manual_entry_allowed`・`imported_entry_allowed`・`calculated_entry_allowed`を定義する。

- ソースが不明なKPIは`active`にしない
- 同一KPIに複数ソースがある場合、優先順位を明記する(`preferred_source_system`)
- source systemの仕様変更時は定義レビューを行う
- 外部サービスの表示値だけを唯一の長期証跡としない
- 手入力を許容する場合も証跡要件を明記する
- API・CSV・画面表示のどれを利用するか明記する
- **本Batchでは、外部サービスの実確認やAPI接続は行っていない**

## 10. KPI観測値との関係
[`kpi-data-entry-and-validation.md`](kpi-data-entry-and-validation.md)の観測値には、最低限`kpi_id`・定義版参照・対象期間・値・単位・ディメンション・元データ・証跡を記録できる必要がある。

**2026-07-14追加:** `kpi-observation-template.yaml`・`kpi-entry-batch-template.yaml`へ、`definition_version`・`definition_reference`・`definition_commit_id`等の定義版トレーサビリティ項目を追加した。ルール・状態定義・GitコミットIDの扱い・旧観測値の後方互換性方針の詳細は、[`kpi-definition-observation-traceability.md`](kpi-definition-observation-traceability.md)を正本として参照する(本Runbookでは重複記載しない)。

**この項目追加はルール・空テンプレートの整備であり、実際の観測値と定義版の対応付け・Git履歴調査・旧観測値の移行は行っていない。** テンプレートに項目が存在することをもって、観測値とのトレーサビリティが完成したとは評価しない。Definition Registry(定義)とAnalytics(観測値)は、実際の解決記録が作成されるまで実質的に独立している。

## 11. Financeとの境界
- 売上・広告費・手数料・原価・費用・利益等の生の金額明細は`_company/finance/actuals/`が正本
- KPI definition registryは金額明細そのものを保存しない
- `revenue`・`cost`・`profit`等をKPIとして定義する場合も、実数値はFinance正本から取得する
- `value_type: currency`のKPI定義がFinance正本を置き換えることはない
- 通貨建て派生KPIはFinance側の根拠を追跡できなければ`blocked`とする(`finance_source_required: true`の場合)
- Finance側とKPI定義側で税区分・通貨・期間・集計方法が競合する場合は使用停止
- FinanceからAnalytics・KPI集約層への自動連携は未実装
- **本Batchでは、Finance実績値を作成・変更していない**

## 12. フェイルクローズ条件
以下のいずれかに該当する場合、KPI定義を`active`にしない。

1. KPIの意味が不明
2. `kpi_id`が重複
3. KPI名称だけで完全な定義がない
4. 値の型が不明
5. 単位が不明
6. percentage scaleが不明
7. currency codeが不明
8. 税区分が不明
9. 計算式が不明
10. 集計方法が不明
11. 対象期間が不明
12. 期間境界が不明
13. タイムゾーンが不明
14. 丸め規則が不明
15. 欠損値の扱いが不明
16. 0とnullの扱いが不明
17. 分母0の扱いが不明
18. 必須ディメンションが不明
19. 許可ディメンションが不明
20. source systemが不明
21. ソースの優先順位が不明
22. Financeとの境界が不明
23. 同一期間に複数のactive版が存在
24. 有効期間が重複
25. 後継定義との関係が不明
26. 人間レビューが未完了
27. CEOレビューが未完了
28. 個人情報を含む設計
29. 秘密情報を含む設計
30. 既存KPI actuals schemaとの関係が不明

**停止時に、推測で単位・計算式・期間・タイムゾーン等を補完しない。**

## 13. Git管理区分

**Git管理対象:** KPI定義ルール、KPI定義スキーマ、空テンプレート、承認済みKPI定義(将来)、版管理情報、変更理由、機密情報を含まない定義情報。

**Git管理外:** KPI実績値、Finance実数値、証跡ファイル、スクリーンショット、CSV、APIレスポンス、非公開URL、顧客単位データ、個人情報、秘密情報、実際の入力Batch、実訂正記録。

**本Batchでは`.gitignore`を変更していない。** KPI定義自体は(承認済みであれば)Git管理対象として想定しているため、既存の`_company/kpi/actuals/`除外規則を流用・変更する必要がなかった。

## 14. 構成物一覧
- [`_company/kpi/schemas/kpi-definition-schema.yaml`](../../_company/kpi/schemas/kpi-definition-schema.yaml) ― 構造定義(既存`kpi-actuals-schema.yaml`と同形式)
- [`_company/kpi/templates/kpi-definition-template.yaml`](../../_company/kpi/templates/kpi-definition-template.yaml) ― 空テンプレート
- [`_company/kpi/definitions/README.md`](../../_company/kpi/definitions/README.md) ― 将来の正本保存場所の説明(実定義ファイルは今回作成していない)

いずれも`sample_only: true`(スキーマ・テンプレート)であり、実在する情報を含まない。

### 14.1 スキーマの性質(重要)
`kpi-definition-schema.yaml`は、既存`_company/kpi/schemas/kpi-actuals-schema.yaml`と同じ記述方式(ヘッダー+コメントによる許容値説明+`records:`サンプル)に合わせた**説明的・規約的なYAMLスキーマ**である。

- **現時点では説明的・規約的なスキーマであり、JSON Schema等の自動検証機構ではない。**
- 自動バリデーションは未実装である。
- スキーマファイルが存在するだけで、実定義ファイルの構文・内容が自動的に保証されるわけではない。
- 将来、既存形式との互換性を確認したうえで、機械検証(バリデータ)を追加する可能性がある。
- **本Batchでは、新しい依存関係やバリデータを追加していない。**
- 既存の`kpi-actuals-schema.yaml`は変更していない。

## 15. 現在の状態(正直な申告)
- 実際のKPI定義は登録していない
- 実在する`kpi_id`は発行していない
- 実目標値・実績値は登録していない
- Finance記録は変更していない
- 事業上の基準タイムゾーンは未確定(`not_decided`)
- 観測値とdefinition_versionを結び付ける項目は追加済みだが、実際の対応付け・解決記録の作成は未実施(詳細は[`kpi-definition-observation-traceability.md`](kpi-definition-observation-traceability.md)参照)
- 自動バリデーション機構は未実装(`kpi-definition-schema.yaml`は説明的スキーマ)
- KPI Definition RegistryとAnalytics観測値の完全なエンドツーエンドのトレーサビリティは未検証
- `.gitignore`は変更していない(定義ファイル自体はGit管理対象として想定するため)
- 外部サービス接続は行っていない

## 16. 変更履歴
| 版 | 日付 | 変更内容 | 変更者 |
|---|---|---|---|
| 1 | 2026-07-14 | 初版作成(環境構築Gate2、KPI Definition Registry管理基盤Batch) | COO |
