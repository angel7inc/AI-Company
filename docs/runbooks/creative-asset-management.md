# AI Company 画像・クリエイティブアセット管理Runbook

## 位置づけ
本Runbookは、Tomo_Angel7等で使用するクリエイティブアセット(Canvaデザイン、元画像、AI生成画像、写真、イラスト、ロゴ、アイコン、OGP画像、Webサイト用画像、Instagram投稿画像・リール素材、YouTube/TikTok用素材、書き出し済み完成画像、サムネイル、固定画像、投稿内の各スライド画像等)について、所在・用途・状態・権利・レビュー状況・書き出し結果・投稿コンテンツとの対応関係を管理するための**ルールとテンプレートの設計**である。

**今回のBatchは管理ルールとテンプレートの設計のみを対象とする。** 実画像の作成・検索・取得・移動・コピー・アップロード、Canvaログイン、外部サービス接続、投稿は行っていない。

## 0. 既存構成との関係(重複回避)
本リポジトリには既に以下が存在する。

- `_company/brands/{ブランドID}/assets/`(例: `_company/brands/tomo-angel7/assets/`) ― ブランド固有のロゴ・フォントファイル・実際のカラーパレット参照等の**実体置き場**
- `_shared/creative-assets/` ― ブランドに依存しない共有テンプレートの仕組み・汎用ストック素材の**実体置き場**
- `_shared/agents/design/shared-creative-asset-manager.md` ― 上記2箇所の整合を管理するAgent定義
- `_shared/knowledge/canva/`・`_shared/knowledge/image-generation/` ― Canva操作・AI画像生成の**理論・使い方**を扱うKnowledge記事

これらは「アセットの実体をどこに置くか」「使い方の一般知識」を扱っており、**アセットごとの状態・権利・レビュー状況・投稿との対応関係を追跡する仕組み(スキーマ・ライフサイクル)は今回まで存在しなかった。** 本Runbook(および`_company/assets/`配下のテンプレート)は、その追跡レイヤーを新設するものであり、上記の実体置き場やAgentの役割を変更・置換しない。`shared-creative-asset-manager`が実体を管理する一方、本Runbookが定義する`asset_id`・状態・対応表は、その実体を指し示す記録として機能する。

第1投稿ドラフト(`tomo-angel7-pilot-01.md`)内に既に存在する`asset_reference`等のfrontmatterフィールドは、そのファイル自身のための個別フィールドであり、本Runbookの汎用スキーマとは役割が異なる。本Batchでは変更しない。

### 0.1 コミット前の実態確認(2026-07-14)
コミット前に、既存2ディレクトリの実態を確認した。

- `_company/brands/tomo-angel7/assets/`・`_shared/creative-assets/`とも、現時点で実ファイルは`README.md`のみ(Git追跡対象もこの1件のみ)。実画像は一切存在しない。
- `_company/brands/tomo-angel7/assets/`は`brand-brief.md`(値の正本)・`_company/org/brands.yaml`(`assets:`フィールド)・`ig-creative-team.md`から参照され、**ブランド固有の持続的な実体(ロゴ・フォント・カラーパレット参照等)** の置き場所として明確に位置づけられている。
- `_shared/creative-assets/`は`shared-creative-asset-manager.md`・`ig-creative-team.md`・`docs/phase3b-creative-division-design.md`から参照され、**ブランド非依存の共有テンプレートの仕組み・汎用ストック素材**の置き場所として位置づけられている。
- 上記に基づき、以下のとおり役割分担を確定する。
  - `_company/assets/templates/` ― 汎用管理スキーマ(空テンプレート)の保存先
  - `_company/assets/local/` ― Git管理外の実記録・作業データ(source/working/exports/previews/records/content-maps/export-manifests/license-evidence)の予定領域
  - `_company/brands/tomo-angel7/assets/` ― 既存のブランド固有実体置き場であり、今回の汎用スキーマとは役割が異なる(変更なし)
  - `_shared/creative-assets/` ― 既存の共有実体置き場であり、今回の汎用スキーマとは役割が異なる(変更なし)
- **実画像の正本保存先は、既存文書と矛盾しない形で別途確定する必要がある。保存先未決定のものを決定済みとして扱わない。**
- **未解決の論点(将来のCEO確認事項):** `_company/brands/tomo-angel7/assets/README.md`は「実際のカルーセルテンプレートインスタンス等の実体」も置き場所に含む記載があり、投稿ごとに書き出された完成画像(`exported asset`)が最終的にここへ格納されるのか、`_company/assets/local/exports/`側の作業領域に留まるのかは、既存文書だけでは一意に確定できない。本Batchではどちらか一方を正本と断定せず、将来の保存先決定時に併せて整理する。

### 何を管理するか
- クリエイティブアセット1件ごとの識別・状態・権利・レビュー状況(`asset-record-template.yaml`)
- コンテンツ(投稿・記事等)と画像スロットの対応関係(`content-asset-map-template.yaml`)
- Canva等からの書き出し結果の記録(`export-manifest-template.yaml`)

### 何を管理しないか
- アセットの実体そのもの(画像ファイル自体は`_company/brands/{ブランドID}/assets/`・`_shared/creative-assets/`等の既存置き場、またはGit管理外のローカル領域に置く)
- Canva操作の具体的な使い方(→`_shared/knowledge/canva/`)
- AI画像生成のプロンプト技法(→`_shared/knowledge/image-generation/`・`_shared/prompts/image/`)
- 実際の権利判断・外部規約調査(本Runbookは記録の型を定めるのみ)

### 実画像そのものと管理記録の違い
管理記録(`asset-record-template.yaml`等から作成される実記録)は、アセットの**所在・状態・権利・対応関係についてのメタデータ**であり、画像ファイルの内容そのものではない。記録が存在しても、画像ファイル本体が存在するとは限らない。

**Git履歴だけでは実画像を復元できない。** 実画像・Canva書き出し画像等は`.gitignore`により除外されるため、GitHubのcloneでは取得できない([`operations-continuity.md`](operations-continuity.md)参照)。

**Canvaだけでも完成画像や投稿との対応を完全復元できない場合がある。** Canva上のデザインが存在しても、どの投稿のどのスロットに使われたか、最終的にどのファイルとして書き出されたかの対応関係は、`content-asset-map-template.yaml`・`export-manifest-template.yaml`側の記録がなければ再現できない。

## 2. 用語

| 用語 | 定義 |
|---|---|
| source asset(元アセット) | 加工前の元画像・元デザイン(Canva上のデザイン、撮影写真、AI生成画像の初期出力等) |
| working asset(作業中アセット) | 編集途中のアセット。まだ完成・書き出し前の状態 |
| exported asset(書き出し済みアセット) | Canva等から実際にファイルとして書き出された完成画像 |
| preview(プレビュー) | レビュー目的の仮の書き出し。公開用ファイルとして扱わない |
| asset record(アセット記録) | `asset-record-template.yaml`に基づき1アセットごとに作成される記録 |
| content asset map(コンテンツ・アセット対応表) | `content-asset-map-template.yaml`に基づき、1コンテンツが必要とする画像スロットとasset_idの対応を記録するもの |
| export manifest(書き出し記録) | `export-manifest-template.yaml`に基づき、1回の書き出し作業の結果を記録するもの |
| required slot(必須スロット) | そのコンテンツの公開に画像が必須のスロット |
| optional slot(任意スロット) | 画像がなくても公開判定に影響しないスロット |
| fixed asset(固定アセット) | CEOまたは承認された担当者が指定した、担当者の独断で置き換えてはならないアセット(例: 第1投稿の固定6枚目画像) |
| reusable asset(再利用可能アセット) | 複数コンテンツで使い回すことが許可されたアセット |
| derivative asset(派生アセット) | 既存アセット(`parent_asset_id`)から加工・書き出しされた別アセット |

## 3. asset_id

アセットごとに一意の`asset_id`を付与する。**例示形式は使用するが、以下は実在するIDではない。**

```
例: ta7-ig-2026-07-a01-photo-001
    (ブランド略称)-(用途略称)-(年月)-(連番等)-(種別)-(通番)
```

- `asset_id`には個人名・顧客名・メールアドレス・電話番号・秘密情報・APIキー・アクセストークン・非公開URLを直接含めない
- `asset_id`の再利用は禁止する
- 削除・廃止(`archived`)後も、同じ`asset_id`を別アセットへ割り当てない

## 4. ファイル命名規則
ファイル名は最低限、以下の要素を考慮する。

- ブランドまたはプロジェクト
- `content_id`
- `asset_id`
- 用途
- スロット番号
- バージョン
- サイズ
- 言語
- 拡張子

ファイル名に個人情報・秘密情報・公開前のセンシティブな内容を直接入れない。**既存ファイルを自動的に改名する処理は今回実装していない。**

## 5. アセット状態(asset_status)

| 状態 | 意味 | 次状態へ進む条件(例) |
|---|---|---|
| `planned` | アセットの必要性だけが決まっている | 元素材の所在が判明したら`source_pending`または`source_identified`へ |
| `source_pending` | 元素材を確認・取得中 | 元素材が特定されたら`source_identified`へ |
| `source_identified` | 元素材(Canvaデザイン・写真等)の所在・所有者・アクセス可否を確認済み | 編集着手で`working`へ |
| `working` | 編集作業中 | 完成し確認依頼可能になったら`review_required`へ |
| `review_required` | 人間またはCEOのレビュー待ち | レビュー結果により`changes_requested`または`approved`へ |
| `changes_requested` | レビューで修正指示が出た | 修正後`working`または`review_required`へ差し戻し |
| `approved` | 画像内容そのものが承認された(**公開承認とは別概念、下記参照**) | 書き出し実施で`exported`へ |
| `exported` | 実ファイルとして書き出し済み | 公開判定条件が揃えば`release_ready`へ |
| `release_ready` | 公開に必要な全条件(下記「8.11 外部公開の停止条件」参照)が揃った | 実際に公開されたら`published`へ |
| `published` | 外部へ公開済み | 役目を終えたら`archived`へ |
| `archived` | 使用終了・保管のみ | (終端状態) |
| `blocked` | 権利・アクセス・個人情報等の理由で先へ進めない | 問題解消後、直前の状態へ復帰 |

**`approved`(画像内容の承認)と`public_release_approved`(公開実行の承認)は別概念として扱う。** 画像自体が`approved`になっても、`public_release_approved: true`と有効なOutbound Action Approval記録([`outbound-action-policy.md`](../../_company/charter/outbound-action-policy.md)参照)が揃わない限り、外部公開してはならない。

## 6. Canva上のデザイン管理
- Canvaは外部サービス内の**原本候補**であり、唯一の正本と扱わない
- Canva上に存在するだけでは**バックアップ完了とは扱わない**
- CanvaデザインURLは、秘密情報ではない場合でも**非公開管理情報**として慎重に扱う
- 非公開URLをGit管理テンプレートへ**実値で記録しない**
- デザインの所有者・編集権限・閲覧権限を確認する
- 所有者不明・権限不明・アクセス不能の場合は`blocked`とする
- Canva上のデザインと書き出しファイルは**別資産**として管理する(`asset_id`を分ける、または`parent_asset_id`で関連付ける)
- 本Batchでは、Canvaデザインを削除・移動・複製する操作は行っていない

## 7. コンテンツとの対応関係
`content_id`(投稿・記事・Webページ等)と、使用する`asset_id`の対応を`content-asset-map-template.yaml`で管理する。最低限、コンテンツの種類・必要画像数・スロット番号・スロットの役割・required/optionalの別・固定画像か可変画像か・対応する`asset_id`・未特定状態・代替可能か・並び順・重複使用の可否を扱う。

**必須スロット(`required: true`)の`asset_id`が未特定の場合、公開可能と判定してはならない。**

**固定画像(`fixed_asset: true`)について、担当者の独断で別画像へ置換してはならない。** 置換にはCEO承認が必要。

## 8. 書き出し管理
`export-manifest-template.yaml`で、書き出し元・書き出し日時・操作者・対象`content_id`・対象`asset_id`・ファイル名・ファイル形式・ピクセル寸法・アスペクト比・ファイルサイズ・checksum・書き出し結果・既存ファイルとの重複・上書きの有無・レビュー状態・公開承認状態・外部アップロード実施の有無を記録する。

**既存ファイルへの無条件上書きは禁止する。** 同名ファイルが存在する場合は作業を停止し、内容・checksum・バージョンを確認してから判断する。

## 9. 権利・ライセンス(rights_status)

| 状態 | 意味 |
|---|---|
| `owned` | 自社が権利を保有 |
| `licensed` | ライセンス取得済み |
| `commissioned` | 制作委託により権利を取得 |
| `ai_generated` | AI生成(利用規約上の扱いは別途確認が必要) |
| `public_domain` | パブリックドメイン |
| `permission_confirmed` | 権利者から使用許諾を確認済み |
| `unknown` | 権利状態を確認できていない |
| `prohibited` | 使用禁止と判明している |

**`unknown`または`prohibited`の場合は公開停止とする。** AI生成画像については、使用サービス(`ai_generation_service`)・作成主体・確認状況を記録する方針とする。**実際のライセンス判断や外部規約調査は今回行っていない。**

## 10. 個人情報・秘密情報の確認対象
以下を確認対象とする。人物の顔、氏名、住所、電話番号、メールアドレス、顧客情報、注文情報、管理画面、APIキー、トークン、QRコード、バーコード、位置情報、EXIF情報、通知表示、ブラウザタブ、外部サービスの非公開URL、ファイルパスに含まれるユーザー名。

確認不能な場合は`blocked`または`review_required`とする。**自動削除・自動加工処理は今回実装していない。**

## 11. 外部公開の停止条件(フェイルクローズ)
以下のいずれかに該当する場合、外部アップロード・投稿・公開・置換を行わず停止する。

1. 必須スロットが未特定
2. ファイルが存在しない
3. 元デザインにアクセスできない
4. Canvaの所有者・権限が不明
5. `asset_id`が重複
6. `content_id`との対応が不明
7. スロット順序が不明
8. 固定画像が別画像へ置き換わっている
9. 寸法が不明または不適合
10. アスペクト比が不明または不適合
11. checksumが必要なのに未取得
12. 権利状態が`unknown`または`prohibited`
13. 個人情報・秘密情報の確認が未完了
14. human reviewが未完了
15. CEO reviewが未完了
16. `public_release_approved`が`false`
17. 書き出しファイルと承認対象が一致しない
18. 投稿先・用途が不明
19. 重複投稿または誤上書きの可能性がある
20. バックアップ・復元関係が不明な重要固定画像

## 12. バックアップとの関係
[`operations-continuity.md`](operations-continuity.md)のA(Git管理対象)/B(Git管理外機密・実績データ)/C(外部サービス内資産)/D(完成画像・制作素材)の分類に対し、クリエイティブアセットは主にC・Dに該当する。

- GitHubは**テンプレートとルールの保存先**であり、実画像の保存先ではない
- 実画像のバックアップ先は別途選定が必要(`selected_creative_asset_backup_destination: not_decided`、`secret-management-policy.md`・`operations-continuity.md`と同一の未決定状態)
- **Canvaだけを唯一のバックアップ先としない**
- 完成画像の復元可能性は将来テストする必要がある(今回未実施)
- 元デザインと完成画像の**両方**が必要となる場合がある(元デザインだけでは再書き出し不可、完成画像だけでは再編集不可)
- backup manifestとの対応方法は将来実装
- **実バックアップは今回作成していない**

## 13. 復元時の確認
復元時は最低限、以下を確認する。

元画像の存在、編集可能な原本の存在、書き出し済み完成画像の存在、`content_id`との対応、`asset_id`との対応、スロット順序、ファイル形式、寸法、checksum、権利情報、レビュー情報、公開承認情報。

**復元した画像を、そのまま投稿・公開してはならない。** 復元後は、外部公開しない状態で対応関係と表示を確認する。

## 14. 現在の状態(正直な申告)
- 実アセット記録(`_company/assets/local/records/`)は未作成
- 実content asset map(`_company/assets/local/content-maps/`)は未作成
- 実export manifest(`_company/assets/local/export-manifests/`)は未作成
- Canva上の画像は確認していない
- 完成画像は特定していない
- バックアップ先は選定していない
- 復元テストは未実施
- 自動チェックは未実装
- 自動書き出しは未実装
- 自動投稿は未実装
- 外部サービス接続は行っていない

## 15. Git管理区分

**Git管理対象:** 管理ルール、空のテンプレート(`_company/assets/templates/`)、スキーマ定義、ステータス定義、命名規則、外部実行を伴わない手順書。

**Git管理外(`_company/assets/local/`):** 実際の元画像、Canvaから書き出した完成画像、編集途中の画像、プレビュー画像、実際のアセット管理記録、実際のコンテンツ・アセット対応表、実際のexport manifest、Canvaの非公開URL、外部サービスの非公開管理URL、個人情報を含む素材、ライセンス証明書の実ファイル、購入素材、顧客から提供された画像、秘密情報を含む画像・スクリーンショット。

## 16. テンプレート一覧
- [`_company/assets/templates/asset-record-template.yaml`](../../_company/assets/templates/asset-record-template.yaml)
- [`_company/assets/templates/content-asset-map-template.yaml`](../../_company/assets/templates/content-asset-map-template.yaml)
- [`_company/assets/templates/export-manifest-template.yaml`](../../_company/assets/templates/export-manifest-template.yaml)

いずれも`sample_only: true`であり、実在する情報を含まない。

## 17. 第1投稿の扱い
本Batchでは、第1投稿(`tomo-angel7-pilot-01`)関連ファイルを一切変更していない。以下の状態を維持している。

- `status`: `ceo_review`
- `human_review_required`: `true`
- `public_release_approved`: `false`
- `asset_reference`: `pending_ceo_asset_identification`

固定6枚目画像の検索・特定・生成・置換・仮登録は行っていない。第1投稿を新しいアセットテンプレートへ実データとして転記してもいない。**第1投稿との実対応付けは、別のCEO承認済みBatchで行う。**

## 18. 変更履歴
| 版 | 日付 | 変更内容 | 変更者 |
|---|---|---|---|
| 1 | 2026-07-14 | 初版作成(環境構築Gate2、画像・クリエイティブアセット管理基盤Batch) | COO |
