# Brand Layer(ブランド層)

AI Companyが運営する各ブランドのSSOT(唯一の正解)を置く場所です。ブランドごとのフォルダ(`_company/brands/{ブランドID}/`)と、全ブランド台帳(`_company/org/brands.yaml`)が対になっています。将来ブランドが増えても、この1ファイルのルールに沿って追加すれば、既存の設計思想を壊さずに拡張できます。

## 1. ブランド命名規則
- **ブランドID(`id`):** 小文字・ハイフン区切り(例: `tomo-angel7`)。`_company/brands/{ブランドID}/`フォルダ名・`brands.yaml`の`id`と完全に一致させる。一度使ったIDは、そのブランドが`retired`になった後も再利用しない(別の新ブランドに同じIDを使い回さない)
- **表示名(`name`):** 人間が読みやすい名称(例: `Tomo_Angel7(Tomo Angel)`)。ロゴ表記やCEOの呼称に合わせてよい
- IDと表示名が乖離してもよい(表示名は変更されうるが、IDは変更しない。他ファイルからの参照はすべてIDで行うため)

## 2. ブランド追加条件
新しい提供物が出てきたとき、「新ブランド」にすべきか「既存ブランド内の新チャネル/新ペルソナ」に留めるべきかを、以下の基準で判断する(2026-07-12のTomo Angel/Tomo Academy/Phone Fortune Company分離で実際に使った基準を一般化したもの)。

| 基準 | 既存ブランドのまま | 新ブランドが必要 |
|---|---|---|
| ターゲット・目的 | 既存ペルソナの延長・バリエーション | 目的・動機が根本的に異なる別セグメント |
| 収益構造 | 既存の導線・オファー形式を流用できる | 収益モデル自体が別物(例: 個人役務 vs 複数人材を抱える企業運営) |
| 運営体制 | 既存の体制・エージェント構成で対応できる | 採用・育成・シフト等、既存の事業ユニット雛形を超える体制が必要 |
| トーン・世界観 | 既存ブランドのトーンを保てる | 既存ブランドのトーンでは対応できない・毀損リスクがある |

**判定:** 上記4項目のうち2つ以上が「新ブランドが必要」に該当する場合、新ブランドとして`brands.yaml`に登録する。1つ以下なら、既存ブランドの新チャネル(`business_units`に追加)または新ペルソナ(`brand-brief.md`内に追記)として扱う。

## 3. ブランドライフサイクル
| 状態 | 内容 | 必須ファイル |
|---|---|---|
| `planned` | `brands.yaml`への登録のみ。コンセプト・ペルソナ・トーン等はまだ未定でよい | `brands.yaml`の1エントリのみ(brief/qa_checklist/pain_point_categoriesは将来のパスを記載するが、ファイル自体は作らない) |
| `independent-pending` | 既存ブランドの一部(ペルソナ・チャネル)から独立ブランドへ昇格を検討中の過渡状態(任意。省略してplanned→activeでもよい) | `planned`と同じ |
| `active` | 実際にコンテンツ・施策を運用中 | 4章の全ファイルが揃っていること |
| `retired` | 運営終了。**削除はしない**(Knowledge/SOPと同じ「消さずstatusを変える」原則)。フォルダ・ファイルはアーカイブとして残す | 既存ファイルをそのまま保持し、`brands.yaml`の`status`のみ変更 |

*`brands.yaml`のヘッダーコメントに`retired`を追加し、この4状態が正本であることを明記する(3-1参照)。*

### 3-1. brands.yamlへの反映
`_company/org/brands.yaml`のヘッダーコメント(`status:`の説明)を以下のとおり更新する。
```
# status: planned(準備中・台帳登録のみ) / independent-pending(既存ブランドからの分離検討中) / active(運用中) / retired(運営終了。削除せず保持)
```

## 4. ブランド共通で必須となるファイル(`active`時点)
| ファイル | 役割 |
|---|---|
| `brand-brief.md` | ミッション/ビジョン/バリュー/ペルソナ/トーン/NG表現/KPI優先順位等、ブランドの唯一の正解。`_shared/templates/brand-brief-template.md`から作成する |
| `qa-checklist.md` | ブランド固有の品質チェック基準。`_shared/templates/qa-checklist-template.md`から作成する |
| `pain-point-categories.yaml` | 顧客の悩みカテゴリ一覧 |
| `assets/` | ブランド固有のロゴ・フォント・カラー等の実ファイル(値の正本は`brand-brief.md`) |
| `brands.yaml`の1エントリ | 上記4ファイルへのポインタ、`business_units`、ライフサイクル状態 |

`brand-brief.md`のfrontmatterには`related_knowledge`・`related_sop`を必ず持たせ、そのブランドが前提とするKnowledge/SOPカテゴリを明記する(内容は複製せずリンクのみ)。

## 5. ブランド間で共有するもの / 共有しないもの
これが最も重要な原則であり、2026-07-12のブランド分離が既存Divisionに一切影響しなかった理由でもある。

| 共有する(全ブランド共通) | 共有しない(ブランドごとに個別) |
|---|---|
| 全共有Division(Creative/SEO/Revenue/Management/Automation/Intelligence)のエージェント本体 | `brand-brief.md`の内容(ミッション・ペルソナ・トーン・NG表現・KPI優先順位) |
| Knowledge Layer(一般理論) | `qa-checklist.md`のブランド固有チェック項目 |
| SOP Layer(汎用手順) | `pain-point-categories.yaml` |
| `docs/naming-conventions.md`・`_company/charter/`(憲章・AI利用ポリシー) | `assets/`内の実ファイル(ロゴ・フォント等) |

**原則:** 共有Divisionのエージェントは、実行時に対象ブランドを受け取るだけで、ブランド固有の値をコード・定義ファイル側に持たない(`brand: null`、対象ブランドのSSOTを都度参照)。新しいブランドを追加しても、共有Division側の変更は原則不要というのがこの設計の骨子であり、実際に3ブランド分離時もこれが成立した。

## 6. 重複防止ルール
- 新ブランド追加時、既存ブランドの`brand-brief.md`をコピーして流用してよいが、内容(ペルソナ・トーン等)は必ずそのブランド用に書き換える(テンプレートの構造のみ流用し、値は複製しない)
- 複数ブランドに共通する知見(例: 「恋愛心理学の一般理論」)は、ブランド個別のファイルに書かず、Knowledge Layerへ還元する
- ブランド追加のたびに新しい共有Divisionを作らない。既存の共有Divisionで対応できないか必ず先に検討する(4章の追加条件表を参照)
