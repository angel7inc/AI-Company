# Phase7設計書 ― Intelligence Division 新設

**ステータス:** 承認済み(2026-07-12。CEO承認のうえ、Fable 5の判断を踏まえて実装)
**策定日:** 2026-07-12
**対象:** 全チャネル横断の情報分析基盤「Intelligence Division」(AI Company全体のBrain)

---

## 00. 本書の位置づけ
`docs/phase2〜6`に続く設計書。Instagram専用ではなく、SEO・WordPress・YouTube・Pinterest・Google検索・LINEを横断するリサーチ機能を確立する。

## 01. 重複解消の決定事項(今回特に重要)
ご提示の12候補エージェントのうち7件が既存資産と衝突していた。

| ご指示の候補 | 決定 |
|---|---|
| competitor-intelligence | 新設しない。既存`shared-competitor-intelligence`の`division`タグを`intelligence`に更新するのみ(ファイル・内容は変更しない) |
| search-intelligence | 新設しない。Phase3で`seo-research`に既に統合済み(同名の提案を重複としてマージした経緯の再確認) |
| traffic-intelligence | 新設しない。既存`shared-traffic-intelligence`の`division`タグを`intelligence`に更新するのみ |
| youtube-intelligence / reddit-intelligence / pinterest-intelligence | 今回は新設しない。該当プラットフォームの事業ユニットがまだ存在しないため(各事業ユニット新設時に再検討) |
| news-intelligence | `intelligence-trend`に統合 |
| (Instagram固有`ig-research`、`docs/phase2-instagram-design.md`計画・未実装) | **今回正式に退役を確定する。** 機能は`intelligence-consumer`・`intelligence-trend`が上位互換する。Instagram固有のリサーチ需要が生じた場合は新エージェントを作らず`intelligence-consumer`のチャネルレンズとして扱う(`ig-carousel-intelligence`と同じパターン) |

## 02. エージェント構成
### 02-1. 新規5体(`_shared/agents/intelligence/`、新設フォルダ)
| エージェントID | 役割概要 |
|---|---|
| `intelligence-market` | 市場規模・ポジショニング・成長市場分析(チャネル横断) |
| `intelligence-consumer` | 悩み・ペルソナ・行動分析(チャネル横断。`ig-research`の代替として機能) |
| `intelligence-trend` | コンテンツ企画のための兆し発見(Google Trends/SNS/ニュースを横断。news-intelligenceを統合)。**`shared-traffic-intelligence`の出力を再利用し、自らTrendsへ再照会しない**(`management-kpi`の「集約は既存値を再利用、再計算しない」と同じパターン) |
| `intelligence-insight-synthesizer` | market/consumer/trend/competitor/traffic/search-researchの出力を統合し、各チャネルのコンテンツ制作へ配信するハブ。**自らリサーチは行わず、配信のみ**を担当する。出力は`management-strategy`への入力の1つにもなる。**再評価基準:** 実運用で「複数レポートを単に貼り合わせているだけ」と判明した場合、`intelligence-market`へ統合を検討する |
| `intelligence-research-quality-checker` | 引用漏れ・重複・古い情報・一次情報優先・ファクトチェック。`shared-brand-quality-checker`とは判定軸が異なる(事実正確性 vs ブランドトーン)ため委譲関係はなく、独立して並行動作する |

### 02-2. 既存2体の再整理(移動・内容変更なし、`division`タグのみ更新)
| エージェントID | 変更内容 |
|---|---|
| `shared-competitor-intelligence` | `division: competitor-intelligence` → `division: intelligence` |
| `shared-traffic-intelligence` | `division: traffic` → `division: intelligence` |

*director的な調整役は新設しない(`core-coo`が直接統括。Revenue/Automation Divisionと同じ判断)。*

## 03. 既存Divisionとの境界線
| 比較対象 | 違い |
|---|---|
| `intelligence-insight-synthesizer` vs `management-strategy` | 前者はコンテンツ制作(何を今週作るか)への示唆、後者は経営判断(成長/撤退・四半期方針)。前者の出力は後者の入力の1つ |
| `intelligence-trend` vs `shared-traffic-intelligence` | 前者は「何を作るべきか・なぜ今か」という質的・企画示唆(traffic-intelligenceの出力を消費する側)、後者は「どれだけの量が・どこから」という量的・アトリビューション |
| `intelligence-consumer`/`intelligence-market` vs `ig-carousel-intelligence` | 前者はチャネル横断の悩み・市場理解、後者はInstagramカルーセルという投稿フォーマット固有の深掘り |
| `intelligence-research-quality-checker` vs `shared-brand-quality-checker` | 前者は事実正確性・引用、後者はブランドトーン・AI臭。判定軸が異なるため委譲せず並行動作 |

## 04. Knowledge Layer拡張
新規カテゴリ: `research-methodology/`(1つのみ)― 情報源評価・引用作法・ファクトチェック手法・一次/二次情報理論。市場・消費者・トレンド・競合・ニュース理論は、既存の`business-strategy/`・`psychology/`・`marketing/`・`analytics/`にすべて統合し、新規カテゴリを作らない。

## 05. SOP Layer拡張
**新規カテゴリなし。** 既存`sop/research/`(Phase1から「悩み調査・競合調査・トレンド調査」を既にスコープ済み)に、市場分析・消費者分析・トレンド分析・insight統合・リサーチ品質チェックの手順ファイルを追加する。

## 06. Gemini利用ルール
新規ポリシーは作らず、既存`_company/charter/ai-usage-policy.md`の「Gemini Deep Researchの利用基準」に、競合分析・市場分析・海外事例・大量比較・長文リサーチ・E-E-A-T調査・検索意図分析・新ジャンル調査を具体例として追記する(重複表現は統合し、二重定義しない)。

## 07. ワークフロー(Research起点の一気通貫)
```
intelligence-market / intelligence-consumer / intelligence-trend / shared-competitor-intelligence / shared-traffic-intelligence / seo-research
→ intelligence-research-quality-checker(品質チェック)
→ intelligence-insight-synthesizer(統合・配信のみ)
→ ig-content-planning / ig-carousel-intelligence(Instagram)
→ seo-keyword-strategy(SEO/WordPress)
→ revenue-crm-manager(LINE)
→ (YouTube/Pinterestは事業ユニット新設時に接続)
```
Loop1(個別コンテンツ制作)の起点である「research」ステップを、Instagram専用から全社共有へ格上げしたものであり、新しいLoopではない。

## 08. 変更履歴
| 版 | 日付 | 変更内容 | 変更者 |
|---|---|---|---|
| 1 | 2026-07-12 | 初版作成。CEO承認済み(Fable 5判断を経て実装) | COO |
