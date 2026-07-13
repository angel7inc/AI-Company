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

## `copywriting/`が空である理由(2026-07-13記録)
`copywriting/`は現時点で共有エージェントが一度も配置されておらず、`.gitkeep`のみの状態です。これは欠落や設定漏れではなく、以下の判断によるものです。
- コピーライティングは現時点ではInstagram(`ig-content-planner`等)・SEO(`seo-wordpress-content`等)・Revenue(`revenue-crm-manager`等)のようなチャネル固有エージェントが、各チャネルのトーン・フォーマットに合わせて担当する
- `design/`・`analytics/`のような共有方法論エージェントを今すぐ新設すると、チャネル固有エージェントとの役割重複が生じるリスクがあるため、共有copywritingエージェントは現時点では未配置とする
- 複数Divisionで同一のコピー業務(例: 複数チャネルで共通するコピーの型・禁止表現チェック等)が重複していると判明した場合は、その時点で別途Gate1提案を行い、共有エージェント新設を検討する
- `_shared/agents/copywriting/`というフォルダ自体は将来の拡張用に維持し、`design/`・`analytics/`と同様にいつでも配置できる状態にしておく(フォルダの存在は現在の欠落・故障を意味しない)
