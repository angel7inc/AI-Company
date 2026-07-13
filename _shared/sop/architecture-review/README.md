# SOP: architecture-review

## 何を書く場所か
Knowledge/SOP/Agent/命名規則/重複の全社監査手順(`architecture-review-controller`が担当)。**検出・報告の手順のみ**を扱う。

## 書いてはいけないもの
**是正の実行手順は書かない。** 発見した問題(重複記事の統合、命名違反の修正、ブランド固定化バグの修正等)の実行は、各Divisionの担当エージェント・COOが通常の二段階承認プロセス(`sop/management/ceo-approval-flow.md`)に従って行う。本カテゴリはあくまで「何が問題か」を検出・報告するところまでを担当する。

## 誰が参照するか
`architecture-review-controller`。

## 更新ルール
新しい監査観点・命名規則の変更があった場合に更新する。

## 関連SOP
- `management/`(月次/四半期レビューへの接続先)

## 重要
- 監査手法は「統計的サンプリング」ではなく「全件走査(フルスイープ)」とする(`management-quality`の較正サンプリングとは手法が異なる)
- Division一覧は、`_company/org/divisions.yaml`が新設されるまで`_company/org/agents.yaml`の`division`フィールドから導出する

## ファイル一覧
| ファイル | 内容 |
|---|---|
| `knowledge-audit.md` | Knowledgeカテゴリ間の重複・sensitivity区分・関連リンクの有効性・命名規則準拠の監査手順 |
| `sop-audit.md` | 「定義」と「実行」の境界遵守・CEO承認ステップの欠落有無・命名規則準拠の監査手順 |
| `agent-registry-audit.md` | `division`/`brand`フィールド整合性・`agents.yaml`と定義ファイルの対応・命名規則準拠・50行閾値到達チェックの監査手順 |
| `cross-duplication-audit.md` | Agent/Knowledge/SOPを横断した機能的重複の検出手順 |
