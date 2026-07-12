# Phase3設計書 ― Search & SEO Division / 全社共有Division 新設

**ステータス:** 承認済み(2026-07-12。CEO承認のうえ、Fable 5の判断を踏まえてGate2個別承認を省略し一括実装)
**策定日:** 2026-07-12
**対象:** Search & SEO Division(`businesses/seo/`)、Instagram部門への追加(Carousel Intelligence)、全社横断4 Division(Competitor Intelligence / Traffic Division / Conversion Division / Brand Quality Division)

---

## 00. 本書の位置づけ
`docs/phase2-instagram-design.md`に続く設計書。Knowledge Layer・SOP Layer・Brand Layーの既存ルール(重複禁止・単一正本・ブランド固有情報の分離)を完全に継承し、100事業・1000エージェント規模への拡張を前提に設計している。Creative Division(Instagram Creative Team / Editorial Image Team等)は`docs/phase3b-creative-division-design.md`を参照。

## 01. 新設する事業ユニット: SEO
| 項目 | 内容 |
|---|---|
| 事業コード | `seo` |
| フォルダ | `businesses/seo/`(`businesses/_template/`をコピー) |
| 所属ブランド | `tomo-angel7`(`brands.yaml`の`business_units`にリスト追加。将来他ブランドも同様に追記するだけで共有可能) |
| 目的 | Google検索・WordPressを通じた集客をInstagramに次ぐ第二の柱とする |
| 収益導線 | Instagram投稿→保存数分析→SEO記事化→WordPress公開→Google検索流入→LINE登録(→鑑定・スクール・電話占いは別途ブランド帰属確認後に接続) |

## 02. エージェント構成

### 02-1. SEO Division専属(`businesses/seo/agents/`)
| エージェントID | 役割概要 |
|---|---|
| `seo-research` | 検索上位分析・検索意図・PAA・関連検索・共起語・検索ボリューム・競合・EEAT・順位変動監視、**および**AI Overview・SERP構成・サジェスト・上位記事構成・FAQ分析(旧「Search Intelligence」を統合)。WordPress記事だけでなくPinterest/Instagramのテーマ選定にもインテリジェンスを提供する全消費者向けの役割 |
| `seo-keyword-strategy` | 親KW・ロングテール・記事クラスタ設計・優先順位付け・検索難易度分析・CV意識のKW選定 |
| `seo-wordpress-content` | SEO記事設計・タイトル・H1〜H4・内部リンク・FAQ・CTA・メタディスクリプション・画像ALT・カテゴリ・タグ・パーマリンク(本文執筆は既存`sop/copywriting/`を再利用) |
| `seo-technical` | Core Web Vitals・Schema・Robots・Sitemap・Canonical・Index管理・404管理・Redirect管理 |
| `seo-internal-link` | サイト全体の内部リンク設計・記事間の最適リンク |
| `seo-eeat-checker` | E-E-A-T観点の採点。`seo-quality-checker`のサブルーチンとして機能し、独立採点は行わない |
| `seo-search-console` | 表示回数・CTR・順位変動・リライト候補・急落記事検知(`ig-analytics`と同性質の役割のため`sop/analytics/`を再利用) |
| `seo-quality-checker` | SEO記事を100点満点で採点。90点未満は差し戻し。EEATスコアは`seo-eeat-checker`から受け取り内包する |

### 02-2. Instagram部門への追加(`businesses/instagram/agents/`)
| エージェントID | 役割概要 |
|---|---|
| `ig-carousel-intelligence` | 競合カルーセル分析・保存率分析・スワイプ率分析・ページ構成分析・フック分析・CTA分析・文字量分析・デザイン分析・改善提案・勝ちパターン抽出 |

### 02-3. 全社共有Division(`_shared/agents/`)
| エージェントID | 置き場所 | 役割概要 |
|---|---|---|
| `shared-competitor-intelligence` | `_shared/agents/research/` | Instagram・Google・SEO・WordPress・LINE・Pinterest・YouTubeを毎日軽量監視し、変化を検知したら該当の専門エージェント(`seo-research`・`ig-carousel-intelligence`等)へ深掘りを引き継ぐ。自らは深掘り分析を行わない |
| `shared-traffic-intelligence` | `_shared/agents/analytics/` | Pinterest・YouTube・Google検索・WordPress・Reddit・Google Trends・AI Overview・被リンク・ブランド検索など、Instagram以外の流入チャネルを横断して監視・アトリビューション分析する |
| `shared-brand-quality-checker` | `_shared/agents/analytics/` | 全チャネル共通のブランドQA(トーン・世界観・AI臭・禁止表現・一貫性)。実行時に対象ブランドの`qa-checklist.md`/`brand-brief.md`を読み込んで判定し、評価できない場合は通過させない(フェイルクローズ)。メディア種別(image/video)をパラメータとして受け取り、メディア固有のブランド基準は各ブランドの`qa-checklist.md`側に記載する(Fable 5判断) |
| `shared-conversion-optimizer` | `_shared/agents/conversion/`(新設フォルダ) | LP改善・導線改善・CTA改善・CVR改善・離脱率改善・UX改善・LINE登録率改善。特定チャネルに属さない横断機能 |

*全4体の共有エージェントには`division`フィールドを付与する(例: `division: competitor-intelligence` / `traffic` / `brand-quality` / `conversion`)。*

## 03. 重複解消の決定事項
1. **Search Intelligence** ― 独立エージェントを新設せず、`seo-research`の役割定義を拡張して吸収した(01章参照)。
2. **Brand Quality Division** ― phase2設計で予定されていた`ig-brand-qa`は個別に作らず、`shared-brand-quality-checker`へ一本化した。Instagram含む全チャネルのブランドQAはこの1エージェントが担当する。この変更は`docs/phase2-instagram-design.md` 07章のエージェント追加案に対する修正である。
3. **Competitor Intelligence** ― 日次の軽量監視・アラートのみを担当し、深掘り分析は行わない(担当は各チャネルの専門エージェント)。監視と分析を分離することで二重の深掘り作業を防いでいる。

## 04. Knowledge Layer拡張
| 変更 | 内容 |
|---|---|
| 新規カテゴリ | `wordpress/`(Platformグループ。WordPressの使い方・仕様) |
| 既存カテゴリの拡張 | `psychology/`のスコープ説明に「行動経済学」を明記(新規カテゴリは作らない) |

*EEAT理論・テクニカルSEO理論・キーワード理論等はすべて既存`seo/`カテゴリに集約し、細分化しない。*

## 05. SOP Layer拡張
| 変更 | 内容 |
|---|---|
| 新規カテゴリ | `sop/seo/`(SEO記事制作・リライト・内部リンク・技術SEO・EEAT採点・品質チェックの手順) |
| 新規カテゴリ | `sop/conversion/`(LP・導線・CTA・CVR改善の手順) |
| 新規カテゴリ | `sop/brand-qa/`(全チャネル共通のブランドQA実行手順) |
| 既存カテゴリの再利用 | `sop/analytics/`(Traffic Division・Search Console AI)、`sop/research/`(Competitor Intelligence)、`sop/copywriting/`(WordPress本文執筆) |

`sop-index.md`のカテゴリ表にこれら3カテゴリを追記する。

## 06. `agents.yaml`スキーマ拡張
共有エージェントが増えるため、`division`フィールド(任意項目)を追加する。`divisions.yaml`のような独立台帳の新設はFable 5の判断により見送る。**見送りの再検討基準:** `agents.yaml`が50行を超えた時点、またはいずれかのDivisionが独自の予算・COO以外のオーナーを持つに至った時点。この基準は`docs/naming-conventions.md`に明記する。

## 07. AI利用ポリシー
`_company/charter/ai-usage-policy.md`を参照。通常業務はClaude Code/ChatGPT/Search Console/Google Trendsで完結させ、Gemini Deep Researchは高難度リサーチ・競合深掘り・大型企画・新ジャンル調査など、確信度が不足する場面でのみ呼び出す。

## 08. 横展開方法(将来チャネル)
YouTube・Pinterest・LINE・広告運用・スクール・電話占い・EC・新ブランドは、`businesses/{channel}/`を追加し、02-3の全社共有Divisionをそのまま再利用するだけで対応できる。新しいチャネル固有の手順が必要な場合のみ、`sop/{channel}/`・`knowledge/{channel}/`を1カテゴリ追加する(過剰分割はしない)。

*(既存の`tiktok`事業ユニットにまだ対応するKnowledgeカテゴリが無いことを調査時に確認済み。今回の対応範囲外だが、将来の検討事項として記録する。)*

## 09. 変更履歴
| 版 | 日付 | 変更内容 | 変更者 |
|---|---|---|---|
| 1 | 2026-07-12 | 初版作成。CEO承認済み(Fable 5判断を経て個別Gate2承認を省略) | COO |
