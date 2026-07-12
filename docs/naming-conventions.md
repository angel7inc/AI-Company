# 命名規則

会社が大きくなっても迷わないように、名前の付け方を統一します。迷ったらこのページに戻ってきてください。

| 対象 | ルール | 例 |
|---|---|---|
| フォルダ名 | 小文字・ハイフン区切り(英語) | `studio-hp` |
| エージェントID | `{事業コード}-{機能}` | `ig-content-planner`、`shared-research-agent` |
| レポートファイル名 | `{YYYYMMDD}-{事業}-{内容}-{版}` | `20260711-instagram-weekly-report-v1.md` |
| Gitブランチ名 | `{種別}/{事業}-{概要}` | `feat/tiktok-launch` |
| コミットメッセージ | `{事業 or shared}: 変更内容` | `instagram: 投稿テンプレート追加` |
| KPI項目名 | 英語・snake_case | `follower_count`、`revenue_jpy` |
| Knowledge記事ファイル名 | 小文字・ハイフン区切り。frontmatterの`id`と必ず一致させる | `instagram-algorithm-basics.md` |

## Knowledge命名規則

- ファイル名とfrontmatterの`id`は必ず一致させる(例: `id: instagram-algorithm-basics` → ファイル名 `instagram-algorithm-basics.md`)
- カテゴリフォルダ名は小文字・ハイフン区切り(例: `prompt-engineering`)。既存25カテゴリの一覧は[`_shared/knowledge/README.md`](../_shared/knowledge/README.md)を参照
- 各カテゴリの説明書は必ず固定名`README.md`にする(カテゴリ名を含めた別名にしない)
- Knowledge記事のテンプレートは`_shared/knowledge/knowledge-template.md`という固定名を使う(コピーはするが、原本のファイル名・場所は変えない)
- Knowledge記事の索引は`_shared/knowledge/knowledge-index.md`という固定名を使う
- `source_type`(情報源の種類)は次の10種類に統一する: `official` / `documentation` / `research-paper` / `book` / `industry-report` / `youtube` / `reddit` / `community` / `experiment` / `internal`(正本の定義は[`_shared/knowledge/knowledge-template.md`](../_shared/knowledge/knowledge-template.md)のフロントマター)

## 事業コード一覧

| 事業 | コード |
|---|---|
| Instagram | `ig` |
| ココナラ | `coconala` |
| Studioホームページ | `studiohp` |
| LINE | `line` |
| YouTube | `yt` |
| TikTok | `tiktok` |
| Search & SEO Division | `seo` |
| 共通(事業横断) | `shared` |
| 経営・全社共通 | `core` |

## 全社共有Division(`_shared/agents/`)のエージェントID
`shared-{機能}`の形式にする(例: `shared-competitor-intelligence`、`shared-brand-quality-checker`)。事業コードは付けない。

**例外:** 独自の専用フォルダ(`_shared/agents/{division名}/`)を持つほど大きなDivision(例: Revenue Division の `_shared/agents/revenue/`)は、`shared-`ではなく`{division名}-{機能}`の形式にする(例: `revenue-funnel-strategy`、`revenue-crm-manager`)。エージェント数が増えても出自(どのDivisionか)が名前から分かるようにするため。判断基準: 専用フォルダを持たない小規模な共有機能(research/copywriting/design/analytics/conversionの既存4+1フォルダ)は`shared-`のまま、新しくDivision名の専用フォルダを切る場合は`{division名}-`に切り替える。

## Management Division(`_company/agents/`)のエージェントID
`management-{機能}`の形式にする(例: `management-strategy`、`management-kpi`)。ただし`core-coo`のように既存の`core`層エージェントは改名しない。`_company/agents/`は経営ガバナンス機能(集計・監査・優先順位付け)専用とし、コンテンツ実行機能は置かない(実行機能は`_shared/agents/`または`businesses/{channel}/agents/`)。

## `agents.yaml`の`division`フィールドについて
`layer: shared`のエージェントが増えてきた際のグルーピング用ラベル(例: `creative`、`competitor-intelligence`、`traffic`、`brand-quality`、`conversion`)。**独立台帳`_company/org/divisions.yaml`は次のいずれかの条件を満たすまで新設しない**(2026-07-12 Fable 5判断):
- `agents.yaml`の行数が50行を超えたとき
- いずれかのDivisionが独自の予算・COO以外のオーナーなど、独立した属性を持つに至ったとき

## Knowledgeカテゴリ新設のタイミング(未使用ツール)
Figma・Photoshop・Illustrator・Premiere Pro・CapCut・DaVinci Resolve等、まだどのエージェントも使用していないツールについては、Knowledgeカテゴリを先回りして作らない。**実際にいずれかのSOP/エージェント定義の「使用ツール」欄に採用された時点で**、該当のKnowledgeカテゴリを1つ追加する。
