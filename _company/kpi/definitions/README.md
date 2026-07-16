# KPI Definitions(承認済みKPI定義の保存候補領域)

## 位置づけ
このフォルダは、将来、KPI観測値の新規作成に使用可能な正本条件(`definition_status: active`かつhuman review・CEO review完了・有効期間内・同一kpi_id/同一期間のactive版重複なし。詳細は[Runbook「1. KPI定義の正本」](../../../docs/runbooks/kpi-definition-governance.md))を満たすKPI定義ファイルを保存する候補領域である。**本Batch(2026-07-14)時点で、実際のKPI定義ファイルはここに作成していない。** ルール・スキーマ・空テンプレートの整備のみを行った。

用語・状態・版管理・Financeとの境界の詳細は[`docs/runbooks/kpi-definition-governance.md`](../../../docs/runbooks/kpi-definition-governance.md)を正本として参照する。空テンプレートは[`_company/kpi/templates/kpi-definition-template.yaml`](../templates/kpi-definition-template.yaml)、構造定義は[`_company/kpi/schemas/kpi-definition-schema.yaml`](../schemas/kpi-definition-schema.yaml)を参照する。

## 1ファイル1定義版の保存ルール
- **1ファイルには1つの`kpi_id`の1つの`definition_version`だけを保存する**(`kpi-definition-template.yaml`をコピーして使用する)
- 新しい`definition_version`を作成する際、旧版ファイルを上書きしない
- 旧版ファイルを削除しない
- Git履歴だけに旧版の保持を依存しない(`deprecated`・`superseded`版も本フォルダ配下で直接ファイルとして確認可能な状態を維持する)
- 同じ内容を複数ファイルへ無断複製しない
- 同一`kpi_id`・同一`definition_version`の重複ファイルを禁止する

## ファイル名規則(将来の実定義作成時に適用)
`{kpi_id}__v{definition_version}.yaml`

- `kpi_id`は既存規則([`docs/naming-conventions.md`](../../../docs/naming-conventions.md)「KPI項目名」)どおり英語・snake_case
- 区切りは二重アンダースコア(`__`)
- `definition_version`の前に`v`を付ける
- 空白を含めない
- スラッシュ・バックスラッシュ・コロン等、パス解釈へ影響する文字を含めない
- 個人情報・実績値・目標値・金額・非公開情報を含めない
- 大文字・小文字の扱いを統一する(小文字を基本とする)
- 同一ファイル名を別の定義へ再利用しない
- `definition_version`にファイル名として不適切な文字が含まれる場合は、除去またはハイフンへの置換により正規化し、正規化前後の対応を`change_summary`等に記録する
- 本README作成時点で、実在する`kpi_id`・`definition_version`を用いたファイル例は作成していない(抽象形式の説明のみ)

## definition_versionの管理
- 1つの`kpi_id`につき、複数バージョンの定義を保持し得る
- **旧版を削除しない。** `deprecated`または`superseded`として保持する(過去の観測値がどの版に基づくか、後から追跡できるようにするため)
- 同一`kpi_id`について、同一期間に複数の`active`版が重複しないようにする

## draftとactiveの分離
- `draft`・`review_required`・`changes_requested`の定義は、本番観測値の入力根拠として使用しない
- **KPI観測値の新規作成に使用可能な正本定義は、`definition_status: active`かつhuman review・CEO review完了・有効期間内・同一kpi_id/同一期間のactive版重複なしをすべて満たすものとする**(詳細はRunbook「1. KPI定義の正本」参照)
- `approved`は必要なレビュー・承認を通過した状態を意味するが、それだけでは新規観測値に使用できるとは限らない。有効期間到来等の利用開始条件を満たして初めて`active`となる

## 保存してはいけないもの
- 実績値(actual)
- 目標値(target)を保存する場合も、実績値と明確に分離する(本フォルダは定義のみを扱い、目標値の実数値そのものの正本ではない。目標値の扱いは`brand-brief.md`等の既存管理場所と重複させない)
- 個人情報(顧客氏名・メールアドレス・電話番号・LINEユーザーID等)
- 秘密情報(APIキー・トークン・非公開URL等)

## KPI定義変更時のレビュー手順
1. 変更内容を`change_summary`・`change_reason`に記録する
2. 計算式・単位・期間境界・対象範囲・ディメンションが変わる場合、`definition_version`を更新するか検討する
3. 過去データへの遡及適用要否を確認し、必要な場合`reprocessing_required: true`とする
4. `human_review_status`→`ceo_review`の順で確認する
5. 承認後、`approved_at`・`approved_by`を記録し、有効期間(`effective_from`)を明示する
6. 旧版は削除せず`superseded_by_definition_version`で新版を参照させる

## 実際のKPI定義ファイルを将来作成する際の最低条件
以下がすべて確定・完了して初めて、実定義ファイルの作成・レビューを開始する。**本Batchでは実定義ファイルを作成していない。**

- `kpi_id`が既存規則([`docs/naming-conventions.md`](../../../docs/naming-conventions.md))に適合している
- `definition_version`が一意である
- 単位が確定している
- 計算式または集計方法が確定している
- 対象期間・期間境界が確定している
- `business_timezone`が必要なKPIでは、タイムゾーンが確定している
- ソースシステムが確定している
- Financeとの境界が確定している(金額明細を重複入力しないことを含む)
- `required_dimensions`と`optional_dimensions`が確定している
- human reviewが完了している
- CEO reviewが完了している
- 同一期間の`active`版重複がない

## 観測値との参照方法(未決定事項)
[`docs/runbooks/kpi-data-entry-and-validation.md`](../../../docs/runbooks/kpi-data-entry-and-validation.md)の観測値テンプレート(`kpi-observation-template.yaml`)には、現時点で`definition_version`を直接参照する項目が存在しない。観測値がどの定義版に基づくかの追跡方法は、**本Batchの範囲外の未決定事項・将来課題**とする(詳細はRunbook「10. KPI観測値との関係」参照)。既存のAnalyticsテンプレートは本Batchで変更していない。**この参照方式が未実装である以上、KPI Definition Registryの整備をもって観測値とのトレーサビリティが完成したとは評価しない。**
