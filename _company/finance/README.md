# Finance Layer(財務層)

全ブランド共通の財務実績(売上・費用・利益・入出金)のSSOTを置く場所です。`finance-controller`(Phase8b Finance Division)が実務管理します。

## この層が持つもの / 持たないもの
- **持つもの:** 実績数値の正本(`actuals/`)、スキーマ・設定(`_company/org/finance.yaml`)
- **持たないもの:** 顧客の氏名・相談内容・個人を特定できる情報、APIキー・アクセストークン・認証情報付きURL、証拠資料そのもの(請求書・明細等はGit管理外の保管場所に置き、`evidence_id`のみ記録する)

## ディレクトリ構造
```
_company/finance/
├── README.md              # 本ファイル
├── schema-sample.yaml      # 全金額0の記入例(sample_only: true / do_not_import: true)
└── actuals/                # 実績数値の正本(Git管理対象外。.gitignoreで除外)
    ├── tomo-angel7/
    │   └── {YYYY-MM}.yaml
    ├── tomo-academy/        # 空(planned。実績入力対象外)
    └── phone-fortune-company/ # 空(planned。実績入力対象外)
```

## Git管理について(重要)
`_company/finance/actuals/**/*.yaml`は`.gitignore`で除外されており、実際の売上・利益・費用・入出金はGitHub(Private Repositoryであっても)にPushされません。ディレクトリ構造の維持には`.gitkeep`を使用します(`.gitkeep`は`.yaml`ではないため除外対象になりません)。記入例が必要な場合は`schema-sample.yaml`(本ファイルと同階層、Git管理対象)を参照してください。

## レコードID
`fin-{yyyymm}-{brand_id}-{allocation_scope}-{scope_id}-{channel}`

| allocation_scope | scope_id |
|---|---|
| company | `ai-company` |
| brand | `{brand_id}` |
| service-line | `{service_line_id}` |
| product | `{product_id}`(`product_id`欄にも同値を記載) |

`channel`が存在しない共通経費等は`channel: none`と明示する。

## 用語
- **service_line_id**: サービスの運営単位(例: `personal-reading`)。`brands.yaml`の`business_units`(instagram/seo等、コンテンツ・集客チャネル)とは別軸の概念。
- **channel**: 実際に売上が発生した経路(例: `coconala`/`line`/`lp`)。

## 導出値について
`net_revenue_jpy`・`operating_profit_jpy`は`actuals/`には保存しません(原始入力値のみ保存)。以下の計算式で、`revenue-analytics`・`management-kpi`・月次レポート側が都度算出します。
```
net_revenue_jpy = gross_revenue_jpy - platform_fee_jpy - refund_jpy
operating_profit_jpy = net_revenue_jpy - ad_spend_jpy - ai_api_cost_jpy - outsourcing_fixed_cost_jpy - other_expenses_jpy
```
`cash_received_jpy`・`cash_paid_out_jpy`(現金収支)はこの計算式に使用しません(損益と現金収支は別軸)。将来、自動検証プログラムを導入した場合のみ、算出値を保存する`computed`セクションの追加を再検討します。

## 他Layer・Divisionとの参照関係
- **Revenue Division(`revenue-analytics`)**: `actuals/`の売上・費用データに加え、Analytics・CRM・広告データを組み合わせてCVR/CPA/ROAS/LTVを算出する(Finance実績だけでは算出できない設計)
- **Management Division(`management-kpi`/`management-resource`)**: `management-resource`はAI/APIコストの生データを提供する側。`management-kpi`は会社全体の集計値を`_company/kpi/`へ生成する(表示用であり正本ではない)
- **Product Division**: `product_id`で`actuals/`を参照するのみ。`products.yaml`には実績数値を複製しない(参照情報のみ保持)
- **Brand Layer**: 一切参照・変更しない(`brand_id`はFinance側が持つ一方向の外部キー)

## サンプルファイルの除外ルール
`sample_only: true`または`do_not_import: true`が設定されたファイル(`schema-sample.yaml`等)は、実績集計・レポート生成の対象から必ず除外する。

## 機密情報の扱い
`actuals/`配下の全ファイルは`sensitivity: confidential`とする。顧客の氏名・相談内容・個人を特定できる情報・APIキー・アクセストークン・認証情報付きURLは、いかなる項目にも記載しない。証拠資料(請求書・明細等)は、`docs/phase2-instagram-design.md` 10章で定めた「人が確認する資料はGoogle Drive」の既存ルールに従い保存し、`actuals/`には`evidence_id`(参照ID)のみを記録する。

## 将来の拡張
- 予算配分手順は、月次実績が1〜2か月分蓄積した段階で`sop/finance/budget-allocation.md`として追加する
- 取引量が増え会計処理が複雑化した場合、`finance-controller`から`finance-budget`等への分割を検討する(トリガーは`_company/agents/finance-controller.md`参照)
- 決済データの自動連携は、Automation Divisionの接続(`mcp/api-registry.yaml`)が有効化された時点で検討する
