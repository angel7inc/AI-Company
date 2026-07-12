# Knowledge: video-generation

## 1. 何を保存する場所か
AI動画生成の理論・技法(プロンプトパターン、モデルごとの得意/不得意、よくある失敗パターン)。編集技法・テンポ・BGM等の「人間/ソフトウェアによる編集の型」は`video-production/`を参照。

## 2. カテゴリの境界線(`video-production/`との違い)
| | video-generation | video-production |
|---|---|---|
| 対象 | AI動画生成モデルへの指示の仕方 | Premiere/CapCut/DaVinci等での編集技法 |
| 例 | 「このモデルはカメラワーク指定にこの言い回しが効く」 | 「冒頭3秒はテンポを速くする」「離脱を防ぐカット割り」 |

迷ったら「AIへの入力(プロンプト)の話か、素材ができた後の編集の話か」で判断する。

## 3. 保存してはいけない情報
特定ブランドの動画プロンプトそのもの(→`_shared/prompts/video/`)。

## 4. 誰が参照するか
`shared-video-prompt-engineer`、`shared-video-creative-team`、`shared-thumbnail-team`。

## 5. 更新ルール
動画生成AIのモデル更新・仕様変更を確認次第、既存記事を更新する。

## 6. 推奨する情報源(Sources)
`official`, `documentation`, `experiment`

## 7. 関連Knowledge
- `video-production/`
- `youtube/`
- `instagram/`

## 8. 機密情報の扱い
一般理論は`public`/`internal`。

## 9. 重複防止ルール
同じプロンプト技法の重複記事を作らず、既存記事へ追記する。
