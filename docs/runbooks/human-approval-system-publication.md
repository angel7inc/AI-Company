# AI Company 人間承認・システム公開モデル Runbook(全社共通)

## 位置づけ
本Runbookは、AI Company全体における外部公開コンテンツ(Instagramカルーセル・リール、Threads、WordPress SEO記事、将来のYouTube・TikTok、LINE配信、その他の外部公開コンテンツ)の**標準運用モデル**を定める全社共通文書である。[`_company/charter/tool-role-responsibility.md`](../../_company/charter/tool-role-responsibility.md)(役割分担)・[`_company/charter/outbound-action-policy.md`](../../_company/charter/outbound-action-policy.md)(承認記録スキーマ・default-deny原則)を上位方針として参照し、両者と矛盾しない(むしろ具体化する)運用ルールを定める。矛盾する場合は`outbound-action-policy.md`を優先する。

既存の個別SOP(例: [`_shared/sop/instagram/carousel-post-pilot.md`](../../_shared/sop/instagram/carousel-post-pilot.md))・個別Division設計書(例: [`docs/phase2-instagram-design.md`](../phase2-instagram-design.md))は、本Runbookの標準モデルの一事例として整合させる。個別文書と本Runbookが矛盾する場合、本Runbookを優先する。

**本方針は、実際には存在しない機能を存在するように記載しない。** 本追記時点(2026-07-18)で、いずれの媒体についても「システムが自動的に外部公開を実行する」実装(投稿実行アダプタ・connector・publisher)は存在しない。本Runbookが定めるのは**目標とする標準運用モデル**であり、媒体ごとの実装状況・準備状況は別途正直に記録する(Instagramの現状は[`docs/phase2-instagram-design.md`](../phase2-instagram-design.md)「17. 投稿実行基盤(Instagram Publisher)準備状況」を参照)。実装が完了していない媒体では、CEO承認後も実際の公開実行手段が存在しないため、当面は人間による手動実行(本Runbック「6. 手動操作の位置付け」参照)のみが技術的に可能な手段である。

## 1. 標準ライフサイクル

```
research(リサーチ)
  → planning(企画)
  → creation(制作: 本文・画像・キャプション等)
  → automated checks(自動チェック: QA・事実確認・権利確認等)
  → publication candidate(投稿候補パッケージの確定)
  → human final review(人間による最終確認)
  → revision(修正) または CEO approval(CEO承認)
  → system publication(システムによる公開実行)
  → publication verification(公開後検証)
  → KPI measurement(KPI計測)
  → optimization(改善)
```

AI・各担当Agentが「research」から「publication candidate」までを完成させる。人間(CEO)が行うのは「human final review」の確認と、修正不要な場合の「CEO approval」のみである。承認後の「system publication」は、投稿実行基盤(システム)が担当する。これは既存の`_company/charter/outbound-action-policy.md`のprepare(準備)/execute(実行)区分に対応する。「research」〜「publication candidate」はprepare、「system publication」はexecuteに該当する。

## 2. 人間(CEO)による最終確認の範囲

人間は、公開直前の完成物について以下を確認する。

- 内容
- デザイン
- 誤字
- 事実関係
- 権利・広告表現
- CTA
- 対象アカウント・サイト
- 公開範囲

## 3. 修正がある場合のフロー

1. CEOが修正内容を指定する
2. AI側(担当Agent)が修正する
3. 修正後の内容に対して新しいハッシュを発行する(`content_hash`・画像/キャプションのSHA-256等)
4. 再度人間確認を行う
5. 新しい承認を取得する(旧ハッシュに対する承認は再利用しない)

## 4. 修正がない場合のフロー

1. CEOがOK承認する
2. 承認対象(画像・キャプション・投稿順・対象アカウント・プロフィール導線等)をハッシュで固定する
3. システムが外部公開を実行する(投稿実行基盤が実装・接続済みの場合)
4. 公開結果(投稿ID・URL・公開日時・エラー有無)を自動記録する
5. KPI計測を開始する

## 5. 承認の再利用条件

CEOによる最終承認は、承認時点でハッシュ固定された対象(画像・キャプション・投稿順・対象アカウント・プロフィール導線等)に対してのみ有効である。

- 投稿実行基盤が接続・検証された後、**承認済みパッケージに変更がない限り、同じ内容について再度CEO承認を求めずシステムが実行できる**。
- 画像、文章、順序、対象アカウント、導線のいずれかが変更された場合は、旧承認は無効とし、上記「3. 修正がある場合のフロー」に従って再承認を必要とする。

## 6. 手動操作の位置付け

以下は**標準運用ではない**(禁止ではなく、標準の手段として使わないという位置付け)。

- CEOが毎回画像を選択する
- CEOがキャプションをコピーする
- CEOが投稿画面へ貼り付ける
- CEOが投稿ボタンを押す
- CEOが公開URLを手作業で記録する
- CEOがSEO記事をWordPressへ手入力する

手動操作は、以下の場合にのみ許可する例外的対応(緊急フォールバック)とする。

- 公式APIまたは投稿基盤の障害
- 認証障害
- 緊急訂正
- システム停止中の臨時対応
- CEOが個別に手動実行を指示した場合

**現時点では、いずれの媒体についても投稿実行基盤(システム)が未実装であるため、実質的に手動操作が技術的に可能な唯一の実行手段である。** これは「手動操作が標準運用である」ことを意味しない。投稿実行基盤の実装が完了するまでの間の、目標モデルへの移行過渡期における必要措置として扱う。

## 7. 対象媒体

本Runbookは、最低限以下の媒体へ適用する。今後追加される外部公開チャネルも、本Runbookの対象として扱う。

- Instagramカルーセル
- Instagramリール
- Threads
- WordPress SEO記事
- 将来のYouTube・TikTok
- LINE配信
- その他の外部公開コンテンツ

## 8. 例外

以下の場合、標準の「system publication」ではなく手動フォールバックを用いてよい。

- API障害
- 緊急訂正
- システム停止
- CEOが個別に手動実行を指定した場合

## 9. 限定的パイロット運用(投稿実行基盤の実装前の暫定措置)(2026-07-18追記)

「system publication」を実現する投稿実行基盤(publisher/connector)が未実装のチャネルでは、CEOがFable 5への個別委任による経営判断提案を採用し、以下の暫定運用を正式に認める。

- 対象チャネルの**最初の投稿5件に限り**、「AI・システムが公開候補パッケージを完成させる → CEOが最終確認・OK承認する → CEOが投稿操作のみを手動で実行する」という限定的パイロット方式を用いてよい(`execution_model: pilot_human_execution_after_ai_preparation`)。
- CEOが行うのは投稿操作(ボタン押下・投稿URL/時刻の取得)のみとする。画像制作・文章作成・キャプション作成・ハッシュタグ選定・投稿情報の再入力/再編集は、パイロット期間中もAI・システム側が担当し、CEOへ移管しない。
- 「system publication」(投稿実行アダプタによるシステム実行)は目標(`permanent_target_model`)として維持し、撤回しない。投稿実行アダプタの実装計画は破棄せず、トリガー到達後にそのまま着手できる状態で保管する。

**投稿実行アダプタ実装開始のトリガー条件(いずれか最初に満たした時点):**
1. 対象チャネルの投稿の累計公開数が5件に到達
2. 週2投稿以上を2週間連続で実施
3. 投稿操作および投稿結果記録に費やす時間が週30分を超過
4. 予約投稿または複数アカウント管理が必要になった
5. 手動投稿がコンテンツ公開頻度の明確なボトルネックになった

トリガー到達前であっても、CEOが個別に指示した場合は投稿実行アダプタの実装に着手できる。トリガー到達後は、本セクションの暫定運用を終了し、「6. 手動操作の位置付け」の緊急フォールバックのみの運用へ移行する。

Instagramでの適用状況の正本は[`businesses/instagram/content/pilot-drafts/tomo-angel7-pilot-01-review.yaml`](../../businesses/instagram/content/pilot-drafts/tomo-angel7-pilot-01-review.yaml)「pilot_manual_publication_authorization」を参照する。

## 10. リサーチ工程の自動化方針(2026-07-18追記)

会社共通のリサーチ方針として、以下を維持・記録する。

- キーワードリサーチツール(ラッコキーワード等)は、**最初からAPIまたはMCP接続を標準方針とする**。CEOによるCSVダウンロード・手渡しを標準工程にしない。
- Google Trends・検索結果・Search Console等のデータと統合する。
- Instagram第2投稿以降は、リサーチ工程を制作前に必ず通す。
- InstagramとSEO記事(WordPress)へ、同じリサーチ結果を展開可能にする(チャネルごとにリサーチを重複させない)。

本追記時点で、実際のAPIまたはMCP接続は行っていない。方針の記録のみである。実装状況は必要に応じて別途記録する。

## 11. 関連文書

- [`_company/charter/outbound-action-policy.md`](../../_company/charter/outbound-action-policy.md) ― 承認記録スキーマ・default-deny原則(上位方針)
- [`_company/charter/tool-role-responsibility.md`](../../_company/charter/tool-role-responsibility.md) ― 役割分担(上位方針)
- [`_shared/sop/instagram/carousel-post-pilot.md`](../../_shared/sop/instagram/carousel-post-pilot.md) ― Instagramカルーセル投稿の8段階status運用(個別事例)
- [`docs/phase2-instagram-design.md`](../phase2-instagram-design.md) ― Instagram事業設計・投稿実行基盤の準備状況・次Batch実装計画
- `businesses/instagram/content/templates/carousel-ceo-approval-template.yaml` ― Outbound Action Approval記録テンプレート(sample_only)
- [`businesses/instagram/content/pilot-drafts/tomo-angel7-pilot-01-review.yaml`](../../businesses/instagram/content/pilot-drafts/tomo-angel7-pilot-01-review.yaml) ― 第1投稿の限定的パイロット運用の正本(`pilot_manual_publication_authorization`)
