# Phase8b設計書 ― Finance Division 新設

**ステータス:** 承認済み(2026-07-12。CEO承認のうえ、Fable 5の判断を踏まえて実装)
**策定日:** 2026-07-12
**対象:** 全ブランド共通の財務基盤(利益・ROI・広告費・会計・予算)

---

## 00. 本書の位置づけ
`docs/phase8-product-division-design.md`に続く設計書。既存の`revenue-analytics`(売上計算)・`management-resource`(AI/APIコスト監視)の出力を合算し、これまで誰も算出していなかった「利益」を初めて可視化する。

## 01. 既存設計との重複チェック
| # | 検出内容 | 判定 |
|---|---|---|
| 1 | `management-resource`が既にAI/APIコストを監視 | 重複ではない。役割は変更せず「AI/APIコストという費用の一部」の監視に留め、生データをFinance Divisionへ提供する側とする |
| 2 | `revenue-analytics`が既に売上・ブランド/商品単位ROIを計算 | 重複ではない。役割は変更せず、Finance Divisionは会社全体のROIという別粒度を扱う(KPIの会社/ブランド/商品階層と同じロールアップ構造) |

**結論:** 新しい計算ロジックを増やさず、既存2エージェントの出力を合算する1階層上のロールアップとして設計する。

## 02. エージェント構成(1体)
| エージェントID | 置き場所 | 役割概要 |
|---|---|---|
| `finance-controller` | `_company/agents/`(division: finance) | `revenue-analytics`(売上)・`management-resource`(AI/APIコスト)の出力を合算し利益を算出。会社全体のROI・広告費(現状ゼロ、構造のみ)・会計記録・予算配分を担当 |

**分割しない理由・将来の分割基準:** 現時点で「会計(記録)」と「予算(意思決定)」の消費者はCEO・`management-kpi`のみで同一、取引量もゼロのため、1エージェントに統合する。**分割トリガー:** 初めて実際の広告出稿が始まった時点、または月次決算の手順が複数日にまたがるほど取引量が増えた時点で、`finance-budget`等への分割を検討する。

## 03. 台帳新設は見送り(Fable 5判断)※2026-07-12付で上書き

> **【上書き注記】** 本章の「台帳新設は見送り」は、Tomo Angelが既に個人鑑定事業として実際の売上を計上している実態と一致しないため、CEO指示により撤回されました。当時の判断過程は記録として残しますが、現在有効な設計は09〜13章です。

`products.yaml`は明示的なCEO指示により新設したが、Finance Divisionの予算台帳は今回**見送る**。現時点で予算配分・広告費の実績がゼロであり、先回りしてファイルを作らないという既存の原則(`divisions.yaml`・Phone Fortune Companyのbrand-brief等と同じ)に従う。

**新設トリガー:** 初めて非ゼロの予算配分、または初めての広告費が発生した時点で`_company/org/finance.yaml`(`products.yaml`と同じ構造: `brand_id`外部キーのみ、逆参照なし)を新設する。それまでの利益・ROIは`_company/kpi/`(既存)へ出力する。

## 09. 実績データの保存場所(2026-07-12改定)
単一ファイル(`_company/org/finance.yaml`)ではなく、以下のディレクトリ構造を採用する。
```
_company/finance/
├── README.md                       # Finance Layerのルール
├── schema-sample.yaml               # 全金額0の記入例(sample_only: true / do_not_import: true)
└── actuals/                         # 実績数値の正本(Git管理対象外)
    └── {brand_id}/{YYYY-MM}.yaml
```
`_company/org/finance.yaml`は、スキーマバージョン・パス規則・ID規則・記録対象ブランドのstatusのみを持つ設定ファイルとし、実績数値・月一覧(手動索引)は一切持たない。

## 10. Git管理からの除外(機密情報保護)
`sensitivity: confidential`のFinance実績は、Private Repositoryであっても外部クラウド(GitHub)へPushしない。`.gitignore`に`_company/finance/actuals/**/*.yaml`を追加し除外する。ディレクトリ維持は`.gitkeep`(除外対象外)、記入例は`schema-sample.yaml`(`actuals/`の外、Git管理対象)で提供する。証拠資料(請求書・明細等)はGoogle Drive(既存の`docs/phase2-instagram-design.md` 10章のルールを踏襲)に保存し、Finance実績には`evidence_id`のみ記録する。

## 11. レコードID・共通経費への対応
`fin-{yyyymm}-{brand_id}-{allocation_scope}-{scope_id}-{channel}`(自然キー方式。連番方式は本プロジェクトの他の全台帳のID設計と整合しないため不採用)。特定商品に紐づかない共通経費に対応するため`allocation_scope`(company/brand/service-line/product)・`scope_id`・`allocation_method`(direct/manual/proportional/none)を持たせる。`service_line_id`(運営単位)は`brands.yaml`の`business_units`(コンテンツ・集客チャネル)とは別軸の概念。

## 12. 導出値の非保存
`net_revenue_jpy`・`operating_profit_jpy`は自動検証プログラムが存在しない現時点では`actuals/`へ保存せず、`revenue-analytics`・`management-kpi`・月次レポート側で都度算出する。`cash_received_jpy`・`cash_paid_out_jpy`(現金収支)はこの計算式に使用しない。

## 13. products.yamlの完全な参照専用化
`products.yaml`から動的な実績数値(revenue_jpy・CVR・CPA・ROAS・LTV・customer_satisfaction・review_score等)を全廃し、`finance_source`・`report_reference`・`computed_by`・`last_computed_at`という参照情報のみを保持する完全な定義台帳とする。あわせて`tomo-angel7-personal-reading`(個人鑑定の包括商品。LINE/ココナラ/Instagram DM等は別商品ではなく同一商品の異なるchannelとして扱う)を1件登録する。

## 04. 既存エージェントとの関係整理
| 比較対象 | 境界 |
|---|---|
| `management-resource` | 役割変更なし。AI/APIコストの生データを`finance-controller`へ提供する側 |
| `revenue-analytics` | 役割変更なし。売上・商品単位ROIのデータを`finance-controller`へ提供する側。`products.yaml`の`refund_rate`欄は本エージェントの管轄外のまま、`finance-controller`が担当 |
| `management-kpi` | 引き続き会社全体KPIの集約担当。`finance-controller`が算出した利益は`_company/kpi/`経由で集約対象に加わる |

## 05. Knowledge Layer拡張
新規カテゴリ: `finance/`(1つのみ)― 会計原則・予算配分フレームワーク・P/L構造の一般理論。CAC/LTV等の指標定義は既存`analytics/`のまま。

## 06. SOP Layer拡張
新規カテゴリ: `sop/finance/`。今回実装するのは**月次決算手順**・**売上コスト突合手順**の2つのみ。**予算配分手順は03章の台帳新設と同時まで見送る**(書き込み先のない手順は実行できないため)。支出のCEO承認は既存`sop/management/ceo-approval-flow.md`をそのまま参照し複製しない。

## 14. 変更履歴
| 版 | 日付 | 変更内容 | 変更者 |
|---|---|---|---|
| 1 | 2026-07-12 | 初版作成。CEO承認済み(Fable 5判断を経て実装) | COO |
| 2 | 2026-07-12 | Tomo Angelの実売上を反映し03章を上書き。09〜13章(ディレクトリ構造・Git除外・ID方式・導出値非保存・products.yaml完全参照化)を追加 | COO |
