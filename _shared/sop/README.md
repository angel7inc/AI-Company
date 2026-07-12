# SOP Layer(標準業務手順層)

AIエージェントがそのまま実行できるレベルまで具体化した「仕事のやり方」を蓄積する場所です。Knowledgeが「知っておくべきこと」であるのに対し、SOPは「どうやるか」です。

## SOPとは何か

SOPは、ある業務を実行するための、番号付きで再現可能な手順書です。読んだAIエージェントが、追加の判断や補足なしにそのまま実行に移せる具体性を持たせます。

## Knowledgeとの違い

| | Knowledge | SOP |
|---|---|---|
| 性質 | 知識(なぜ・何が) | 手順(どうやって) |
| 例 | Instagramのアルゴリズムの仕組み | 週次のInstagram投稿企画〜公開の手順 |
| 単体で仕事を実行できるか | できない(判断材料) | できる(実行手順そのもの) |
| 保存場所 | `_shared/knowledge/` | `_shared/sop/` |

SOPの「Procedure(手順)」を実行する中で必要な背景知識は、`related_knowledge`でKnowledge記事にリンクして補います。

## Playbookとの違い

Knowledge Layerの`knowledge_type: playbook`は、これまで「標準手順・SOP・チェックリスト」を指す分類として用意していましたが、SOP Layerの新設によりこの役割はSOP Layerに一本化しました。**手順・SOPに類する内容は、今後Knowledgeではなくここ(`_shared/sop/`)に作成してください。** `knowledge_type: playbook`は廃止済みです([`knowledge-template.md`](../knowledge/knowledge-template.md)から削除済み)。

## Case Studyとの違い

Case Study(`knowledge_type: case-study`、Knowledge Layer側)は「実際にやってみた結果どうだったか」という事後的な記録です。SOPは「これから何度でも再現できるように、正しいやり方を定めたもの」です。Case Studyで得た教訓は、SOPの`Failure Pattern`や`Procedure`の改善材料として反映し、SOP自体の`version`を上げます。

## 全体像 ― Knowledge / SOP / Playbook / Case Study / Archive の関係

これらは「物理的にどこに保存されるか(2つのレイヤー)」と「どんな性質の情報か(横断的な分類)」という、別々の軸の概念です。5つを並列の保存場所だと考えると迷うため、次の整理を正としてください。

```
物理レイヤー(実際のフォルダ)
├── _shared/knowledge/  … 知識(なぜ・何が)
└── _shared/sop/        … 手順(どうやって)

横断的な分類
├── Knowledge Layer内の knowledge_type
│   ├── source     … 一次情報
│   ├── distilled  … 整理済みの再利用可能な知識
│   └── case-study … 実施結果・成功/失敗事例(過去の記録)
│   └── playbook    … [廃止済み] SOP Layer新設によりSOP Layerへ統合済み
└── 両レイヤー共通の status
    └── draft / active / deprecated / archived(archiveは独立フォルダではなく「状態」)
```

**書く前の判断フロー**
1. 「なぜ・何」を説明する恒久知識か? → **Knowledge Layer**(`_shared/knowledge/`)
2. 「どうやるか」を説明する再現可能な手順か? → **SOP Layer**(`_shared/sop/`、このフォルダ)
3. 古くなった記事・SOPは削除せず、両レイヤーとも`status: deprecated`または`archived`にする

## カテゴリの拡張方針(将来の新チャネル対応)
`instagram/`はInstagram固有の手順専用カテゴリです。`research/`・`copywriting/`・`design/`・`publishing/`・`analytics/`・`customer/`はチャネルを問わない汎用機能のカテゴリで、複数チャネルのSOPが将来ここに増えていく想定です。新しいチャネル(YouTube、TikTok等)を追加する際は、Knowledge Layerと同様に`sop/youtube/`のようなチャネル固有カテゴリを追加します(既存カテゴリの構造は変更しません)。今回はこの方針を明文化するのみで、新カテゴリの追加は行いません。

## 更新ルール
- SOPを実行してみて手順に不備が見つかったら、すぐに修正し`version`を上げ、`Revision History`に記録する
- 古い手順は削除せず`status: deprecated`にする(Knowledge Layerと同じ考え方)
- 実際に検証されていない手順は`status: draft`のままにし、`active`にするのは実行して機能することを確認してから

## 運用ルール
- SOPは必ず`Procedure`を番号付きの具体的な手順にする。「適切に対応する」のような曖昧な指示は禁止
- 新規作成前に[`sop-index.md`](./sop-index.md)で類似SOPが無いか確認する(Knowledge Layerと同じ重複防止の考え方)
- `sensitivity`が`confidential`/`restricted`のSOPは外部AI・外部サービスへ送信しない
- 投稿公開・予約投稿など、会社の承認ルールで「CEO承認必須」とされている操作は、SOPの手順内に必ず承認確認のステップを明記する。SOP自体がその承認を代替することはない
- 今回のPhase2では基盤(フォルダ・テンプレート・索引・カテゴリREADME)のみを作成しており、実際の業務手順(例: Instagram投稿手順)はまだ作成していません
