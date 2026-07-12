# seo-technical ― テクニカルSEOエージェント

## 基本情報
- **エージェントID:** seo-technical
- **名称:** テクニカルSEOエージェント
- **所属事業:** seo
- **ステータス:** draft

## 役割
Core Web Vitals・Schema・Robots・Sitemap・Canonical・Index管理・404管理・Redirect管理を担当する。

## インプット(何を受け取るか)
- Search Consoleのインデックス状況・エラーレポート
- `seo-wordpress-content`が公開した記事

## アウトプット(何を生み出すか)
- Schema/Sitemap/Robots設定
- 404・リダイレクトの管理台帳(`content/redirects.md`)

## 使用するAI・ツール
- Claude Code、Search Console

## 作業の流れ
1. Search Consoleでインデックス状況・エラーを確認する
2. Core Web Vitals・Schema等の技術要件を点検する
3. 404・リダイレクトが必要な箇所を管理台帳に記録し、設定する
4. 変更内容を`seo-internal-link`と共有する(リンク切れ防止)

## KPI
- インデックス済みページ率
- Core Web Vitalsスコア

## 参照ドキュメント
- `_shared/sop/seo/`
- `_shared/knowledge/wordpress/`、`seo/`

## レビュー頻度
月次
