# AI Company

AIを活用したコンテンツ事業会社の運営基盤です。CEOの指示のもと、COO(Claude Code)が全体管理・自動化・実装を担当し、事業ごとのAIエージェントがコンテンツ制作から分析までを担います。

## この会社の考え方

- 事業が増えても、フォルダ構造とルールは変わらない(テンプレート駆動)
- すべてのAIエージェントは [`_company/org/agents.yaml`](_company/org/agents.yaml) の台帳で一元管理する
- 詳しい運営ルールは [`_company/charter/charter.md`](_company/charter/charter.md) を参照する

## フォルダの見取り図

| フォルダ | 役割 |
|---|---|
| `_company/` | 経営レイヤー。憲章・エージェント台帳・KPI・レポート |
| `_shared/` | 事業をまたいで使う共通エージェント・テンプレート・自動化 |
| `businesses/` | 事業ユニット(Instagram / ココナラ / Studioホームページ / LINE / YouTube / TikTok) |
| `mcp/` | Claude Codeが外部ツールと連携するための設定 |
| `docs/` | 命名規則・ロードマップなどの参照ドキュメント |

## 新しい事業を追加するには

`businesses/_template/` をそのままコピーして、事業名のフォルダにリネームするだけです。詳細は [`businesses/_template/README.md`](businesses/_template/README.md) を参照してください。

## 現在のフェーズ

Phase 1(基盤構築)が完了した段階です。各事業のエージェントはまだ稼働していません。次のフェーズはCEOの確認後に着手します。
