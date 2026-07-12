# Phase5設計書 ― Revenue Division 新設

**ステータス:** 承認済み(2026-07-12。CEO承認のうえ、Fable 5の判断を踏まえて実装)
**策定日:** 2026-07-12
**対象:** 全チャネルの売上・収益ループを管理するRevenue Division(Funnel/Conversion/CRM/LTV/Analytics/Pricing)

---

## 00. 本書の位置づけ
`docs/phase2-instagram-design.md`・`phase3-search-seo-division-design.md`・`phase3b-creative-division-design.md`・`phase4-management-division-design.md`に続く設計書。「集客→教育→信頼構築→コンバージョン→LTV最大化」という収益ループを、既存のKnowledge/SOP/Brand/Creative/Search/Management Layerと整合させたまま追加する。

## 01. 重複解消の決定事項
| # | 内容 | 決定 |
|---|---|---|
| 1 | Conversion Team(CVR分析・LP改善・CTA改善・ABテスト) | 新規エージェントを作らず、既存`shared-conversion-optimizer`の役割を拡張(ABテスト手法・LINE固有のCVR改善を追加)。`division`タグに`revenue`を並記 |
| 2 | revenue-director | 新設しない。`core-coo`が直接統括する(SEO/Creative Divisionと同じ型) |
| 3 | customer-success | 独立エージェント化せず、`revenue-ltv-manager`に統合 |

## 02. 組織構造の型
```
_shared/agents/revenue/          … Revenue Division専属の実行エージェント(新設)
_shared/agents/conversion/        … 既存shared-conversion-optimizerを拡張(Conversion Team相当)
```
Revenue Divisionは実行機能(施策の提案・実行・分析)であり、Management Division(監査・集計専用)とは性質が異なるため`_shared/agents/`に置く。

## 03. エージェント構成
| チーム | エージェントID | 種別 | 置き場所 | 役割概要 |
|---|---|---|---|---|
| Funnel Team | `revenue-funnel-strategy` | 新規 | `_shared/agents/revenue/` | 複数の並行ファネルを管理できる構造で設計(下記04章参照)。現時点ではファネルA(鑑定客向け)のみ実装 |
| Conversion Team | `shared-conversion-optimizer`(既存拡張) | 既存拡張 | `_shared/agents/conversion/` | LP・CTA・単一ページCVR改善+ABテスト・LINE固有CVR改善を追加 |
| CRM Team | `revenue-crm-manager` | 新規 | `_shared/agents/revenue/` | LINE配信・ステップ配信・顧客管理・セグメント管理・再購入施策の実行 |
| LTV Team | `revenue-ltv-manager` | 新規 | `_shared/agents/revenue/` | リピート率・クロスセル・アップセル・紹介・顧客維持(customer-successの役割を統合) |
| Analytics Team | `revenue-analytics` | 新規 | `_shared/agents/revenue/` | 売上・CPA・CAC・ROAS・ROI・LTV・MRR・ARPUの計算元(`management-kpi`はここから集約するのみ) |
| Pricing/Offer | `revenue-pricing-offer-designer` | 新規(pricing-strategy+offer-designerを統合) | `_shared/agents/revenue/` | 価格・オファーの収益最適化。鑑定とスクールで異なる価格心理・オファー構造を扱う。商品内容そのものはPhase8 Product Divisionが将来担当 |

*全5体(新規4体+既存拡張1体)は`division: revenue`を持つ。*

## 04. ファネル構造(鑑定とスクールは別ペルソナ・並列ファネル)

> **【2026-07-12 追記・上書き注記】** 本章末尾の「ブランド帰属について」は、CEOの直接判断により**上書きされました**。鑑定(`tomo-angel7`)とスクール(`tomo-academy`)は同一ブランド内の第2ペルソナではなく、**完全に別ブランド**として分離されています。当時の判断過程は記録として残しますが、現在有効な設計は`docs/phase7-intelligence-division-design.md`以降および`_company/org/brands.yaml`を参照してください。詳細な分離理由・影響範囲は本改定の対応で更新した各ファイル(`brand-brief.md`・`revenue-funnel-strategy.md`・`revenue-pricing-offer-designer.md`・`brands.yaml`)を正とします。
CEO確認により、鑑定客(悩みを抱える顧客)と鑑定士志望者(スクール生)は**別ターゲット・別ペルソナ**であり、「鑑定→スクール」という順序関係ではないことが判明した。`revenue-funnel-strategy`は、複数の並行ファネルをリストとして管理できる構造(ファネルごとにペルソナ・集客チャネル・オファーを紐づけ)で設計する。

| ファネル | 状態 | 内容 |
|---|---|---|
| ファネルA(鑑定客向け) | **実装対象** | Instagram/SEO(恋愛・悩み系コンテンツ)→LINE→鑑定→LTV(リピート鑑定・紹介)。ペルソナは`brand-brief.md`5章の既存ペルソナと同一 |
| ファネルB(鑑定士志望者向け) | **プレースホルダー(未設計)** | 集客チャネル・ペルソナ・オファーいずれも未定。スクール事業の具体的着手(Phase8 Product Division、または別途指示)時に設計する。今回は「将来ここに追加される」ことを明示するのみ |

**ブランド帰属について(Fable 5判断、旧・未解決事項の決着):** 鑑定・スクールは両方とも「Tomo(実践者)への信頼」を基盤にするため、ブランドを分けず**同一ブランド(Tomo_Angel7)内の第2ペルソナ**として扱う。ファネルB設計時、`brand-brief.md`5章を「ペルソナA(鑑定客)/ペルソナB(鑑定士志望者)」の構成に拡張する。**再検討条件:** スクールが実際に稼働し、トーン・世界観そのものが鑑定業と異なると判明した場合のみ、`brands.yaml`への新規ブランド登録(ブランド昇格)を検討する。この条件を満たさない限り、ブランドは分けない。

## 05. Knowledge Layer拡張(必要最小限)
新規カテゴリ: `crm/`(CRM運用理論+LTV/リテンション理論を統合。両者とも「顧客との継続関係」という同じ対象のため細分化しない)。ファネル理論・CVR理論・価格戦略理論は既存`marketing/`・`psychology/`・`analytics/`に含まれるため新設しない。

## 06. SOP Layer拡張(必要最小限)
新規カテゴリ: `sop/crm/`(LINE運用・CRM運用・LTV改善の手順)。LP改善・CVR改善・ABテストは既存`sop/conversion/`を拡張して収容する(新規カテゴリ不要)。売上レビューは独立SOPを作らず、既存`sop/management/review-cycle.md`の月次レビューに`revenue-analytics`の数値を組み込む。

## 07. ワークフロー(Loop3: 収益ループ)
既存のLoop1(個別コンテンツ制作)・Loop2(経営ガバナンス、Phase4)に加え、Loop3(収益ループ)を新設する。
```
Loop3(収益ループ、Revenue Division、ファネルAのみ稼働):
Instagram/SEO/WordPress(Loop1が生成したコンテンツ) → LINE → 鑑定 → LTV
→ revenue-analytics(分析) → revenue-funnel-strategy/revenue-ltv-manager(改善提案) → revenue-crm-manager(再配信・再アプローチ)
→ 結果は次サイクルのLoop1企画・Loop2月次レビューへフィードバック
```

## 08. KPIとbrand-brief.mdへの影響
`_company/brands/tomo-angel7/brand-brief.md` 22章の既存KPI表に、CPA・CAC・LTV・ROAS・ROI・MRR・ARPUを追加する(Revenue Division側で新たに定義せず、既存のKPI優先順位表を正本として拡張する)。

## 09. 将来Divisionへの接続
- Phase8 Product Division(鑑定・スクール・電話占い)は、`revenue-pricing-offer-designer`が切り出した「価格・オファーの収益最適化」の隣に「商品内容そのもの」を追加するだけで接続できる。ファネルBの設計もこのタイミングで行う
- Phase9 Finance Divisionは`revenue-analytics`が計算した値を利益・ROI計算の入力として使うだけで、収益データの再収集は不要

## 10. 変更履歴
| 版 | 日付 | 変更内容 | 変更者 |
|---|---|---|---|
| 1 | 2026-07-12 | 初版作成。CEO承認済み(Fable 5判断を経て実装) | COO |
