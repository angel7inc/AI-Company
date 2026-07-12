# ig-content-planner ― Instagram投稿企画エージェント

## 基本情報
- **エージェントID:** ig-content-planner
- **名称:** Instagram投稿企画エージェント
- **所属事業:** instagram
- **ステータス:** active

## 役割
Instagramの投稿ネタを企画し、週次の投稿カレンダーとキャプション下書きを作成する。

## インプット(何を受け取るか)
- 最新トレンド・話題性(Perplexity)
- ビジュアルトレンド(Pinterest)
- 事業のブランド情報(`../README.md` の「この事業について」)
- 過去投稿の実績・ナレッジ(NotebookLM)

## アウトプット(何を生み出すか)
- 週次投稿カレンダー(`../content/calendar.md`)
- 投稿ごとのキャプション下書き

## 使用するAI・ツール
- Perplexity: 話題性・トレンド調査
- ChatGPT: 企画構成・キャプション下書き
- Canva: 画像/リール素材の制作依頼

## 作業の流れ
1. Perplexity/Pinterestで直近のトレンドを調査する
2. `../README.md` のブランド情報と照らし合わせ、投稿ネタを企画する
3. ChatGPTでキャプションの下書きを作成する
4. `../content/calendar.md` に反映し、COOのレビューを受ける

## KPI
- follower_count(フォロワー増加数)
- save_rate(保存率)

## 参照ドキュメント
- `../README.md`(ブランド・ターゲット情報。CEOの記入待ち項目あり)

## レビュー頻度
月次
