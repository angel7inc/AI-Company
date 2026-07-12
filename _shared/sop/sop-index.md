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

`analytics/`・`conversion/`の分析結果は`research/`・`instagram/`・`seo/`の次サイクルへフィードバックされ、循環します。

## SOP一覧

| 実行順 | カテゴリ | タイトル | Status | Owner | 更新頻度(frequency) | 依存SOP | 関連Knowledge |
|---|---|---|---|---|---|---|---|
| (まだSOPがありません) | | | | | | | |

*「実行順」は上の「カテゴリ一覧・用途・参照順」の順番列と対応させてください。同一カテゴリ内で複数SOPがある場合は`1-a`, `1-b`のように枝番を振ります。*

## 将来1000本規模への備え
このテーブルが50〜100行を超えたあたりから、手作業での一覧管理は限界を迎えます。その段階では、各SOPのフロントマター(id/category/tags/status/owner/frequency等)から本テーブルを自動生成する仕組みへの切り替えを検討してください(Knowledge Layerの`knowledge-index.md`と同じ課題・同じ解決方針です)。
