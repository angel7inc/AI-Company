# Creative Assets(共有クリエイティブ素材)

`shared-creative-asset-manager`が一元管理する、ブランドに依存しない共有の仕組み・汎用素材を置く場所です。

## ここに置くもの / 置かないもの
- **ここに置く:** テンプレートの仕組み(構造・可変領域の定義)、汎用ストック素材(特定ブランドに紐づかない背景・BGM・効果音のライブラリ)
- **ここに置かない:** ブランド固有のロゴ・フォントファイル・実際のカラー値(→`_company/brands/{ブランドID}/assets/`)、画像/動画生成のプロンプトそのもの(→`_shared/prompts/image/`・`_shared/prompts/video/`)

## フォルダ構成
| フォルダ | 内容 |
|---|---|
| `templates/` | Instagramカルーセル等、複数管理可能なテンプレートの構造定義 |
| `stock/` | ブランド非依存の汎用背景素材・BGM・効果音ライブラリ |

## 重複防止ルール
ブランド固有の値(色・フォント名)はここに書かず、必ず各ブランドの`brand-brief.md`を参照する。同じ用途のテンプレート・素材を複数作らず、既存のものを更新する。

## 誰が参照するか
`shared-creative-asset-manager`、`ig-creative-team`、`seo-editorial-image`、`shared-video-creative-team`、`shared-thumbnail-team`。
