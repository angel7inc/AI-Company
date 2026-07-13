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
- **状態:** resolved(2026-07-13実施・検証済み。詳細は本書末尾「Batch C 再監査結果」参照)
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
- **状態:** resolved(2026-07-13実施・検証済み。詳細は本書末尾「Batch C 再監査結果」参照)
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
- **状態:** resolved(2026-07-13実施・検証済み。詳細は本書末尾「Batch C 再監査結果」参照)
- **修正後にArchitecture Reviewが再確認する項目:** 次回監査時点で`copywriting/`が実際に空のままか、新設判断がなされたか

### チケット RT-009(対応: ARC-009)
- **重要度:** low
- **問題:** `related_sop`欄がファイル単位ID とカテゴリ単位名称を無区別に混在させている
- **担当Division:** COO
- **担当Agent:** COO
- **修正対象ファイル:** `docs/naming-conventions.md`
- **修正内容:** 「カテゴリ全体を指す場合は`related_sop_categories`、特定の手順を指す場合は`related_sop_ids`を用いる」というフィールド分離規則を`docs/naming-conventions.md`・`sop-template.md`へ追記し、既存14件のSOPも移行する
- **依存関係:** なし
- **受入条件:** `docs/naming-conventions.md`・`sop-template.md`に本規則が明記され、既存SOPの`related_sop`値のうち意味が明確なものは新フィールドへ移行されていること。曖昧な値が残る場合はresolvedにせずpartially resolvedとする
- **CEO承認の要否:** 不要
- **推奨実施Batch:** C
- **状態:** resolved(2026-07-13実施・検証済み。既存14件全てで意味が明確に分類でき、曖昧値は0件だったため完全移行。詳細は本書末尾「Batch C 再監査結果」参照)
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
| 新規(別Batch) | 3(RT-011〜013。RT-011は実施済み) | 1/3(RT-013のみ) | 2/3(RT-013は分割等の設計判断のため不可) |
| **合計** | **13** | **4/13** | ― |

**Batch A・B・C実施状況(2026-07-13時点):** Batch A(RT-001・RT-002)コミット済み(`e9cf122`)・push済み。Batch B(RT-003〜005)コミット済み(`9ec3acf`)・push済み。Batch C(RT-006〜009)は本改訂で`resolved`へ更新、RT-010は`accepted`のまま(下記「Batch C 再監査結果」参照)。ARC-011・012・013を正式採番し、RT-011を今回実施、RT-012・RT-013は別Batchへ繰越。HR Division・Tomo Academyは未着手。

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

## Batch C 再監査結果(2026-07-13実施)

**再監査実行:** `architecture-review-controller`(検出・検証のみ。是正実行はCOOが個別に実施)

### RT-006 再監査
- **修正前の状態:** `sop-index.md`の「SOP一覧」表が「(まだSOPがありません)」のまま
- **検証根拠:** 実在確認済みの14件(`sop-template.md`を除く)全てを、実行順・カテゴリ・id・タイトル・Status・Owner・更新頻度・依存SOP・関連Knowledge・定義ファイルパスの10列で追記。「まだSOPがありません」の文言を削除。件数(14件)が2026-07-13時点の確認件数であり長期固定値でないこと、SOP新設・退役・改名時は同じ変更単位で本表を更新する保守ルールを明記
- **status:** resolved

### RT-007 再監査
- **修正前の状態:** QA/監査系9エージェント中8体に「してはいけないこと」セクションが無い
- **検証根拠:** 対象8ファイル(`shared-brand-quality-checker`・`shared-image-quality-checker`・`shared-video-quality-checker`・`seo-quality-checker`・`seo-eeat-checker`・`management-quality`・`product-quality-standards`・`intelligence-research-quality-checker`)全てに「してはいけないこと」セクションを追加。各セクションは(1)判定対象・エージェント定義・設定・データを自ら修正しない、(2)公開・送信・配信・外部API書き込み等のexecuteを行わない、(3)合否判定・スコアリング・問題検出・根拠提示・改善提案・報告のみ行う、(4)修正は担当Division・Agentへ差し戻す、(5)CEO承認があっても本エージェント自身が修正担当に変わらない、の5点を明記。監査報告書・評価結果・改善提案の作成自体は明示的に許容(禁止していない)。対象8ファイル以外への横展開なし(`grep`で追加後の該当セクション数8件を確認)。なお`product-quality-standards`は既存の`products.yaml`メトリクス更新という正規出力を損なわないよう文言を調整
- **status:** resolved

### RT-008 再監査
- **修正前の状態:** `_shared/agents/copywriting/`が空である理由の記録が無い
- **検証根拠:** `_shared/agents/README.md`に「コピーライティングは現時点ではチャネル固有エージェントが担当」「共有copywritingエージェントは役割重複回避のため現時点で未配置」「重複が判明した場合は別途Gate1」「フォルダの存在は欠落・故障を意味しない」という判断根拠を記載。新規エージェントは作成していない
- **status:** resolved

### RT-009 再監査
- **修正前の状態:** `related_sop`欄がファイル単位IDとカテゴリ単位名称を無区別に混在
- **検証根拠:** `docs/naming-conventions.md`に`related_sop_ids`(特定ファイル)・`related_sop_categories`(カテゴリ全体)への分離規則を追記。`_shared/sop/sop-template.md`の frontmatter も同様に分離。既存14件のSOP全件の`related_sop`値を確認した結果、全ての値が「実在するSOP id」または「実在するSOPカテゴリフォルダ名(`_shared/sop/{category}/`)」のいずれかに明確に分類可能であることを`ls`・`grep`で確認し、**曖昧な値は0件だったため全14件を新フィールドへ完全移行**。旧`related_sop:`フィールドの残存が無いことを確認済み
- **status:** resolved(曖昧値なし。partially resolvedへの該当なし)

---

## 正式指摘(ARC-011・ARC-012・ARC-013)

### ARC-011(正式採番)
- **発見ID:** ARC-011
- **分類:** 旧判断の誤読リスク/未実装機能の誤表示
- **重要度:** medium
- **対象ファイル数:** 13ファイル(全件調査で確定。うち現在の参照として問題があるのは8ファイル)
- **全出現箇所:** `docs/phase2-instagram-design.md`(8箇所、歴史的記述として保護対象)、`docs/phase4-management-division-design.md`:49(既に「phase2計画、未実装」と明記済み、問題なし)、`_company/agents/management-strategy.md`:10(既に「将来の」と明記済み、問題なし)、`_company/brands/tomo-angel7/brand-brief.md`:114(現在参照として問題あり)、`_shared/knowledge/{ai,branding,chatgpt,gemini,instagram,marketing,psychology}/README.md`(計7ファイル、現在参照として問題あり)、監査報告書・是正計画書(自己言及、対象外)
- **歴史的記述か現在の参照か:** 混在。問題があるのは`brand-brief.md`1件+Knowledge README 7件の計8ファイルのみ
- **agents.yamlへの登録:** 無し(確認済み)
- **後継エージェント:** **存在しない。** Phase2原案の`ig-strategy`(Instagram月次アカウント戦略・KPI目標設定)は、`intelligence-consumer`等のように退役・置換されたのではなく、機能自体が現時点で誰も担っていない未実装領域
- **単純置換してよい箇所:** 無し(後継が存在しないため推測での単一エージェント置換は行わない)
- **過去の設計記録として残す箇所:** `docs/phase2-instagram-design.md`の8箇所全て
- **CEO判断(2026-07-13):** (1)`ig-strategy`の新エージェントは現時点で作成しない、(2)`management-strategy`等の既存エージェントへ無理に割り当てない、(3)Instagram固有の戦略機能は「未実装・担当未割当」として扱う、(4)必要性を将来再評価してからエージェント新設または役割配分を行う
- **状態:** open
- **対応する是正チケット:** RT-012(別Batch。今回未実施)

### ARC-012(正式採番)
- **発見ID:** ARC-012
- **分類:** 存在しない参照(監査項目17)
- **重要度:** **medium**(CEO判定により、当初案のlowから引き上げ。理由: (1)現役エージェント定義が、存在しないエージェント名を連携先として参照している、(2)正式IDはagents.yaml登録済みの`ig-content-planner`で確定している、(3)将来のエージェント実行や連携判断を誤らせる可能性がある)
- **対象ファイル数:** 17ファイル(全件調査で確定。うち現在有効な参照として修正が必要なのは14ファイル)
- **各表記の出現ファイル数:** `ig-content-planning` 17ファイル、正式ID`ig-content-planner` 12ファイル
- **正式IDの根拠:** `_company/org/agents.yaml`90行目に`id: ig-content-planner`・`status: active`・`definition: businesses/instagram/agents/ig-content-planner.md`として登録済み(definitionファイル実在確認済み)。`docs/naming-conventions.md`8行目の命名規則例としても採用
- **歴史的記述か現在の参照か:** `docs/phase2-instagram-design.md`(6箇所)は、Phase2が「`ig-content-planner`を`ig-content-planning`に改名する計画」だったことを示す歴史的原本(改名は実行されなかった)。監査報告書・是正計画書(自己言及)を除く14ファイルは現在有効な参照であり、稼働中エージェント自身の定義ファイルが誤った連携先名を使用している実質的なバグ
- **状態:** resolved(RT-011として今回実施・検証済み。詳細は下記参照)
- **対応する是正チケット:** RT-011(今回実施)

### ARC-013(正式採番)
- **発見ID:** ARC-013
- **タイトル:** Architecture Review Divisionの拡張性・独立性・優先順位付け不足
- **重要度:** medium
- **内容:**
  1. 1エージェント(`architecture-review-controller`)に複数監査観点(Knowledge/SOP/Agent/命名/重複)が集中している
  2. Division/Agent数が増加した場合の分割トリガーが未定義(`finance-controller`にはある明示的な分割トリガーが本エージェントには無い)
  3. 全件走査が「Claude Codeによる手動実行」を前提としており、自動化されていない
  4. 自己監査(本エージェント自身・本SOP群の命名・重複)の独立性が弱い(第三者視点が無い)
  5. severity以外の定量的な優先順位付け基準(影響範囲・修正コスト等)が未定義
- **RT-007との関係:** RT-007は他のQA/監査系8エージェントへ非実行境界を横展開する内容であり、`architecture-review-controller`自身の上記5項目の構造的弱点は解消しない
- **状態:** open
- **対応する是正チケット:** RT-013(別Batch。今回未実施)

---

## 新規是正チケット(RT-011・RT-012・RT-013)

### チケット RT-011(対応: ARC-012)
- **重要度:** medium
- **問題:** `ig-content-planning`という未実装名称が、稼働中エージェントの定義ファイルを含む14ファイルで正式ID`ig-content-planner`の代わりに使用されている
- **担当Division:** COO横断対応
- **担当Agent:** COO
- **修正対象ファイル(14件、今回実施):** `businesses/instagram/agents/ig-carousel-intelligence.md`、`businesses/instagram/agents/ig-creative-team.md`、`docs/phase7-intelligence-division-design.md`、`_shared/agents/intelligence/intelligence-insight-synthesizer.md`、`_shared/knowledge/canva/README.md`、`_shared/knowledge/chatgpt/README.md`、`_shared/knowledge/copywriting/README.md`、`_shared/knowledge/design/README.md`、`_shared/knowledge/instagram/README.md`、`_shared/knowledge/pinterest/README.md`、`_shared/knowledge/psychology/README.md`、`_shared/sop/design/README.md`、`_shared/sop/instagram/README.md`、`_shared/sop/research/insight-synthesis.md`
- **対象外(変更しない):** `docs/phase2-instagram-design.md`の歴史的記述、監査報告書・是正計画書内の自己言及部分
- **修正内容:** `ig-content-planning`を`ig-content-planner`へ置換。`docs/phase7-intelligence-division-design.md`は置換後の文意を目視確認
- **依存関係:** なし
- **受入条件:** (1)14ファイルに`ig-content-planning`が残っていない、(2)`ig-content-planner`が`agents.yaml`に存在する、(3)definitionファイルが存在する、(4)各文章の意味が置換後も成立する、(5)歴史的記述が保持されている
- **CEO承認の要否:** 不要(軽微修正)
- **推奨実施Batch:** 今回実施済み
- **状態:** resolved(2026-07-13実施・検証済み。14ファイル全てで`ig-content-planning`の残存なしを`grep`で確認。`docs/phase7-intelligence-division-design.md`の置換後の文「→ ig-content-planner / ig-carousel-intelligence(Instagram)」は文意が成立することを目視確認。`docs/phase2-instagram-design.md`の歴史的記述・監査報告書/是正計画書の自己言及部分は無変更)
- **修正後にArchitecture Reviewが再確認する項目:** 新規Instagram関連ファイル作成時に、正式ID`ig-content-planner`が一貫して使用されているか

### チケット RT-012(対応: ARC-011)
- **重要度:** medium
- **問題:** `ig-strategy`という未登録・未実装の名称が、現在の参照として8ファイルに残存
- **担当Division:** COO
- **担当Agent:** COO
- **修正対象ファイル(将来実施):** `_company/brands/tomo-angel7/brand-brief.md`(現在参照1件)、`_shared/knowledge/{ai,branding,chatgpt,gemini,instagram,marketing,psychology}/README.md`(現在参照7件)
- **対象外(変更しない):** `docs/phase2-instagram-design.md`等の歴史的記述、`docs/phase4-management-division-design.md`・`_company/agents/management-strategy.md`(既に「未実装」と正しく明記済み)
- **修正方針(CEO承認済み、実装は別Batch):** 現在稼働中のエージェントとして記載せず、「Instagram固有戦略機能: planned / unassigned」または既存文書形式に合う同義表現へ変更する
- **依存関係:** なし
- **受入条件:** 8ファイル全てで、`ig-strategy`が現役エージェントであるかのような記載が無くなっていること
- **CEO承認の要否:** 不要(表現修正のみ。ただし修正方針自体は今回のCEO判断により確定済み)
- **推奨実施Batch:** 別Batch(今回未実施)
- **状態:** open
- **修正後にArchitecture Reviewが再確認する項目:** Instagram戦略機能の必要性が将来再評価された際、本ファイル群が新しい担当エージェント名へ正しく更新されるか

### チケット RT-013(対応: ARC-013)
- **重要度:** medium
- **問題:** Architecture Review Division自身の拡張性・独立性・優先順位付けの構造的弱点(ARC-013参照)
- **担当Division:** COO(Architecture Review Division自身の設計変更のため、通常の是正実行とは性質が異なる)
- **担当Agent:** 未定(分割する場合は新規Agent検討が必要)
- **修正対象ファイル(将来検討):** `_company/agents/architecture-review-controller.md`、`docs/phase10-architecture-review-division-design.md`、関連SOP(`_shared/sop/architecture-review/`)
- **修正内容(将来検討、今回未実装):** 分割トリガーの明文化、優先順位付けロジックの定義、自己監査の独立性確保方法の検討。**今回はArchitecture Reviewエージェントの分割・自動化・第三者監査エージェントの新設は行わない**
- **依存関係:** なし
- **受入条件:** 未定(次回のGate1相当の設計提案時に定義)
- **CEO承認の要否:** 要(Architecture Review Division自身の構造変更のため)
- **推奨実施Batch:** 別Batch(今回未実施)
- **状態:** open
- **修正後にArchitecture Reviewが再確認する項目:** 該当なし(現時点で未実装のため)

---

## 変更履歴
| 版 | 日付 | 変更内容 | 変更者 |
|---|---|---|---|
| 1 | 2026-07-13 | 初版作成。CEO確認待ち | architecture-review-controller(実行: COO) |
| 2 | 2026-07-13 | Batch B(RT-003〜005)実施。各チケットのstatusを`resolved`へ更新し「Batch B 再監査結果」を追記。実施中に判明した`ig-strategy`・`ig-content-planning`表記問題をARC-011・ARC-012(候補)として追記 | architecture-review-controller(実行: COO) |
| 3 | 2026-07-13 | Batch C(RT-006〜009)実施(「案Aプラス」)。各チケットのstatusを`resolved`へ更新し「Batch C 再監査結果」を追記。RT-010は`accepted`のまま維持。ARC-011・ARC-012・ARC-013を正式採番し、新規チケットRT-011(今回実施・resolved)・RT-012(別Batch・open)・RT-013(別Batch・open)を発行 | architecture-review-controller(実行: COO) |
