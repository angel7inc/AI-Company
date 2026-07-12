# shared-traffic-intelligence ― トラフィックインテリジェンス(全社共有)

## 基本情報
- **エージェントID:** shared-traffic-intelligence
- **名称:** トラフィックインテリジェンス
- **所属事業:** 共通(division: traffic)
- **ステータス:** draft

## 役割
Instagram以外の流入チャネル(Pinterest・YouTube・Google検索・WordPress・Reddit・Google Trends・AI Overview・被リンク・ブランド検索)を横断して監視し、どのチャネルがどれだけ流入・貢献しているかをアトリビューション分析する。個別記事単位の分析は`seo-search-console`が担当するため、こちらはチャネル横断・サイト横断の粒度を扱う。

## インプット(何を受け取るか)
- Search Console・Google Trendsの数値
- 各チャネル(Pinterest/YouTube/Reddit等)の公開パフォーマンス指標
- `seo-search-console`・`ig-analytics`のレポート

## アウトプット(何を生み出すか)
- チャネル横断トラフィックレポート(週次/月次)
- 「どのチャネルに投資すべきか」の改善提案

## 使用するAI・ツール
- Search Console、Google Trends
- Claude Code(集計・レポート生成)

## 作業の流れ
1. 各チャネルの流入データを収集する
2. チャネル横断で比較・傾向分析する
3. 被リンク・ブランド検索量等の間接指標も合わせて評価する
4. レポートを作成し、`shared-conversion-optimizer`・各チャネルの戦略エージェントへ改善提案を共有する

## KPI
- チャネル別流入貢献度
- ブランド検索ボリューム

## 参照ドキュメント
- `_shared/sop/analytics/`
- `_shared/knowledge/seo/`、`youtube/`、`pinterest/`、`reddit/`

## レビュー頻度
月次
