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
- **状態:** open
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
- **状態:** open
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
- **状態:** open
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

## 変更履歴
| 版 | 日付 | 変更内容 | 変更者 |
|---|---|---|---|
| 1 | 2026-07-13 | 初版作成。CEO確認待ち | architecture-review-controller(実行: COO) |
