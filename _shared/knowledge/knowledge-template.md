---
id:                        # 一意のID。related/supersedesから参照される安定した識別子。ファイル名やタイトルが変わっても変更しない
title: 
categories: []             # 複数カテゴリ可(例: [instagram, psychology])。保存フォルダは主カテゴリを選ぶ
tags: []
knowledge_type: distilled  # source(一次情報) / distilled(整理済み知識) / case-study(実施事例)。playbookは廃止 ― 手順・SOPは_shared/sop/(SOP Layer)へ一本化
importance: medium         # low / medium / high ― この知識が使われたときの価値
priority: medium           # low / medium / high / critical ― この知識の作成・更新の緊急度
status: draft              # draft / active / deprecated / archived
version: 1
owner:                     # 更新責任を持つエージェントまたは人間
reviewers: []              # 内容を確認したエージェントまたは人間
related_agents: []         # このKnowledgeを使うエージェントID
related: []                # 関連するKnowledgeのid(必須。本当に関連が無い場合のみ空配列)
sources: []                 # 引用元(URL・書籍名等)
source_type:                # official / documentation / research-paper / book / industry-report / youtube / reddit / community / experiment / internal(正本。docs/naming-conventions.mdにも同じ一覧を掲載)
sensitivity: internal       # public / internal / confidential / restricted ― confidential/restrictedは外部AI・外部サービスへ絶対に送信しない
supersedes: []              # このKnowledgeが置き換えた過去Knowledgeのid
author: 
confidence: draft           # draft / verified / expert-reviewed
created_at: "YYYY-MM-DD"
updated_at: "YYYY-MM-DD"
last_verified_at: "YYYY-MM-DD"  # 内容が事実として最後に確認された日(updated_atは編集日、こちらは検証日)
review_due: "YYYY-MM-DD"
---

# タイトル

## AI Summary
(5行以内。AIエージェントがここだけ読んでも要点を把握できるように書く)

## 概要

## いつ使うか

## 適用範囲
この知識を利用できる事業、媒体、状況。

## 適用しないケース
この知識を使用すべきでない状況。

## 重要ポイント

## ベストプラクティス

## 注意点

## 実務チェックリスト
- [ ] 

## 関連Knowledge(必須)
- 

## 参考リンク
- 

