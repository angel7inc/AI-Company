# Knowledge Layer(知識資産層)

すべてのAIエージェント・すべての事業が共通で参照する「会社の社内Wiki」です。一度調べた・学習した知識をここに蓄積し、同じ調査を繰り返さないようにします。

> **3行でわかるKnowledge Layer**
> 1. 書く前に [`knowledge-index.md`](./knowledge-index.md) で重複がないか確認する
> 2. [`knowledge-template.md`](./knowledge-template.md) をコピーして該当カテゴリに保存し、`knowledge-index.md`に1行追加する
> 3. `sensitivity`が`confidential`/`restricted`の内容は外部AI(ChatGPT等)へ絶対に送らない

## Knowledgeとは何か / 何ではないか

- **Knowledgeである:** 恒久的に使える一般知識(例: Instagramのアルゴリズム、コピーライティングの型、行動心理学、薬機法の基礎)
- **Knowledgeではない:**
  - プロンプト(→ `_shared/prompts/`)
  - ブランド固有の情報(→ `_company/brands/{ブランドID}/`)
  - 事業固有の実績データ(→ `businesses/{事業}/data/`)

迷ったら「この情報は、Tomo_Angel7が無くなっても、Instagram事業が無くなっても価値が残るか?」を基準にしてください。残るならKnowledge、残らないならブランド情報か事業データです。

## カテゴリ一覧

34カテゴリを、意味のまとまりで7グループに整理しています(フォルダ構造自体はフラットで、これは読む順序の目安です)。

**Foundation ― AI活用・経営の基礎理論**
| フォルダ | 内容 |
|---|---|
| `ai/` | AI/LLMそのものに関する一般知識 |
| `automation/` | 自動化の設計パターン・ワークフロー理論 |
| `prompt-engineering/` | プロンプト設計の技法・パターン(プロンプトそのものではなく「書き方の知識」) |
| `business-strategy/` | 成長/撤退判断・事業ポートフォリオ理論など経営戦略の一般理論 |
| `research-methodology/` | 情報源評価・引用作法・ファクトチェック手法・一次/二次情報理論 |
| `product-design/` | サービス構造化理論・カリキュラム設計理論 |
| `finance/` | 会計原則・予算配分フレームワーク・P/L構造の一般理論 |

**Tools ― 個別ツールの使い方**
| フォルダ | 内容 |
|---|---|
| `claude-code/` | Claude Codeの使い方・仕様 |
| `chatgpt/` | ChatGPTの使い方・仕様 |
| `gemini/` | Geminiの使い方・仕様 |
| `perplexity/` | Perplexityの使い方・仕様 |
| `notebooklm/` | NotebookLMの使い方・仕様 |

**Marketing ― マーケティング・クリエイティブ理論**
| フォルダ | 内容 |
|---|---|
| `marketing/` | マーケティング理論・フレームワーク |
| `branding/` | ブランディングの一般理論(個別ブランドの実データではない) |
| `design/` | デザイン理論・配色・レイアウトの原則 |
| `copywriting/` | コピーライティングの型・技法 |
| `psychology/` | 行動心理学・認知バイアス・恋愛心理学など一般知識 |
| `seo/` | 検索エンジン最適化の知識 |
| `analytics/` | 分析手法・指標定義・統計の基礎 |
| `crm/` | CRM運用理論+LTV/リテンション理論(顧客との継続関係) |

**Platform ― 各プラットフォームの仕様**
| フォルダ | 内容 |
|---|---|
| `instagram/` | Instagramのアルゴリズム・仕様・機能に関する知識 |
| `youtube/` | YouTubeのアルゴリズム・仕様 |
| `pinterest/` | Pinterestの使い方・仕様 |
| `reddit/` | Redditの使い方・仕様 |
| `line/` | LINE公式アカウントの仕様・機能 |
| `studio/` | Studio(ホームページ制作ツール)の使い方・仕様 |
| `canva/` | Canvaの使い方・機能 |
| `coconala/` | ココナラの仕組み・出品ルール |
| `wordpress/` | WordPressの使い方・仕様 |

**Creative ― クリエイティブ生成の理論・技法**
| フォルダ | 内容 |
|---|---|
| `image-generation/` | AI画像生成の理論・技法・プロンプトパターン |
| `video-generation/` | AI動画生成の理論・技法・プロンプトパターン |
| `video-production/` | 動画編集理論(構成・テンポ・BGM・テロップ。ツール非依存) |

**Legal**
| フォルダ | 内容 |
|---|---|
| `legal/` | 薬機法・景品表示法・著作権などの法律知識 |

**Templates**
| フォルダ | 内容 |
|---|---|
| `_templates/` | Knowledge記事自体のテンプレート・型に関する知識(先頭に来るよう`_`を付与) |

## 使い方

1. 新しい知識を書くときは [`knowledge-template.md`](./knowledge-template.md) をコピーし、該当カテゴリのフォルダに保存する
2. 保存したら [`knowledge-index.md`](./knowledge-index.md) に1行追加する
3. ファイル名は [命名規則](../../docs/naming-conventions.md) に従う(小文字ケバブケース)

## 更新ルール
- 内容が古くなった場合は削除せず、信頼度を下げるか更新日を書き換えて修正する
- 明らかに間違っていた場合のみ削除する(削除・大量変更は承認が必要なため、必ずCEOに確認する)

## Sources(情報源)の統一語彙
各カテゴリREADMEの「推奨する情報源」、および各記事フロントマターの`source_type`は、次の10種類で統一します(正本は[`knowledge-template.md`](./knowledge-template.md)のフロントマターコメント)。

`official` / `documentation` / `research-paper` / `book` / `industry-report` / `youtube` / `reddit` / `community` / `experiment` / `internal`

新しい種類が必要になった場合は、ここと`knowledge-template.md`・[命名規則](../../docs/naming-conventions.md)の3箇所を同時に更新してください。

## 機密情報の取り扱い(重要)
`sensitivity`が`confidential`または`restricted`のKnowledgeは、ChatGPT・Gemini・Perplexity等の外部AIサービスや外部ツールへ絶対に送信しないでください。これらの記事はClaude Codeの範囲内でのみ参照します。

## 知識の作成ルール(重複防止)
- 新しい記事を作る前に、必ず`knowledge-index.md`で同じ内容が無いか確認する
- 似た内容が既にある場合は、新規作成せず既存記事を更新する(細分化しすぎない)
- ある知識について「正本」は常に1つ。複数の記事が同じ範囲を扱う場合は、古い方を`supersedes`で置き換え、`status: deprecated`にする
- 削除は行わず、`status`を`deprecated`または`archived`にする

## 将来のフォルダ構造への移行計画
現在はカテゴリ(トピック)別にフォルダを分けています。将来、記事数が増えたら、次のような「種類別」構造への移行を想定しています。

```
_shared/knowledge/
├── _templates/
├── sources/       # 公式資料・論文・一次情報
├── distilled/      # 整理済みの再利用可能な知識
├── case-studies/   # 実施結果・成功/失敗事例
└── archive/        # 非推奨・廃止・旧版
```

各記事は最初から`knowledge_type`(source/distilled/case-study)を持っているため、移行時は「`knowledge_type`ごとにフォルダへ移動し、`categories`をサブフォルダまたはタグとして維持する」だけで対応できます。`archive`は新しい`knowledge_type`を作らず、既存の`status: archived`をそのまま使います。**現時点ではこの移行は行わず、今のカテゴリ構成のまま運用します。**

**変更履歴:** 旧`knowledge_type: playbook`(標準手順・SOP・チェックリスト)は、SOP Layer(`_shared/sop/`)の新設に伴い廃止しました。手順・SOPに類する内容は今後SOP Layerへ作成してください。詳細は[`_shared/sop/README.md`](../sop/README.md)を参照。

## 将来の拡張
NotebookLM・RAG・ベクトルDB・MCP経由のAI検索への移行を想定し、各記事はフロントマター(先頭のメタデータ)を持たせています。将来的には`knowledge-index.md`をこのメタデータから自動生成する仕組みに置き換える想定です。
