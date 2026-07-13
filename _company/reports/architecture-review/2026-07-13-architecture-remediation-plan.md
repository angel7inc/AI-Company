---
report_type: architecture-remediation-plan
source_audit: _company/reports/architecture-review/2026-07-13-architecture-baseline-audit.md
executed_by: architecture-review-controller
status: draft-for-ceo-review
---

# AI Company アーキテクチャ是正計画書(初回ベースライン監査対応)

**策定日:** 2026-07-13
**参照元:** `_company/reports/architecture-review/2026-07-13-architecture-baseline-audit.md`
**制限事項:** 本計画書は是正の**計画のみ**を提示する。実際の是正実行はCEO確認後、各担当Division/Agentへ個別に指示する。

---

## Batch構成の考え方
| Batch | テーマ | 含まれるARC ID |
|---|---|---|
| A | セキュリティ・外部送信・Git除外 | ARC-001, ARC-007 |
| B | 退役済み参照・存在しない参照・旧ステータス | ARC-002, ARC-003, ARC-004 |
| C | 中・低重要度の文書・用語・構造整理 | ARC-005, ARC-006, ARC-008, ARC-009, ARC-010 |

**推奨実施順序:** Batch A → Batch B → Batch C(重要度の高い順、かつBatch Aは本番接続前に完了させる必要があるため最優先)。

---

## Batch A: セキュリティ・外部送信・Git除外

### チケット RT-001(対応: ARC-001)
- **重要度:** high
- **問題:** `_company/kpi/`・`_company/reports/`・`businesses/*/data/`・`businesses/*/reports/`が、実際に機密数値の書き込み先になるにもかかわらず`.gitignore`未対応
- **担当Division:** Management Division(全社設定のためDivision専属ではないが、`core-coo`が管理)
- **担当Agent:** `core-coo`(`.gitignore`変更)、`finance-controller`・`management-kpi`(構造レビュー協力)
- **修正対象ファイル:** `.gitignore`、`_company/kpi/README.md`、`_company/reports/README.md`(新設サブフォルダの説明追記)
- **修正内容:** 監査報告書ARC-001の改訂案どおり、`_company/kpi/{schemas/, actuals/}`・`_company/reports/{templates/, confidential/}`を新設し、`actuals/`・`confidential/`のみ`.gitignore`で除外する。`businesses/*/data/`・`businesses/*/reports/`は実データ発生時に同原則で個別設計する
- **依存関係:** なし
- **受入条件:** (1)`.gitignore`に新規除外ルールが追加されている、(2)`git check-ignore`で`actuals/`・`confidential/`配下のyaml/mdが除外されることを確認できる、(3)`README.md`等の構造ファイルは除外されないことを確認できる、(4)既存の`_company/finance/actuals/`の除外ルールに影響がないこと
- **CEO承認の要否:** 要
- **推奨実施Batch:** A
- **状態:** open
- **修正後にArchitecture Reviewが再確認する項目:** 新設フォルダに機密数値が誤って追跡対象になっていないか、既存Finance実績の除外ルールと重複・矛盾していないか

### チケット RT-002(対応: ARC-007)
- **重要度:** high
- **問題:** WordPress公開(`seo-wordpress-content`/`seo-technical`)・LINE配信(`revenue-crm-manager`)にCEO承認ゲートが未明記。Instagram公開・API/MCP接続には既にあるが、全社で一貫していない
- **担当Division:** Management Division(全社ポリシー新設)、適用対象はRevenue Division・SEO Division
- **担当Agent:** `core-coo`(ポリシー新設)、`revenue-crm-manager`・`seo-wordpress-content`・`seo-technical`(自身の定義に参照リンクを追記)
- **修正対象ファイル:** `_company/charter/outbound-action-policy.md`(新設)、`_shared/agents/revenue/revenue-crm-manager.md`、`businesses/seo/agents/seo-wordpress-content.md`、`businesses/seo/agents/seo-technical.md`、`_shared/sop/publishing/README.md`(SEOへの関連付け追記)
- **修正内容:** 監査報告書ARC-007の「案B」どおり、prepare/executeの定義とexecute判定基準を一元管理する全社ポリシーを新設し、既存`sop/publishing/`・`sop/automation/`の承認要件をその具体例として整理し直す。各対象エージェントの役割定義に、本ポリシーへの参照リンクのみを追記する
- **依存関係:** RT-001と独立して実施可能。ただし`mcp/api-registry.yaml`の`line-api`・`wordpress-api`が`active`化される前に完了させる必要がある
- **受入条件:** (1)新設ポリシーにprepare/execute定義とexecute対象行為の一覧が明記されている、(2)`revenue-crm-manager`・`seo-wordpress-content`・`seo-technical`がポリシーを参照している、(3)承認基準の文言が複数ファイルに重複記載されていない
- **CEO承認の要否:** 要
- **推奨実施Batch:** A
- **状態:** open
- **修正後にArchitecture Reviewが再確認する項目:** 新ポリシーが既存`ai-usage-policy.md`と同じ「参照のみ・複製しない」パターンを守れているか、他の未確認チャネル(広告出稿・予約決済等)にも同ポリシーが適用可能な汎用性を持っているか

---

## Batch B: 退役済み参照・存在しない参照・旧ステータス

### チケット RT-003(対応: ARC-002)
- **重要度:** high
- **問題:** `pain-point-categories.yaml`(6・28行目)が退役済み`ig-research`を現役の検証担当として記載
- **担当Division:** Product Division
- **担当Agent:** `product-service-designer`(またはCOOの軽微修正)
- **修正対象ファイル:** `_company/brands/tomo-angel7/pain-point-categories.yaml`
- **修正内容:** 6・28行目の`ig-research`を`intelligence-consumer`に置換
- **依存関係:** RT-004・RT-005と同種の修正のため同時実施を推奨
- **受入条件:** ファイル内に`ig-research`の文字列が残存しないこと
- **CEO承認の要否:** 不要(軽微修正)
- **推奨実施Batch:** B
- **状態:** resolved(2026-07-13 実施・検証済み。詳細は本書末尾「Batch B 再監査結果」参照)
- **修正後にArchitecture Reviewが再確認する項目:** 他のブランド固有ファイル(将来のtomo-academy等)に同種の記載が発生していないか

### チケット RT-004(対応: ARC-003)
- **重要度:** high
- **問題:** `docs/phase2-instagram-design.md`のステータス欄が「CEO承認待ち・未実装」のままで、実態(後続Phaseにより大部分が置換・退役済み)と乖離
- **担当Division:** Management Division(設計書管理はCOO)
- **担当Agent:** `core-coo`
- **修正対象ファイル:** `docs/phase2-instagram-design.md`
- **修正内容:** ステータス欄に「一部内容は後続Phase(3・3b・7・8)により置換・退役済み。現在の正本は各後継設計書を参照」という上書き注記を追加(Phase5設計書と同じ方式)
- **依存関係:** なし
- **受入条件:** ステータス欄に上書き注記が追加されており、後継設計書(phase3/phase3b/phase7/phase8)へのリンクが明記されていること
- **CEO承認の要否:** 要(5-part形式の軽微確認: 変更対象/目的/メリット/デメリット/既存設計への影響)
- **推奨実施Batch:** B
- **状態:** resolved(2026-07-13 実施・検証済み。詳細は本書末尾「Batch B 再監査結果」参照)
- **修正後にArchitecture Reviewが再確認する項目:** 他の古いPhase設計書(phase3・phase4等)にも同様の「実態と乖離したステータス」が無いか、次回監査で全Phase設計書を横断確認する

### チケット RT-005(対応: ARC-004)
- **重要度:** medium
- **問題:** 7件のKnowledgeカテゴリREADME(gemini/instagram/perplexity/pinterest/reddit/seo/youtube)が退役済み`ig-research`を参照
- **担当Division:** COO横断対応(Knowledge Layer管理者不在のため)
- **担当Agent:** COO
- **修正対象ファイル:** `_shared/knowledge/{gemini,instagram,perplexity,pinterest,reddit,seo,youtube}/README.md`
- **修正内容:** 各READMEの「誰が参照するか」欄の`ig-research`を、カテゴリに応じて`intelligence-consumer`・`intelligence-market`・`intelligence-trend`等の適切な後継エージェントに置換
- **依存関係:** RT-003と同時実施を推奨(同一原因のため)
- **受入条件:** 7ファイルすべてで`ig-research`の文字列が残存しないこと
- **CEO承認の要否:** 不要(軽微修正)
- **推奨実施Batch:** B
- **状態:** resolved(2026-07-13 実施・検証済み。詳細は本書末尾「Batch B 再監査結果」参照)
- **修正後にArchitecture Reviewが再確認する項目:** 置換先のエージェント名がそのカテゴリの実際の利用実態と一致しているか(機械的な一括置換による誤りがないか)

---

## Batch C: 中・低重要度の文書・用語・構造整理

### チケット RT-006(対応: ARC-005)
- **重要度:** medium
- **問題:** `sop-index.md`の「SOP一覧」表が「まだSOPがありません」のままだが、実際は14件のSOPが存在
- **担当Division:** COO横断対応
- **担当Agent:** COO(各SOPのownerと確認)
- **修正対象ファイル:** `_shared/sop/sop-index.md`
- **修正内容:** 14件のSOP(id/タイトル/status/owner/frequency/依存SOP/関連Knowledge)を「SOP一覧」表へ追記
- **依存関係:** なし
- **受入条件:** 表に14行すべてが記載され、「まだSOPがありません」の文言が削除されていること
- **CEO承認の要否:** 不要
- **推奨実施Batch:** C
- **状態:** open
- **修正後にArchitecture Reviewが再確認する項目:** 今後SOP追加のたびに本表が更新される運用が徹底されているか(次回監査で再発有無を確認)

### チケット RT-007(対応: ARC-006)
- **重要度:** medium
- **問題:** QA/監査系9エージェント中8体に「してはいけないこと(非実行境界)」の明示的セクションが無い
- **担当Division:** Management/Creative/SEO/Product/Intelligence各Division(対象エージェントの所属先)
- **担当Agent:** 各Division担当、COOが統一フォーマットを横断適用
- **修正対象ファイル:** `_shared/agents/analytics/shared-brand-quality-checker.md`、`_shared/agents/design/shared-image-quality-checker.md`、`_shared/agents/design/shared-video-quality-checker.md`、`businesses/seo/agents/seo-quality-checker.md`、`businesses/seo/agents/seo-eeat-checker.md`、`_company/agents/management-quality.md`、`_shared/agents/product/product-quality-standards.md`、`_shared/agents/intelligence/intelligence-research-quality-checker.md`
- **修正内容:** 各ファイルに`architecture-review-controller.md`と同様の「してはいけないこと」セクションを追加し、判定・スコアリングのみで対象の修正は行わない旨を明記
- **依存関係:** なし
- **受入条件:** 8ファイルすべてに「してはいけないこと」セクションが存在すること
- **CEO承認の要否:** 不要(既存役割の明文化)
- **推奨実施Batch:** C
- **状態:** open
- **修正後にArchitecture Reviewが再確認する項目:** 将来新設されるQA/監査系エージェントが、テンプレート段階からこのセクションを含む運用になっているか

### チケット RT-008(対応: ARC-008)
- **重要度:** low
- **問題:** `_shared/agents/copywriting/`にPhase1以来、共有エージェントが一度も配置されていない(design/等の兄弟フォルダとの非対称)
- **担当Division:** COO(判断の記録のみ)
- **担当Agent:** COO
- **修正対象ファイル:** `_shared/agents/README.md`
- **修正内容:** 「コピーライティングはチャネル固有性が強く共有方法論エージェントは現時点で不要」という判断を明記するか、将来必要と判断する場合は別途Gate1提案を行う旨を記録
- **依存関係:** なし(新設する場合のみ別途Gate1が必要)
- **受入条件:** `_shared/agents/README.md`に本件の判断根拠が記載されていること
- **CEO承認の要否:** 不要(現状維持の記録のみの場合)。新設する場合は要
- **推奨実施Batch:** C
- **状態:** open
- **修正後にArchitecture Reviewが再確認する項目:** 次回監査時点で`copywriting/`が実際に空のままか、新設判断がなされたか

### チケット RT-009(対応: ARC-009)
- **重要度:** low
- **問題:** `related_sop`欄がファイル単位ID とカテゴリ単位名称を無区別に混在させている
- **担当Division:** COO
- **担当Agent:** COO
- **修正対象ファイル:** `docs/naming-conventions.md`
- **修正内容:** 「カテゴリ全体を指す場合はカテゴリ名、特定の手順を指す場合はそのファイルの`id`を用いる」という表記規則を追記
- **依存関係:** なし
- **受入条件:** `docs/naming-conventions.md`に本規則が明記されていること(既存の`related_sop`値の遡及修正は任意)
- **CEO承認の要否:** 不要
- **推奨実施Batch:** C
- **状態:** open
- **修正後にArchitecture Reviewが再確認する項目:** 新規SOP作成時にこの規則が実際に守られているか

### チケット RT-010(対応: ARC-010)
- **重要度:** info
- **問題:** `finance.yaml`の`active_recording`/`not_recording`と`brands.yaml`の`active`/`planned`が3ブランドすべてで完全相関
- **担当Division:** なし(監視のみ)
- **担当Agent:** `architecture-review-controller`(次回監査で再確認)
- **修正対象ファイル:** なし(今回是正不要)
- **修正内容:** なし。次回四半期監査で相関の継続有無を再確認する
- **依存関係:** なし
- **受入条件:** 該当なし(監視のみのため)
- **CEO承認の要否:** 不要
- **推奨実施Batch:** C(次回監査サイクルでの再確認事項として繰越)
- **状態:** accepted(是正不要と判断)
- **修正後にArchitecture Reviewが再確認する項目:** 次回監査時点でも相関が続いているか、ブランド追加時に相関が崩れる実例が出ていないか

---

## サマリー
| Batch | チケット数 | CEO承認必須 | 自動修正可能 |
|---|---|---|---|
| A | 2(RT-001, RT-002) | 2/2 | 一部(構造作成は可、基準決定は不可) |
| B | 3(RT-003〜005) | 1/3(RT-004のみ) | 3/3(RT-004は注記追加のみ) |
| C | 5(RT-006〜010) | 0/5 | 4/5(RT-008は新設判断時のみ要) |
| **合計** | **10** | **3/10** | ― |

**Batch A・B実施状況(2026-07-13時点):** Batch A(RT-001・RT-002)コミット済み(`e9cf122`)・push済み。Batch B(RT-003〜005)は本改訂で`resolved`へ更新(下記「Batch B 再監査結果」参照)。Batch C・HR Division・Tomo Academyは未着手。

---

## Batch B 再監査結果(2026-07-13実施)

**再監査実行:** `architecture-review-controller`(検出・検証のみ。是正実行はCOOが個別に実施)
**制限事項:** 過去の監査内容(上記ARC-002〜004・RT-003〜005の原文)は削除せず、本節に再監査日・検証根拠・修正前の状態を追記する形式で記録する。

### RT-003 再監査
- **修正前の状態:** `pain-point-categories.yaml` 6・28行目に`ig-research`(退役済み)が現役の検証担当として記載
- **検証根拠:** (1)`intelligence-consumer`が`_company/org/agents.yaml`615行目に登録済み、status: draft、division: intelligence。(2)definitionファイル`_shared/agents/intelligence/intelligence-consumer.md`の実在を確認。(3)役割記述「悩み・ペルソナ・行動分析」「`pain-point-categories.yaml`の`status`更新提案(draft→validated)」を確認し、対象ファイルとの整合性が取れている。(4)`grep`により、修正後のファイル内に`ig-research`の文字列が残存しないことを確認(初回置換時に「旧ig-research計画を上位互換」という補足文言で文字列が再混入していたため、「旧研究エージェント計画を上位互換」に再修正し、完全除去を確認)。(5)YAML構造(`pain_point_categories`配下の3カテゴリ・`status: draft`・`created_at`・`updated_at`・`version`)は無変更であることを`git diff`で確認
- **status:** resolved

### RT-004 再監査
- **修正前の状態:** `docs/phase2-instagram-design.md` 3行目のステータスが「設計フェーズ ― CEO承認待ち(未実装)」のまま
- **検証根拠:** (1)ステータスを「実装済み・一部後続Phaseで置換(implemented with superseding changes)」に更新し、実態(Instagram事業は稼働中、一部内容は後続Phaseで置換)と一致させた。(2)`phase5-revenue-division-design.md`40行目・`phase8b-finance-division-design.md`29行目の既存「上書き注記」形式(blockquote)を踏襲した注記を追加。(3)注記内で参照した`docs/phase3-search-seo-division-design.md`・`phase3b-creative-division-design.md`・`phase5-revenue-division-design.md`・`phase7-intelligence-division-design.md`・`phase8-product-division-design.md`・`docs/roadmap.md`の実在をすべて`test -f`で確認済み(**CEOご指示の「Phase8a」は実際のファイル名として存在しないため、実在する`docs/phase8-product-division-design.md`(Product Division新設)に置き換えて参照した。**意図が異なる場合はご指摘ください)。(4)`git diff docs/phase2-instagram-design.md`で、ステータス行の更新と注記1段落の追加(3行追加・1行削除)のみであることを確認し、本文(00章以降)が削除・改変されていないことを確認
- **status:** resolved

### RT-005 再監査
- **修正前の状態:** 7件のKnowledge README(gemini/instagram/perplexity/pinterest/reddit/seo/youtube)の「誰が参照するか」欄に`ig-research`が残存
- **検証根拠(ファイル別):**
  - `gemini/README.md`→`intelligence-consumer`: `agents.yaml`615-628行目でtools「Gemini Deep Research」を保持することを確認。**高確信度**
  - `perplexity/README.md`→`intelligence-consumer`: `agents.yaml`同箇所でtools「Perplexity」を保持することを確認。**高確信度**
  - `reddit/README.md`→`shared-traffic-intelligence`: `agents.yaml`285-298行目の役割記述に「Reddit」が明記されていることを確認。**高確信度**
  - `youtube/README.md`→`shared-competitor-intelligence`: `agents.yaml`270-283行目の役割記述に「YouTube」が明記され、「競合の変化を検知する」がKnowledge側の「競合動画調査の観点」と一致することを確認。**高確信度**
  - `instagram/README.md`・`pinterest/README.md`・`seo/README.md`: 複数の候補エージェント(`intelligence-consumer`/`shared-traffic-intelligence`/`shared-competitor-intelligence`)がいずれも部分的にしか一致せず、単一エージェントへの断定に十分な根拠が得られなかったため、**推測による置換は行わず**、CEO指定の代替表現「当該Knowledgeを使用するIntelligence関連エージェント」を採用した(`ig-strategy`・`ig-content-planning`等、当該箇所以外の既存記載は変更していない)
  - 7ファイル全てで`grep`により`ig-research`の文字列が残存しないことを確認済み
- **status:** resolved(7/7ファイル対応済み。うち4件は特定の後継エージェント名へ、3件はCEO承認済みの役割ベース表現へ置換。根拠不足のまま推測で特定エージェント名を割り当てた箇所は無し)

---

## 追加発見事項(Batch B実施中に判明・次回CEO承認対象の候補)

以下は今回のBatch B(RT-003〜005)の対象範囲外のため修正していません。既存のARC ID・RT IDとは別の新規IDを付与し、次回監査での正式な指摘化・CEO承認を経てから是正します。

### ARC-011(候補)
- **発見ID:** ARC-011
- **対象ファイル数:** 約12ファイル
- **代表的な対象ファイル:** `docs/phase2-instagram-design.md`、`_company/agents/management-strategy.md`、`_company/brands/tomo-angel7/brand-brief.md`、`_shared/knowledge/{ai,branding,chatgpt,gemini,instagram,marketing,psychology}/README.md`
- **問題内容:** `ig-strategy`という名称が`_company/org/agents.yaml`に一度も登録されていないにもかかわらず、Phase2設計書由来の名称として複数ファイルに残存している。エージェントとして実装されなかったのか、別名で実装されたのかが未確認
- **想定される正式名称または後継:** 未確定。`management-strategy`(全社戦略)または将来のInstagram専用戦略エージェントのいずれかが該当する可能性があるが、本調査では特定に至らず
- **全件調査が必要:** 要(12ファイルの前後文脈を確認し、`ig-strategy`が指していた機能が現在どのエージェントに帰属しているかを特定する必要がある)
- **重要度案:** medium(実害は限定的だが、ARC-002〜004と同種の「存在しない参照」であるため)
- **状態:** open(候補。次回監査での正式ARC化待ち)
- **次回CEO承認対象:** 要(正式な是正チケット化の際に承認を得る)

### ARC-012(候補)
- **発見ID:** ARC-012
- **対象ファイル数:** 約16ファイル
- **代表的な対象ファイル:** `businesses/instagram/agents/ig-carousel-intelligence.md`、`businesses/instagram/agents/ig-creative-team.md`、`docs/phase7-intelligence-division-design.md`、`_shared/agents/intelligence/intelligence-insight-synthesizer.md`、`_shared/knowledge/{canva,chatgpt,copywriting,design,instagram,pinterest,psychology}/README.md`、`_shared/sop/{design,instagram,research}/`配下
- **問題内容:** `agents.yaml`上の正式ID`ig-content-planner`に対し、16ファイルで`ig-content-planning`という表記(末尾が異なる)が使われている。誤字による表記揺れか、「役割・機能カテゴリ名」と「エージェントID」を意図的に使い分けているのかが未確認
- **想定される正式名称または後継:** `ig-content-planner`(`agents.yaml`90行目に登録済み、definitionファイル`businesses/instagram/agents/ig-content-planner.md`実在確認済み)。ただし16ファイル全件が単純な誤字か、意図的な使い分けかの判断が先に必要
- **全件調査が必要:** 要(表記揺れか意図的区分かをファイルごとに確認したうえで、統一するか、区別する表記規則(ARC-009の`related_sop`表記規則と同種の対応)を定めるかを判断する必要がある)
- **重要度案:** low(実行時の参照エラーにはならず、可読性・将来の混乱リスクの問題であるため)
- **状態:** open(候補。次回監査での正式ARC化待ち)
- **次回CEO承認対象:** 要(正式な是正チケット化の際に承認を得る)

---

## 変更履歴
| 版 | 日付 | 変更内容 | 変更者 |
|---|---|---|---|
| 1 | 2026-07-13 | 初版作成。CEO確認待ち | architecture-review-controller(実行: COO) |
| 2 | 2026-07-13 | Batch B(RT-003〜005)実施。各チケットのstatusを`resolved`へ更新し「Batch B 再監査結果」を追記。実施中に判明した`ig-strategy`・`ig-content-planning`表記問題をARC-011・ARC-012(候補)として追記 | architecture-review-controller(実行: COO) |
