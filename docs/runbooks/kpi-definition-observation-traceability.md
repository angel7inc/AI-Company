# AI Company KPI定義版・観測値トレーサビリティ統合Runbook

## 位置づけ
本Runbookは、[`kpi-definition-governance.md`](kpi-definition-governance.md)(KPI定義台帳)と[`kpi-data-entry-and-validation.md`](kpi-data-entry-and-validation.md)(KPI観測値)の間で、**どのKPI観測値がどのKPI定義版を根拠として入力・計算・検証されたかを将来確実に追跡できる**ための、ルール・空テンプレート・既存テンプレートへの項目追加・後方互換性方針を定義する。

**今回のBatchはルール・空テンプレート・既存テンプレートの項目追加・後方互換性方針の設計だけを対象とする。** 実際のKPI定義・実観測値・実定義版解決記録・実績値・実金額は作成していない。

## 0. 既存構造の調査結果とケース判定

### 0.1 調査結果
- `kpi-observation-template.yaml`・`kpi-entry-batch-template.yaml`・`kpi-evidence-record-template.yaml`・`kpi-correction-record-template.yaml`(いずれも`schema_version: 1`) ― 観測値・入力Batch・証跡・訂正の各記録を扱うが、`definition_version`・定義ファイル参照・Gitコミット参照のいずれの項目も持たない。
- `kpi-definition-schema.yaml`・`kpi-definition-template.yaml` ― KPI定義自体の版管理(`definition_version`・`supersedes_definition_version`等)は持つが、観測値側からの参照方法は扱わない。
- `_company/analytics/local/reconciliation/`(`.gitignore`で既存除外済み) ― これまで実データもテンプレートも存在せず、明確な既存用途が確定していなかった。本Batchで、定義版解決記録の保存候補として用途を明確化する。
- `kpi-definition-governance.md`「10. KPI観測値との関係」・`kpi-data-entry-and-validation.md`双方に、「観測値とdefinition_versionの参照方法は未確定」という将来課題としての記載が既に存在する(新規の食い違いではなく、本Batchが着手する対象そのもの)。
- Gitコミットを証跡として使う既存ルール、旧観測値の移行・解決ルール、定義変更に伴う再計算ルールは、いずれも本リポジトリ内に確認されなかった。
- 既存4テンプレートの`schema_version`更新規則(項目追加時に必ず版を上げる等)は、本リポジトリ内に確認されなかった。

### 0.2 ケース判定: ケースA
KPI定義台帳と観測値管理は存在するが、観測値からdefinition_version・定義ファイル・Gitコミットを追跡する構造は存在しないため、**ケースAとして実装を続行する。**

## 1. トレーサビリティの基本原則
- KPI観測値は`kpi_id`だけでなく、使用した`definition_version`を追跡できなければならない。
- `definition_version`だけでなく、定義ファイルのリポジトリ相対パス(`definition_reference`)を記録する。
- 定義ファイルを確定したGitコミットID(`definition_commit_id`)を記録する。
- **GitコミットIDは、定義ファイルの内容を後から確認するための証跡である。** ブランチ名だけを定義版の証跡として使用しない。
- `latest`・`current`・「最新版」等の**可変的な参照だけ**を保存しない(必ず固定されたコミットID・版番号で記録する)。
- 観測値の対象期間に対して、その定義版が有効だったかを確認する(`definition_effective_for_period`)。
- 定義版参照が未解決の観測値を`validated`・`locked`・`current_effective_record`・`external_reporting_approved`にしない。
- 推測で定義版を割り当てない。
- 同じ`kpi_id`でも`definition_version`が異なる観測値を無条件に比較・合算しない。
- 過去観測値の解釈には`deprecated`または`superseded`版が必要な場合がある。
- Git履歴だけでなく、観測値側にも使用版を記録する(Git履歴の存在だけでは、どの観測値がどの版を使ったかを引けない)。
- 実観測値・実解決記録はGit管理外とする。

## 2. active定義の意味内容の保護

### 2.1 意味内容を変える変更(新しいdefinition_versionが必要)
以下の変更は、同じ`definition_version`へ上書きせず、**新しい`definition_version`を作成する対象**とする。

KPIの意味、`value_type`、`unit`、`percentage_scale`、`currency_code`、`tax_treatment`、計算式、集計方法、期間境界、`business_timezone`、丸め規則、欠損値の扱い、分母0の扱い、必須ディメンション、許可ディメンション、ソースシステム、Financeとの境界。**2026-07-14追加:** `business_timezone`・期間境界の全社統一ルールは[`business-time-and-reporting-period-governance.md`](business-time-and-reporting-period-governance.md)を正本として参照する。

### 2.2 ライフサイクル情報
`active`版を`deprecated`または`superseded`へ変更する等のライフサイクル更新は、意味内容の変更と区別する。**ライフサイクル情報の変更もGitコミットとして記録する。** 過去観測値が参照する`definition_version`自体は変更しない。

### 2.3 上書き禁止
- `active`版の意味内容を同じ`definition_version`で上書きしない
- 観測値が参照済みの定義版を削除しない
- 定義ファイル名を無断で変更しない
- 定義ファイルを別パスへ無断で移動しない
- 変更が必要な場合は新しい`definition_version`と後継関係(`supersedes_definition_version`/`superseded_by_definition_version`)を使用する

## 3. 観測値テンプレートへの追加項目
[`kpi-observation-template.yaml`](../../_company/analytics/templates/kpi-observation-template.yaml)へ以下を追加した(`schema_version`は据え置き。詳細は「9. schema_versionの取扱い」参照)。

| 項目 | 初期値 | 意味 |
|---|---|---|
| `definition_version` | `null` | 使用したKPI定義の版 |
| `definition_reference` | `null` | 定義ファイルへのリポジトリ相対参照(下記「4. definition_referenceの形式」参照) |
| `definition_commit_id` | `null` | 定義ファイルを確認できるフルGitコミットID |
| `definition_resolution_status` | `null` | 定義版解決状態(「6. 定義版解決状態」参照) |
| `definition_resolution_id` | `null` | 対応する解決記録(`kpi-definition-resolution-template.yaml`)のID |
| `definition_effective_for_period` | `false` | 定義版が観測期間に対して有効だったか |
| `definition_resolved_at` | `null` | 解決日時 |
| `definition_resolved_by` | `null` | 解決した担当者 |

実在する`definition_version`・パス・コミットID・氏名・日時は入れていない。

## 4. definition_referenceの形式
`definition_reference`は、将来、定義ファイルへのリポジトリ相対参照を保持する項目である(例: `_company/kpi/definitions/{kpi_id}__v{definition_version}.yaml`という抽象形式)。**本Batchでは実在する参照例を作成していない。** 抽象形式の説明のみとする。

## 5. 入力Batchテンプレートへの追加項目
[`kpi-entry-batch-template.yaml`](../../_company/analytics/templates/kpi-entry-batch-template.yaml)へ以下を追加した。

| 項目 | 初期値 | 意味 |
|---|---|---|
| `definition_resolution_status` | `null` | Batch全体としての定義版解決状態 |
| `all_definitions_resolved` | `false` | Batch内の全観測値の定義版が解決済みか |
| `unresolved_observation_ids` | `[]` | 定義版未解決の観測値ID一覧 |
| `definition_references` | `[]` | Batch内で使用された定義ファイル参照一覧(実在する参照は入れない) |

- 1件でも定義版未解決の観測値が含まれる場合、`all_definitions_resolved`を`true`にしない。
- 定義版未解決のBatchを`validated`または`external_reporting_approved`にしない。
- 同一Batchに複数のKPI・定義版が含まれる場合がある。
- **Batchレベルの解決済み状態だけで、各観測値の定義版参照を代替しない**(観測値ごとの`definition_version`等は別途必須)。

## 6. 定義版解決状態(definition_resolution_status)

| 状態 | 意味 |
|---|---|
| `unresolved` | 定義版が未特定 |
| `candidate_identified` | 候補が挙がったが確定していない(**確定を意味しない**) |
| `review_required` | レビュー待ち |
| `resolved` | 証跡・期間・内容の確認が完了した状態 |
| `ambiguous` | 複数の定義版候補があり確定できない状態 |
| `rejected` | 解決候補が否認された |
| `blocked` | 必要な定義ファイル・Git履歴・証跡へアクセスできない状態 |

`unresolved`・`candidate_identified`・`review_required`・`ambiguous`・`rejected`・`blocked`はいずれも観測値を`validated`にできない。**`resolved`でもKPI実績値の外部報告承認を意味しない。**

## 7. 定義版解決方法(resolution_method)

| 方法 | 説明 |
|---|---|
| `direct_reference` | 観測値に定義参照が直接記録されている |
| `source_document` | 元資料から定義版を特定 |
| `git_history` | Gitコミット履歴から定義ファイルの当時の内容を特定 |
| `period_matching` | 対象期間と定義の有効期間の一致から推定 |
| `formula_matching` | 計算式・単位等の内容一致から推定 |
| `manual_review` | 人間による総合判断 |
| `unresolved` | 解決できていない |

- `period_matching`だけで`resolved`にしない
- `formula_matching`だけで`resolved`にしない
- 複数の証拠を組み合わせる場合がある
- `direct_reference`が存在しても参照先が存在しなければ`resolved`にしない
- Gitコミットが存在しても、対象定義ファイルとの対応が確認できなければ`resolved`にしない
- 解決根拠を`evidence_ids`で追跡する
- **本Batchでは、実際のGit履歴調査や旧観測値解決を行っていない**

## 8. 旧観測値・後方互換性
定義版参照項目を持たない旧観測値の扱い:

- 旧観測値を削除しない
- 定義版不明を`0`・空文字・`latest`等で補完しない
- 期間だけを根拠に自動確定しない
- KPI名称だけを根拠に自動確定しない
- 候補となる定義版を抽出しても、人間レビュー前に`resolved`にしない
- 解決不能な旧観測値は`blocked`または`ambiguous`として扱う
- 外部報告・比較・集計へ使用する前に影響を評価する
- **本Batchでは、旧観測値を新形式へ移行する実処理・migration scriptの作成・既存実データの一括更新は行っていない**

## 9. schema_versionの取扱い
既存4テンプレートの`schema_version`について、項目追加時に必ず版を上げるという既存規則は本リポジトリ内に確認されなかった。そのため、本Batchでは`schema_version`を独断で大きく変更していない(`kpi-observation-template.yaml`・`kpi-entry-batch-template.yaml`とも`schema_version: 1`のまま)。

今回の追加は、新規オプション項目(初期値`null`・`false`・`[]`)の追加のみであり、既存項目の意味・型・必須性は変更していない**追加専用(additive)の変更**である。既存の空テンプレートをそのまま使い続ける限り、後方互換性は保たれる。将来、`schema_version`の正式な運用規則(いつ・どのように版を上げるか)を定める場合は、別途将来課題として扱う。

## 10. 定義変更と再計算・訂正
- `definition_version`が変わっても、過去観測値を自動的に新定義へ置き換えない
- `backward_compatible`かを確認する
- `reprocessing_required`を確認する
- 数値が変わる場合は訂正記録([`kpi-correction-record-template.yaml`](../../_company/analytics/templates/kpi-correction-record-template.yaml))が必要になる可能性がある
- 元観測値を無条件で上書きしない
- 再計算後の観測値は新しい`observation_id`を使用する
- 元観測値との`supersedes`関係を追跡する
- correction recordとの連携方法は、既存項目で不足する場合、将来課題として記録する(下記「13. 現在の状態」参照)
- **本Batchでは、実再計算・実訂正・実集計は行っていない**

## 11. フェイルクローズ条件
以下のいずれかに該当する場合、観測値の`validated`化・`locked`化・`current_effective_record`化・外部報告使用を停止する。

1. `definition_version`がない
2. `definition_reference`がない
3. `definition_commit_id`がない
4. 定義ファイルが存在しない
5. 指定Gitコミットで定義ファイルを確認できない
6. 定義版が観測期間に対して有効でない
7. 同一期間に複数の`active`版が存在する
8. `definition_resolution_status`が`resolved`でない
9. 解決根拠の証跡がない
10. `kpi_id`と定義ファイル内の`kpi_id`が一致しない
11. `definition_version`と定義ファイル内の版が一致しない
12. 定義の単位と観測値の単位が一致しない
13. 定義の`value_type`と観測値の`value_type`が一致しない
14. 定義の期間境界と観測値の期間解釈が一致しない
15. 必須ディメンションが欠落している
16. 許可されていないディメンションが使用されている
17. Finance根拠が必要なのに追跡できない
18. 旧観測値の定義版候補が複数存在する
19. 定義変更による再計算要否が不明
20. 訂正要否が不明
21. human reviewが未完了
22. CEO reviewが必要なのに未完了
23. 定義ファイルの意味内容が同一版のまま上書きされた可能性がある
24. 正本定義と観測値の対応を再現できない

**停止時に、`latest`・`current`・前回版・最も近い日付の版を推測で割り当てない。**

## 12. GitコミットIDの取扱い
- `definition_commit_id`は、定義ファイルを確認できるGitコミットを指す
- **短縮IDではなく、原則として完全なコミットIDを保存する**
- ブランチ名やタグ名だけで代替しない
- 観測値入力時点で未コミットの定義を本番根拠として使用しない
- 定義ファイルが指定コミットに含まれることを確認する
- **GitHubへのpush完了だけでKPI観測値の正しさが保証されるわけではない**
- 実観測値テンプレートには実コミットIDを入れない
- **本Batchでは、実コミットとの対応付けは行っていない**

## 13. Git管理区分

**Git管理対象:** トレーサビリティルール、空テンプレート、状態定義、参照項目のスキーマ、後方互換性方針、フェイルクローズ条件。

**Git管理外:** 実観測値、実定義版解決記録、実証跡、実再計算結果、実訂正記録、Finance実数値、外部サービス書き出しデータ、スクリーンショット、非公開URL、個人情報、秘密情報。

実定義版解決記録の保存候補領域は、既存の`_company/analytics/local/reconciliation/`除外規則(`.gitignore`)をそのまま使用する。**本Batchでは`.gitignore`を変更していない。**

## 14. 構成物一覧
- [`_company/analytics/templates/kpi-definition-resolution-template.yaml`](../../_company/analytics/templates/kpi-definition-resolution-template.yaml)(新規)
- [`_company/analytics/templates/kpi-observation-template.yaml`](../../_company/analytics/templates/kpi-observation-template.yaml)(項目追加)
- [`_company/analytics/templates/kpi-entry-batch-template.yaml`](../../_company/analytics/templates/kpi-entry-batch-template.yaml)(項目追加)

いずれも`sample_only: true`であり、実在する情報を含まない。

## 15. 現在の状態(正直な申告)
- 実観測値と定義版の対応付けは未実施
- 実定義版解決記録は未作成
- 旧観測値の移行は未実施
- 実GitコミットIDの記録は未実施
- 実再計算は未実施
- 実訂正は未実施
- 自動解決は未実装
- 自動照合は未実装
- 自動バリデーションは未実装
- migration scriptは未実装
- 外部サービス接続は行っていない
- **完全なエンドツーエンドのトレーサビリティは未検証**(ルール・テンプレートの整備のみ)

## 16. 変更履歴
| 版 | 日付 | 変更内容 | 変更者 |
|---|---|---|---|
| 1 | 2026-07-14 | 初版作成(環境構築Gate2、KPI定義版・観測値トレーサビリティ統合基盤Batch) | COO |
