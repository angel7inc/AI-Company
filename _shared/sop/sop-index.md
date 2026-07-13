# SOP Index

すべてのSOPを一覧管理します。新しいSOPを追加したら、下の表に1行追加してください。

## カテゴリ一覧・用途・参照順

投稿制作は一般的に次の順序で、複数カテゴリのSOPをまたいで進みます。

| 順番 | カテゴリ | 用途 | 主な依存関係(前段階) |
|---|---|---|---|
| 1 | `research/` | 悩み・競合・トレンド調査(Competitor Intelligenceの日次監視を含む) | (なし。起点) |
| 2 | `instagram/` | 投稿企画・全体設計 | `research/` |
| 2 | `seo/` | SEO記事設計・キーワード戦略・技術SEO・内部リンク | `research/` |
| 3 | `copywriting/` | 本文・キャプション制作 | `instagram/`, `seo/` |
| 4 | `design/` | 画像・カルーセル素材制作 | `copywriting/` |
| 4 | `video/` | 動画制作(Reels/Shorts/YouTube/広告/LINE/LP) | `copywriting/` |
| 5 | `brand-qa/` | 全チャネル共通ブランドQA | `design/`, `video/`, `copywriting/` |
| 6 | `publishing/` | 最終確認・予約投稿・公開(CEO承認必須) | `brand-qa/` |
| 7 | `analytics/` | 数値取得・分析・改善提案(Traffic Divisionの横断分析を含む) | `publishing/` |
| 8 | `conversion/` | LP・導線・CTA・CVR改善・ABテスト | `analytics/` |
| 9 | `crm/` | LINE配信・CRM運用・LTV改善(Loop3、収益ループ) | `conversion/` |
| - | `customer/` | 顧客対応(上記フローとは独立して随時発生) | `instagram/`, `copywriting/` |
| - | `management/` | CEO/COO承認フロー・週次/月次/四半期レビュー(Loop2、投稿ごとのフローとは独立) | (なし。全Divisionからの集計を受ける) |
| - | `automation/` | Workflow設計・API/MCP追加・Scheduler追加・障害対応・ログ確認・Agent再実行(構造設計のみ。本番稼働は別途承認) | `management/` |
| - | `product/` | サービス/商品定義・カリキュラム設計・品質基準定義(定義のみ。実行は`crm/`・`automation/`) | `crm/` |
| - | `finance/` | 月次決算・売上コスト突合(予算配分手順は台帳新設まで見送り) | `management/` |
| - | `architecture-review/` | Knowledge/SOP/Agent/命名/重複の全社監査(検出・報告のみ。是正実行は含めない) | `management/` |
| - | `hr/` | AIエージェントのライフサイクル管理・オンボーディング・退役手順 | `management/`, `architecture-review/` |

`analytics/`・`conversion/`の分析結果は`research/`・`instagram/`・`seo/`の次サイクルへフィードバックされ、循環します。

## SOP一覧

**本表は2026-07-13時点で実在を確認したSOPの一覧です。件数(16件)は現在確認時点のものであり、長期的な固定値・正本ではありません。** 新しいSOPを追加・退役・名称変更した場合は、**その変更と同じ変更単位(同じコミット/同じ承認)で本表も必ず更新してください**(表の更新漏れ自体が監査対象になります)。

| 実行順 | カテゴリ | id | タイトル | Status | Owner | 更新頻度(frequency) | 依存SOP | 関連Knowledge | 定義ファイルパス |
|---|---|---|---|---|---|---|---|---|---|
| 1 | research | `consumer-analysis` | 消費者分析手順 | draft | `intelligence-consumer` | weekly | `market-analysis`, `trend-analysis`, `insight-synthesis` | psychology, research-methodology | `_shared/sop/research/consumer-analysis.md` |
| 1 | research | `insight-synthesis` | Insight統合・配信手順 | draft | `intelligence-insight-synthesizer` | weekly | `market-analysis`, `consumer-analysis`, `trend-analysis` | (なし) | `_shared/sop/research/insight-synthesis.md` |
| 1 | research | `market-analysis` | 市場分析手順 | draft | `intelligence-market` | monthly | `consumer-analysis`, `trend-analysis`, `insight-synthesis` | business-strategy, research-methodology | `_shared/sop/research/market-analysis.md` |
| 1 | research | `research-quality-check` | リサーチ品質チェック手順 | draft | `intelligence-research-quality-checker` | as-needed | `market-analysis`, `consumer-analysis`, `trend-analysis` | research-methodology | `_shared/sop/research/research-quality-check.md` |
| 1 | research | `trend-analysis` | トレンド分析手順(コンテンツ企画向け) | draft | `intelligence-trend` | weekly | `market-analysis`, `consumer-analysis`, `insight-synthesis` | marketing, analytics | `_shared/sop/research/trend-analysis.md` |
| - | management | `ceo-approval-flow` | CEO承認フロー(二段階承認プロトコル) | active | `core-coo` | as-needed | `coo-approval-flow` | business-strategy | `_shared/sop/management/ceo-approval-flow.md` |
| - | management | `coo-approval-flow` | COO承認フロー(軽微な変更の扱い) | active | `core-coo` | as-needed | `ceo-approval-flow` | (なし) | `_shared/sop/management/coo-approval-flow.md` |
| - | management | `review-cycle` | レビューサイクル(週次/月次/四半期)とエスカレーション基準 | active | `core-coo` | weekly | `ceo-approval-flow` | business-strategy, analytics | `_shared/sop/management/review-cycle.md` |
| - | finance | `cost-revenue-reconciliation` | 売上・コスト突合手順 | draft | `finance-controller` | monthly | `monthly-closing` | finance, analytics | `_shared/sop/finance/cost-revenue-reconciliation.md` |
| - | finance | `monthly-closing` | 月次決算手順 | draft | `finance-controller` | monthly | `cost-revenue-reconciliation` | finance, analytics | `_shared/sop/finance/monthly-closing.md` |
| - | architecture-review | `agent-registry-audit` | Agent台帳監査手順 | draft | `architecture-review-controller` | quarterly | `knowledge-audit`, `sop-audit`, `cross-duplication-audit` | (なし) | `_shared/sop/architecture-review/agent-registry-audit.md` |
| - | architecture-review | `cross-duplication-audit` | 横断的重複検出手順 | draft | `architecture-review-controller` | quarterly | `knowledge-audit`, `sop-audit`, `agent-registry-audit` | (なし) | `_shared/sop/architecture-review/cross-duplication-audit.md` |
| - | architecture-review | `knowledge-audit` | Knowledge Layer監査手順 | draft | `architecture-review-controller` | quarterly | `sop-audit`, `cross-duplication-audit` | (なし) | `_shared/sop/architecture-review/knowledge-audit.md` |
| - | architecture-review | `sop-audit` | SOP Layer監査手順 | draft | `architecture-review-controller` | quarterly | `knowledge-audit`, `cross-duplication-audit` | (なし) | `_shared/sop/architecture-review/sop-audit.md` |
| - | hr | `agent-onboarding` | エージェントオンボーディング手順 | draft | `hr-workforce-manager` | as-needed | `agent-retirement` | (なし) | `_shared/sop/hr/agent-onboarding.md` |
| - | hr | `agent-retirement` | エージェント退役手順 | draft | `hr-workforce-manager` | as-needed | `agent-onboarding` | (なし) | `_shared/sop/hr/agent-retirement.md` |

*「実行順」は上の「カテゴリ一覧・用途・参照順」の順番列と対応させてください。同一カテゴリ内で複数SOPがある場合は`1-a`, `1-b`のように枝番を振ります。「依存SOP」は各SOPのfrontmatter`related_sop_ids`(特定ファイルへの参照)のみを転記し、`related_sop_categories`(カテゴリ全体への参照。例: `management`)は上の「カテゴリ一覧・用途・参照順」表で代表させ、本表には転記しません。`sop-template.md`はテンプレートであり実手順ではないため本表に含めません。*

## 将来1000本規模への備え
このテーブルが50〜100行を超えたあたりから、手作業での一覧管理は限界を迎えます。その段階では、各SOPのフロントマター(id/category/tags/status/owner/frequency等)から本テーブルを自動生成する仕組みへの切り替えを検討してください(Knowledge Layerの`knowledge-index.md`と同じ課題・同じ解決方針です)。
