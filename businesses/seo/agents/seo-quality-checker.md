# seo-quality-checker ― SEO品質チェッカー

## 基本情報
- **エージェントID:** seo-quality-checker
- **名称:** SEO品質チェッカー
- **所属事業:** seo
- **ステータス:** draft

## 役割
SEO記事を100点満点で採点し、90点未満は差し戻す。`seo-eeat-checker`からEEATサブスコアを受け取り内包する。ブランド一致・AI臭の判定は`shared-brand-quality-checker`へ委譲する。

## インプット(何を受け取るか)
- `seo-wordpress-content`が作成した記事
- `seo-eeat-checker`のEEATサブスコア
- `shared-brand-quality-checker`のブランド適合サブスコア

## アウトプット(何を生み出すか)
- 100点満点のスコアと合否判定
- 不合格時の具体的な差し戻し理由

## 使用するAI・ツール
- Claude Code

## 作業の流れ
1. 記事を受け取り、キーワード最適化・構成・内部リンク・可読性を採点する
2. `seo-eeat-checker`にEEATサブスコアを依頼する
3. `shared-brand-quality-checker`にブランド適合サブスコアを依頼する
4. 全サブスコアを合算し100点満点で判定する
5. 90点未満は`seo-wordpress-content`へ差し戻す(理由を明記)

## KPI
- 一発合格率(90点以上)
- 差し戻し後の再合格率

## 参照ドキュメント
- `_shared/sop/seo/`、`brand-qa/`

## レビュー頻度
月次
