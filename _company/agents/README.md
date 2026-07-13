# 経営・全社レイヤー エージェント

`businesses/{事業}/agents/`(チャネル専属の実行機能)、`_shared/agents/`(チャネル横断の実行機能)のどちらにも属さない、**ブランドコンテンツを一切生み出さない全社レイヤー機能**を置く場所です。

## 何を置く場所か / 置かない場所か
- **ここに置く:** 統治(governance)機能 ― 戦略・KPI集計・リソース監視・品質メタ監査(Management Division)、基盤(infrastructure)機能 ― Workflow実行・接続管理・Scheduler・実行監視(Automation Division)、財務(Finance Division)、アーキテクチャ監査(Architecture Review Division)
- **ここに置かない:** 実際にコンテンツ・施策を実行する機能(画像生成、記事執筆、CRM配信等)。これらは`_shared/agents/{機能}/`または`businesses/{事業}/agents/`に置く

## Division一覧
| フォルダ内のエージェント | Division | 内容 |
|---|---|---|
| `core-coo.md`、`management-*.md` | Management | 各Division管理・戦略・KPI集計・リソース監視・品質メタ監査 |
| `automation-*.md` | Automation | Workflow実行・API/MCP接続管理・Scheduler・実行監視 |
| `finance-controller.md` | Finance | 利益算出(売上−AI/APIコストの合算)・会社全体ROI・会計・予算配分 |
| `architecture-review-controller.md` | Architecture Review | Knowledge/SOP/Agent/命名規則/重複の全件監査(検出・報告のみ) |

## 命名・管理ルール
- エージェントIDは`{division名}-{機能}`とする(`core-coo`は既存のため例外)
- `_company/org/agents.yaml`の`division`フィールドは、このフォルダに属するエージェントについて**必須**とする
- 新しいDivisionがこのフォルダに追加される場合、上記表に1行追加するだけでよい
- `agents.yaml`が50行を超えた場合、または各Divisionが独自予算/オーナーを持つに至った場合、`_company/org/divisions.yaml`の新設を検討する(`architecture-review-controller`が行数を定期監視する)
