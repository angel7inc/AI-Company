# Phase6設計書 ― Automation Division 新設

**ステータス:** 承認済み(2026-07-12。CEO承認のうえ、Fable 5の判断を踏まえて実装)
**策定日:** 2026-07-12
**対象:** 全Divisionが利用する自動化基盤(Workflow・接続管理・Scheduler・実行監視)

---

## 00. 本書の位置づけ
`docs/phase2〜5`(Instagram/SEO/Creative/Management/Revenue)に続く設計書。「AI同士が連携し、人が毎回指示しなくても会社が回る状態」を目指すが、**今回は構造設計のみ**であり、実際の自動化本番稼働は行わない(06章参照)。

## 01. 重複解消の決定事項
| # | 内容 | 決定 |
|---|---|---|
| 1 | Workflow Engine と `core-coo`の「優先順位管理・タスク配分」 | 高度(altitude)で分離。`core-coo`は優先順位の**決定**(戦略判断)、`automation-workflow-manager`は決定済み優先順位の**機械的実行**(キュー・依存関係グラフ)のみ。役割ファイルの「してはいけないこと」に、優先順位の決定・変更を行わない旨を明記する |
| 2 | Integration Layer と Workflow Engine(Agent連携) | 同一機能のため`automation-workflow-manager`に統合。独立エージェント化しない |
| 3 | Logging(API使用量・トークン・コスト管理)と`management-resource`の既存役割 | Loggingは実行履歴・失敗ログという技術記録のみに限定。コスト・トークンの経済指標は引き続き`management-resource`が担当し、`automation-monitor`は生データを提供するのみ(データフローの向きを役割ファイルに明記) |
| 4 | Logging と automation-monitor | 同一機能のため`automation-monitor`に統合(記録と監視・リトライ判断は表裏一体) |
| 5 | automation-director | 新設しない。Revenue Divisionでの`revenue-director`不採用と同じ理由(Division内中間管理職の前例化を回避)。`core-coo`が直接統括する |

## 02. 組織構造
Automation Divisionは、Management Divisionと同じ「非実行・全社レイヤー機能」であるため、新たな3つ目の置き場所を作らず**`_company/agents/`を共有**する(Fable 5判断)。
```
_company/agents/
├── core-coo.md, management-*.md   … Management Division(既存)
└── automation-*.md                … Automation Division(新設)
```
`_company/agents/README.md`を新設し、このフォルダが「governance(統治)とinfrastructure(基盤)の両方を含む、ブランドコンテンツを一切生み出さない全社レイヤー機能」の置き場所であることを明記する。`agents.yaml`の`division`フィールドは、`_company/agents/`配下のエージェントについて**必須項目**とする(management/automationの2つを区別するため)。

## 03. エージェント構成(4体、ご指示の8例からの再編)
| ご指示の例 | 対応 |
|---|---|
| automation-director | 新設しない |
| workflow-manager + integration-manager | `automation-workflow-manager`に統合 |
| api-manager + mcp-manager | `automation-connection-manager`に統合 |
| scheduler-manager | `automation-scheduler` |
| logging-manager + automation-monitor | `automation-monitor`に統合 |

| エージェントID | 役割概要 |
|---|---|
| `automation-workflow-manager` | Agent連携・タスクの依存関係グラフ・Queue管理。`core-coo`が決定した優先順位を機械的に実行するのみで、優先順位の決定・変更は行わない |
| `automation-connection-manager` | API(OpenAI/Claude/Gemini/Google/Meta/WordPress/LINE/YouTube/Pinterest等)・MCP(Filesystem/GitHub/Browser/Search/Memory)の接続登録台帳・死活監視。既存`mcp/servers.yaml`を拡張し、新規`mcp/api-registry.yaml`(直接API用)を追加。各APIの具体的な業務ロジックは各機能エージェントが引き続き担当 |
| `automation-scheduler` | 日次/週次/月次/イベント実行/条件実行の定義・トリガー管理。`sop/management/review-cycle.md`の週次/月次データ収集を将来起動するトリガーとして接続する想定 |
| `automation-monitor` | 実行履歴・失敗ログの記録、リトライ・エスカレーション判断。コスト・トークンの経済指標は`management-resource`が担当し、本エージェントは生データ(成否・処理時間)のみを扱う |

*全4体は`layer: core`、`division: automation`とする。全エージェントは`status: draft`のまま登録する(06章)。*

## 04. Knowledge Layer拡張
**新規カテゴリなし。** 既存`_shared/knowledge/automation/`が「自動化の設計パターン・ワークフロー理論・MCP連携の一般パターン」を既にスコープ済みのため、API接続パターン・Scheduler設計パターン・実行監視パターンもこの1カテゴリに統合する(README説明文にサブトピックを追記する軽微な更新のみ)。

## 05. SOP Layer拡張(必要最低限)
新規カテゴリ: `sop/automation/`(1カテゴリのみ)。Workflow設計・API追加・MCP追加・Scheduler追加・障害対応・ログ確認・Agent再実行の手順を格納する。**API追加・MCP追加の手順には、既存の承認ルール(API課金・有料ツール契約・自動化本番稼働はCEO承認必須)に基づく承認確認ステップを必ず含める。**

## 06. 今回のスコープ(重要、Fable 5指摘のガードレール)
今回構築するのは自動化の**構造(エージェント定義・接続台帳の型・手順書)のみ**であり、実際の自動化本番稼働は含まない。
1. 今回作成する接続台帳のエントリはすべて`status: planned`または`candidate`とし、`active`にはしない。認証情報・APIキーは一切記載しない(`.env`/`mcp/configs/`の既存ルールを踏襲)
2. Automation Divisionの4エージェントを`draft`から昇格させる、n8n等を実際に稼働させる、台帳エントリを`active`にする等は、**別途CEO承認を要する将来のイベント**(段階的自動化のStage2)とする
3. 投稿公開・予約投稿・アカウント設定変更は今後も自動化しない(既存の`docs/phase2-instagram-design.md`の原則を維持)

## 07. ワークフロー(既存3ループの自動化基盤としての位置づけ)
新しいLoopを追加するものではなく、既存のLoop1(個別コンテンツ制作)・Loop2(経営ガバナンス)・Loop3(収益ループ)を**将来自動実行するための基盤**として位置づける。
```
research → instagram/seo → copywriting → design/video → brand-qa(Loop1、将来の自動化対象)
→ publishing ←【CEO承認必須。自動化しない】
→ analytics → conversion/revenue-analytics(Loop1/Loop3、将来の自動化対象)
→ management-kpi等の週次/月次集計(Loop2、automation-schedulerが起動タイミングを管理)
```

## 08. KPIの置き場所
自動化率・Agent成功率・API成功率・平均処理時間・失敗率・リトライ率は、特定ブランドに紐づかない全社インフラの健全性指標のため、`brand-brief.md`には追加せず`_company/kpi/`(`management-kpi`が管理)側で扱う。`automation-monitor`が生データの計算元となる。

## 09. 外部サービス接続(拡張前提の構造)
既存`mcp/servers.yaml`の`status: planned/candidate`という型を踏襲し、Claude/ChatGPT/Gemini/Search Console/Trends/Google Analytics/Meta/WordPress/LINE/Pinterest/YouTube/GitHub/n8nを、新規`mcp/api-registry.yaml`(MCP以外の直接API)・既存`mcp/servers.yaml`(MCP)に`status: candidate`で登録する。実際の接続・認証は行わない。

## 10. 将来Phaseへの接続
- Phase7〜10(Intelligence/Product/Finance/HR)は`_company/agents/`(非実行・全社機能)か`_shared/agents/{division}/`(実行機能)の既存パターンに当てはめるだけで良い
- Phase11 Architecture Review Divisionは、`management-quality`の「メタ監査」パターンをコンテンツQAからアーキテクチャ全体(Knowledge/SOP/Agent/命名/重複)へ拡張したものと位置づけられる
- Phase12 CEO Dashboardは、`_company/kpi/`・`_company/reports/`(既存)を可視化するUI層であり、新しいデータ収集の仕組みは不要

## 11. 変更履歴
| 版 | 日付 | 変更内容 | 変更者 |
|---|---|---|---|
| 1 | 2026-07-12 | 初版作成。CEO承認済み(Fable 5判断を経て実装) | COO |
