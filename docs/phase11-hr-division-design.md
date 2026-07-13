# Phase11設計書 ― HR Division 新設

**ステータス:** 設計完了・CEO承認待ち(未実装。ファイル作成・エージェント追加・設定変更は行っていません)
**策定日:** 2026-07-13
**対象:** AIエージェントのライフサイクル管理(登録・配置・オンボーディング・評価・異動・休止・退役・アクセス権停止)。Human Workforceは将来拡張の設計方針のみ

---

## 00. 本書の位置づけ
`docs/phase10-architecture-review-division-design.md`に続く設計書。Architecture Reviewが「検出・報告」を担う一方、検出された問題(退役済みエージェントへの参照残留等)への意思決定・実行の受け皿が存在しなかった空白を埋める。

## 01. 既存設計との重複チェック
| # | 検出内容 | 判定 |
|---|---|---|
| 1 | `core-coo`が既に「Agent管理・タスク配分・進捗管理」を担う | 重複ではない。`core-coo`はCEOへの最終確認窓口として残り、HRはライフサイクル記録・オンボーディング・退役手順という実務を正式な手順として引き継ぐ |
| 2 | `management-resource`が既にコスト・トークンを監視 | 重複ではない。金額軸はmanagement-resourceのまま、HRは役割スコープという構造的観点のみ |
| 3 | `management-quality`が既にQAチェッカーの合格率メタ監査を実施 | 重複ではない。対象がQAチェッカーに限定される既存範囲はそのまま、HRの評価はより広いエージェント全般のライフサイクル判断に限定する |
| 4 | `architecture-review-controller`が既に退役済み参照等を検出 | 重複ではない。検出のみという既存の非実行境界を維持し、意思決定と実行はHR+Managementの承認フローが担う |

**結論:** 新しい実行ロジックを増やさず、既存の「Agent管理」という未整理領域を正式化する。

## 02. エージェント構成(1体、最小構成)
| エージェントID | 置き場所 | 役割概要 |
|---|---|---|
| `hr-workforce-manager` | `_company/agents/`(division: hr) | AIエージェントのライフサイクル記録・オンボーディング手順実行・退役手順実行。能力評価基準そのものは定義せず、各Divisionの既存基準を尊重する |

**分割しない理由:** 現時点でHuman Workforceが存在せず、評価・オンボーディング量も少ないため1エージェントに統合する(`finance-controller`・`architecture-review-controller`と同じ判断)。
**将来の分割トリガー:** (1)Human Workforceのオンボーディングが実際に始まった時点、(2)AIエージェント数の増加で1エージェントでの評価サイクルが追いつかなくなった時点で、`hr-onboarding-manager`・`hr-capability-evaluator`等への分割を検討する。

## 03. AI Workforceライフサイクル
```
planned → proposed → approved → onboarding → active ⇄ paused → retiring → retired
```
どの段階からでも`retiring`へ直接遷移してよい(`products.yaml`の`lifecycle_status`と同じ「消さずstatusを変える」原則)。`retired`になった行は`agents.yaml`から削除せず保持する。

**既存`agents.yaml`の`status`フィールドの拡張(新規フィールドを作らずSSOTを更新):**
| 現状の値 | 移行後 | 該当数 | 移行理由 |
|---|---|---|---|
| `draft` | `approved` | 44件 | 役割・ツール・definitionは確定済みだが、日常的な自動実行にはまだ入っていない段階のため |
| `active` | `active`(変更なし) | 3件(core-coo, ig-content-planner, ig-analytics) | 既に稼働中 |
| `paused` | `paused`(変更なし) | 0件 | 該当なし |
| `retired` | `retired`(変更なし) | 0件(ig-researchは台帳から既に削除済み) | 該当なし |

`planned`・`proposed`・`onboarding`・`retiring`の4段階は、今後新規提案されるエージェントにのみ適用し、既存47件の遡及的な再分類は行わない。**44件の一括移行(draft→approved)は本設計書のCEO確認事項として提示し、本バッチでは実施しない**(`agents.yaml`側は移行を予告するコメントのみ追加し、実際の値変更は別途の明示的なCEO確認後に行う)。

## 04. 既存Divisionとの責任境界
| Division | 役割 | HRとの関係 |
|---|---|---|
| Management | 全社戦略・KPI・リソース配分 | HRのライフサイクル判断はManagementの承認フロー(`ceo-approval-flow.md`)に従う |
| Architecture Review | 構造監査・重複検出・参照整合性(検出・報告のみ) | HRは検出結果を受けて意思決定・実行する側。Architecture Review自身は実行権限を持たない(既存の非実行境界を維持) |
| Finance | 給与・報酬・外注費等の金額記録 | HRは金額を保持しない。参照が必要な場合は`evidence_id`的な参照のみ |
| Product | 商品ライフサイクル | 対象が異なる(商品 vs 人員・エージェント) |

## 05. Human Workforceへの拡張方針(将来、今回は設計のみ)
AI WorkforceとHuman Workforceを混在させない。将来Human Workforce(社員・業務委託・鑑定師・講師)を扱う際は、`_company/org/workforce-human.yaml`を新設する(今回は作成しない。`finance.yaml`・`divisions.yaml`と同じ「トリガー到達まで作らない」原則)。Tomo Academyの講師管理・Phone Fortune Companyの鑑定師採用管理は、Human Workforce設計が固まった時点で個別に接続する(今回は接続しない)。

## 06. 機密情報の管理方針(将来、今回は方針のみ)
AIエージェントのメタデータ(status・role等)は現状通り`public`/`internal`でGit管理対象のまま。将来のHuman Workforceデータ(氏名・評価内容等)は、Finance実績・KPI実績と同じ型(README+スキーマ=Git管理、実データ=`.gitignore`除外)を適用する方針とする。今回は実データを一切作成しない。

## 07. Outbound Action Approval Policy・RT-013との関係
HR Divisionが行う新設・休止・退役等は社内ガバナンス行為であり、`_company/charter/outbound-action-policy.md`が扱う「社外への公開・送信」とは別軸である。ただし、エージェントを`retiring`/`retired`へ遷移させた場合、そのエージェントに付与されていたexecute権限(outbound-action-policy.md準拠)は自動的に失効するものとする(この1点のみ、明示的にHR手順へ組み込む)。RT-013(Architecture Review自身の分割トリガー)とは無関係(HR Division新設はagents.yamlの件数を47→48にするのみで、RT-013のトリガー条件(open指摘25件超/エージェント総数50超/2巡連続の単一観点偏重)のいずれにも該当しない)。

## 08. 実装順序
1. 本設計書のCEO承認
2. `hr-workforce-manager.md`作成
3. `agents.yaml`拡張(列挙値コメント+新規1行。既存44件のdraft→approved移行は別途)
4. `_company/agents/README.md`更新
5. `_shared/sop/hr/`新設(README+2手順)
6. `_shared/sop/sop-index.md`更新(hr行追加)
7. `docs/roadmap.md`更新
8. `_company/org/brands.yaml`の古いPhase番号表記を修正
9. 検証→報告→個別の明示的指示を経てコミット・push

## CEO確認事項
1. Phase番号を11で確定してよいか(roadmap.mdとの整合。もし本当に9をご意図なら別途採番調整が必要)
2. `draft`→`approved`への一括移行(44件)を、いつ・どのバッチで実施するか(本バッチでは実施せず予告のみ)
3. Human Workforce・Tomo Academy・Phone Fortune Companyとの接続は今回一切行わない、という理解でよいか

## 変更履歴
| 版 | 日付 | 変更内容 | 変更者 |
|---|---|---|---|
| 1 | 2026-07-13 | 初版作成。CEO確認待ち(Fable5判断を踏まえPhase11として提示。移行対象件数を42→44に訂正) | hr-workforce-manager(実行: COO) |
