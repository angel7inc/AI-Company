# Phase2設計書 ― Instagram事業 共通基盤設計(Tomo_Angel7)

**ステータス:** 実装済み・一部後続Phaseで置換(implemented with superseding changes)― Instagram事業の基本構造は実装済み。本書はhistorical design documentとして保持する(詳細は下記上書き注記参照)
**策定日:** 2026年7月11日
**対象:** Instagram事業(MVP)/ 将来の全チャネル(ココナラ・LINE・Studio・YouTube・TikTok・X・Threads・Facebook・Pinterest等)へ展開する共通基盤

> **【2026-07-13 追記・上書き注記】** Instagram事業の基本構造(`business_unit: instagram`)は実装済みで、稼働中です。本書は**historical design document**として保持し、本文(過去時点の名称・記述)は削除・改変しません。ただし、本書内の一部のエージェント名・責任範囲・統合方式は、後続Phase設計書により置換・退役済みです ― `docs/phase3-search-seo-division-design.md`(Search & SEO・全社横断4 Division新設)、`docs/phase3b-creative-division-design.md`(Creative Division新設)、`docs/phase5-revenue-division-design.md`(Revenue Division新設、ブランド分離判断)、`docs/phase7-intelligence-division-design.md`(Intelligence Division新設、`ig-research`計画を`intelligence-consumer`等へ統合)、`docs/phase8-product-division-design.md`(Product Division新設、Brand LayerとProduct Divisionの責務分離)。現在の正本は`docs/roadmap.md`・`_company/org/agents.yaml`および上記の各後継設計書を参照してください。

---

## 00. 読む前に(重要な前提)

**「エージェント」は自動で動くロボットではありません(第1段階では)。** 現時点では、ChatGPT・Gemini・Perplexity等をAPIで自動連携する仕組みはまだ導入していません。Phase2の「エージェント」とは、**役割・入出力・使うツール・手順を1つのファイルに定義した「担当表」**です。第1段階では、COO(Claude Code)がこの担当表に沿って各AIツールを人間主導で操作し、結果を取りまとめます。慣れて型が安定してから、段階的に自動化します(詳細は05)。

### 用語ミニ辞典
- **SSOT**: Single Source of Truth。「これを見れば全部わかる」唯一の正解データ。
- **MCP**: Claude Codeが外部ツール(Google Sheets等)と繋がるための接続規格。
- **YAML**: 設定やリストを書くための、人が読みやすい記法。台帳(agents.yaml)に使用。
- **SOP**: Standard Operating Procedure。「毎回同じ手順で行う」ための手順書。
- **監査ログ**: 「誰が・いつ・何をしたか」の記録。
- **匿名化**: 名前・連絡先などを取り除き、個人を特定できない状態にすること。

---

## 01. Instagram事業の基本設計

| 項目 | 内容 |
|---|---|
| 目的 | 恋愛に悩む20〜40代女性に寄り添うコンテンツで信頼関係を築き、LINE・自社鑑定・ココナラへ自然につなげる入口を作る |
| ターゲット | 20代〜40代、恋愛に悩む女性 |
| 顧客の悩み | 恋愛関係での不安・自己理解不足・「分かってもらえない」という孤独感(想定。実際の悩みの言語化はResearch Agentが継続的に収集) |
| 提供価値 | 安心・希望・自己理解。断定的な答えではなく「そっと軽くする」寄り添い |
| 収益導線 | Instagram(認知・信頼構築)→ LINE登録(関係構築)→ 自社鑑定 / ココナラ(収益化)。将来的にカードリーディングスクール・電話占い会社への送客も同経路上に位置づけ |
| 半年目標 | フォロワー1万人。ただしブランド毀損を伴う施策は不採用(目標より基準を優先) |

### KPI優先順位

フォロワー数を最重要指標にしません。優先順位は次のとおりです。基準値は運用開始後のデータをもとに設定し、毎月見直します。

| 優先度 | KPI | フィールド名 |
|---|---|---|
| 1 | 保存率 | `save_rate` |
| 2 | シェア率 | `share_rate` |
| 3 | プロフィールアクセス率 | `profile_access_rate` |
| 4 | フォロー転換率 | `follow_conversion_rate` |
| 5 | LINE登録率 | `line_registration_rate` |
| 6 | 鑑定申込率 | `booking_rate` |
| 7 | リピート率 | `repeat_rate` |
| 8 | 売上 | `revenue_jpy` |
| 9 | フォロワー数 | `follower_count` |

---

## 02. エージェント構成の考え方

ご提示いただいた6エージェント案を検討した結果、**6体のまま採用**を推奨します。理由は各エージェントの「実行頻度」と「役割の性質」が明確に異なり、統合すると重要なチェックポイントが失われるためです。

### 意思決定1: エージェントは6体のままか、統合して減らすか

| 案 | 内容 | メリット/デメリット |
|---|---|---|
| **6体を維持(推奨)** | Strategy(月次)/ Research(週次)/ Content Planning(週次)/ Copywriting(投稿ごと)/ Brand QA(投稿ごと)/ Analytics(週次) | 実行頻度と役割の性質が異なるため無理に減らすと担当が曖昧になる。他チャネル展開時も同じ型を再利用できる |
| Content PlanningとCopywritingを統合 | 構成とコピーを1エージェントにまとめる | メリット: ファイル数が減る。デメリット: 構成(安い手戻り)とコピー(高い手戻り)の間の承認チェックポイントが失われる |
| Brand QAをCopywriting内工程にする | 自己レビュー化 | デメリット: 自己レビューは客観性が弱く、「ブランド一貫性を最優先」という方針に反する |

### 意思決定2: Research Agentはツールごとに分けるか、1体に集約するか

| 案 | 内容 |
|---|---|
| **Research Agent 1体に集約(推奨)** | Perplexity/Google Trends/Meta広告ライブラリ/Pinterest/Reddit/YouTubeをすべて1つの役割定義の中で使い分ける。MVPの最小構成に合致 |
| ツールごとに専任エージェント化 | メリット: 各ツールへの理解が深まる。デメリット: いきなり6〜7体増え、CEOの「最初は必要最小限」方針に反する。需要が増えてから分割すれば十分 |

### 意思決定3: ブランド情報(Tomo_Angel7)はどこに置くか

| 案 | 内容 |
|---|---|
| **`_company/brands/tomo-angel7/` を新設(推奨)** | Tomo_Angel7は複数チャネルにまたがるブランド。ブランド概要とQAチェックリストを1箇所にまとめ、各チャネルはそこを参照するだけにする。ルール変更時は1ファイルの修正で全チャネルに反映される |
| 各チャネルのREADMEに直接記載 | メリット: 最初は分かりやすい。デメリット: チャネルが増えるたびに複製することになり、「事業ごとに同じ機能を重複実装しない」というCEO方針に反する |

### MVPエージェント一覧

| エージェントID | 実行頻度 | 役割 | 使用ツール |
|---|---|---|---|
| `ig-strategy` | 月次 | アカウント戦略・月間テーマ・KPI目標設定・改善方針の決定 | ChatGPT, Gemini |
| `ig-research` | 週次 | 競合分析・悩み調査・トレンド調査・根拠確認・リサーチツールの振り分け | Perplexity, Gemini, Google Trends, Reddit, YouTube, Pinterest, Meta広告ライブラリ |
| `ig-content-planning` | 週次 | カルーセル5枚構成・リール構成・フック・CTA・投稿目的の明確化 | ChatGPT |
| `ig-copywriting` | 投稿ごと | 本文・見出し・短文・キャプション・CTA文言・ブランドトーン統一 | ChatGPT |
| `ig-brand-qa` | 投稿ごと | ブランドチェックリストとの照合。独立した第三者的レビュー | ChatGPT |
| `ig-analytics` | 週次 | 保存率・シェア率・フォロー転換率等の分析と改善提案 | Meta Business Suite, ChatGPT |

**既存ファイルとの関係:** Phase2着手前に作成した `ig-content-planner.md` / `ig-analytics.md` は、承認後に本設計へ合わせて再編します(`ig-content-planner` → `ig-content-planning` に役割を絞り、`ig-research`・`ig-copywriting`・`ig-brand-qa`・`ig-strategy` を新規追加)。ブランド情報が未確定だったため作られた暫定版であり、作り直しであって「無駄になった」わけではありません。

---

## 03. データフロー・エージェント間の受け渡し

ご指定の必須ワークフローをそのまま採用しています。「(人間)」は人が判断する工程です。

1. **市場・悩み調査** ― `ig-research` が Perplexity/Reddit/YouTube等から一次情報を収集
2. **投稿テーマ候補** ― `ig-research` が候補と根拠をリスト化
3. **優先順位付け** ― `ig-strategy` の月間テーマ・KPI方針に照らして順位付け
4. **企画** ― `ig-content-planning` が投稿目的を明確化
5. **構成** ― `ig-content-planning` が5枚構成・フック・CTAを設計
6. **コピー作成** ― `ig-copywriting` が本文・見出し・キャプションを執筆
7. **ブランドQA** ― `ig-brand-qa` がチェックリストと照合(不合格時は5または6へ差し戻し、最大2回)
8. **CEO確認(人間)** ― 内容・コピーの承認
9. **Canva制作(人間)** ― 既存テンプレートで画像/リール制作
10. **最終確認(人間)** ― 完成ビジュアルの承認(投稿公開・予約投稿はここで正式承認)
11. **予約投稿(人間トリガー)** ― Meta Business Suiteで設定
12. **数値取得** ― `ig-analytics` がインサイトを取得
13. **分析** ― `ig-analytics` が投稿別評価と改善提案を作成
14. **改善** ― 改善提案を `ig-research`(次週の優先度)と `ig-strategy`(月次方針)へ反映
15. **知識ベース更新** ― `ig-research`・`ig-analytics`の知見をNotebookLM(instagram_ナレッジ)へCOOが蓄積

---

## 04. 承認フロー

以下はCEOの承認なしに実行しません(ご指定の承認ルールをそのまま採用)。

| 承認が必要な操作 | 本設計での該当ポイント |
|---|---|
| 投稿公開・予約投稿 | ワークフロー10(最終確認) |
| 外部サービスへの送信・LINE配信・広告出稿 | 各チャネル展開時に都度 |
| 価格変更・アカウント設定変更 | 該当なし(発生時に別途相談) |
| API課金・有料ツール契約 | MCP導入時(Phase2以降) |
| ファイル削除・GitHubへの公開 | リポジトリ操作全般 |
| 顧客データ操作・ブランドルール変更 | ブランド層(`_company/brands/`)の変更全般 |
| 新規エージェント追加・MCP追加・自動化の本番稼働 | Phase2以降のすべての拡張 |

本設計書自体もこの承認ルールの対象です。内容のご承認をいただくまで、上記いずれも実行しません。

---

## 05. 自動化方針

| 段階 | 内容 | 自動化される範囲 |
|---|---|---|
| 第1段階(今回) | 人間主導+AI補助。COOがエージェント定義に沿って各AIツールを手動操作 | なし(すべて人間が都度実行・確認) |
| 第2段階 | 半自動化。型が安定した定型作業から段階的にMCP連携 | 数値取得(Meta Business Suite)、ナレッジ蓄積(NotebookLM)など |
| 第3段階 | 十分検証された部分のみ自動化 | 週次レポート生成、リサーチの一次スクリーニング等。**投稿公開・予約投稿・アカウント設定変更は今後も自動化しない**(2026-07-18注記: 本行はPhase2策定時点の方針。下記参照) |

**自動化できる箇所:** 数値取得、レポート生成、リサーチの一次スクリーニング、ナレッジベースへの蓄積
**自動化しない箇所:** 投稿公開・予約投稿・LINE配信・広告出稿・アカウント設定変更・価格変更・ブランドルール変更・新規エージェント追加・画像生成(既存テンプレート活用が前提のため)

> **2026-07-18追記(方針更新):** CEOにより、投稿公開の標準運用モデルが「CEOによる手動投稿」から「CEO最終承認後のシステム実行」へ正式に更新された。詳細な正本は[`docs/runbooks/human-approval-system-publication.md`](runbooks/human-approval-system-publication.md)を参照する。上記表・「自動化しない箇所」の投稿公開・予約投稿に関する記述は、この更新により置き換えられる(superseded)。ただし、**本追記時点でInstagram投稿実行アダプタ(publisher/connector)は未実装であり、実際に自動公開できる状態ではない**(現状の実装状況は本書「17. 投稿実行基盤(Instagram Publisher)準備状況」を参照)。実装完了までの間、投稿実行は「投稿実行基盤の障害・緊急訂正・システム停止中の臨時対応・CEOが個別に指示した場合」に限る手動フォールバックのみで行う。LINE配信・広告出稿・アカウント設定変更・価格変更・ブランドルール変更・新規エージェント追加・画像生成の自動化方針は、本追記の対象外であり変更していない。

---

## 06. フォルダ・ファイル構成

Phase1の構造に「ブランド層」を1つ追加します。1ブランドが複数チャネルにまたがる、という今回判明した実態に対応するための拡張です。

```
AI-Company/
├── _company/
│   ├── brands/                        # [新設]ブランド層(チャネル横断のブランド情報)
│   │   └── tomo-angel7/
│   │       ├── brand-brief.md         # コンセプト・ターゲット・提供価値・方針
│   │       └── qa-checklist.md        # ig-brand-qa等が参照するチェック項目
│   └── org/agents.yaml                # 6エージェントを追記(07参照)
│
├── _shared/templates/
│   ├── brand-brief-template.md        # [新設]
│   ├── qa-checklist-template.md       # [新設]
│   ├── weekly-report-template.md      # [新設]
│   └── research-log-template.md       # [新設]
│
└── businesses/instagram/
    ├── agents/
    │   ├── ig-strategy.md             # [新設]
    │   ├── ig-research.md             # [新設]
    │   ├── ig-content-planning.md     # 既存ig-content-planner.mdを再編
    │   ├── ig-copywriting.md          # [新設]
    │   ├── ig-brand-qa.md             # [新設]
    │   └── ig-analytics.md            # 既存(詳細化)
    ├── content/
    │   ├── calendar.md                # 既存
    │   └── research-log.md            # [新設]過去テーマ・重複防止用
    └── reports/                       # 週次/月次レポート置き場(既存)
```

---

## 07. agents.yaml 追加案(未反映・案のみ)

承認後、この内容を `_company/org/agents.yaml` に追加します。`brand`項目を新設し、ブランド層と紐づけられるようにします。

```yaml
  - id: ig-strategy
    name: Instagram戦略エージェント
    layer: business
    business_unit: instagram
    brand: tomo-angel7
    role: アカウント戦略・月間企画・KPI設計・改善方針
    tools: [ChatGPT, Gemini]
    kpi: [save_rate, share_rate, line_registration_rate]
    status: draft
    owner: COO
    review_cadence: monthly

  - id: ig-research
    name: Instagramリサーチエージェント
    layer: business
    business_unit: instagram
    brand: tomo-angel7
    role: 競合分析・悩み調査・トレンド調査・根拠確認・ツール振り分け
    tools: [Perplexity, Gemini, Google Trends, Reddit, YouTube, Pinterest, Meta広告ライブラリ]
    kpi: []
    status: draft
    owner: COO
    review_cadence: weekly

  # ig-content-planning / ig-copywriting / ig-brand-qa / ig-analytics も同形式で追加(承認後に全項目を記載)
```

---

## 08. テンプレート・プロンプト一覧

| 種別 | ファイル | 用途 |
|---|---|---|
| テンプレート | `agent-definition-template.md` | 既存。全エージェント共通の型 |
| テンプレート | `brand-brief-template.md` | ブランド概要の型(将来の新ブランドにも使用) |
| テンプレート | `qa-checklist-template.md` | ブランドQAチェックリストの型 |
| テンプレート | `weekly-report-template.md` | ig-analyticsの週次レポート形式 |
| テンプレート | `research-log-template.md` | ig-researchの調査ログ形式(重複投稿防止に使用) |
| プロンプト | `research-topic-discovery.md` | 悩み・トレンド調査用(Perplexity/Reddit向け) |
| プロンプト | `competitor-analysis.md` | 競合分析用(Gemini/YouTube向け) |
| プロンプト | `content-structure.md` | 5枚構成・フック生成用 |
| プロンプト | `copywriting.md` | ブランドトーンでの本文生成用 |
| プロンプト | `brand-qa-review.md` | チェックリスト照合用 |
| プロンプト | `weekly-analytics-summary.md` | 週次数値の要約・改善提案生成用 |

---

## 09. 運用フロー(週次/月次/エラー対応)

**週次レビュー:** `ig-analytics`が週次レポートを作成し、KPI表・良かった投稿・改善提案をCOOがCEOへ簡潔に共有します。`ig-research`はその内容を踏まえて次週の候補を調整します。

**月次レビュー:** `ig-strategy`が月間累計KPIと目標を突き合わせ、翌月のテーマ優先度とKPI基準値を見直します。ブランド毀損リスクがないかも同時に確認します。

**エラー時の対応:**
- ブランドQA不合格が2回続いた場合 ― 自動での再修正を止め、COO・CEOが直接方針を判断する
- 根拠確認ができない情報 ― 使用せず「要確認」として保留し、断定的な投稿にしない
- Meta Business Suiteの数値取得に失敗 ― 数字を推測で埋めず、レポートに欠損として明記する
- ブランド適合の判断に迷うケース ― エージェントは保守的な判断をし、CEO確認へエスカレーションする

---

## 10. データ保存・セキュリティ

| データ種別 | 保存先 |
|---|---|
| エージェント定義・プロンプト・テンプレート・SOP・命名規則・変更履歴 | GitHub(Private) |
| 画像・動画・PDF・Canva納品物・人が確認する資料 | Google Drive |
| APIキー・認証情報 | `.env`(Gitには保存しない。`.gitignore`で除外済み) |
| 顧客・鑑定申込者の個人情報 | アクセス制限つき専用領域のみ。匿名化した上でなければAIツールへ渡さない |

- APIキーはコードへ直接書かず、必ず `.env` を使用する
- 個人情報をAIへ渡す前に匿名化する
- 公開リポジトリは勝手に作成しない(現状Privateのみ)
- 外部送信(LINE配信・広告出稿等)の前は必ずCEO確認を挟む
- 最初はすべて最小権限で設定し、自動投稿は十分な検証後にのみ導入する
- 誰が・いつ・何を出力したかの監査ログは、Phase3で自動化するまでは週次レポートに手動で残す

---

## 11. 品質基準(ig-brand-qaのチェック項目)

| 観点 | 基準 |
|---|---|
| ブランド一致 | コンセプト・トーンと一致しているか |
| 感情への配慮 | 女性の感情に寄り添っているか。不安を過度に煽っていないか |
| 表現 | 断定的な未来予言をしていないか。依存を促していないか。スピリチュアル過多でないか。上から目線でないか |
| 読みやすさ | 短く、1投稿1メッセージになっているか |
| 行動喚起 | CTAが自然か。販売色が強すぎないか |
| 独自性 | 他アカウントの模倣になっていないか。既存投稿との重複が少ないか(`research-log.md`で確認) |
| 正確性 | 事実確認が必要な内容に根拠があるか |

---

## 12. 想定される問題と改善案

| 想定される問題 | 改善案 |
|---|---|
| ブランドQAが厳しく投稿ペースが落ちる | 差し戻しは最大2回まで。それでも通らない場合はCEO・COOが直接判断し、待ち時間を作らない |
| 既存投稿とテーマが重複する | `research-log.md`と`calendar.md`に過去テーマを蓄積し、企画前に必ず参照する運用にする |
| フォロワー増加とブランド毀損回避の両立が難しい局面が出る | 月次レビューでKPIとブランドリスクを両方評価。フォロワー増だけを目的にした施策は不採用にする基準を`ig-strategy`の役割に明記 |
| エージェント・プロンプトが増えて把握しづらくなる | `_shared/prompts/`にエージェントIDとの対応表(README)を用意する |
| 他チャネル展開時にブランドチェックがバラバラになる | ブランド層(`_company/brands/`)を導入済みのため、QAチェックリストは一元管理される |

---

## 13. 採用しなかった案

- **Content PlanningとCopywritingの統合** ― 構成(安い手戻り)とコピー(高い手戻り)の間の承認チェックポイントが失われるため不採用
- **Brand QAをCopywriting内の自己レビューにする** ― 客観性が弱く、ブランド一貫性の最優先方針に反するため不採用
- **StrategyとResearchの統合** ― 実行頻度(月次/週次)と役割の性質が異なるため不採用
- **リサーチツールごとの専任エージェント化** ― MVP段階でのエージェント数増加を避けるため不採用。将来手が回らなくなった時点で再検討
- **ブランド情報を各チャネルのREADMEに個別記載** ― 重複実装となりブランド変更時の保守コストが増えるため不採用
- **初期段階からの全自動化(投稿公開・分析まで自動)** ― CEOの3段階実装方針・承認ルールに反するため不採用
- **Canva制作・画像生成の専任エージェント化** ― 既存テンプレート活用が前提でAI画像生成が前提でないため不採用

---

## 14. 横展開方法(将来の他事業展開)

1. `businesses/_template/`を新チャネル名でコピーする(例: `businesses/tiktok/`)
2. 同じブランド(Tomo_Angel7)であれば `_company/brands/tomo-angel7/` をそのまま参照する(作り直さない)。将来のアロマキャンドル事業のような別ブランドの場合のみ、新しい `_company/brands/{新ブランド}/` を新設する
3. 6エージェントの「型」をチャネル特性に合わせて調整する(例: YouTubeでは content-planning が台本構成、TikTokでは短尺構成になる。役割の数と構造は変えない)
4. `agents.yaml`に新チャネル分のエージェントを追加する
5. `_shared/templates`・`_shared/prompts`の共通部分はそのまま使い、チャネル差分だけ追記する

個人鑑定・ココナラ・カードリーディングスクール・電話占い会社がTomo_Angel7ブランドに含まれるかどうかは、16章で確認します。

---

## 15. ロードマップ

| 段階 | 内容 |
|---|---|
| Phase2-a | 本設計書の承認。6エージェント定義・ブランド層・テンプレート・プロンプトの作成(ファイル作成のみ、自動化なし) |
| Phase2-b | 人間主導+AI補助で1〜2週間分の投稿を試験運用し、型を検証 |
| Phase2-c | 検証結果をもとに数値取得・ナレッジ蓄積から半自動化を開始 |
| Phase3 | ココナラ・LINE・Studio等への横展開 |

---

## 16. CEOへの確認事項

以下は私(COO)が推測で決めるべきではない項目です。ご判断をお願いします。

1. 本設計書全体(6エージェント構成・ブランド層新設・フォルダ構成)を承認するか
2. 個人鑑定(自社鑑定・ココナラ)・カードリーディングスクール・カードリーディング電話占い会社は、Tomo_Angel7と同一ブランドとして扱ってよいか、別ブランドとして扱うか
3. 「顧客の悩み」欄(01)は暫定的な想定で記載しています。実際の悩みの言語化はig-researchが継続調査しますが、現時点でCEOから追加・修正したい情報はあるか
4. 承認後、まず着手する範囲は「6エージェント定義ファイル+ブランド層+テンプレート/プロンプトの作成」までとし、実際のInstagram投稿の試験運用(Phase2-b)は別途合図をいただいてから開始する、で良いか
5. 既存の `ig-content-planner.md` を `ig-content-planning.md` に再編することに問題はないか

---

## 17. 投稿実行基盤(Instagram Publisher)準備状況(2026-07-18追記)

[`docs/runbooks/human-approval-system-publication.md`](runbooks/human-approval-system-publication.md)で定めた「CEO最終承認後のシステム実行」を、Instagramで実際に動かすための現状の実装状況を、推測せずリポジトリ調査に基づいて記録する。**存在しない機能を実装済みと記載しない。**

| 項目 | 現状 |
|---|---|
| 投稿アダプタ・connector・publisher(実コード) | 存在しない |
| Instagram/Meta向けOAuth認証フロー(実コード) | 存在しない |
| 秘密情報管理の仕組み | 概念レベルでは存在する([`_company/charter/secret-management-policy.md`](../_company/charter/secret-management-policy.md)、`.env`/`.gitignore`運用)。Instagram固有のトークン管理は未整備(`.env.example`にInstagram/Meta変数なし) |
| 画像をMetaへ渡す方法(メディアホスティング) | 存在しない |
| カルーセル投稿API呼び出し処理 | 存在しない |
| dry-run機能 | 存在しない |
| 投稿結果(投稿ID・URL)の記録機構 | 存在しない |
| 二重投稿防止(冪等性)機構 | 存在しない(`docs/runbooks/operations-continuity.md`等でも「今回実装していない・将来課題」と明記済み) |
| KPI取得との自動連携 | 存在しない(`_company/kpi/`は手動週次記録が前提) |
| `mcp/servers.yaml`上の関連エントリ | `meta-business-api`が`status: candidate`(未接続)として登録済みだが、用途は「Instagram/TikTok分析の自動取得」(数値取得)であり、投稿実行(execute)用ではない |

**結論:** 本リポジトリはドキュメント・コンテンツ中心の構成であり、アプリケーションコード(`src/`等)自体が存在しない。Instagram投稿を実際にシステムが実行するための実装は、今回のBatch時点で一切存在しない。第1投稿(`tomo-angel7-pilot-01`)をシステム公開するには、下記「18. 投稿実行基盤 次Batch実装計画」に列挙する項目すべてが不足している。

## 18. 投稿実行基盤 次Batch実装計画(2026-07-18追記・計画のみ、本Batchでは実装しない)

本Batchでは、公式ドキュメント確認やネットワーク接続を伴う実装を一切行っていない。次Batch以降の実装範囲を、CEO確認・承認のための計画として整理する。

1. 公式Instagram投稿方式(Meta Graph API等)の最新仕様確認(公式ドキュメント参照が必要)
2. 必要なアカウント種別・権限(Instagramビジネスアカウント/プロアカウント、Facebook連携要否等)の確認
3. Metaアプリ設定(Meta for Developers上でのアプリ作成・権限申請)
4. 初回OAuth認証フロー設計・実施方法
5. アクセストークンのGit管理外での保管方法([`_company/charter/secret-management-policy.md`](../_company/charter/secret-management-policy.md)準拠)
6. InstagramアカウントID・関連IDの管理方法
7. 公開対象画像(`publication-candidate-v1/01.png`〜`06.png`)をMeta APIへ渡すための配信・ホスティング方法
8. カルーセル作成(メディアコンテナ作成〜カルーセルまとめ)処理の設計
9. 投稿実行(publish)処理の設計
10. 投稿結果取得(投稿ID・URL・エラー内容)処理の設計
11. 冪等性キー設計(同一承認パッケージに対する二重投稿防止)
12. 二重投稿防止機構の実装
13. リトライ方針(何回まで・どの失敗で再試行するか)
14. エラーログ設計(`docs/runbooks/operations-continuity.md`の障害記録区分との整合)
15. dry-run機能(実際に投稿せず検証のみ行うモード)の実装
16. CEO承認ハッシュ照合処理(承認記録の`content_hash`・画像/キャプションSHA-256と、実行直前の対象が完全一致することを検証する処理)
17. 投稿URL・投稿日時の自動記録処理
18. KPI計測開始処理との連携
19. 緊急停止(実行中断)手段の設計
20. 緊急手動フォールバック手順との接続(本Runbook「6. 手動操作の位置付け」・既存`manual-posting-instructions.md`を、実装後は緊急時のみ使用する手順として位置付け続ける)

本計画は次Batchでの実装範囲の整理であり、今回のBatchで着手・実装したものは何もない。

---

本設計書はCOO(Claude Code)が作成しました。承認をいただくまで、ファイル作成・コード作成・エージェント追加・設定変更は行いません。
