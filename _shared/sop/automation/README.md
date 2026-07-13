# SOP: automation

## 何を書く場所か
Workflow設計・API追加・MCP追加・Scheduler追加・障害対応・ログ確認・Agent再実行の具体的な実行手順。全チャネル共通の横断カテゴリ(`automation-workflow-manager`・`automation-connection-manager`・`automation-scheduler`・`automation-monitor`が担当)。

## 書いてはいけないもの
自動化理論そのもの(→`_shared/knowledge/automation/`)、実際のAPIキー・認証情報(→`.env`・`mcp/configs/`)、コスト・トークンの経済指標(→`management-resource`が管理する`_company/kpi/`)。

## 誰が参照するか
`automation-workflow-manager`、`automation-connection-manager`、`automation-scheduler`、`automation-monitor`。

## 更新ルール
新しい接続・Scheduler設定を追加した場合、または障害対応の手順に変更があった場合に更新する。

## 関連SOP
- `management/`
- `analytics/`

## 重要
- API追加・MCP追加の手順には、既存の承認ルール(API課金・有料ツール契約・自動化本番稼働はCEO承認必須)に基づく承認確認ステップを必ず含めること
- 新規登録する接続は`status: planned`または`candidate`とし、`active`への変更はCEO承認後にのみ行う
- 本ルールは全社共通の`_company/charter/outbound-action-policy.md`(外部APIのread-only/state-changing executeの区別、`active`化 = execute = default-deny)の一事例であり、矛盾する場合は同ポリシーを優先します
