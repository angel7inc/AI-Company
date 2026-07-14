# ツール・役割分担方針(Tool & Role Responsibility Policy)

## 位置づけ
本方針は、AI Company全体に適用される全社共通の憲章・方針(`_company/charter/`)であり、特定チャネル・特定Divisionに専属するSOPではない(`_shared/sop/sop-index.md`は今回変更しない)。CEO・GPT・Claude Code・Fable 5・Canva・人間の投稿担当者・48 Agentのそれぞれが、現状事実として何を担当し、何を担当しないかを明確化する。

**本方針は、実際には存在しない機能を存在するように記載しない。** 現時点でAI Companyは、Claude Code上でCEOが指示を入力し、COO役のClaude CodeがAgent定義・SOPを参照しながら作業する、人間主導・ドキュメント中心の運用形態である。Agentが自動で会社を運営する機構、COOが常駐して自動処理する機構、外部サービスから自動で情報取得する機構は、現時点で存在しない。

## 1. CEO
- 最終意思決定
- Gate1(設計・方針)承認
- Gate2(実装内容)承認
- 公開承認(Outbound Action Approval Policyに基づく)
- 外部書き込み承認
- 素材・アカウント・サービス情報の提供
- 作業の停止・再開の判断
- 破壊的操作(revert/reset/rebase/force push/履歴書き換え等)の承認

## 2. GPT
- CEOへの相談対応・要件整理
- Claude Codeへ渡すプロンプトの作成支援
- Claude Codeの成果報告のレビュー
- 進行順序の管理支援

**現状、GPTがGitHubリポジトリへ直接接続し自動で変更する役割は担っていない。** GPTの入出力はCEOとの対話に留まり、リポジトリへの操作はCEOの指示を経てClaude Codeが行う。

## 3. Claude Code(COO役)
- リポジトリの読み取り
- 設計レビュー
- CEOが許可した範囲内でのファイル変更
- Git検証(status/diff/log等の確認)
- CEOの明示的な許可があった場合のみのコミット・push
- 既存Agent定義・SOPを参照した作業の実施
- CEOへの実施結果報告

**CEOの明示的指示なしに、外部execute(実際の外部サービスへの書き込み・投稿・接続)や次のBatch・次のフェーズへ進まない。**

## 4. Fable 5
- 設計判断・実装方針に対する第二意見
- 計画レビュー
- 補助的な検討

**正式な利用形態(CLI/API/Webサービス等のいずれか)は、本リポジトリ内の証拠だけでは断定しない。** 現時点でFable 5は、CEOが個別に呼び出し、COOへその判断内容を伝達する形で利用されている。

## 5. Canva
- 人間によるカルーセル画像等の制作
- ブランドテンプレートの利用
- 完成画像の書き出し

**現時点でCanvaへのAPI接続・自動操作は存在しない。** 画像制作は人間が手動でCanva上で行う。

## 6. 人間の投稿担当者
- 公開前承認(`public_release_approved`・Outbound Action Approval記録)が揃っていることの確認
- 実際のInstagram等への手動投稿
- 投稿完了の証拠記録
- **自分の判断だけで公開しない**(CEO承認・Outbound Action Approval Policyの条件が揃った場合のみ投稿する)

## 7. 48 Agent(`_company/org/agents.yaml`登録分)
- 独立して実行するソフトウェアではない
- 各Agentは、役割・責任範囲・判断基準を記述したMarkdown定義(`_company/agents/`・`_shared/agents/`・`businesses/{事業}/agents/`配下)である
- Claude Codeが作業時にこれらの定義を参照し、その役割を人間主導の作業へ適用する
- 同時に48体が稼働しているわけではなく、Agent自動選択機構・Agent間自動通信機構も存在しない

具体的な手動適用手順は[`docs/runbooks/ai-company-local-operations.md`](../../docs/runbooks/ai-company-local-operations.md)「Agentの手動呼び出し方法」章を参照する。

## 8. 外部サービス接続状態の表現ルール
Instagram・Meta・Canva・LINE・WordPress・Google系・Gemini・Perplexity・NotebookLM・ココナラ・GPT等について、リポジトリ内に証拠がないものを「接続済み」「利用可能」と断定しない。分類は`mcp/api-registry.yaml`・`mcp/servers.yaml`の`status`値(`active`/`candidate`/`planned`)、または`not configured`/`not connected`/`confirmation required`/`not required currently`を用いる。`filesystem-git`以外を`active`へ変更する場合は、実際の接続確認後、CEO承認を経て`mcp/api-registry.yaml`・`mcp/servers.yaml`側を別途更新する(本方針は今回それらのファイルを変更しない)。

## 9. 関連文書
- 具体的な作業開始・終了手順、環境確認手順、停止・再開ルールは[`docs/runbooks/ai-company-local-operations.md`](../../docs/runbooks/ai-company-local-operations.md)を参照する。
- 秘密情報の扱いは[`secret-management-policy.md`](secret-management-policy.md)を正本とする(本方針では重複記載しない)。
- 外部書き込み・公開の承認プロセスは[`outbound-action-policy.md`](outbound-action-policy.md)を正本とする。

## 10. 変更履歴
| 版 | 日付 | 変更内容 | 変更者 |
|---|---|---|---|
| 1 | 2026-07-14 | 初版作成(環境構築Gate2 Batch A) | COO |
