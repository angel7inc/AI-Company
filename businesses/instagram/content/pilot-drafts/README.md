# pilot-drafts ― パイロット投稿ドラフト領域

`_shared/sop/instagram/carousel-post-pilot.md`に基づくパイロット投稿の**作成中(status: draft〜qa_passed)の成果物**を置く場所です。

## 正本ルール
- 作成中(status: `draft`〜`qa_passed`)は本フォルダが正本
- CEO承認後(status: `approved`以降)は`../pilot-approved/`が正本となり、本フォルダ側の元ファイルはstatus: `archived`とするか`pilot-approved/`側への参照のみを残す(同一成果物が複数の正本にならないようにする)

## 使用するテンプレート
`../templates/carousel-pilot-brief-template.md`をコピーして1投稿ごとに1ファイルを作成する。

## Git管理について
本フォルダはテキストベースの企画・原稿であり、顧客情報や実KPI数値のような機密情報は含まないため、Git管理対象とする(`_company/kpi/actuals/`のような除外対象ではない)。ただし、テーマ調査の根拠として実際の相談内容を扱う場合は、個人が特定できないカテゴリ・集計情報へ変換したうえで記載すること。

## 現在の状態
2026-07-13時点で実際の投稿原稿は未作成(Gate2は構造・テンプレートの整備までのため)。
