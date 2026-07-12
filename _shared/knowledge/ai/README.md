# Knowledge: ai

## 1. 何を保存する場所か
AI/LLMそのものに関する一般知識(モデルの特性、得意不得意、活用パターン等)。特定ブランドでの活用実績はブランド層へ、実際のプロンプトは`_shared/prompts/`へ置く。

## 2. 保存してはいけない情報
実際のプロンプト本文(→`prompt-engineering/`は技法のみ、実体は`_shared/prompts/`)、APIキー・認証情報、顧客データ。

## 3. 誰が参照するか
COO(Claude Code)、`ig-strategy`、将来の全事業のAI活用担当エージェント。

## 4. 更新ルール
新しいAIモデルの登場、既存モデルの仕様変更を確認したら更新する。

## 5. 推奨する情報源(Sources)
`official`, `documentation`, `research-paper` ― 具体例: 各AIベンダーの公式ドキュメント・リリースノート

## 6. 関連Knowledge
- `prompt-engineering/`
- `automation/`
- `claude-code/`
- `chatgpt/`
- `gemini/`
- `perplexity/`

## 7. 機密情報の扱い
このカテゴリで機密情報を扱うことは想定しないが、社内限定の運用ノウハウは`sensitivity: internal`とする。

## 8. 重複防止ルール
新規作成前に`knowledge-index.md`を確認する。ツール個別の使い方は各ツール専用フォルダ(`chatgpt/`等)へ、AI全般の話のみここに置く。
