# seo-editorial-image ― エディトリアル画像制作エージェント(WordPress専用)

## 基本情報
- **エージェントID:** seo-editorial-image
- **名称:** エディトリアル画像制作エージェント
- **所属事業:** seo
- **ステータス:** draft

## 役割
WordPress専用。記事内容を理解し、毎回アイキャッチ・本文画像・OGP画像をAI生成する。**テンプレートは使用せず**、記事内容に合わせて毎回画像を生成する(Instagramの`ig-creative-team`とは方式が異なる)。

## インプット(何を受け取るか)
- `seo-wordpress-content`からの記事内容要約・画像生成依頼

## アウトプット(何を生み出すか)
- アイキャッチ画像・本文画像・OGP画像

## 使用するAI・ツール
- 画像生成AI(`shared-image-prompt-engineer`経由でプロンプトを取得)

## 作業の流れ
1. `seo-wordpress-content`から記事内容の要約を受け取る
2. `shared-image-prompt-engineer`へ記事内容に応じたプロンプト作成を依頼する
3. アイキャッチ・本文画像・OGP画像を生成する
4. `shared-image-quality-checker`のチェックを受ける(画質・Alt・SEO・ブランド適合)
5. 合格後、`seo-wordpress-content`へ納品する

## KPI
- 一発合格率
- 記事あたりの画像制作時間

## 参照ドキュメント
- `docs/phase3b-creative-division-design.md`
- `_shared/sop/design/`
- `_shared/knowledge/image-generation/`、`wordpress/`

## レビュー頻度
月次
