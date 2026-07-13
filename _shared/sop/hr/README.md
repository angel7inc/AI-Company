# SOP: hr

## 何を書く場所か
AIエージェントのオンボーディング・退役手順(`hr-workforce-manager`が担当)。現時点では**オンボーディング手順**・**退役手順**のみを格納する。

## 今回実装しないもの
能力評価サイクル手順・業務量レビュー手順は、対象となる評価量・Human Workforceが未着手のため見送っている。Human Workforceのオンボーディングが始まった時点、またはAIエージェント評価量が増えた時点で追加する。

## 書いてはいけないもの
能力評価の判定基準そのもの(→各Divisionの既存基準)、金額(→`_shared/sop/finance/`)、Architecture Reviewの監査手順そのもの(→`_shared/sop/architecture-review/`。参照するが複製しない)。

## 誰が参照するか
`hr-workforce-manager`。

## 更新ルール
オンボーディング・退役手順に変更があった場合に更新する。

## 関連SOP
- `management/`(CEO承認フロー)
- `architecture-review/`(退役候補の検出元)

## ファイル一覧
| ファイル | 内容 |
|---|---|
| `agent-onboarding.md` | 新規エージェント提案からactive稼働までのオンボーディング手順 |
| `agent-retirement.md` | 退役候補の確認から`retired`確定までの退役手順 |
