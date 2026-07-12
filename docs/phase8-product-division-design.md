# Phase8設計書 ― Product Division 新設

**ステータス:** 承認済み(2026-07-12。CEO承認のうえ、Fable 5の判断を踏まえて実装)
**策定日:** 2026-07-12
**対象:** 各ブランドの「サービス→商品→運用」を管理するProduct Division。Phone Fortune Companyは対象外(将来HR/Finance/Management/Revenue/Automationが揃った段階で個別設計)

---

## 00. 本書の位置づけ
`docs/phase2〜7`に続く設計書。Brand Layer(思想・トーン)とProduct Division(商品・提供プロセス)の責務を分離する。

## 01. 発見事項と修正(Product Divisionと合わせて実施)
レビュー中に、SEO Division(9エージェント)が`agents.yaml`上で`brand: tomo-angel7`に固定されていたことが判明した。Phase3設計書は「他ブランドもbusiness_unitsに追記するだけで共有可能」と明記しており、実装が設計意図とズレていた。エージェント定義ファイル自体にはブランド固有の内容が一切無く(検証済み)、ブランド情報はすべて実行時に`brand-brief.md`・`assets/`から参照する設計のため、**9エージェントすべての`brand`を`null`に修正する**(Instagram側の4エージェントは、カルーセルテンプレートという「ブランドごとに持続する成果物」を扱う設計上の理由により、ブランド固定のまま変更しない)。

## 02. 責務分離: Brand Layer vs Product Division
| | Brand Layer | Product Division |
|---|---|---|
| 答える問い | 「私たちは誰か、どう話すか」 | 「私たちは何を売り、どう届けるか」 |
| 参照方向 | Product Divisionから参照される(逆方向の参照は持たない) | ブランドIDをキーに自身の記録を保持する |

**階層:** ブランド(思想)→チャネル(既存`businesses/{channel}/`)→サービス→商品→運用。「サービス→商品→運用」が今回新設する層。

## 03. エージェント構成(2体、`_shared/agents/product/`新設)
| エージェントID | 役割概要 |
|---|---|
| `product-service-designer` | 各ブランドのサービス→商品構造を定義(例: Tomo Angelの鑑定セッション種別、Tomo Academyのコース構成)。決定後は`revenue-pricing-offer-designer`(価格・オファー)・`shared-video-creative-team`等(実制作)へ引き継ぐ |
| `product-quality-standards` | サービス提供の品質基準を**定義**する(コンテンツQAではなく、鑑定セッション・コース教材等の提供品質基準)。現時点では定義のみで、アクティブな監査は行わない。将来監査機能が必要になった場合は`management-quality`のメタ監査パターンを踏襲する |

*両エージェントとも`division: product`、`brand: null`(実行時パラメータ、統合しない。理由: 下流の消費者・成長軌道が異なるため)。*

## 04. 既存Divisionとの境界
| 比較対象 | 境界 |
|---|---|
| Brand Layer | Product Divisionがブランドを参照する一方向のみ。`brand-brief.md`は変更しない(Product固有のフィールドを持ち込まない) |
| Revenue Division | Product=何を売るか、Revenue=いくら・どう売るか。`product-service-designer`の出力が`revenue-pricing-offer-designer`の入力になる |
| Creative Division | Product Divisionは構成を定義するのみ。実制作(動画・画像)は既存`shared-video-creative-team`等をそのまま利用し複製しない |
| Management Division | `product-quality-standards`は基準定義のみ。将来アクティブ化する場合は`management-quality`と同じメタ監査パターンを踏襲する(新しい監査の型を発明しない) |
| Automation Division | 予約→提供→フォローの実行は将来`automation-workflow-manager`・`automation-scheduler`(既存、構造のみ)が担う。Product Divisionはプロセスを定義するのみ |

## 05. Knowledge Layer拡張
新規カテゴリ: `product-design/`(1つのみ)― サービス構造化理論・カリキュラム設計理論を統合。

## 06. SOP Layer拡張
新規カテゴリ: `sop/product/`(1つのみ)― サービス/商品定義手順・カリキュラム設計手順・品質基準定義手順。**定義手順のみを扱い、実行手順(予約対応・配信等)は含めない**(実行手順は既存`sop/crm/`・将来`sop/automation/`・`sop/management/`を使う)。

## 06-1. Product Lifecycle(2026-07-12追記)
個人鑑定・スクール・教材・会員サイト・電話占い・AIツール・書籍・サブスク等、将来100ブランド・1000商品規模になることを見据え、商品単位のライフサイクル管理を`_company/org/products.yaml`(新設)で行う。

```
Idea → Planning → Developing → Beta → Released → Growing → Mature → Retired
```

- **Brandライフサイクル(`brands.yaml`)とは完全に独立。** ブランドは永続的・少数、商品は流動的・大量という性質差のため、あえて別ファイル・別軸で管理する
- Product記録はBrandを`brand_id`で参照するのみで、Brand側からの逆参照は持たない(Product→Brandの一方向参照。`brand-brief.md`は変更しない)
- どの段階からでも`Retired`へ直接遷移可能(企画中止・早期終了に対応)。削除はせず`status`のみ変更する
- 実務管理は`product-service-designer`が担当

## 06-2. Product Metrics(2026-07-12追記)
`products.yaml`の各商品エントリに`metrics`ブロックを追加し、「商品の設計情報」だけでなく「商品の健康状態」も同じ場所で参照できるようにした。初期値はすべて`null`。

| 指標 | 更新元 |
|---|---|
| Revenue / CVR / CPA / ROAS / LTV / Retention Rate | `revenue-analytics`(company/brand階層のKPIと同じ計算元を商品粒度に分解) |
| Refund Rate | (将来)Finance Division。未実装のため当面`null` |
| Customer Satisfaction / Review Score | `product-quality-standards`(収集窓口は`revenue-crm-manager`等) |

`products.yaml`の構造(スキーマ)は`product-service-designer`が所有するが、`metrics`配下の値は上記エージェントのみが更新する(構造所有者は値を書き換えない)。company/brand階層のKPIとは粒度が異なるだけで、計算の重複ではない。

## 07. Phone Fortune Companyの扱い
今回設計しない。`brands.yaml`に`status: planned`で登録済み。HR/Finance/Management/Revenue/Automationが揃った段階で個別設計する(標準の共有Divisionパターンに収まらない可能性が高いため)。

## 08. 変更履歴
| 版 | 日付 | 変更内容 | 変更者 |
|---|---|---|---|
| 1 | 2026-07-12 | 初版作成。CEO承認済み(Fable 5判断を経て実装) | COO |
| 2 | 2026-07-12 | Product Lifecycle(`_company/org/products.yaml`)を追加。Brandライフサイクルとは独立した別軸として設計 | COO |
| 3 | 2026-07-12 | Product Metrics(`metrics`ブロック)を追加。構造所有者はproduct-service-designer、値の更新はrevenue-analytics/product-quality-standards | COO |
