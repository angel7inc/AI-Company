# Phase3b設計書 ― Creative Division 新設

**ステータス:** 承認済み(2026-07-12。CEO承認のうえ、Fable 5の判断を踏まえてGate2個別承認を省略し一括実装)
**策定日:** 2026-07-12
**対象:** 全社共通のCreative Division(画像・動画・デザイン・ブランド表現の統括)。Instagram専用ではなく、WordPress・YouTube・Pinterest・LINE・広告・LP・スクール・電話占いまで共通利用する。

---

## 00. 本書の位置づけ
`docs/phase3-search-seo-division-design.md`と並行する設計書。Company Foundation(`_company/charter/charter.md`)・Knowledge Layer・SOP Layer・Brand Layerの既存ルールを完全に継承する。

## 01. 組織構造の型
```
businesses/{channel}/agents/     … チャネル固有の薄いフロント役(例: ig-creative-team, seo-editorial-image)
_shared/agents/design/           … 全社共有の専門エージェント(Prompt Engineer/Quality Checker/Asset Manager等)
_company/brands/{brand}/         … ブランド固有の判断基準(色・フォント等の値の正本)
```
チャネル専属エージェントは実際の成果物を持つため`businesses/`に置き、横断機能(プロンプト作成・品質チェック・素材管理)は`_shared/agents/design/`(Phase1で「ビジュアル制作」とスコープ済みの既存フォルダ)に集約する。将来YouTube・Pinterest・LINE等が増えても、チャネル専用の薄いフロント役を1つ追加するだけで済む。

## 02. エージェント構成(9体)
| # | エージェントID | 種別 | 置き場所 | 役割概要 |
|---|---|---|---|---|
| ① | `ig-creative-team` | チャネル専属 | `businesses/instagram/agents/` | カルーセルテンプレート管理・ブランドカラー/フォント適用・テンプレート改善・保存率改善。内容をテンプレートへ流し込む方式(ゼロから作らない)。複数テンプレートを管理できる構造とする |
| ② | `seo-editorial-image` | チャネル専属 | `businesses/seo/agents/` | WordPress記事専用。記事内容を理解し、毎回アイキャッチ・本文画像・OGP画像をAI生成する(テンプレート方式は使わない) |
| ③ | `shared-video-creative-team` | 全社共有 | `_shared/agents/design/` | Instagram Reels・YouTube Shorts・YouTube通常動画・広告動画・LINE動画・LP動画を制作。ブランドトーン・世界観・テンポ・CTA・視聴維持率・離脱率を最適化 |
| ④ | `shared-image-prompt-engineer` | 全社共有 | `_shared/agents/design/` | 画像生成AI用プロンプト作成。ブランド維持・CTR改善・SEO・読者心理・構図設計・画像スタイル管理。①②⑧から呼ばれる内部ユーティリティ |
| ⑤ | `shared-video-prompt-engineer` | 全社共有 | `_shared/agents/design/` | 動画生成AI用プロンプト作成。動画構成・シーン設計・カメラワーク・BGM・テロップ・演出・CTA・ブランド維持。③⑧から呼ばれる内部ユーティリティ |
| ⑥ | `shared-image-quality-checker` | 全社共有 | `_shared/agents/design/` | 画質・構図・CTR・OGP・SEO・Alt・人物違和感・文字可読性を採点。ブランド一致・AI臭判定は`shared-brand-quality-checker`へ委譲する |
| ⑦ | `shared-video-quality-checker` | 全社共有 | `_shared/agents/design/` | 冒頭3秒・視聴維持率・テンポ・離脱率・CTA・字幕・音声・画質を採点。ブランド一致・AI臭判定は`shared-brand-quality-checker`へ委譲する |
| ⑧ | `shared-thumbnail-team` | 全社共有 | `_shared/agents/design/` | YouTube・Pinterest・LP・広告のサムネイル制作(プラットフォームごとのCTR戦略)。実際のプロンプト作成は④⑤へ委譲する |
| ⑨ | `shared-creative-asset-manager` | 全社共有 | `_shared/agents/design/` | ブランド素材(ロゴ・アイコン・フォント・カラー・テンプレート・画像/動画プロンプト・背景素材・BGM・効果音)の一元管理。ブランド固有の値(色・フォント名)は複製せず各ブランドの`brand-brief.md`を参照する |

*全7体の共有エージェントには`division: creative`を付与する(前回Fable 5承認の`division`フィールドを使用)。*

## 03. 重複解消の決定事項
1. **ブランドQAの委譲** ― Image/Video Quality Checkerは画像・動画固有の技術基準のみを担当し、「ブランド一致」「AI臭チェック」は`shared-brand-quality-checker`へ委譲する(サブルーチン呼び出し。SEO側のEEAT Checker→SEO Quality Checkerと同じパターン)。`shared-brand-quality-checker`はメディア種別(image/video)をパラメータとして受け取り、メディア固有のブランド基準(動きのテンポ・BGMのトーン等)は各ブランドの`qa-checklist.md`に記載する(Fable 5判断)。
2. **Thumbnail Teamとプロンプトエンジニアの分担** ― Thumbnail Teamはプラットフォームごとの戦略のみを担当し、実際のプロンプト作成は行わない。
3. **Creative Asset Managerとbrand-brief.mdの関係** ― Asset Managerは「共有の仕組み(テンプレート構造・プロンプトライブラリ・汎用ストック素材)」のみを保有し、ブランド固有の値(カラーコード・フォント名)は複製しない。ブランド固有の実ファイル(ロゴ画像等)は`_company/brands/{ブランドID}/assets/`に置くが、値の正本は引き続き`brand-brief.md`。

## 04. brand-brief.md 自動化方針の改定(CEO承認事項)
`_company/brands/tomo-angel7/brand-brief.md` 21章「画像デザインルール」の記述を、Creative Division新設に伴い以下のとおり改定する。

**改定前:** 「既存Canvaテンプレートの活用を前提とし、新規デザイン作成は行わない」(`docs/phase2-instagram-design.md` 05章の「自動化しない箇所」に画像生成を含む)

**改定後:**
- Instagram: `ig-creative-team`によるテンプレートへの流し込み(自動化)を許可する。テンプレート自体のゼロからのデザイン作成は引き続き行わない
- WordPress: `seo-editorial-image`による記事内容に応じたAI画像生成(テンプレートを使わない、記事ごとに毎回生成)を許可する

*これは組織図の承認だけでブランドSSOTの記述を暗黙に上書きするものではなく、本設計書の承認をもって`brand-brief.md`側の該当箇所も明示的に更新することとする(Fable 5判断)。*

## 05. Knowledge Layer拡張
新規グループ「Creative」を追加し、以下3カテゴリを新設する(既存カテゴリを壊さないよう、`design/`・`branding/`は現状のMarketingグループのまま維持する)。

| カテゴリ | 内容 |
|---|---|
| `image-generation/` | AI画像生成の理論・技法・プロンプトパターン |
| `video-generation/` | AI動画生成の理論・技法・プロンプトパターン |
| `video-production/` | 動画編集理論(構成・テンポ・BGM・テロップの型。ツール非依存の一般理論) |

*色彩心理・構図理論・広告クリエイティブ・Instagramデザイン・Pinterestデザイン・YouTubeサムネ理論・ブランドデザインは、既存の`design/`・`branding/`・`instagram/`・`pinterest/`・`youtube/`・`marketing/`へ統合し、新規カテゴリは作らない。`video-generation/`と`video-production/`は重複しやすい境界のため、両READMEに「どちらに何を書くか」を明記する。*

## 06. SOP Layer拡張
既存`sop/design/`(画像・カルーセル・WordPress画像・Pinterest画像制作の手順)をそのまま再利用する。新規カテゴリは`sop/video/`(Reels/Shorts/YouTube/広告動画/LINE動画/LP動画の制作手順、チャネルごとに分割しない)のみ。

`sop/design/`の現行README(「画像・リール素材制作」)は、リール(動画)が画像カテゴリに混在しているため、リールに関する記述を`sop/video/`側へ移し、画像専用に整理する(既存ファイルの軽微な訂正)。

## 07. 素材・プロンプトの保存構造
- **新設 `_shared/creative-assets/`** ― テンプレートの仕組み・汎用ストック素材(ブランド非依存)
- **既存 `_shared/prompts/` を拡張** ― `image/`・`video/`サブフォルダを追加(新規トップフォルダを増やさず既存資産を再利用)
- **新設 `_company/brands/{ブランドID}/assets/`** ― ブランド固有のロゴ・フォントファイル等の実体(値の正本は引き続き`brand-brief.md`)

## 08. 将来ツール連携
Figma・Photoshop・Illustrator・Premiere Pro・CapCut・DaVinci Resolveは現時点でどのエージェントも使用していないため、Knowledgeカテゴリを先回りして作らない。**実際にいずれかのSOPの「使用ツール」欄に採用されたタイミングで、該当のKnowledgeカテゴリを1つ追加する**というルールを`docs/naming-conventions.md`に明記する。

## 09. 変更履歴
| 版 | 日付 | 変更内容 | 変更者 |
|---|---|---|---|
| 1 | 2026-07-12 | 初版作成。CEO承認済み(Fable 5判断を経て個別Gate2承認を省略) | COO |
