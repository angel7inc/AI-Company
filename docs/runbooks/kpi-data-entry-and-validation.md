# AI Company KPI実績値入力・検証・証跡管理Runbook

## 位置づけ
本Runbookは、Instagram・LINE・Webサイト・ココナラ・広告・投稿実績等のKPIについて、実績値(observation)の入力・検証・証跡・訂正を管理するための**ルールと空テンプレートの設計**である。**今回のBatchはKPI実績値の入力・検証・証跡・訂正に関するルールと空テンプレートの設計だけを対象とする。** 実際のKPI数値の取得・入力・集計・計算、ダッシュボード作成、外部サービス接続は行っていない。

**2026-07-14追加:** `kpi-observation-template.yaml`・`kpi-entry-batch-template.yaml`には、使用したKPI定義版を追跡するための項目(`definition_version`等)を追加した。定義版とのトレーサビリティの詳細ルールは[`kpi-definition-observation-traceability.md`](kpi-definition-observation-traceability.md)を正本として参照する(本Runbookでは重複記載しない)。

## 0. 既存構成との関係(重複回避)

### 0.1 既存のKPI・Finance構造の調査結果
本リポジトリには既に以下が存在する。

- `_company/kpi/schemas/kpi-actuals-schema.yaml`・`_company/kpi/actuals/`(Git管理外、`.gitkeep`のみ追跡) ― `kpi-actuals-schema.yaml`自身が明記するとおり、**会社/ブランド/事業単位の集約結果を表示するための構造のみ**を定義する。「実際の数値の正本は各計算元エージェント側にある」と明記されており、生の観測値(raw observation)の入力・検証・証跡・訂正を扱う場所ではない。
- `_company/finance/schema-sample.yaml`・`_company/finance/actuals/`(Git管理外) ― `finance-controller`が管理する、**金額を伴うP&L明細(売上・プラットフォーム手数料・返金・広告費・AI/APIコスト等)の正本**。`evidence_id`参照フィールド・`status`フィールド・独自のレコードID体系(`fin-{yyyymm}-{brand_id}-...`)を既に持つ。
- `_company/brands/tomo-angel7/brand-brief.md`22章 ― `save_rate`・`share_rate`・`follower_count`・`revenue_jpy`等、**KPIの名称・優先順位のみ**を定義。実績値そのもの・検証・証跡・訂正の仕組みは持たない。
- `_shared/sop/analytics/README.md` ― 数値取得・分析・レポート作成の**実行手順**を扱う場所であり、「実際のKPI実績データそのもの」は明示的に対象外としている。

### 0.2 判定
- **売上・広告費・AI/APIコスト等、金額を伴うP&L明細**については、`_company/finance/actuals/`が既に実績入力・証跡参照(`evidence_id`)・状態(`status`)を持つ正本として存在する。**本Batchの新スキーマ(`kpi_id`)は、これらの金額P&L明細を重複して入力対象としない。** Finance側の実績は引き続き`_company/finance/actuals/`のみに記録する。
- **Instagram・LINE・Webサイト・ココナラ等の非財務KPI(保存率・シェア率・フォロワー数・エンゲージメント・リーチ・インプレッション・投稿単位の実績等)** については、名称・優先順位のみが存在し、実績値の入力・検証・証跡・訂正管理スキーマは存在しない。**この部分についてケースAと判定し、本Batchで新設する。**
- `_company/kpi/actuals/`(集約結果)と本Batchの`_company/analytics/local/observations/`(生の観測値)は役割が異なる。将来、検証済み(`validated`)観測値を`_company/kpi/actuals/`側の集約スナップショットへどう反映するかは、本Batchの範囲外とし、未決定事項として扱う。

### 0.3 総合ケース判定: ケースA(範囲限定つき)
既存のP&L明細管理(Finance)と重複しない範囲(非財務・チャネル横断の操作系KPI)において、実績値の入力・検証・証跡・訂正管理スキーマが存在しないため、**ケースAとして実装を続行する。** 金額を伴うP&L明細はFinance側の既存正本を変更せず、本Runbookの対象からも明示的に除外する。

### 0.4 Finance・Analytics・KPI集約層の役割分担
本リポジトリには、実績値に関わる3つの層が存在する。役割を以下のとおり明確化する。

| 層 | 保存場所 | 扱うもの |
|---|---|---|
| Finance | `_company/finance/actuals/`(既存、Git管理外) | 売上・広告費・手数料・原価・AI/APIコスト・その他費用・利益等、**金額P&L明細の正本** |
| Analytics | `_company/analytics/local/`(本Batch新設、Git管理外) | **非財務KPI**の観測値・入力Batch・証跡・検証結果・訂正記録、および上流の根拠を追跡できる派生分析値 |
| KPI集約 | `_company/kpi/actuals/`(既存、Git管理外) | 上流データ(Finance・Analytics)から作成される**集約結果・表示用スナップショット・レポート用結果** |

**`_company/kpi/actuals/`は、生データ・Finance明細・非財務KPI観測値のいずれの正本でもない。** あくまで上流データから作成される集約・表示レイヤーであり、`_company/kpi/schemas/kpi-actuals-schema.yaml`が既に明記するとおり「実際の数値の正本は各計算元エージェント側にある」。

## 1. 目的と適用範囲
- KPI実績値の入力・検証・証跡・訂正を管理するRunbookである
- KPIの戦略的な定義や目標設定そのものは別管理である(`brand-brief.md`・`docs/phase2-instagram-design.md`等が正本)
- 実績値(`actual`)と目標値(`target`)を混同しない
- 実績値と予測値(`forecast`)を混同しない
- 生データ(`raw metric`)と計算済み指標(`derived metric`)を区別する
- Git履歴は実KPIデータの正本ではない
- GitHubだけでは実績値や証跡を復元できない([`operations-continuity.md`](operations-continuity.md)参照)
- 外部サービス画面だけでも過去時点の数値を完全再現できない場合がある(集計仕様変更・表示遅延・サンプリング等により、後日同じ画面を見ても当時と同じ値が出るとは限らない)

## 2. 用語

| 用語 | 定義 |
|---|---|
| KPI definition | KPIの名称・意味・単位・計算式・対象期間・期間境界・ディメンション・集計方法・丸め規則等(本Runbookの対象外)。`brand-brief.md`等は名称・目標・優先順位の**既存の管理場所**だが、これらすべてを含む完全な定義が確定していることを保証するものではない(詳細は「3. KPI定義と観測値の分離」参照) |
| KPI observation | 1回の観測・入力を表す実績値レコード(`kpi-observation-template.yaml`) |
| raw metric | 元データそのままの値(計算前) |
| derived metric | 他の値から計算された値 |
| target | 目標値 |
| actual | 実績値 |
| forecast | 予測値 |
| reporting period | 報告対象期間 |
| source period | 元データの対象期間(reporting periodと異なる場合がある) |
| collection timestamp | データを取得した日時(`collected_at`) |
| source system | 元データの取得元システム・サービス |
| source reference | 元データの参照情報(非公開URL・実ファイルパス等の実値はテンプレートへ記載しない) |
| evidence record | 証跡記録(`kpi-evidence-record-template.yaml`) |
| entry batch | 1回の入力作業単位(`kpi-entry-batch-template.yaml`) |
| correction record | 訂正記録(`kpi-correction-record-template.yaml`) |
| dimension | ブランド・チャネル・アカウント・コンテンツ・キャンペーン等の分類軸 |
| aggregation level | 集計の粒度(日次/週次/月次/累計等) |
| current effective record | 現在有効な記録(訂正により無効化されていない) |
| superseded record | 訂正により置き換えられた過去記録 |

## 3. KPI定義と観測値の分離
KPIの定義と実際の数値は別物として扱う。観測値を入力するには、原則として以下が明確であることを要求する。

`kpi_id`・KPI名称・定義・単位・集計方法・対象期間・対象チャネル・対象ブランド・対象コンテンツまたは集計範囲・元データ・計算式・丸め規則。

**KPI定義が未確定または複数の意味に解釈できる場合は`blocked`とする。** 本Batchでは、既存KPI定義の変更や新しいKPIの実登録は行っていない。

**`brand-brief.md`等の位置付け:** `brand-brief.md`等は、KPI名称・目標値・優先順位の既存の管理場所である。ただし、そこに記載された名称だけでは、観測値の入力・検証の根拠として不十分である。単位・計算式・対象期間・期間境界・ディメンション・集計方法・丸め規則等が確定していないKPIは、名称が`brand-brief.md`等に存在していても`blocked`として扱う。**本Batchでは、正式なKPI definition registry(上記の各要素を網羅する定義台帳)を新設していない。KPI定義の正式な保存方法は未決定であり、将来課題とする。本Runbookおよび今回作成した4テンプレート自体を、KPI定義の正本として扱わない。**

## 4. ID規則
最低限、以下のIDを定義する。`observation_id`・`entry_batch_id`・`evidence_id`・`correction_id`。

各IDは一意とし、再利用を禁止する。IDには個人名・顧客名・メールアドレス・電話番号・注文番号・APIキー・トークン・非公開URL・実際の売上金額・実際のKPI値を直接含めない。**削除・無効化・訂正後も、別の記録へ同じIDを再割当てしない。**

## 5. 記録状態(observation_status)

| 状態 | 意味 |
|---|---|
| `planned` | 観測の必要性だけが決まっている |
| `awaiting_source` | 元データの取得待ち |
| `captured` | 入力済み。**ただし正しいことを意味しない**(検証前) |
| `validation_required` | 検証待ち |
| `validated` | 検証済み。**ただし公開承認や経営判断への使用承認とは同義ではない** |
| `rejected` | 検証の結果、無効と判断された |
| `superseded` | 訂正により置き換えられた |
| `locked` | 確定済み報告期間として凍結された |
| `blocked` | KPI定義未確定・権利/アクセス不明等により先へ進めない |

`captured`と`validated`、`validated`と`external_reporting_approved`をそれぞれ混同しない設計とする。

## 6. 入力方法(entry_method)
`manual`(手入力)・`imported`(取込み)・`calculated`(計算値)を区別する。

- 手入力: 入力者(`recorded_by`)と入力日時(`recorded_at`)を必要とする
- インポート: 元ファイル・元システム(`source_system`)・取得日時(`collected_at`)を必要とする
- 計算値: 計算式(`calculation_formula_reference`)・計算式のバージョン(`calculation_version`)・入力元observation_id(`input_observation_ids`)・丸め規則(`rounding_rule`)・欠損値の扱いを必要とする

**本Batchでは、CSV取込み・API取込み・自動計算処理は実装していない。**

## 7. 期間・タイムゾーン
`source_period_start`・`source_period_end`・`collected_at`・`recorded_at`・`validated_at`を区別する。期間の境界(開始・終了を含むか含まないか)が不明な場合、入力を確定しない(`period_boundary_rule`で明示)。日次・週次・月次・累計値を区別する。

事業上の基準タイムゾーンは、本リポジトリ内の既存文書で明示的に確定されていなかったため、独断で決定せず`not_decided`として扱う(下記「20. 現在の状態」参照)。外部サービスの表示タイムゾーンと事業報告タイムゾーンが異なる場合、変換前(`source_timezone`)と変換後(`reporting_timezone`)を区別する。**2026-07-14追加:** 事業基準タイムゾーン・週次/月次/年次の期間境界を統一管理するルール・空テンプレートは[`business-time-and-reporting-period-governance.md`](business-time-and-reporting-period-governance.md)を正本として参照する(本Runbookでは重複記載しない)。

## 8. 単位とデータ型
`count`・`percentage`・`currency`・`duration`・`ratio`・`score`・`boolean`を区別する。

- パーセント値が0〜1か0〜100かを明示する
- 通貨コードを必須とする(`currency`)
- 金額の税込・税抜を明示する(`tax_treatment`)
- 小数点以下の桁数を明示する(`decimal_precision`)
- 単位が異なる値を直接合算しない
- 不明な単位の値を登録確定しない
- `0`と`null`を区別する(未取得を`0`で代用しない)

### 8.1 金額データの重複防止
売上・広告費・手数料・原価・費用・利益等の**生の金額実績**を、Analytics層(本Batchの観測値スキーマ)へ重複入力しない。`value_type: currency`は、Finance正本(`_company/finance/actuals/`)を置き換えるために使用しない。FinanceとAnalyticsの双方に、同一金額について独立した正本を作らない。Finance側の記録が存在する金額については、**Finance側を正本として扱う。** Finance正本とAnalytics観測値のどちらが有効か不明な場合は、検証済み化および外部報告への使用を停止する。**本Batchでは、Finance関連ファイル・既存Financeスキーマを一切変更していない。**

### 8.2 通貨建て派生KPIの根拠要件
通貨建ての派生KPI(例: Finance実績から算出するCVR・CPA等)をAnalytics層で扱う場合、以下を必須とする。

- Finance側の根拠記録を追跡できる
- `source_system`が設定されている
- `source_reference`が設定されている
- 必要な`evidence_ids`が設定されている
- 計算値の場合は`calculation_formula_reference`が設定されている
- 計算値の場合は`calculation_version`が設定されている
- 入力元となる記録を`input_observation_ids`等で追跡できる
- 丸め規則が明確である
- Finance正本との整合を確認できる

**Finance側の根拠が不明・欠落・競合、またはアクセス不能の場合は`blocked`とする。** 既存の4テンプレート(`source_system`・`source_reference`・`evidence_ids`・`input_observation_ids`・`calculation_formula_reference`・`calculation_version`の各項目を含む)は、この根拠追跡要件を満たす設計となっている。これらが未設定のまま`validated`にできることを示唆する記載は、本Runbookのいかなる箇所にも存在しない。

## 9. ディメンション
`brand_id`・`channel`・`account`・`content_id`・`campaign_id`・`product`・`service`・`region`・`audience_segment`・`device`・`organic_or_paid`等を必要に応じて区別できる構造とする(`dimensions`フィールド)。**顧客個人を特定するディメンションは扱わない。** ディメンションの順序違いだけで別記録として扱わないよう、キー名を正規化して比較する方針とする。

## 10. 重複入力防止
重複確認キーの候補: `kpi_id`・`source_system`・`source_period_start`・`source_period_end`・`aggregation_level`・`dimension_set`・`calculation_version`。同一条件の記録が存在する可能性がある場合、無条件で新規登録せず停止する(`duplicate_check_status`)。

確認対象: 同じ期間の二重入力、累計値と期間値の混同、日次値の合計と月次値の二重計上、organicとpaidの重複、訂正前記録と訂正後記録の同時集計、同一CSVの再取込み、タイムアウト後の再入力、同一画面を複数人が入力した可能性。

**idempotency機構は今回実装していない(将来課題)。**

## 11. 検証
最低限の検証項目: 必須項目、データ型、単位、通貨、期間、タイムゾーン、重複、数値範囲、前回値との急変、合計と内訳の整合、元データとの一致、計算式、丸め誤差、欠損値、負数が許容される指標か、将来期間の値ではないか、証跡の有無、ソースの取得時点、訂正記録との関係。(全18項目)

**急変だけを理由に値を自動修正しない。検証不能な値は`validated`にしない。**

## 12. 証跡
証跡には`evidence_id`・`source_system`・`source_type`・`source_reference`・`source_period`・`collected_at`・`collected_by`・`evidence_location`・`screenshot_present`・`export_file_present`・`access_status`・`retention_status`・`personal_data_status`・`secret_information_status`・`notes`を記録できるようにする。

**Git管理テンプレートには、実URL・非公開URL・実ファイルパス・実アカウント名を記載しない。** 証跡が外部サービスにしか存在しない場合、そのサービスを唯一のバックアップ先として扱わない。

## 13. 訂正ルール
検証済み記録を無条件で上書きしない。訂正時は`correction_id`・`original_observation_id`・`replacement_observation_id`・`correction_reason`・`requested_by`・`approved_by`・`corrected_at`・`evidence_id`・`impact_scope`・`downstream_reports_affected`・`notification_required`を記録する。

元記録は削除せず、原則として`superseded`にする。単純な誤字訂正(`typo_correction`)と数値訂正(`value_correction`)を区別する。**訂正後、関連する集計・レポート・意思決定資料への影響確認が必要である。**

## 14. ロック
確定済みの報告期間について、記録を`locked`にできる設計とする。ロック後の変更には訂正記録と承認が必要とする。**ロック機構そのものは今回実装していない。**

## 15. 機密性・個人情報
Git管理外・慎重取扱いの対象: 売上実数、利益実数、広告費、顧客数、注文数、成約率、外部サービス管理画面、顧客氏名、顧客ID、メールアドレス、電話番号、LINEユーザー情報、決済情報、注文詳細、APIレスポンス、非公開URL、アカウント識別情報、スクリーンショット内の通知、ブラウザタブ、ローカルパス内のユーザー名。

**KPI管理では、可能な限り集計済み数値だけを扱い、顧客単位データを保存しない方針とする。**

## 16. フェイルクローズ条件
以下のいずれかに該当する場合、入力確定・検証済み化・外部報告への使用を停止する。

1. KPI定義が存在しない
2. KPI定義が曖昧(KPI名称は存在するが、単位・計算式・対象期間・期間境界・ディメンション・集計方法・丸め規則等が確定していない場合を含む)
3. 単位が不明
4. 通貨が不明
5. 税込・税抜が不明
6. 対象期間が不明
7. 期間境界が不明
8. タイムゾーンが不明
9. source systemが不明
10. 証跡がない
11. 元データへアクセスできない
12. 重複の可能性がある
13. 累計値と期間値を区別できない
14. 手入力値と計算値を区別できない
15. 計算式が不明
16. 計算式バージョンが不明
17. ディメンションが不明
18. 合計と内訳が一致しない
19. 不可能な数値範囲
20. 訂正前後の有効記録が不明
21. 検証担当者が未確認
22. 秘密情報・個人情報の混入可能性がある
23. 外部報告への使用承認がない
24. バックアップ・復元関係が不明な重要実績値
25. 金額実績についてFinance正本との役割分担が不明である
26. 通貨建て派生KPIについて、Finance側の根拠記録を追跡できない
27. Finance側の実績値とAnalytics側の観測値が競合し、どちらが有効か判定できない
28. 集約結果(`_company/kpi/actuals/`等)と、生データ・Finance明細・Analytics観測値の正本を区別できない、または集約値を正本と誤認するおそれがある

**停止時に、推測値・仮値・前回値・`0`を代入しない。**

## 17. バックアップとの関係
- GitHubはルールと空テンプレートの保存先
- 実KPI値のバックアップ先は別途選定が必要(`not_decided`)
- 外部サービス画面だけを唯一の保存先としない
- CSV・スクリーンショット・証跡の取扱いは別途決定する必要がある
- **KPI実績値は[`operations-continuity.md`](operations-continuity.md)上のGit管理外機密実績データ(区分B)に該当する**
- backup manifestとの連携は将来実装
- 今回、実バックアップは作成していない。復元テストも今回実施していない

## 18. Git管理区分

**Git管理対象:** KPI入力ルール、KPI検証ルール、空テンプレート(`_company/analytics/templates/`)、状態定義、ID規則、訂正手順、フェイルクローズ条件、実データを含まないスキーマ。

**Git管理外(`_company/analytics/local/`):** 実際のKPI実績値、実際の売上値・Finance実数値(既存の`_company/finance/actuals/`側で管理)、実際のフォロワー数・表示数・反応数、実際の広告費、実際のコンバージョン数、実際の顧客数・注文数、実際のKPI入力Batch、実際の証跡記録、管理画面のスクリーンショット、CSV等の外部サービス書き出しデータ、実際の訂正記録、実際の検証結果、非公開URL、個人情報、顧客単位データ、APIレスポンス、認証情報。

## 19. テンプレート一覧
- [`_company/analytics/templates/kpi-observation-template.yaml`](../../_company/analytics/templates/kpi-observation-template.yaml)
- [`_company/analytics/templates/kpi-entry-batch-template.yaml`](../../_company/analytics/templates/kpi-entry-batch-template.yaml)
- [`_company/analytics/templates/kpi-evidence-record-template.yaml`](../../_company/analytics/templates/kpi-evidence-record-template.yaml)
- [`_company/analytics/templates/kpi-correction-record-template.yaml`](../../_company/analytics/templates/kpi-correction-record-template.yaml)

いずれも`sample_only: true`であり、実在する情報を含まない。

## 20. 現在の状態(正直な申告)
- 実KPI観測値は未作成
- 実入力Batchは未作成
- 実証跡記録は未作成
- 実訂正記録は未作成
- 外部サービスの数値は取得していない
- KPI定義の実登録・変更はしていない
- 事業上の基準タイムゾーンは未確定(`not_decided`)
- バックアップ先は未選定
- 復元テストは未実施
- 自動取得は未実装
- 自動検証は未実装
- 自動集計は未実装
- idempotency機構は未実装
- ロック機構は未実装
- ダッシュボードは未実装
- 外部サービス接続は行っていない
- FinanceからAnalyticsへの自動連携は未実装
- FinanceとAnalyticsの自動照合は未実装
- Analyticsから`_company/kpi/actuals/`への自動集約は未実装
- 集約スナップショットの生成方法は未確定
- 正式なKPI definition registryは未実装

## 21. 変更履歴
| 版 | 日付 | 変更内容 | 変更者 |
|---|---|---|---|
| 1 | 2026-07-14 | 初版作成(環境構築Gate2、KPI実績値入力・検証・証跡管理基盤Batch) | COO |
