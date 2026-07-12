# Phase4設計書 ― Management Division 新設

**ステータス:** 承認済み(2026-07-12。CEO承認のうえ、Fable 5の判断を踏まえて実装)
**策定日:** 2026-07-12
**対象:** 全Divisionを統括する経営レイヤー「Management Division」(CEO Office / COO Office / Strategy / KPI / Resource / Quality)

---

## 00. 本書の位置づけ
`docs/phase2-instagram-design.md`(Instagram)・`docs/phase3-search-seo-division-design.md`(SEO・4横断Division)・`docs/phase3b-creative-division-design.md`(Creative Division)に続く設計書。「Instagramの会社」ではなく「AI Company」として完成させるため、全Division・将来のRevenue/Automation/Intelligence/Product/Finance/HR Divisionを統括できる経営レイヤーを今回確立する。

## 01. 組織構造の型
```
_company/                        … 経営レイヤー(既存)
├── charter/                     … 会社の理念・原則・AI利用ポリシー(既存を拡張)
├── org/                         … agents.yaml・brands.yaml(既存)
├── kpi/                         … 全社/Division/Agent KPIの集計(既存の空フォルダを実体化)
├── reports/                     … 週次/月次/四半期レポート・承認ログ(既存の空フォルダを実体化)
└── agents/                      … Management Division専属エージェント定義(新設)
```
Management Divisionは、Creative/SEO Divisionのような「コンテンツを作る/チェックする」実行機能ではなく、**全Divisionの上位に立つガバナンス層**であるため、`_shared/agents/`ではなく`_company/agents/`に置く(ルートREADMEが既に`_company/`を「経営レイヤー」と定義しているため、その構造に整合する)。

## 02. CEO Office(ドキュメント層。エージェントではない)
自律的に経営判断を行うエージェント(`management-ceo`等)は作らない。憲章原則5「重要な意思決定はCEOが確認する」と構造的に矛盾するため。

| 要素 | 置き場所 |
|---|---|
| 会社Vision/Mission | `_company/charter/charter.md`(既存を拡張。ブランドごとのVision/Missionとは別に、AI-Company自体のものを追加) |
| 年間戦略・ロードマップ | `docs/roadmap.md`(既存を拡張。新規ファイルは作らない) |
| 承認ログ | `_company/reports/`(既存フォルダを実体化。運用ルールを新設) |

**将来検討事項(今回は見送り):** CEO向けの意思決定材料の要約を作成するアシスタント的エージェントは、意思決定そのものを行わない範囲であれば正当な用途だが、入力元となる`management-kpi`/`management-quality`/`management-resource`のレポートがまだ存在しないため、初回の月次レビュー実施後に再検討する(Fable 5判断)。

## 03. エージェント構成
| # | エージェントID | 種別 | 置き場所 | 役割概要 |
|---|---|---|---|---|
| COO Office | `core-coo`(既存エージェントを拡張。新規作成しない) | core | `_company/agents/core-coo.md`(★初めて定義ファイルを作成。id・ファイル名の対応規則に合わせ`management-coo.md`ではなく`core-coo.md`とする) | 各Division管理・Agent管理・優先順位管理・タスク配分・進捗管理 |
| Strategy | `management-strategy` | core | `_company/agents/` | 事業戦略・成長戦略・新規事業判断・撤退判断・市場分析(月次/四半期)。`shared-competitor-intelligence`の日次監視結果を入力の1つとして使うが、日次監視そのものは行わない |
| KPI | `management-kpi` | core | `_company/agents/` | 会社/Division/Agent KPIの集計。既存の値(agents.yamlの`kpi`欄、各ブランドの`brand-brief.md`KPI表、各事業の`data/`)を再定義せず集約するのみ |
| Resource | `management-resource` | core | `_company/agents/` | 予算・APIコスト(Claude/Gemini/OpenAI/画像/動画生成)・稼働時間・トークンの監視。`_company/charter/ai-usage-policy.md`(既存)の遵守状況を監視し、方針自体は再定義しない |
| Quality | `management-quality` | core | `_company/agents/` | 既存QAエージェント群(`shared-brand-quality-checker`・`shared-image-quality-checker`・`shared-video-quality-checker`・`seo-quality-checker`/`seo-eeat-checker`)の合格率・稼働状況のメタ監査。加えて月次で**較正サンプリング**(既に合格した投稿から5〜10件を抽出し、チェッカーの判定がブランドSSOTと整合しているか検証)を行う。個別コンテンツの再採点は行わない |

*全5体は`layer: core`、`division: management`とする。*

## 04. 既存Divisionとの境界線(重複防止の要点)
| 比較対象 | 違い |
|---|---|
| `management-strategy` vs `shared-competitor-intelligence` | 前者は月次/四半期の戦略判断、後者は日次の軽量監視。後者の結果は前者への入力の1つ |
| `management-strategy` vs `ig-strategy`(phase2計画、未実装) | 前者は全社・複数事業をまたぐ戦略、後者はInstagram単体の月次戦略 |
| `management-quality` vs 既存Quality Checker群 | 前者はチェッカーの合格率・稼働状況のメタ監査+月次較正サンプリング、後者は個別コンテンツの判定(実行)。判定の最終権限は引き続き実行層のチェッカーにある |
| `management-resource` vs `ai-usage-policy.md` | 前者は実際の利用状況の監視・報告、後者は方針そのもの(前者は方針を変更しない) |
| `management-kpi` vs 各ブランド`brand-brief.md`のKPI表 | 前者は集約・ダッシュボード化、後者がKPI優先順位の正本 |

## 05. Knowledge Layer拡張
新規カテゴリ: `business-strategy/`(Foundationグループ)― 成長/撤退判断・事業ポートフォリオ理論など、恒久的に使える一般的な経営理論。KPI理論は既存`analytics/`、コスト管理は`ai-usage-policy.md`(charter)側のため新規カテゴリは追加しない。

## 06. SOP Layer拡張(必要最低限)
新規カテゴリ: `sop/management/`(1カテゴリのみ)。以下3ファイルで構成する。
- **CEO承認フロー** ― 二段階承認プロトコル(Gate1: 目的/メリット/デメリット/代替案/長期的な影響/推奨案 → Gate2: ファイル名/保存場所/内容要約/全文)を初めて社内文書として明文化する。これまでCOO(Claude Code)のセッションメモリにのみ存在していたルールであり、Fable 5が「最大の継続性リスク」として最優先の明文化を指摘した項目
- **COO承認フロー** ― CEO承認が不要な軽微な変更(既存ファイルの誤字修正等)の扱い
- **レビューサイクル** ― 週次(KPI/リソースの簡易確認、COO主導)・月次(品質較正・戦略入力を含む本格レビュー、CEO同席)・四半期(月次レビューの拡張として同一ファイル内で定義。独立SOPにしない)。加えてLoop1(個別コンテンツ制作)からLoop2(経営ガバナンス)への即時エスカレーション条件を明記する:
  1. いずれかのQAチェッカーの合格率が閾値を下回った場合
  2. `ai-usage-policy.md`基準のAPIコストが予算ラインを超過した場合
  3. いずれかのKPIが2期連続で目標未達の場合

## 07. ワークフロー(2ループ構成)
**Loop1: 個別コンテンツの制作・承認(既存、変更なし)**
```
research → instagram/seo → copywriting → design/video → brand-qa → publishing(CEO承認・投稿ごと) → analytics → conversion
```

**Loop2: 経営ガバナンス(Management Division、新設・週次/月次/四半期)**
```
Instagram・SEO・Creative各Divisionの実績(KPI・品質・コスト)
→ management-kpi(集計) / management-quality(メタ監査+月次較正) / management-resource(コスト監視)
→ management-strategy(戦略への統合)
→ core-coo(優先順位付け・タスク配分)
→ CEO Office(週次/月次/四半期レビューで方向性を確認・承認)
```
「Instagram→SEO→Creative→Management→CEO→公開」はLoop2として実装し、投稿ごとの経路ではない。「公開」自体は引き続きLoop1の`publishing/`で行う。

## 08. 将来Divisionへの接続(設計のみ、今回実装しない)
Revenue/Automation/Intelligence/Product/Finance/HR Divisionは、追加時に以下のパターンに当てはめるだけで済む。
- 実行機能 → `_shared/agents/{機能}/`または`businesses/{channel}/agents/`
- 経営ガバナンス機能 → `_company/agents/`(今回のManagement Divisionと同じ型)
- 各Divisionの実績データは`management-kpi`・`management-resource`・`management-quality`が既存フォーマットで吸い上げるだけで、Management側の再設計は不要

## 09. 変更履歴
| 版 | 日付 | 変更内容 | 変更者 |
|---|---|---|---|
| 1 | 2026-07-12 | 初版作成。CEO承認済み(Fable 5判断を経て実装) | COO |
