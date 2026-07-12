# Knowledge: finance

## 1. 何を保存する場所か
会計原則・予算配分フレームワーク・P/L(損益計算)構造の一般理論。CAC/LTV/ROAS等の指標定義そのものは既存`analytics/`が正本のため、ここでは再定義しない。

## 2. 保存してはいけない情報
特定ブランドの実際の売上・コスト・予算配分の実績値(→`_company/kpi/`、将来`_company/org/finance.yaml`)。

## 3. 誰が参照するか
`finance-controller`。

## 4. 更新ルール
有効な会計・予算配分のフレームワークを追加・更新する。

## 5. 推奨する情報源(Sources)
`book`, `documentation` ― 具体例: 会計・財務管理の専門書、スタートアップの予算配分フレームワーク

## 6. 関連Knowledge
- `analytics/`
- `business-strategy/`

## 7. 機密情報の扱い
一般理論は`public`/`internal`。自社の実際の財務数値は含めない。

## 8. 重複防止ルール
同じフレームワークの重複記事を作らず、既存記事へ追記する。
