# AI Company 運用継続性Runbook(ログ・障害対応・バックアップ／復元基盤)

## 位置づけ
本Runbookは、[`ai-company-local-operations.md`](ai-company-local-operations.md)を補完し、作業履歴の記録方法・障害記録方法・Git管理対象外データのバックアップ区分・復元手順を定義する。**今回の目的は自動監視システムや自動バックアップを実装することではなく、現状の人間主導運用に対応した最小限で安全な記録・停止・復元ルールを作ることである。** 技術的に自動化されている機構は今回何も実装していない。実ログ・実障害記録・実バックアップも今回は作成しない。作成するのは本文書、空のテンプレート(`_company/operations/templates/`配下)、`.gitignore`の除外規則のみである。

秘密情報の扱いは[`secret-management-policy.md`](../../_company/charter/secret-management-policy.md)を、役割分担は[`tool-role-responsibility.md`](../../_company/charter/tool-role-responsibility.md)を正本として参照する(本文書では重複記載しない)。

## 1. 記録の3種類
現状、Git履歴・作業ログ・障害記録は目的が異なり、**Git履歴だけで作業ログや障害記録を完全に代替することはできない。**

| 種類 | 対象 | 正本 |
|---|---|---|
| A. Git履歴 | Git管理対象ファイルの変更・コミット・push・差分 | GitおよびGitHub |
| B. 作業ログ | 実施task・作業者・参照したAgent定義・確認/変更したファイル・実施した検証・CEO承認待ちか・次回再開地点・外部接続の有無 | 将来のローカル作業ログ領域(`_company/operations/local/logs/`) |
| C. 障害記録 | エラー・意図しない変更・秘密情報事故・外部execute失敗・Git競合・状態不一致・二重実行疑い・データ破損・復元作業 | 将来のローカル障害記録領域(`_company/operations/local/incidents/`) |

## 2. ローカル記録領域とGit管理範囲
実ログ・実障害記録には、ローカルパス・エラー内容等の運用情報が含まれ得るため、実記録は原則Git管理対象外とする。

- `_company/operations/local/logs/` ― 実作業ログ(Git管理対象外)
- `_company/operations/local/incidents/` ― 実障害記録(Git管理対象外)
- `_company/operations/local/backup-manifests/` ― 実バックアップ台帳(Git管理対象外)
- `_company/operations/templates/` ― テンプレート(Git管理対象。実在する作業情報・秘密情報・個人情報は含めない)

今回、`_company/operations/local/`配下に実ファイルは作成していない。`.gitignore`には除外規則のみを追加した。

## 3. 作業ログテンプレート
[`_company/operations/templates/operation-log-template.yaml`](../../_company/operations/templates/operation-log-template.yaml)を参照する。`sample_only: true`のテンプレートであり、実在するユーザー名・メールアドレス・コミットID・作業記録は入力していない。秘密情報・個人情報を記録する項目は設けていない。

## 4. 障害記録テンプレート
[`_company/operations/templates/incident-record-template.yaml`](../../_company/operations/templates/incident-record-template.yaml)を参照する。`description_sanitized`は秘密値・個人情報を除去した説明のみとする。APIキー・アクセストークン・パスワード・Cookie・秘密鍵・二要素認証コード・顧客氏名・DM本文・相談本文・個人特定情報・秘密情報を含むURLは、いかなる項目にも記録しない。

## 5. 障害の重大度(severity)
既存の重大度定義は本リポジトリ内に確認できなかったため、以下の4段階を新たに提案する。

| 重大度 | 定義 |
|---|---|
| low | 作業を継続可能な軽微な問題。外部execute・秘密情報・データ破損への影響なし |
| medium | 作業を一時停止して確認が必要。限定的なファイル不一致や検証失敗等 |
| high | 外部execute・機密情報・複数成果物・本番運用へ影響する可能性がある |
| critical | 秘密情報漏えい、無承認公開、重大な個人情報事故、広範なデータ破損等 |

**high・criticalの場合、作業停止・外部execute停止・CEOへの即時報告を必須とする。** 自動通知機構は存在しないため、これは人間が判断・報告する手動ルールである。

`incident-record-template.yaml`の`status`は`open`/`contained`/`recovering`/`resolved`/`closed`の5値を用いる。既存の列挙値規則と衝突する場合は、本Runbookでは追加せず別途報告する。

## 6. フェイルクローズ
以下を検出した場合、原則として作業を停止する。

- working treeに意図しない変更
- 対象外ファイルの変更
- Git競合
- HEADとorigin/mainの想定外不一致
- 正本とcalendarのstatus不一致
- 秘密情報らしき値
- 個人情報らしき内容
- 承認記録不足
- `content_hash`不一致
- `asset_reference`未確定のまま公開工程へ進もうとした
- `public_release_approved`が`false`
- 外部executeが依頼範囲外
- 同一`action_id`の二重実行疑い
- YAMLまたはMarkdownの破損
- 復元元が不明
- バックアップの完全性を確認できない

**停止時の順序:**
1. 変更を広げない
2. 外部executeを止める
3. 現状を読み取り専用で確認する
4. ログまたは障害記録へ事実を残す
5. CEOへ報告する
6. 明示承認なしに削除・`reset`・`revert`・force push・履歴書き換えを行わない

## 7. 再試行と二重実行防止
自動再試行は存在しない。手動で再試行する場合の原則は以下のとおり。

- 最初の失敗原因を確認する
- 同じ操作が外部で完了していないか確認する
- `action_id`または投稿IDを確認する
- 公開済み・送信済み・決済済み等の可能性がある場合、確認できるまで再実行しない
- 内容・対象・投稿先・公開範囲・日時を再確認する
- CEO承認が有効か確認する
- 必要なら新しい承認を受ける
- 再試行結果を記録する

外部executeのタイムアウトを単純に「失敗」と判断して再実行しない。**将来API接続時にはidempotency key等の仕組みを検討する必要があるが、今回idempotency機構は実装していない(将来課題)。**

## 8. バックアップ対象の分類

| 区分 | 例 | 主なバックアップ |
|---|---|---|
| A. Git管理対象 | Markdown・YAML・Agent定義・SOP・Brand Layer・Knowledge・テンプレート・Runbook | GitHub Private Repository |
| B. Git管理対象外の機密・実績データ | `_company/kpi/actuals/`、`_company/finance/actuals/`、`_company/reports/confidential/`、ローカル`.env`、認証情報、実ログ、障害記録、バックアップ台帳 | GitHubからは復元できない |
| C. 外部サービス内の資産 | Canvaデザイン、固定6枚目画像、Instagram投稿、LINE設定、WordPress、ココナラ情報、Meta Business Suite設定 | サービスごとの別バックアップまたは書き出しが必要 |
| D. 完成画像・制作素材 | PNG、JPEG、Canva書き出し画像、ブランド素材、ロゴ、固定誘導画像 | 保存先未定(下記参照) |

## 9. バックアップ保存先(現時点、いずれも未決定)
特定のクラウドストレージ・製品は今回選定しない。保存先が未決定であることを完成済みと評価しない。

```yaml
git_managed_assets_backup: "GitHub Private Repository"
selected_local_data_backup_destination: not_decided
selected_creative_asset_backup_destination: not_decided
selected_password_manager: not_decided   # secret-management-policy.mdと同一値
```

保存先を決定する際は、暗号化・アクセス権・復元可能性を必須条件として確認すること。

## 10. バックアップ頻度
根拠のない固定頻度は強制しない。

- **Git管理対象:** 重要な変更をCEO承認後にcommitし、明示承認後にGitHubへpushする(既存のGate1/Gate2運用のまま)。
- **Git管理外データ:** 実データの更新頻度・データ量・重要性を確認したうえで、CEOが頻度を決定する(現時点では未確定)。
- **Canva・完成画像:** 投稿公開前または公開時に、最終版を識別可能な形で保存する案を提案する(具体的な保存先・方式はCEO確認後)。
- **復元テスト:** 環境構築完了後に初回テストを行い、その後の頻度はCEO判断とする。

日次・週次・月次等の具体的頻度は、今回確定事項として設定していない。

## 11. バックアップ台帳(backup-manifest)テンプレート項目
実際の台帳ファイルは今回作成しない。将来`_company/operations/local/backup-manifests/`配下に置く台帳の最低項目を、ここに定義のみ行う。

```yaml
# 項目定義のみ(実在する台帳レコードではない)
backup_id: null
created_at: null
created_by: null
asset_category: null           # A/B/C/D(上記「8. バックアップ対象の分類」参照)
source_location: null
destination_type: null
destination_reference: null
encryption_status: null
access_control: null
included_scope: null
excluded_scope: null
source_version: null
checksum_available: null
restore_test_status: null
restore_tested_at: null
retention_status: null
notes: null
```

台帳には、秘密値・顧客情報・実際のパスワード等を保存しない。

## 12. 復元手順
1. 障害範囲を確認する
2. 外部executeを停止する
3. 復元対象と正本を特定する
4. 現在状態を保全する
5. CEOへ復元計画を報告する
6. CEO承認を得る
7. 安全な復元先または別ディレクトリで復元する(現在の作業ディレクトリへいきなり上書きしない)
8. 差分を確認する
9. 秘密情報を承認済み保存先から別途安全に設定する
10. Git管理外データを承認済みバックアップから復元する
11. Canva等の外部資産を別途確認する
12. status・calendar・承認状態を確認する
13. 読み取り専用検証を行う
14. 投稿しない空運転を行う
15. CEOの再開承認を得る

`reset`・force checkout・履歴書き換え等は、CEO承認なしに行わない。

## 13. 復元テストの合格条件(実施は今回行わない)
- Private Repositoryを取得できる
- `main`ブランチを確認できる
- 最新コミットを確認できる
- Git管理対象ファイルが復元される
- `.env`等の秘密情報がGitから復元されない
- KPI実績等のGit除外データがGitHubから復元されないことを理解している
- 秘密情報を承認済み保存先から別途設定できる
- Git管理外データを別途復元できる
- Canva素材等の所在を特定できる
- 対象成果物のstatusを確認できる
- calendarとの同期を確認できる
- 外部executeなしで空運転できる
- CEO承認地点で停止できる

## 14. PC故障・紛失時
1. 外部処理を停止する
2. CEOへ報告する
3. GitHub・外部サービス・秘密情報への不正アクセスリスクを確認する
4. 必要に応じて認証情報を失効する(`secret-management-policy.md`のローテーション手順に従う)
5. 新PCで[`ai-company-local-operations.md`](ai-company-local-operations.md)のRunbookに従いcloneする
6. 秘密情報を安全な保存先から再設定する
7. Git管理外データをバックアップから復元する
8. 外部サービスのログイン履歴を確認する
9. 空運転後に再開する

端末暗号化の有無は現在確認できないため、有効であると断定しない。

```yaml
device_encryption_status: confirmation_required
```

## 15. GitHubだけでは復元できないもの
- `.env`
- APIキーやトークン
- KPI実数値
- Finance実数値
- 機密レポート
- 実ログ
- 障害記録
- ローカルバックアップ台帳
- Canvaデザイン
- 固定6枚目画像
- 書き出した完成画像
- 外部サービスの設定
- WordPressデータベース
- LINE設定
- Meta設定
- 外部サービス内の投稿履歴

**具体例:** 第1投稿(`tomo-angel7-pilot-01`)の固定6枚目画像がGitHub外(CEOのローカル環境・Canva等)にのみ存在する場合、GitHubのcloneだけでは第1投稿を完成させることはできない。別途、CEOによる画像そのものの提供・確認が必要である。

## 16. 現在保留中の第1投稿
- `pilot_post_id`: `tomo-angel7-pilot-01`
- `status`: `ceo_review`
- `human_review_required`: `true`
- `public_release_approved`: `false`
- `asset_reference`: `pending_ceo_asset_identification`

本Runbook作成にあたり、第1投稿ドラフト・calendar・SOPは変更していない。

## 17. 変更履歴
| 版 | 日付 | 変更内容 | 変更者 |
|---|---|---|---|
| 1 | 2026-07-14 | 初版作成(環境構築Gate2 Batch F) | COO |
