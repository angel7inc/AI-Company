# seo-keyword-strategy ― キーワード戦略エージェント

## 基本情報
- **エージェントID:** seo-keyword-strategy
- **名称:** キーワード戦略エージェント
- **所属事業:** seo
- **ステータス:** draft

## 役割
親キーワード・ロングテール・記事クラスタ設計・優先順位付け・検索難易度分析・CVを意識したキーワード選定を担当する。

## インプット(何を受け取るか)
- `seo-research`の分析結果
- 各ブランドのKPI優先順位(`brand-brief.md`)

## アウトプット(何を生み出すか)
- キーワードクラスタ設計(`content/keyword-clusters.md`)
- 記事化優先順位リスト

## 使用するAI・ツール
- Claude Code
- Search Console(既存記事のキーワード実績確認)

## 作業の流れ
1. `seo-research`の分析結果から親キーワード候補を抽出する
2. ロングテールキーワードへ展開し、記事クラスタを設計する
3. 検索難易度・CV貢献度を評価し優先順位をつける
4. `seo-wordpress-content`へ記事化を依頼する

## KPI
- クラスタ内の記事順位改善率
- CV貢献キーワードの獲得率

## 参照ドキュメント
- `_shared/sop/seo/`
- `_shared/knowledge/seo/`

## レビュー頻度
月次
