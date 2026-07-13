---
report_type: architecture-baseline-audit
audit_id_prefix: ARC
executed_by: architecture-review-controller
methodology: full-sweep(全件走査。統計的サンプリングではない)
executed_at: "2026-07-13"
status: draft-for-ceo-review
---

# AI Company アーキテクチャ初回ベースライン監査報告書

**実行エージェント:** `architecture-review-controller`(Phase10 Architecture Review Division)
**実行日:** 2026-07-13
**手法:** 全件走査(フルスイープ)。`management-quality`の統計的較正サンプリングとは異なる
**制限事項:** 本監査は検出・分類・報告のみを行う。是正の実行は一切行っていない。

---

## 監査対象サマリー

| 項目 | 件数 |
|---|---|
| 監査対象ファイル数(`.gitkeep`除く) | 154 |
| 監査対象エージェント数(`agents.yaml`) | 47 |
| Knowledgeカテゴリ数 | 34 |
| SOPカテゴリ数 | 17(うち実手順ファイル14) |
| ブランド数 | 3(active: 1、planned: 2) |
| Phase設計書数 | 10 |
| 発見件数 | 10 |

### 重要度別件数(2026-07-13 再集計)
| 重要度 | 件数 | 該当ARC ID |
|---|---|---|
| critical | 0 | ― |
| high | 4 | ARC-001, ARC-002, ARC-003, ARC-007 |
| medium | 3 | ARC-004, ARC-005, ARC-006 |
| low | 2 | ARC-008, ARC-009 |
| info | 1 | ARC-010 |
| **合計** | **10** | |

**再集計の経緯(初版の誤り):** 初版の概要表は「high 3 / info 2」としていたが、本文で`ARC-007`を`high`と明記していたにもかかわらず概要表の集計に含めておらず、また`info`は`ARC-010`の1件のみであるにもかかわらず2件と誤記していた。本文各ARC IDの`重要度`欄を全件突き合わせた結果、正しい内訳は上表のとおり(high 4 / medium 3 / low 2 / info 1 = 10件)。原因は概要表を本文とは別に手作業で集計し、本文更新後に再突き合わせを行わなかったことによる転記漏れであり、判定基準そのものの誤りではない。是正チケット化はしない(本報告書自体の訂正のため)が、`architecture-review-controller`の今後の運用ルールとして「概要表は本文の各ARC IDの重要度から自動集計し、手動転記しない」ことを`sop/architecture-review/`側の留意事項として記録する。

### 総合判定
- **SSOT二重管理:** 明確な二重管理は検出されず。ただし監視すべき軽微な相関あり(ARC-010)
- **存在しない参照:** `definition:`パス47件すべて実在を確認、破損なし。ただし**旧判断・退役済みエージェントへの参照**が複数箇所に残留(ARC-002〜004)
- **セキュリティ上の問題:** 現時点でのAPIキー・トークン・パスワードの混入は無し(全件スキャン済み)。ただし**将来のデータ格納先の保護漏れ**を検出(ARC-001)、**将来の自動配信における承認ゲート欠落**を検出(ARC-007)

---

## 発見事項一覧

### ARC-001(2026-07-13 改訂)
- **重要度:** high
- **分類:** セキュリティ / Git管理対象外リスク(監査項目14)
- **対象ファイル:** `_company/kpi/`, `_company/reports/`, `businesses/*/data/`, `businesses/*/reports/`
- **対象エージェントまたはSOP:** `management-kpi`, `finance-controller`, `sop/management/review-cycle.md`
- **問題の具体的内容:** `_company/finance/actuals/`は`sensitivity: confidential`として`.gitignore`で明示的に除外されているが、`management-kpi`・`finance-controller`が実際に**利益・ROI等の集計値を書き込む先**である`_company/kpi/`・`_company/reports/`、および各事業の`data/`・`reports/`は`.gitignore`の対象外のまま。
- **根拠:** `.gitignore`には`_company/finance/actuals/**/*.yaml`の1行のみが機密除外ルールとして存在。`_company/kpi/README.md`は「`finance-controller`が算出した利益は`_company/kpi/`経由で集約対象に加わる」と明記しており、書き込み先自体は無防備。現時点で両フォルダの中身はREADME.mdのみ(実データなし)。
- **影響:** 今後`finance-controller`・`management-kpi`が実際に月次利益・ROIレポートを記録し始めた時点で、Private Repositoryとはいえその内容がGitHubへ蓄積され続ける。Finance実績を保護した設計思想と一貫しない。

**推奨する是正方法(改訂: 一律除外ではなく領域分離)**
CEOのご指摘のとおり、`_company/reports/`全体や`_company/kpi/`全体を一律にGit管理対象外にする案は不適切(構造監査報告書・ルール・スキーマ・是正計画等、機密数値を含まない資産まで隠してしまう)。代わりに、既に`_company/finance/`で確立済みの「README+スキーマ(Git管理)/実データ(Git除外)」という型をそのまま再利用し、以下のように領域を分離することを提案する。

```
_company/kpi/
├── README.md              # 既存。Git管理対象のまま(数値を含まない説明文書)
├── schemas/                # [新設案] ダッシュボードの構造定義(数値を含まないスキーマ・サンプル)。Git管理対象
└── actuals/                # [新設案] 実際のKPI数値(週次/月次スナップショット)。Git管理対象外

_company/reports/
├── README.md               # 既存。Git管理対象のまま
├── architecture-review/    # 既存(本報告書はここに保存済み)。構造監査は機密数値を含まないためGit管理対象のまま
├── templates/               # [新設案] レポート雛形(数値を含まない)。Git管理対象
└── confidential/             # [新設案] 実際の週次/月次スナップショット・実数値を含むCEO承認ログ。Git管理対象外
```
`businesses/*/data/`・`businesses/*/reports/`は現時点で完全に空(`.gitkeep`のみ)のため、同じ「機密数値の有無で領域を分ける」原則を適用しつつ、**実データが実際に発生する段階で具体的なサブフォルダを設計する**(先回りしてフォルダを作らないという、`divisions.yaml`等で既に採用している既存の先送り原則をここでも踏襲する)。

**この案が最も重複が少ない理由:** `_company/finance/`が確立した「README+スキーマ=Git管理、実データ=Git除外」というパターンを再利用するだけで、新しい除外方式・新しいディレクトリ思想を発明していない。`_company/reports/architecture-review/`は既に本報告書によって実在するため、その位置づけ(機密数値なし・Git管理対象)を追認するだけでよく、移動・改名は不要。

- **是正担当Division/Agent:** COO主導(`.gitignore`はいずれのDivisionにも専属しない全社設定のため)。`finance-controller`・`management-kpi`と構造レビューを行う。*(初版で「Automation Division」としていたのは誤り。`automation-connection-manager`はAPI/MCP接続管理であり`.gitignore`管理とは無関係のため訂正)*
- **CEO承認が必要か:** 要(`.gitignore`変更・新規サブディレクトリ構造は機密性に関わる設計判断のため)
- **自動修正可能か:** 可能(`.gitignore`への追記・空フォルダ作成のみ。ただし境界線の最終確認はCEO承認後)
- **現在の状態:** open
- **依存関係:** なし(即時対応可能)
- **推奨対応順序:** 1位

---

### ARC-002
- **重要度:** high
- **分類:** 旧判断の誤読リスク(監査項目17)
- **対象ファイル:** `_company/brands/tomo-angel7/pain-point-categories.yaml`(6行目、28行目)
- **対象エージェントまたはSOP:** 記載上は`ig-research`(Phase7で正式退役済み)、後継は`intelligence-consumer`
- **問題の具体的内容:** 6行目「validated(**ig-researchの調査**で裏付けが取れた段階)」、28行目「各カテゴリの妥当性検証は **ig-research エージェント**が継続的に行う想定」と、退役済みエージェントが現在も稼働しているかのように記載されている。
- **根拠:** `docs/phase7-intelligence-division-design.md` 01章で`ig-research`は「今回正式に退役を確定する」と明記され、後継は`intelligence-consumer`(`_shared/agents/intelligence/intelligence-consumer.md`)。`brand-brief.md`は変更履歴4版で同様の修正を反映済みだが、`pain-point-categories.yaml`は未修正のまま残っている。
- **影響:** このファイルを参照した人間・AIエージェントが「ig-researchが今も稼働している」と誤認し、存在しないエージェントへ作業を割り振る可能性がある。
- **推奨する是正方法:** 6行目・28行目の`ig-research`を`intelligence-consumer`に置換する。
- **是正担当Division/Agent:** Product Division(`product-service-designer`)またはCOO(ブランドファイルの軽微修正のため`sop/management/coo-approval-flow.md`の範囲内)
- **CEO承認が必要か:** 不要(誤字・旧参照の修正であり、設計判断を伴わない軽微修正に該当)
- **自動修正可能か:** 可能
- **現在の状態:** resolved(2026-07-13 再監査で確認。RT-003として実施)
- **依存関係:** なし
- **推奨対応順序:** 2位

**【2026-07-13 再監査追記】** 修正前の状態(6行目「validated(ig-researchの調査で裏付けが取れた段階)」、28行目「各カテゴリの妥当性検証は ig-research エージェントが継続的に行う想定」)は上記のとおり。`intelligence-consumer`が`agents.yaml`615行目に登録済み・definitionファイル実在・役割が「悩み・ペルソナ・行動分析」かつ「`pain-point-categories.yaml`のstatus更新提案」を担うことを確認したうえで置換した。置換後、ファイル内に`ig-research`の文字列が残存しないこと、YAML構造・値(3カテゴリ・status・created_at等)が無変更であることを確認済み。詳細は`_company/reports/architecture-review/2026-07-13-architecture-remediation-plan.md`の「Batch B 再監査結果」を参照。

---

### ARC-003
- **重要度:** high
- **分類:** 旧判断の誤読リスク(監査項目17)
- **対象ファイル:** `docs/phase2-instagram-design.md`(3行目のステータス欄)
- **対象エージェントまたはSOP:** `ig-strategy`・`ig-research`・`ig-content-planning`・`ig-copywriting`・`ig-brand-qa`(Phase2で提案された6エージェント構成)
- **問題の具体的内容:** ステータス欄が「設計フェーズ ― CEO承認待ち(未実装。ファイル作成・エージェント追加・設定変更は行っていません)」のままだが、実際には該当する6エージェント構成は当初案どおりには実装されず、個別に別の意思決定で置き換えられている(`ig-research`→`intelligence-consumer`へ退役・置換、`ig-brand-qa`→`shared-brand-quality-checker`へ統合、`ig-strategy`・`ig-content-planning`・`ig-copywriting`は未着手のまま他エージェントで代替運用)。またブランド構造自体もPhase8で3ブランドに分離されており、本書16章の「CEO確認事項2」(ブランド統合可否)は既に別の形で決着している。
- **根拠:** `docs/phase3-search-seo-division-design.md`・`phase3b-creative-division-design.md`・`phase7-intelligence-division-design.md`・`docs/roadmap.md`の「ブランド構造」節を参照。
- **影響:** 最も古い基盤設計書であるため、将来のPhase設計時に本書を「現在も有効な未承認提案」として参照し、既に退役・変更された内容に基づいて再提案してしまうリスクが高い。
- **推奨する是正方法:** ステータス欄に「一部内容は後続Phase(3・3b・7・8)により置換・退役済み。現在の正本は各後継設計書を参照」という上書き注記を追加する(Phase5設計書で採用した「上書き注記」方式と同じ扱い)。
- **是正担当Division/Agent:** COO(設計書への注記追加、二段階承認プロセスのGate2相当)
- **CEO承認が必要か:** 要(過去の設計書の位置づけを変更する記述のため、5-part形式の軽微確認を推奨)
- **自動修正可能か:** 可能(注記追加のみ)
- **現在の状態:** resolved(2026-07-13 再監査で確認。RT-004として実施)
- **依存関係:** なし
- **推奨対応順序:** 3位

**【2026-07-13 再監査追記】** 修正前の状態(3行目「設計フェーズ ― CEO承認待ち(未実装)」)は上記のとおり。ステータスを「実装済み・一部後続Phaseで置換(implemented with superseding changes)」に更新し、`phase5`・`phase8b`設計書と同じ上書き注記(blockquote)形式で、後継設計書(phase3・phase3b・phase5・phase7・phase8)へのリンクを追加した。全リンク先の実在を`test -f`で確認済み(CEOご指示の「Phase8a」は実ファイルとして存在しないため、実在する`phase8-product-division-design.md`に置き換えて参照。詳細は是正計画書を参照)。`git diff`で本文(00章以降)が削除・改変されていないことを確認済み。詳細は`_company/reports/architecture-review/2026-07-13-architecture-remediation-plan.md`の「Batch B 再監査結果」を参照。

---

### ARC-004
- **重要度:** medium
- **分類:** 旧判断の誤読リスク(監査項目17)
- **対象ファイル:** `_shared/knowledge/gemini/README.md`・`instagram/README.md`・`perplexity/README.md`・`pinterest/README.md`・`reddit/README.md`・`seo/README.md`・`youtube/README.md`(各「誰が参照するか」欄)
- **対象エージェントまたはSOP:** `ig-research`(退役済み)
- **問題の具体的内容:** 7つのKnowledgeカテゴリREADMEが、退役済みの`ig-research`を現役の参照エージェントとして記載したまま。
- **根拠:** ARC-002・ARC-003と同じくPhase7の退役決定が反映されていない。`_shared/sop/research/README.md`は同様の退役注記を既に正しく反映済みで、対比すると更新漏れが明確。
- **影響:** 新規Knowledge記事作成時に「このカテゴリは誰が読むか」を誤認する可能性がある。実害は限定的(実行時の参照エラーにはならない)。
- **推奨する是正方法:** 各READMEの該当箇所を`intelligence-consumer`(該当する場合は`intelligence-market`・`intelligence-trend`等、カテゴリに応じて)に置換する。
- **是正担当Division/Agent:** COO(軽微修正)
- **CEO承認が必要か:** 不要(旧参照の一括修正、設計判断を伴わない)
- **自動修正可能か:** 可能
- **現在の状態:** resolved(2026-07-13 再監査で確認。RT-005として実施)
- **依存関係:** ARC-002と同時に対応することを推奨
- **推奨対応順序:** 4位

**【2026-07-13 再監査追記】** 一括文字列置換ではなく、ファイルごとに後継の妥当性を個別確認した。`gemini`→`intelligence-consumer`、`perplexity`→`intelligence-consumer`、`reddit`→`shared-traffic-intelligence`、`youtube`→`shared-competitor-intelligence`の4件は`agents.yaml`のtools/役割記述との直接一致を確認し置換(高確信度)。`instagram`・`pinterest`・`seo`の3件は複数候補が部分的にしか一致せず単一エージェントへの断定根拠が不足していたため、推測での特定エージェント置換は行わず「当該Knowledgeを使用するIntelligence関連エージェント」という役割ベースの表現に置換した。7ファイル全てで`ig-research`の文字列残存なしを確認済み。詳細は`_company/reports/architecture-review/2026-07-13-architecture-remediation-plan.md`の「Batch B 再監査結果」を参照。

---

### ARC-005
- **重要度:** medium
- **分類:** README・index・実際の構造の不一致(監査項目15)
- **対象ファイル:** `_shared/sop/sop-index.md`(「SOP一覧」表)
- **対象エージェントまたはSOP:** 全SOP(該当14ファイル)
- **問題の具体的内容:** 「SOP一覧」表が「(まだSOPがありません)」のままだが、実際には`sop/research/`(5件)・`sop/finance/`(2件)・`sop/management/`(3件)・`sop/architecture-review/`(4件)の計14件の実手順ファイル(frontmatter付き)が既に存在する。
- **根拠:** `grep`による全件走査で14ファイルの`id:`フロントマターを確認済み(本監査で実施)。
- **影響:** `sop-index.md`を「SOPの正本一覧」として信頼した場合、実在するSOPを見落とす。Knowledge Layerの`knowledge-index.md`も同種の課題を抱えているが、記事自体がまだ0件のため実害は発生していない。SOPは既に14件存在するため、こちらは実害が生じている。
- **推奨する是正方法:** 14件のSOPを「SOP一覧」表へ追記する。将来的な自動生成への移行は`sop-index.md`自身が既に「将来1000本規模への備え」として言及済みのため、今回は手動更新で対応する。
- **是正担当Division/Agent:** COO、または各SOPのowner(`finance-controller`・`intelligence-*`・`core-coo`・`architecture-review-controller`)
- **CEO承認が必要か:** 不要(索引の更新であり設計判断を伴わない)
- **自動修正可能か:** 可能
- **現在の状態:** open
- **依存関係:** なし
- **推奨対応順序:** 5位

---

### ARC-006
- **重要度:** medium
- **分類:** 責任範囲・越権リスク(監査項目10)
- **対象ファイル:** `_shared/agents/analytics/shared-brand-quality-checker.md`・`_shared/agents/design/shared-image-quality-checker.md`・`_shared/agents/design/shared-video-quality-checker.md`・`businesses/seo/agents/seo-quality-checker.md`・`businesses/seo/agents/seo-eeat-checker.md`・`_company/agents/management-quality.md`・`_shared/agents/product/product-quality-standards.md`・`_shared/agents/intelligence/intelligence-research-quality-checker.md`
- **対象エージェントまたはSOP:** 上記8エージェント(QA/監査系エージェント全体の8/9)
- **問題の具体的内容:** `architecture-review-controller`(本Division、最新)には「してはいけないこと」として明示的な非実行境界(是正を実行しない)が明記されているが、既存の8体のQA/監査系エージェントには同様の明示的境界宣言が無い。役割説明の文脈からは「判定のみ」であることが読み取れるが、明文化はされていない。
- **根拠:** 全QA/監査系9エージェントの定義ファイルを`grep`で走査し、「してはいけないこと」セクションの有無を確認(本監査で実施)。
- **影響:** 現時点で実害は無いが、将来これらのエージェントが実際に稼働し始めた際、「チェックのついでに軽微な修正も自動で行ってよいのでは」という実装判断がなされるリスクがある(監査項目10が懸念する越権)。
- **推奨する是正方法:** 8エージェントの定義ファイルに、`architecture-review-controller`と同様の「してはいけないこと」セクションを追加し、判定・スコアリングのみを行い、対象コンテンツ・対象エージェント定義そのものの修正は行わない旨を明記する。
- **是正担当Division/Agent:** 各エージェントの所属Division(Management/Creative/SEO/Product/Intelligence)、COOが横断的に統一フォーマットを適用
- **CEO承認が必要か:** 不要(既存役割の明文化であり、新しい権限や制限の追加ではない)
- **自動修正可能か:** 可能
- **現在の状態:** open
- **依存関係:** なし
- **推奨対応順序:** 6位

---

### ARC-007(2026-07-13 改訂: 全社的な外部送信ルールとして再評価)
- **重要度:** high
- **分類:** 将来のAPI/MCP接続リスク(監査項目20)
- **対象ファイル:** `_shared/agents/revenue/revenue-crm-manager.md`、`businesses/seo/agents/seo-wordpress-content.md`、`businesses/seo/agents/seo-technical.md`、`_shared/sop/publishing/README.md`、`_shared/sop/customer/README.md`、`mcp/api-registry.yaml`
- **対象エージェントまたはSOP:** `revenue-crm-manager`(LINE配信)、`seo-wordpress-content`/`seo-technical`(WordPress公開・技術設定)、将来の広告出稿・予約決済・顧客対応担当(現状owning agentなし)

**再評価の経緯:** CEOのご指摘を受け、`revenue-crm-manager`単体ではなく、外部へ情報を送信・書き込む全エージェントを全件確認した。各行為を**prepare(下書き・配信案作成。CEO承認前でも可)**と**execute(実際の公開・送信・配信・外部書き込み。CEO承認必須)**に区分した結果は以下のとおり。

| 対象 | 行為 | 区分 | 現状のCEO承認ゲート |
|---|---|---|---|
| `ig-content-planner` | Instagram投稿の下書き作成 | prepare | 不要(正しい) |
| (Instagram公開実行) | Instagram投稿公開・予約投稿 | execute | ✅ `sop/publishing/`に明記済み |
| `seo-wordpress-content` | WordPress記事の下書き作成 | prepare | 不要(正しい) |
| `seo-wordpress-content`/`seo-technical`(公開実行の担当は未確定) | WordPress記事公開・サイト技術設定変更 | execute | ❌ **未明記(新たに発見)。`sop/publishing/`の「関連SOP」は`instagram/`・`analytics/`のみで`seo/`への接続が無い** |
| `revenue-crm-manager` | LINEステップ配信シナリオ作成 | prepare | 不要(正しい) |
| `revenue-crm-manager` | LINE配信の実行 | execute | ❌ **未明記(初版で指摘した内容)** |
| `revenue-pricing-offer-designer` | 価格・オファーの提案 | prepare | 不要(正しい) |
| (広告出稿) | 広告出稿の実行 | execute | owning agentが現状存在しない(未着手・手動運用中と推定) |
| (予約・決済) | 予約受付・決済処理 | execute | owning agentが現状存在しない(未着手・手動運用中と推定) |
| (顧客への返信) | DM/コメントへの返信送信 | execute | `sop/customer/`に「将来の顧客対応担当エージェント」と正直に明記されており、未整備であることは既に自覚的(問題ではなく正しい現状表示) |
| `automation-connection-manager` | API/MCP接続の有効化 | execute | ✅ `sop/automation/`に明記済み |

- **問題の具体的内容:** Instagram公開とAPI/MCP接続の2件は承認ゲートが正しく機能しているが、**WordPress公開・LINE配信という2つの実行系統に、明文化されたCEO承認ゲートが欠落**している。承認要件の有無がチャネルごとにバラバラで、一貫した全社ルールになっていない。
- **根拠:** 上表の各対象ファイルを全件走査し「承認」の出現有無を確認(本監査で実施)。
- **影響:** 将来`wordpress-api`・`line-api`(`mcp/api-registry.yaml`にいずれも`status: candidate`で登録済み)が本番接続された場合、Instagram投稿と同等かそれ以上のブランド毀損・顧客対応リスクを伴う行為が、人間承認なしに自動化されうる。

**是正案の比較(2案)**

| 案 | 内容 | 長所 | 短所 |
|---|---|---|---|
| A. 個別エージェントへの追記 | `revenue-crm-manager.md`・`seo-wordpress-content.md`・`seo-technical.md`にそれぞれ承認ステップを追記 | 各エージェントの文脈に即した具体的な記述ができる | 承認基準のロジックが複数ファイルに分散し、将来YouTube・Pinterest等のチャネルが増えるたびに同じ判断基準を繰り返し記述することになる。`ai-usage-policy.md`が確立した「複製せず参照する」というDRY原則と矛盾する |
| **B(推奨). 全社共通のOutbound Action Approval Policyを新設** | `_company/charter/outbound-action-policy.md`(仮称)に、prepare/executeの定義・execute判定基準・承認が必要な行為の一覧(投稿公開・LINE配信・メール送信・広告出稿・予約決済・外部API書き込み等)を一元管理する。既存の`sop/publishing/`・`sop/automation/`の承認要件はこのポリシーの具体例として整理し直し、各エージェントは本ポリシーへのリンクのみを記載する(`ai-usage-policy.md`と同じ参照パターン) | 重複を避けられる。新チャネル追加時も新エージェントが本ポリシーを参照するだけで済み、100事業規模でも同じ基準を維持できる | 新しいポリシー文書を1つ増やす(ただし`ai-usage-policy.md`の前例があり学習コストは低い) |

**推奨する是正方法:** 案B(全社共通ポリシー新設)を推奨する。理由は既存の`ai-usage-policy.md`パターンとの一貫性、および将来のチャネル増加への拡張性のため。

- **是正担当Division/Agent:** Management Division(`core-coo`がポリシー文書を管理)。適用対象はRevenue Division(`revenue-crm-manager`)・SEO Division(`seo-wordpress-content`・`seo-technical`)、将来の広告出稿・予約決済担当
- **CEO承認が必要か:** 要(承認要否の基準そのものを決定する全社ポリシーの新設のため)
- **自動修正可能か:** 不可(CEOによる承認基準の意思決定を要する)
- **現在の状態:** open
- **依存関係:** `mcp/api-registry.yaml`の`line-api`・`wordpress-api`が実際に`active`化される前に解消することを推奨
- **推奨対応順序:** 1位タイ(本番稼働前に必須。ただし両APIとも現状`candidate`のため緊急度はARC-001と並列)

---

### ARC-008
- **重要度:** low
- **分類:** 責任者不在の業務(監査項目4)
- **対象ファイル:** `_shared/agents/copywriting/`(`.gitkeep`のみ)
- **対象エージェントまたはSOP:** なし(未整備)
- **問題の具体的内容:** Phase1でスキャフォールドされた共有4フォルダ(research/copywriting/design/analytics)のうち、`research/`・`design/`・`analytics/`は後続Phaseで共有エージェントが配置されたが、`copywriting/`だけは`.gitkeep`のみで、共有の「コピーライティング方法論」エージェントが一度も配置されていない。
- **根拠:** `_shared/agents/`配下の全ファイルを走査し確認(本監査で実施)。Creative Divisionでは画像・動画それぞれに`shared-image-prompt-engineer`・`shared-video-prompt-engineer`という再利用可能な方法論エージェントが配置されているが、コピーライティングには対応するものがない。
- **影響:** 実害は現状無い(各チャネル固有エージェントが個別に本文執筆を担っているため業務は回っている)。ただし意図的な設計判断か単なる見送りかが記録されていない。
- **推奨する是正方法:** 「コピーライティングは画像・動画と異なりチャネル固有性が強く、共有方法論エージェントは不要」という判断であれば、その旨を`_shared/agents/README.md`に明記する。逆に将来必要と判断するなら、Creative Divisionと同様の共有エージェント新設を検討する(ただし新規実装は本監査の範囲外)。
- **是正担当Division/Agent:** COO(判断の記録のみ。新規実装は別途Gate1提案が必要)
- **CEO承認が必要か:** 不要(現状維持を記録するだけの場合)。新設する場合は要(新規エージェント追加のため)
- **自動修正可能か:** 部分的(記録の追記のみなら可能。新設は不可)
- **現在の状態:** open
- **依存関係:** なし
- **推奨対応順序:** 8位

---

### ARC-009
- **重要度:** low
- **分類:** 用語・参照方式の混同(監査項目16)
- **対象ファイル:** `_shared/sop/*/*.md`のfrontmatter`related_sop`欄全般
- **対象エージェントまたはSOP:** 全SOPファイル
- **問題の具体的内容:** `related_sop`欄が、ファイル単位のID(例: `ceo-approval-flow`・`monthly-closing`)とカテゴリ単位の名称(例: `management`・`brand-qa`)を、区別する記法なく混在させている。
- **根拠:** 全SOPファイルの`related_sop:`値を`grep`で収集し比較(本監査で実施)。
- **影響:** 実害は無いが、将来SOPが増えた際に「これはファイルを指すのかカテゴリを指すのか」の判断が属人化する。
- **推奨する是正方法:** `docs/naming-conventions.md`または各SOPカテゴリのREADMEに、「カテゴリ全体を指す場合はカテゴリ名、特定の手順を指す場合はそのファイルの`id`を用いる」という表記規則を明記する。
- **是正担当Division/Agent:** COO
- **CEO承認が必要か:** 不要
- **自動修正可能か:** 可能(規則の明文化のみ)
- **現在の状態:** open
- **依存関係:** なし
- **推奨対応順序:** 9位

---

### ARC-010
- **重要度:** info
- **分類:** SSOTの潜在的重複(監査項目7・8)
- **対象ファイル:** `_company/org/finance.yaml`(`brands.status`)・`_company/org/brands.yaml`(`status`)
- **対象エージェントまたはSOP:** `finance-controller`
- **問題の具体的内容:** `finance.yaml`の`active_recording`/`not_recording`と`brands.yaml`の`active`/`planned`が、現状3ブランドすべてで完全に相関している(tomo-angel7: active⇔active_recording、他2ブランド: planned⇔not_recording)。
- **根拠:** 両ファイルを比較(本監査で実施)。
- **影響:** 現状は実害なし。両者は概念的に異なる軸(ブランドの運営状態 vs Finance記録対象か否か)であり、将来「ブランドは稼働中だがFinance記録はまだ」等のズレが生じうるため、意図的に分離された設計と判断する。ただし今後もこの相関が続く場合、片方を他方から導出する設計への統合を検討する余地がある。
- **推奨する是正方法:** 現時点では是正不要。次回四半期監査で相関が崩れていないか(意図的な分離の妥当性)を再確認する。
- **是正担当Division/Agent:** なし(監視のみ)
- **CEO承認が必要か:** 不要
- **自動修正可能か:** 該当なし
- **現在の状態:** accepted(現時点では設計上の意図的分離として許容)
- **依存関係:** なし
- **推奨対応順序:** 10位(次回監査での再確認事項)

---

## 今すぐ直すべき上位5件
1. **ARC-001**(high) ― `_company/kpi/`・`_company/reports/`等、将来の実データ格納先のGit除外ルール未整備
2. **ARC-007**(high) ― LINE/CRM自動配信にCEO承認ゲートが未明記(`line-api`本番接続前に必須)
3. **ARC-002**(high) ― `pain-point-categories.yaml`の退役エージェント参照残留
4. **ARC-003**(high) ― `phase2-instagram-design.md`のステータス欄が実態と乖離
5. **ARC-004**(medium) ― 7件のKnowledge README にわたる同種の退役エージェント参照残留

---

## Architecture Review Division自身の設計上の弱点

1. **単一エージェントへの5観点集約:** `architecture-review-controller`は5つの監査観点(Knowledge/SOP/Agent/命名/重複)を1エージェントに集約しており、`finance-controller`と同様「必要最小限」の判断だが、監査範囲が拡大し続けた場合(Phase11以降でDivisionが増えるたび)、単一責任原則の観点で将来の分割トリガーをまだ定義していない(`finance-controller`には明示的な分割トリガーがあるが、本エージェントには無い)。
2. **監査の実行主体が結局COO(人間相当)である:** 現状すべてのSOPが「Claude Codeが手動で全件走査する」前提であり、実際に自動化されているわけではない。「全件走査」を謳っていても、将来エージェント数・ファイル数が1000規模になった際、人手による全件確認が現実的に維持可能かは未検証。
3. **監査自身のセルフチェックが手薄:** 本報告書自体が「監査対象に含まれるべきファイル」であるにもかかわらず、`architecture-review-controller`・本SOP群自身の命名・重複を本監査内で完全にセルフレビューできているかは、第三者視点が無いため保証が弱い(自己参照の限界)。
4. **是正の優先順位付けロジックが未定義:** 本報告書では推奨対応順序を人力で判断したが、重要度(critical/high/medium/low/info)以外の定量的な優先順位付けアルゴリズム(影響範囲・修正コスト等)は定義されていない。

---

## 関連ドキュメント
- 是正計画: `_company/reports/architecture-review/2026-07-13-architecture-remediation-plan.md`

## 変更履歴
| 版 | 日付 | 変更内容 | 変更者 |
|---|---|---|---|
| 1 | 2026-07-13 | 初回ベースライン監査。CEO確認待ち | architecture-review-controller(実行: COO) |
| 2 | 2026-07-13 | CEO指摘により修正: (1)重要度別件数の再集計(high 3→4、info 2→1、原因を明記)、(2)ARC-001をGit除外領域の分離案に改訂、(3)ARC-007を全社的なprepare/execute区分の再評価に拡大し、Outbound Action Approval Policy新設案を追加。是正計画書を新設し本報告書からリンク | architecture-review-controller(実行: COO) |
| 3 | 2026-07-13 | Batch B(RT-003〜005)実施に伴う再監査。ARC-002・ARC-003・ARC-004の状態を`open`→`resolved`へ更新し、各項目に再監査追記(修正前の状態・検証根拠)を追加。過去の記述は削除していない | architecture-review-controller(実行: COO) |
