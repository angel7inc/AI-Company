# pilot-approved ― パイロット投稿CEO承認済み成果物領域

`_shared/sop/instagram/carousel-post-pilot.md`に基づくパイロット投稿の**CEO承認後(status: approved以降)の正本**を置く場所です。

## 正本ルール
- CEO承認後(status: `approved`〜`archived`)は本フォルダが正本
- `../pilot-drafts/`側の元ファイルは、本フォルダへの移動後にstatus: `archived`とするか、本フォルダ側への参照のみを残す

## 承認記録との関係
各承認済み成果物には、`../templates/carousel-ceo-approval-template.yaml`に基づく承認記録を紐付ける。承認記録には顧客情報・DM本文・LINE登録者情報・APIキー・アクセストークン・パスワードを直接保存しない。承認後に本文・キャプション・画像・投稿先・公開範囲・CTA・投稿日時のいずれかが変更された場合、承認は無効化し再承認とする。

## Git管理について
本フォルダはテキストベースの承認済み原稿・承認記録を含み、機密の個人情報・実KPI数値は含まないため、Git管理対象とする。

## 現在の状態
2026-07-13時点でCEO承認済みの投稿成果物は未作成(Gate2は構造・テンプレートの整備までのため)。
