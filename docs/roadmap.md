# 運営ロードマップ

初心者でも迷わず進められるように、段階を追って拡張します。前のフェーズが安定してから次に進みます。

## Phase 1 ― 基盤構築(完了)
フォルダ構成・命名規則・GitHub管理ファイル・エージェント台帳・設定ファイルを整備。

## Phase 2 ― MVP運用(設計完了、実装承認待ち)
事業を1つに絞り(Instagram/Tomo_Angel7)、エージェント2〜3体だけで実際に運用し、型を検証する。詳細は`docs/phase2-instagram-design.md`。

## Phase 3 ― Search & SEO Division・全社共有Division(設計・実装完了)
Google検索・WordPress経由の集客を担う`businesses/seo/`、および全社横断の4 Division(Competitor Intelligence/Traffic/Conversion/Brand Quality)を追加。詳細は`docs/phase3-search-seo-division-design.md`。

## Phase 3b ― Creative Division(設計・実装完了)
画像・動画・デザイン・ブランド表現を統括する全社共有Creative Divisionを追加。詳細は`docs/phase3b-creative-division-design.md`。

## Phase 4 ― Management Division(設計・実装完了)
全Division・将来Divisionを統括する経営レイヤー(CEO Office/COO Office/Strategy/KPI/Resource/Quality)を追加。個別コンテンツの承認ループ(Loop1)と経営ガバナンスループ(Loop2)を分離。詳細は`docs/phase4-management-division-design.md`。

## Phase 5 ― Revenue Division(設計・実装完了)
Funnel/Conversion/CRM/LTV/Analytics/Pricingで構成される収益管理層を追加。鑑定客・鑑定士志望者は別ペルソナ・並行ファネルとして設計。詳細は`docs/phase5-revenue-division-design.md`。

## Phase 6 ― Automation Division(設計・実装完了)
Workflow実行・API/MCP接続管理・Scheduler・実行監視で構成される自動化基盤を追加。今回は構造設計のみで、実際の自動化本番稼働・有料API接続は別途承認が必要。詳細は`docs/phase6-automation-division-design.md`。

## Phase 7 ― Intelligence Division(設計・実装完了)
市場/消費者/トレンド分析・インサイト統合・リサーチ品質チェックで構成される、全チャネル横断の情報分析基盤(AI Companyの「Brain」)を追加。既存の`shared-competitor-intelligence`・`shared-traffic-intelligence`を`division: intelligence`として再整理し、Instagram専用の`ig-research`(Phase2計画・未実装)は正式に退役。詳細は`docs/phase7-intelligence-division-design.md`。

## ブランド構造(2026-07-12 CEO決定)
AI Companyは単一ブランドの会社ではなく、**収益構造・運営体制・AIエージェント・SOPが全く異なる3ブランド**を運営する。詳細は`_company/org/brands.yaml`。
- **Tomo Angel**(`tomo-angel7`、既存・稼働中) ― 個人鑑定。Instagram/WordPress/SEO/LINE/LP/ココナラ
- **Tomo Academy**(`tomo-academy`、台帳登録のみ) ― スクール・教材・動画講座・会員サイト・コミュニティ
- **Phone Fortune Company**(`phone-fortune-company`、仮称・台帳登録のみ) ― 複数鑑定師を抱える電話占い企業。標準の事業ユニット雛形に収まらないため専用設計が必要(下記Phase8c)

## Phase 8 ― Product Division(設計・実装完了)
各ブランドの「サービス→商品→運用」を管理する共有Division(`product-service-designer`・`product-quality-standards`、ブランドパラメータ化)を追加。あわせてSEO Division(9エージェント)の`brand`固定を修正し、Tomo AcademyもWordPress/SEOを共有利用できる状態にした。詳細は`docs/phase8-product-division-design.md`。Phone Fortune Companyは対象外(下記Phase8c)。

## Phase 8b ― Finance Division(設計・実装完了、2026-07-12改訂)
`finance-controller`(1体、`_company/agents/`)が、`_company/finance/actuals/{brand_id}/{YYYY-MM}.yaml`(実績原始データの正本、Git管理対象外)を実務管理する。Tomo Angelは既に実売上があるため記録対象、Tomo Academy/Phone Fortune Companyは`not_recording`のまま。`products.yaml`は動的な実績数値を持たない完全な参照専用台帳に改定し、`tomo-angel7-personal-reading`(個人鑑定の包括商品)を1件登録。導出値(利益等)はactualsへ保存せず都度算出。詳細は`docs/phase8b-finance-division-design.md`。

## Phase 10 ― Architecture Review Division(設計・実装完了)
`architecture-review-controller`(1体、`_company/agents/`)が、Knowledge/SOP/Agent/命名規則/重複を**全件走査(フルスイープ)**で監査する(`management-quality`の統計的較正サンプリングとは手法が異なる)。検出・報告のみを行い、是正の実行は各Division・COOが通常の二段階承認プロセスで行う。`agents.yaml`行数の50行閾値到達も監視項目に含め、`divisions.yaml`新設判断のトリガーを仕組みで検知する。詳細は`docs/phase10-architecture-review-division-design.md`。

## Phase 11 ― HR Division(設計完了、2026-07-13)
AIエージェントのライフサイクル(planned/proposed/approved/onboarding/active/paused/retiring/retired)を管理する`hr-workforce-manager`(1体、`_company/agents/`)を追加。Architecture Reviewは検出・報告のみを維持し、エージェントの新設・異動・退役の意思決定と実行はHR+Managementの承認フローが担う。Human Workforce(将来の社員・業務委託・鑑定師・講師)は今回の対象外とし、設計方針のみ提示(実装はトリガー到達時)。詳細は`docs/phase11-hr-division-design.md`。

## Phase 12以降(構想段階、未設計)
1. **Phase 8a: Tomo Academy 事業ユニット新設** ― `businesses/academy/`。会員サイト・コミュニティ運営等、チャネル固有の実行エージェントが必要になった時点で新設(サービス/商品定義自体はPhase8のProduct Divisionが既に対応可能)
2. **Phase 8c: Phone Fortune Company 専用設計** ― 鑑定師採用・育成・品質管理・顧客管理・予約・シフト・決済・CRM・ブランド運営。標準の事業ユニット雛形を超える複合設計のため、Finance Division(Phase8b、実装済み)・HR Division(Phase11、設計完了)と連動して個別に設計する
3. **Phase 12: CEO Dashboard** ― `_company/kpi/`・`_company/reports/`を可視化するUI層(新しいデータ収集の仕組みは不要)

## 横展開の型(YouTube・Pinterest・LINE・TikTok・ココナラ等)
新規事業は`businesses/_template`を複製するだけで開始できる。共有Division(Creative/Competitor Intelligence/Traffic/Conversion/Brand Quality/Management)はそのまま再利用し、チャネル固有の薄いフロントエージェントのみを追加する。100体以上のエージェント運用体制はこの型のまま拡張する。
