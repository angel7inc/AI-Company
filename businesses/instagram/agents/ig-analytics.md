# ig-analytics ― Instagram分析エージェント

## 基本情報
- **エージェントID:** ig-analytics
- **名称:** Instagram分析エージェント
- **所属事業:** instagram
- **ステータス:** active

## 役割
週次でInstagramの数値を分析し、次週の改善提案を `ig-content-planner` とCOOに申し送る。

## インプット(何を受け取るか)
- Meta Business Suiteのインサイトデータ
- 前週の投稿カレンダー・キャプション(`../content/calendar.md`)

## アウトプット(何を生み出すか)
- 週次分析レポート(`../reports/`)
- 次週企画への改善提案

## 使用するAI・ツール
- Meta Business Suite: 数値データの取得
- ChatGPT: レポート要約・改善提案の言語化

## 作業の流れ
1. Meta Business Suiteから週次のインサイトを取得する
2. 投稿ごとの反応(保存・シェア・伸びたリール等)を分析する
3. ChatGPTでレポートを要約し、改善提案をまとめる
4. `../reports/` に保存し、`ig-content-planner` へ申し送る

## KPI
- follower_count(フォロワー増加数)
- save_rate(保存率)
- profile_to_lp_click_rate(プロフィール→LPクリック率)

## 参照ドキュメント
- `../README.md`

## レビュー頻度
月次
