# Knowledge: instagram

## 1. 何を保存する場所か
Instagramというプラットフォームそのものに関する恒久的な知識(アルゴリズムの傾向、投稿フォーマットごとの仕様、機能アップデートの影響等)。特定ブランドの投稿内容やKPI実績はここに置かない(→`_company/brands/`または`businesses/instagram/`)。

## 2. 保存してはいけない情報
特定ブランドの投稿実績・フォロワー数等の実データ(→`businesses/instagram/data/`)、顧客の個人情報。

## 3. 誰が参照するか
当該Knowledgeを使用するIntelligence関連エージェント、`ig-strategy`、`ig-content-planner`(および将来、他ブランドのInstagram担当エージェントが増えた場合はそちらも)。

## 4. 更新ルール
Instagram側の仕様変更を確認したら、該当記事の`updated_at`と`version`を更新する。削除は明らかな誤りの場合のみ行う。

## 5. 推奨する情報源(Sources)
`official`, `documentation`, `industry-report` ― 具体例: Meta公式のヘルプセンター・For Creators発表、信頼できる業界メディア

## 6. 関連Knowledge
- `marketing/`
- `copywriting/`
- `seo/`
- `analytics/`

## 7. 機密情報の扱い
プラットフォーム仕様は基本`public`/`internal`。自社アカウントの内部データと紐づく分析は含めない。

## 8. 重複防止ルール
新規作成前に`knowledge-index.md`を確認し、既存記事の追記で対応できないか検討する。
