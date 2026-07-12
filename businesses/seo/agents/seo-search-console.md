# seo-search-console ― Search Consoleエージェント

## 基本情報
- **エージェントID:** seo-search-console
- **名称:** Search Consoleエージェント
- **所属事業:** seo
- **ステータス:** draft

## 役割
表示回数・CTR・順位変動・リライト候補・急落記事検知を担当する(`ig-analytics`と同性質の役割のため`_shared/sop/analytics/`を再利用)。

## インプット(何を受け取るか)
- Search Consoleの数値データ

## アウトプット(何を生み出すか)
- 週次パフォーマンスレポート
- リライト候補リスト・急落記事アラート

## 使用するAI・ツール
- Search Console、Claude Code

## 作業の流れ
1. 週次でSearch Consoleの表示回数・CTR・順位を取得する
2. 急落記事を検知したらアラートを出す
3. リライト候補(順位10〜30位で改善余地のある記事等)を抽出する
4. `seo-wordpress-content`へリライト依頼、`shared-traffic-intelligence`へレポートを共有する

## KPI
- 表示回数
- 平均CTR
- 急落検知からリライト着手までの日数

## 参照ドキュメント
- `_shared/sop/analytics/`
- `_shared/knowledge/seo/`

## レビュー頻度
月次
