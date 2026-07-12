# SOP: finance

## 何を書く場所か
会社全体の財務手順(`finance-controller`が担当)。現時点では**月次決算手順**・**売上コスト突合手順**のみを格納する。

## 今回実装しないもの
予算配分手順は、書き込み先の台帳(`_company/org/finance.yaml`)が未作成のため見送っている。同台帳の新設(初めての非ゼロ予算配分または広告費発生時)と同時に追加する。

## 書いてはいけないもの
会計・予算理論そのもの(→`_shared/knowledge/finance/`)、実際の財務数値(→`_company/kpi/`)、支出のCEO承認手順そのもの(→`_shared/sop/management/ceo-approval-flow.md`。参照するが複製しない)。

## 誰が参照するか
`finance-controller`。

## 更新ルール
決算手順・突合手順に変更があった場合に更新する。

## 関連SOP
- `management/`(CEO承認フロー・レビューサイクル)
- `analytics/`

## ファイル一覧
| ファイル | 内容 |
|---|---|
| `monthly-closing.md` | 月次決算手順 |
| `cost-revenue-reconciliation.md` | 売上・コスト突合手順 |
