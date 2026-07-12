# 共通機能エージェント

事業をまたいで使い回すエージェントの定義を置く場所です。

| フォルダ | 担当領域 |
|---|---|
| `research/` | 市場・トレンドのリサーチ、競合の日次監視(Competitor Intelligence) |
| `copywriting/` | 文章・コピーの作成 |
| `design/` | ビジュアル制作(画像・動画のプロンプト作成・品質チェック・素材管理。Creative Division) |
| `analytics/` | 分析・品質チェック(QA)。トラフィック横断分析(Traffic Division)・全社共通ブランドQA(Brand Quality Division)を含む |
| `conversion/` | LP・導線・CTA・CVR改善・ABテスト(Conversion Division) |
| `revenue/` | ファネル戦略・CRM運用・LTV戦略・収益分析・価格/オファー設計(Revenue Division) |
| `intelligence/` | 市場/消費者/トレンド分析・インサイト統合・リサーチ品質チェック(Intelligence Division) |
| `product/` | サービス/商品構造設計・品質基準定義(Product Division) |

Phase 2以降、実際のエージェント定義ファイル(.md)をここに追加していきます。エージェントが多い共有Divisionは、`_company/org/agents.yaml`の`division`フィールドでグループ管理します。
