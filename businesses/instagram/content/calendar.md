# Instagram 投稿カレンダー

このカレンダーは `ig-content-planner` が更新し、`ig-analytics` が参照します。形式は [共通テンプレート](../../../_shared/templates/content-calendar-template.md) に準拠します。

| 日付 | 投稿タイプ | テーマ・ネタ | ステータス | 担当エージェント |
|---|---|---|---|---|
| (未登録) | | | | |

## パイロット投稿枠(`tomo-angel7-carousel-pilot-2026-07`、詳細は`pilot-scope.md`参照)

**注意(既存テンプレートとの関係):** 上表の「ステータス」列は[共通テンプレート](../../../_shared/templates/content-calendar-template.md)で「下書き/レビュー中/確定/公開済み」の4値と定義されています。パイロット枠はまだテーマも投稿日も決まっていない「予約枠」の段階のため、この4値のいずれにも当てはまりません。上表を流用せず、以下に独立した表を設けます(既存の4値enumを変更・拡張していません)。具体的な投稿日はCEOが決定するまで設定していません。

| pilot_post_id | カテゴリ | status | 投稿順序 | CEO承認ポイント | 正本成果物 |
|---|---|---|---|---|---|
| tomo-angel7-pilot-01 | A(恋愛・パートナーシップ) | ceo_review | 1 | テーマ選定時・公開前 | `businesses/instagram/content/pilot-drafts/tomo-angel7-pilot-01.md` |
| tomo-angel7-pilot-02 | B(人間関係・家族・仕事) | planned | 2 | テーマ選定時・公開前 | (未着手) |
| tomo-angel7-pilot-03 | C(自己肯定感・不安・人生の選択) | planned | 3 | テーマ選定時・公開前 | (未着手) |
| tomo-angel7-pilot-04 | A(恋愛・パートナーシップ) | planned | 4 | テーマ選定時・公開前 | (未着手) |
| tomo-angel7-pilot-05 | B(人間関係・家族・仕事) | planned | 5 | テーマ選定時・公開前 | (未着手) |
| tomo-angel7-pilot-06 | C(自己肯定感・不安・人生の選択) | planned | 6 | テーマ選定時・公開前 | (未着手) |

`status: planned`は、投稿成果物のライフサイクル(`_shared/sop/instagram/carousel-post-pilot.md`の`draft`〜`archived`8段階)よりも前の「枠だけ確保され、着手前」の段階を表す。着手時に`businesses/instagram/content/templates/carousel-pilot-brief-template.md`をコピーし、`pilot-drafts/`配下で`status: draft`から開始する。投稿順序は目安であり、実際の着手順はCEOの指示による。

**【2026-07-14 Batch B: calendar同期ルール】** 上表の`status`列は、正本成果物ファイル(「正本成果物」列に記載)の状態を反映する**ミラー情報**であり、calendar自体はいかなる投稿本文・画像・承認内容の正本にもならない。**成果物側のstatusが常に正本であり、calendarだけを先に変更してはならない。** 成果物側のstatusを変更した場合は、同じ作業単位でこの表のstatus列を同期する。不一致を発見した場合は、必ず成果物側ファイルを基準に本表を修正する。投稿本文・キャプション・画像制作指示・承認記録そのものは本表に複製しない(詳細は`_shared/sop/instagram/carousel-post-pilot.md`「投稿成果物のstatus」章を参照)。

`tomo-angel7-pilot-01`は、正本成果物ファイル(`pilot-drafts/tomo-angel7-pilot-01.md`)側のstatusが`ceo_review`であることに合わせ、本表も`ceo_review`へ同期した(2026-07-14)。第2〜第6投稿は未着手のため`planned`のまま維持している。
