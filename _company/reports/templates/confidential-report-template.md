---
sample_only: true
do_not_import: true
sensitivity: public
report_type: confidential-snapshot-template
---

# 機密レポートテンプレート(週次/月次スナップショット)

このテンプレートは`_company/reports/confidential/`配下に実際のレポートを作成する際のひな形です。**本テンプレート自体には実数値・顧客情報・認証情報を一切記載しません。** 実際のレポート作成後、ファイルは`_company/reports/confidential/`に保存し(Git管理対象外)、本テンプレートは`_company/reports/templates/`に構造のみを残します(Git管理対象)。

## 対象期間
`YYYY-MM` または `YYYY-MM-DD`〜`YYYY-MM-DD`

## 対象範囲(scope)
company / brand({brand_id}) / business_unit({business_unit}) 等、いずれか

## 売上・利益(Finance Division)
- 売上総額: (実数値。本テンプレートには記載しない)
- 営業利益: (実数値。本テンプレートには記載しない)
- 参照元: `_company/finance/actuals/{brand_id}/{YYYY-MM}.yaml`

## KPI(該当するもののみ)
- (各KPI名・値は実際のレポートでのみ記載。テンプレートには項目名の例のみ)
- 例: save_rate, line_registration_rate, automation_success_rate 等

## CRM・広告実績(該当する場合)
- (実数値は実際のレポートでのみ記載)

## 特記事項・エスカレーション
- (`sop/management/review-cycle.md`のエスカレーション基準に該当する事項があれば記載)

## 禁止事項(実際のレポート作成時の注意)
- 顧客の氏名・連絡先・相談内容等、個人を特定できる情報は記載しない
- APIキー・アクセストークン・パスワード・認証情報付きURLは記載しない
- 証拠資料そのもの(請求書画像等)は添付せず、`evidence_id`等の参照のみとする

## 作成者・作成日
- 作成エージェント:
- 作成日:
