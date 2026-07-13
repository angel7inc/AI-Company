# Outbound Action Approval Policy(社外向けアクション承認方針)

## 位置づけ
本ポリシーは、AI Company全体における「社外に影響する、または社外へ情報を送信するアクション」に対する共通ルールを定める全社方針です。個別のDivision・Agent・SOP(`_shared/sop/publishing/`・`_shared/sop/automation/`等)は、本ポリシーを上位フレームワークとして参照し、各自のチャネル固有の手順を定めます。既存の個別ルール(Instagram投稿承認・API/MCP接続承認等)は本ポリシーの一事例であり、矛盾する場合は本ポリシーを優先します。

## 対象領域(最低10分野)
本ポリシーは以下の10分野における全てのAgent・SOPに適用されます。
1. Instagram
2. WordPress
3. LINE
4. メール(Email)
5. DM(ダイレクトメッセージ)
6. 広告(Ads)
7. CRM(顧客管理システム)
8. 外部API(External API)
9. 予約(Booking)
10. 決済(Payment)

新たなチャネル・外部連携を追加する場合も、本ポリシーの対象領域として扱います。

## prepare(準備)と execute(実行)の定義

### prepare(準備)
社内での下書き・提案作成のみを行い、**社外の状態を一切変更しない**作業。CEO承認なしで実施可能です。
- 例: 投稿文・記事・メール文面の下書き作成、配信対象リストの提案、広告クリエイティブ案の作成、CRMへの提案的なタグ付け案の作成(実際の反映は含まない)

### execute(実行)
社外の状態を変更する、または社外へ情報を送信する作業。**CEO承認記録が無い限り、実行は禁止(default-deny)**です。以下を含みます。
- 公開(publish)・投稿(post)
- 送信(send)・配信(deliver)
- 外部APIへの書き込み(external API write)
- 外部システムの設定変更(external system configuration change)
- 広告出稿(ad placement)
- 顧客への返信(customer reply)
- 予約確定(booking confirmation)
- 決済操作(payment operation)

## default-deny原則
executeに該当するアクションは、**有効なCEO承認記録が確認できない限り、常に禁止**とします。以下のいずれかに該当する場合も「承認記録なし」として扱い、実行してはいけません。
- 承認記録が存在しない、または参照できない
- 承認記録の有効期限(`expires_at`)が切れている
- 承認後に内容(content)・送信先(target)・金額(amount)・範囲(scope)のいずれかが変更された(この場合は再承認が必要)

技術的な承認検証の仕組み(承認記録を自動確認する仕組み)が存在しない場合、Agentは自動実行を有効化してはならず、CEOによる手動確認を待つものとします。

## 承認記録(approval record)の最小フィールド
承認記録は以下のフィールドを最低限含みます。
- `action_id`: アクションの一意識別子
- `action_type`: prepare/executeの別、および具体的な種別(publish/send/booking-confirm等)
- `target`: 対象(投稿先アカウント・送信先リスト・APIエンドポイント等)
- `content_reference` または `content_hash`: 対象コンテンツへの参照ID、またはハッシュ値
- `approved_by`: 承認者(CEO)
- `approved_at`: 承認日時
- `expires_at`: 有効期限
- `status`: 承認状態(pending/approved/expired/revoked等)

### 承認記録に含めてはいけない情報
承認記録には以下を直接保存してはいけません。
- 顧客情報(氏名・連絡先・相談内容等)
- メッセージ本文そのもの
- APIキー・アクセストークン・パスワード等の認証情報

コンテンツ本体は`content_reference`(参照ID)経由で別途管理し、承認記録自体は参照情報のみを保持します。

## 外部APIの read-only と state-changing execute の区別
- **read-only(読み取り専用)**: 公開情報の取得、許可された範囲のデータ読み取り、状態を変更しない分析処理。原則としてprepareに準じ、CEO承認なしで実施可能です。
- **state-changing execute(状態変更を伴う実行)**: 作成(create)・更新(update)・削除(delete)・公開(publish)・送信(send)・予約確定(booking confirm)・決済(payment)・設定変更(config change)。CEO承認が必須です。
- **注意**: 名目上read-onlyであっても、顧客情報・財務情報・機密情報を外部へ送信する処理(例: 顧客データを外部APIへ問い合わせ目的で送信する等)は、単純なread-onlyとして扱ってはいけません。この種の処理は明示的に承認されるか、明示的に禁止されるかのいずれかとします。

## 各Agentのprepare/execute権限

### revenue-crm-manager
- LINE配信・ステップ配信・顧客管理・セグメント管理・再購入施策の**準備(prepare)**および**承認申請の提出**を担当する。
- **execute(実際の配信・送信)は、有効なCEO承認記録が確認された場合のみ**実施する。
- 現時点で承認記録を技術的に自動検証する仕組みが存在しないため、自動実行(auto-execute)を有効化してはならず、**手動確認を待つ**ものとする。

### seo-wordpress-content
- 記事の準備(タイトル・見出し・メタ情報・ALT等の作成)のみを担当する。**publish/公開はprepareの範囲外であり、実施しない**。

### seo-technical
- サイト設定変更・本番公開環境への反映は**execute**に該当し、**CEO承認が必須**である。

### Architecture Review / QA / Quality Checker系Agent
- 検出・報告のみを担当し、社外への実行権限(execute権限)は**一切付与されない**。これは既存の制約(是正・削除・改名・移動を行わない)を、社外アクションの文脈でも再確認するものである。

## 関連ドキュメント
- `_shared/sop/publishing/README.md`
- `_shared/sop/automation/README.md`
- `_shared/sop/management/ceo-approval-flow.md`
