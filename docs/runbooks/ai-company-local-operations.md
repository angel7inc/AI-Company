# AI Company ローカル運用Runbook

## 位置づけ
本Runbookは、CEOまたは許可された担当者が、別PCまたは再起動後でも安全に作業を再開できるようにするための、現状事実に基づく最低限の手順書である。**実際には存在しない機能を存在するように記載しない。** 実行コード・Agent自動オーケストレーション・Dashboard・API接続・MCP接続の追加は本Runbookの対象外であり、将来それらを導入する際の別Batchで扱う。

役割分担の詳細は[`_company/charter/tool-role-responsibility.md`](../../_company/charter/tool-role-responsibility.md)を参照する。秘密情報の扱いは[`_company/charter/secret-management-policy.md`](../../_company/charter/secret-management-policy.md)を正本とし、本Runbookでは要約のみを記載する。作業ログ・障害記録・バックアップ/復元の詳細は[`operations-continuity.md`](operations-continuity.md)を参照する。

## 1. 現在の運用形態(事実)
- 本リポジトリは実行アプリケーションではない。`package.json`・`requirements.txt`等の実行依存関係定義は存在しない。
- `_company/org/agents.yaml`に登録された48件のAgentは、いずれもMarkdownで書かれた役割定義であり、独立した実行プロセスではない。
- Agentを自動選択する機構、Agent間で自動通信する機構は存在しない。
- 専用Dashboardは存在しない。
- 外部API・外部サービスへの接続は、`filesystem-git`(Claude Code自体のリポジトリアクセス)を除き、原則未接続(`candidate`または`planned`)である。
- 実際の運用は、Claude Code上でCEOが指示を入力し、COO役のClaude CodeがAgent定義・SOPを参照しながら、許可された範囲のファイルを読み書きする形で行われている。

## 2. ローカル環境情報(2026-07-14、現PCで読み取り専用確認)

| 項目 | 現在検出された値 | 備考 |
|---|---|---|
| OS / シェル | Windows(MINGW64/Git Bash上で確認) | PowerShellも利用可 |
| Git | 2.55.0.windows.2 | |
| Node.js | v24.18.0 | 現時点で実行コードなし。将来の自動化導入時に必要になる可能性 |
| npm | 11.16.0 | 同上 |
| pnpm | not_detected | 現時点では not_required_currently |
| yarn | not_detected | 現時点では not_required_currently |
| Python | not_detected | 現時点では not_required_currently |
| Docker | not_detected | 現時点では not_required_currently |
| Claude Code | 2.1.207 | |
| VS Code | 1.128.0 | |
| リポジトリのローカルパス | `c:\Development\AI-Company` | PCにより異なり得る |
| GitHub origin | `https://github.com/angel7inc/AI-Company.git` | |
| mainブランチ | `main` | |
| 文字コード | UTF-8 | 主要ファイルで確認 |
| 改行コード | LF | 主要ファイルで確認(CRLFではない) |
| タイムゾーン | UTC+0900(JST) | 現PCの設定値。将来複数拠点運用時は`AI_COMPANY_TIMEZONE`(`.env.example`参照、現時点未使用)で明示化を検討 |

**現時点の最低要件:** Git、テキストエディタ(VS Code等)、Claude Code、GitHub Private Repositoryへのアクセス権。これだけで現状の運用(ドキュメント読み書き・Git操作)は再現できる。

**将来の自動化導入時に必要になる可能性があるもの(現時点では不要):** Node.js/npm(または将来選定するランタイム)、Python、Docker、pnpm/yarn。これらは実行コードが実際に追加されるDivision別Batchで、必要性が確定した時点で要件化する。

## 3. 環境バージョンの扱い
- `current_detected_version`: 上表のとおり、現PCで確認された値。
- `minimum_supported_version`: 現時点では未定義(バージョン固定の必要性が生じていないため)。
- `version_pin_status`: `not_implemented`(`.nvmrc`・`.tool-versions`・`package.json`のenginesフィールド等によるバージョン固定は今回新設しない)。

バージョン固定は、実行コードを実際に導入するBatch(自動化Division関連)の候補として扱う。

## 4. リポジトリの取得と初期確認(別PC・再構築時)
実際のGitHub認証情報・秘密値はここに記載しない。clone URLは公開可能なリポジトリURLのみ使用する。

1. Gitを準備する(インストール済みであることを確認する)。
2. GitHub認証を行う(CEOのGitHubアカウントで、承認済みの方法によりログインする)。
3. Private Repositoryへアクセスできることを確認する(`https://github.com/angel7inc/AI-Company` を開けるか確認する)。
4. リポジトリをcloneする: `git clone https://github.com/angel7inc/AI-Company.git`
5. `main`ブランチへ移動する: `git checkout main`
6. originを確認する: `git remote -v`
7. 最新状態を取得する: `git pull origin main`
8. `git status -sb` で作業ツリーの状態を確認する。
9. `git log -1 --oneline` で最新コミットを確認する。
10. `.env`・`.env.local`・`.env.production`等の秘密情報ファイルが、cloneしても取得されないこと(`.gitignore`により除外されていること)を確認する。
11. Claude Codeからリポジトリのフォルダを開く。
12. 会社構造(`README.md`)、Agent数(`_company/org/agents.yaml`)、SOP数(`_shared/sop/sop-index.md`)を確認する。
13. 今回の作業対象ファイルの正本がどれかを確認する(重複した草稿ファイルを新たな正本と誤認しない)。
14. CEOの指示があるまで、いかなるファイルも変更しない。

## 5. 日常の作業開始手順
作業を始める前に、以下を読み取り専用コマンドで確認する。

| 確認項目 | コマンド | 何を確認するか |
|---|---|---|
| 現在のディレクトリ | `pwd` | 今どのフォルダで作業しようとしているか |
| 現在のブランチ | `git branch --show-current` | `main`以外のブランチで誤って作業していないか |
| 作業ツリーの状態 | `git status -sb` | 未コミットの変更が残っていないか |
| 最新コミット | `git log -1 --oneline` | 直前の作業内容を思い出せるか |
| ローカルの現在地点 | `git rev-parse HEAD` | ローカルが指しているコミットのフルID |
| リモートの現在地点 | `git rev-parse origin/main` | GitHub側が指しているコミットのフルID(HEADと比較する) |
| リモートURL | `git remote -v` | 接続先リポジトリを誤認していないか |

追加で確認すること: 対象成果物の`status`(該当ファイルのfrontmatter等)、対象ファイルの正本がどれか、CEO承認の有無、禁止されている次工程(直前のCEO指示の停止条件)、秘密情報がここまでの会話やファイルに入力されていないこと。

## 6. 日常の作業終了手順
作業を終える前に、以下を確認する。

- 変更ファイル一覧(`git status -s`)
- 意図しない変更が含まれていないか(`git diff`で許可範囲外の変更がないか)
- `git diff --check`(空白エラー等の混入がないか)
- 秘密情報が変更内容に混入していないか
- 変更内容がCEO承認範囲と一致しているか
- コミットの有無
- GitHub pushの有無
- HEADとorigin/mainが一致しているか(`git rev-parse HEAD`と`git rev-parse origin/main`)
- working treeの状態

以下の3状態を区別して報告する。
1. **未コミット変更がある状態**: `git status -s`に変更が表示される。
2. **コミット済みだが未push**: `git status -sb`が`ahead`を表示する。
3. **push済みでclean**: `git status -sb`が`## main...origin/main`のみを表示し、`git rev-parse HEAD`と`git rev-parse origin/main`が一致する。

次回再開地点、保留中の条件、禁止されている次工程を明記して終える。

## 7. 停止と再開のルール

| 状態 | 確認コマンド | 再開してよい条件 | 実行してはいけない操作 | CEO確認 | 正本とする記録 |
|---|---|---|---|---|---|
| A: 変更前、working tree clean | `git status -sb` | いつでも新規作業を開始してよい | ― | 新規作業の指示のみ必要 | `git log` |
| B: ファイル変更済み、未コミット | `git status -s`, `git diff` | 変更内容がCEO承認範囲と一致していることを確認後 | 承認範囲外への変更拡大 | コミット前に必要な場合あり | 変更中のファイル自体 |
| C: コミット済み、未push | `git log -1`, `git status -sb` | CEOがpushを明示的に許可した後 | 許可なしのpush | push可否について必要 | コミットログ |
| D: GitHub push済み、HEADとorigin一致 | `git rev-parse HEAD`, `git rev-parse origin/main` | 次のCEO指示を待つ、または新規作業の指示があれば開始 | 指示なしの次Batch着手 | 次工程について必要 | GitHub上のコミット |
| E: CEO承認待ち | 直前のCEO指示内容を確認 | CEOから承認・却下の返信があった後 | 承認前の実装継続・先行コミット | 必須(承認そのもの) | CEOの承認メッセージ |
| F: 外部サービスまたは素材確認待ち(例: 第1投稿の固定6枚目画像) | 該当成果物ファイルのfrontmatter(`asset_reference`等) | 必要な素材・情報がCEOから提供された後 | 素材内容の推測による作業続行 | 必須 | 該当成果物ファイル |
| G: 秘密情報事故・セキュリティ停止中 | `secret-management-policy.md`の手順 | 失効・交換等の対応が完了し、CEOが再開を許可した後 | 通常作業の継続、対応前の追加コミット・push | 必須(全ステップ) | `secret-management-policy.md`の対応記録 |

## 8. 役割分担
詳細は[`_company/charter/tool-role-responsibility.md`](../../_company/charter/tool-role-responsibility.md)を正本として参照する(本Runbookでは重複記載しない)。

## 9. Agentの手動呼び出し方法
現在の実態(Agentは独立実行するソフトウェアではなく、参照される役割定義である)に合わせ、以下の手順で「適用」する。「Agentを起動する」という独立プロセスが動くような表現ではなく、「Agent定義を参照して役割を適用する」という表現を用いる。

1. CEOが目的を提示する。
2. GPTまたはClaude Codeが、必要になりそうなAgent定義の候補を`_company/org/agents.yaml`から整理する。
3. core-coo(COO役のClaude Code)が、候補Agentの定義ファイル(`_company/agents/`・`_shared/agents/`・`businesses/{事業}/agents/`)を確認する。
4. 目的に対して本当に必要なAgent定義だけを選ぶ。
5. 各Agent定義の責任範囲(その定義に書かれた役割・判断基準)を明記する。
6. 同じ作業を複数のAgent定義に重複して割り当てない。
7. 作業の結果(文章・データ)は、口頭やチャットではなく正本ファイルへ記録する。
8. QA相当のAgent定義(例: Brand QA)を適用する場合、その定義は本文を直接書き換えず、確認結果を報告する役割として扱う。
9. CEO承認ポイント(Gate1/Gate2/公開承認等)で作業を止める。
10. 承認された結果をGitへ記録する(コミット・pushはそれぞれ別途のCEO許可が必要)。

## 10. 作業依頼の標準入力(テンプレート項目のみ。実在する依頼レコードは作成しない)

```yaml
task_id:
目的:
対象ブランド:
対象商品またはプロジェクト:
対象ファイル:
許可する変更範囲:
禁止事項:
使用するAgent定義候補:
CEO承認ポイント:
外部executeの有無:
Gitコミットの可否:
GitHub pushの可否:
停止条件:
報告してほしい内容:
```

## 11. 標準報告形式(Claude CodeからCEOへの最低報告項目)
1. 実施内容
2. 新規ファイル
3. 修正ファイル
4. 削除ファイル
5. 実行した検証
6. 検証結果
7. 意図しない変更の有無
8. 秘密情報の有無
9. 外部接続の有無
10. `git status`の結果
11. コミットID
12. push状態
13. 保留中の条件
14. 次に進んでよい範囲
15. 進んではいけない範囲

## 12. Git操作の権限境界

| 操作 | 許可条件 |
|---|---|
| 読み取り確認(`git status`/`git diff`/`git log`/`git show`/`git branch`/`git remote`等) | CEOの個別承認なしでも、依頼範囲の確認として実行可能 |
| ファイル変更 | CEOが承認したGate2の範囲内だけ |
| `git add` / `git commit` | CEOが明示的にコミットを許可した場合だけ |
| `git push` | CEOが明示的にpushを許可した場合だけ |
| `revert` / `reset` / `rebase` / force push / 履歴書き換え | 破壊的または影響が大きいため、CEOの個別かつ明示的な承認が必要 |

**いずれの操作も、完了後に自動で次の操作へ連続進行しない。** 各操作の後、CEOの次の指示を待つ。

## 13. 秘密情報の扱い(要約。正本は`secret-management-policy.md`)
- 実値をGitへ保存しない
- Claude Code・GPT・Fable 5等のAIチャットへ実値を貼らない
- `.env.example`は変数名のみを記載する
- `.env`はGit除外(`.gitignore`)
- 漏えい疑い時は、削除より先に外部サービス側で失効させる
- Git履歴の書き換えはCEOの明示的承認が必須

## 14. 外部サービスの扱い
Instagram・Meta・Canva・LINE・WordPress・Google系・Gemini・Perplexity・NotebookLM・ココナラ・GPT等について、リポジトリ内に証拠がないものを接続済み・利用可能と断定しない。分類は`mcp/api-registry.yaml`・`mcp/servers.yaml`の`status`(`active`/`candidate`/`planned`)、または`not configured`/`not connected`/`confirmation required`/`not required currently`を用いる。`filesystem-git`以外を`active`へ変更しない(本Runbookは`mcp/api-registry.yaml`・`mcp/servers.yaml`を変更していない)。

## 15. 別PCへの移行
1. Git・VS Code・Claude Codeを準備する。
2. GitHub Private Repositoryへのアクセス権があることを確認する。
3. 本Runbook「4. リポジトリの取得と初期確認」の手順でcloneする。
4. `main`ブランチであることを確認する。
5. originが`https://github.com/angel7inc/AI-Company.git`であることを確認する。
6. 最新コミットを確認する。
7. `.env`等の秘密情報ファイルが、cloneしても復元されないことを確認する。
8. 秘密情報は、承認済みの保存先(ローカル`.env`・OS環境変数・CEO承認済みパスワード管理サービス)から、別PC側で改めて設定する。**GitHubのclone操作だけでは秘密情報は一切復元されない。**
9. `.gitignore`により除外されているローカルKPI実績・機密レポート等のデータも、GitHubからは復元されない。必要であれば、Git管理外の方法(承認済みの別バックアップ等)で別途復元する。
10. Canva上の素材・CEOが準備した固定画像等、GitHub外に存在するものも別途復元・再取得が必要になる場合がある。
11. 第1投稿等、進行中の成果物の`status`を確認する(本Runbook「16. 現在保留中の第1投稿」参照)。
12. CEO承認待ちの条件が残っていないか確認する。
13. 実際の投稿・書き込みを伴わない空運転で問題がないことを確認したうえで、本番作業を再開する。

**注意:** 秘密情報およびGit管理外データは、GitHubリポジトリのcloneだけでは完全に復元できない。

## 16. 現在保留中の第1投稿
- `pilot_post_id`: `tomo-angel7-pilot-01`
- `status`: `ceo_review`
- `human_review_required`: `true`
- `public_release_approved`: `false`
- `asset_reference`: `pending_ceo_asset_identification`
- 正本: `businesses/instagram/content/pilot-drafts/tomo-angel7-pilot-01.md`
- calendar: `businesses/instagram/content/calendar.md`に`ceo_review`としてミラー表示済み

**再開条件:**
1. 固定6枚目画像の正式特定
2. 人間による6枚目の確認
3. 実運用環境構築Gate2の各Batch完了
4. 投稿しない空運転の合格
5. CEOによる再開指示

本Runbook作成時点で、第1投稿ファイル自体は変更していない。

## 17. 障害・不一致発見時の対応
以下を発見した場合、対応する。

- working treeに意図しない変更がある
- HEADとorigin/mainが不一致
- 成果物ファイルとcalendarのstatusが不一致
- 秘密情報らしき値が見つかった
- 承認記録が不足している
- 外部executeが実行されそうな状態
- 対象外ファイルが変更されている
- Git競合が発生している
- YAMLが破損している
- 不明な未追跡ファイルがある

**原則:**
1. 変更を広げない
2. 外部executeを止める
3. 現状を読み取り専用で確認する
4. CEOへ報告する
5. CEOの承認なしに削除・`reset`・`revert`等を行わない

## 18. 変更履歴
| 版 | 日付 | 変更内容 | 変更者 |
|---|---|---|---|
| 1 | 2026-07-14 | 初版作成(環境構築Gate2 Batch A) | COO |
