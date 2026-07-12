# Knowledge: image-generation

## 1. 何を保存する場所か
AI画像生成の理論・技法(プロンプトパターン、モデルごとの得意/不得意、よくある失敗パターンとその回避法)。「何を作るか」の一般理論は`design/`、「AIにどう指示するか」はこちら。

## 2. 保存してはいけない情報
特定ブランドの画像プロンプトそのもの(→`_shared/prompts/image/`)、特定ブランドのカラー・フォント等の実際の指定(→`_company/brands/{ブランドID}/brand-brief.md`)。

## 3. 誰が参照するか
`shared-image-prompt-engineer`、`shared-thumbnail-team`、`ig-creative-team`、`seo-editorial-image`。

## 4. 更新ルール
画像生成AIのモデル更新・仕様変更を確認次第、既存記事を更新する。

## 5. 推奨する情報源(Sources)
`official`, `documentation`, `experiment` ― 具体例: 各画像生成AIの公式ドキュメント、COOによる生成実験の記録

## 6. 関連Knowledge
- `design/`
- `instagram/`
- `pinterest/`

## 7. 機密情報の扱い
一般理論は`public`/`internal`。未公開のブランド専用プロンプトは含めない(→`_shared/prompts/image/`側で管理し、機密度に応じて扱う)。

## 8. 重複防止ルール
同じプロンプト技法の重複記事を作らず、既存記事へ追記する。
