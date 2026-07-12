# SOP: management

## 何を書く場所か
Management Division専用の手順(CEO承認フロー・COO承認フロー・週次/月次/四半期レビュー)。全社ガバナンスに関する横断カテゴリで、`management-strategy`・`management-kpi`・`management-resource`・`management-quality`・`core-coo`が参照する。

## 書いてはいけないもの
経営戦略理論そのもの(→`_shared/knowledge/business-strategy/`)、実際のKPI実績・コスト実績データ(→`_company/kpi/`、各事業の`data/`)、ブランド固有の判断基準(→`_company/brands/{ブランドID}/`)。

## 誰が参照するか
`core-coo`、`management-strategy`、`management-kpi`、`management-resource`、`management-quality`。

## 更新ルール
承認プロトコル・レビュー頻度・エスカレーション基準に変更があった場合、都度更新し`version`を上げる。このカテゴリの変更は全社に影響するため、Gate1相当の重要な意思決定として扱う。

## 関連SOP
- `brand-qa/`
- `analytics/`

## ファイル一覧
| ファイル | 内容 |
|---|---|
| `ceo-approval-flow.md` | 二段階承認プロトコル(Gate1/Gate2)の正式な社内文書化 |
| `coo-approval-flow.md` | CEO承認が不要な軽微な変更の扱い |
| `review-cycle.md` | 週次/月次/四半期レビューとLoop1→Loop2のエスカレーション基準 |
