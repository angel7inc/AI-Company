---
id: ceo-approval-flow
title: CEO承認フロー(二段階承認プロトコル)
category: management
tags: [approval, governance, gate]
owner: core-coo
status: active
priority: critical
risk_level: critical
version: 1
frequency: as-needed
estimated_time: "案件による(数分〜数十分)"
requires_ceo_approval: true
automation_possible: false
automation_status: not-automated
related_sop: [coo-approval-flow]
related_knowledge: [business-strategy]
sensitivity: internal
last_execution: null
success_rate: null
created_at: "2026-07-12"
updated_at: "2026-07-12"
review_due: "2027-01-12"
---

# CEO承認フロー(二段階承認プロトコル)

## Purpose(目的)
新規の設計・実装をCEOの意図と異なる形で進めてしまうリスクを防ぐ。これまでCOO(Claude Code)のセッションメモリにのみ存在していたルールを、初めて会社の正式な手順として文書化する。

## Scope(適用範囲)
AI-Companyプロジェクト内の新しいアーキテクチャ・Division・エージェント・フォルダ構造・既存ファイルの実質的な変更。誤字修正等の軽微な変更は対象外(→`coo-approval-flow.md`)。

## Trigger(実行のきっかけ)
CEOから新しい設計・機能追加の依頼を受けたとき。または既存の承認済み設計を変更する提案を行うとき。

## Inputs(必要な入力)
- CEOからの依頼内容
- 既存アーキテクチャ(Knowledge/SOP/Brand/Creative/Search Layer等)の現状

## Outputs(成果物)
- Gate1: チャット上での設計提案(目的/メリット/デメリット/代替案/長期的な影響/推奨案)
- Gate2: ファイルごとの承認記録
- 承認ログ(`_company/reports/`)

## Tools(使用ツール)
Claude Code

## Preconditions(前提条件)
なし(起点となる手順)

## Procedure(手順)
1. **Gate1(設計承認)** ― 新規提案(新しいアーキテクチャ・Division・エージェント種別)の場合、以下6項目をチャットで提示し、ファイルの作成・編集は一切行わない。
   1. 目的
   2. メリット
   3. デメリット
   4. 代替案
   5. 長期的な影響(100事業・1000エージェント規模を見据えて)
   6. 推奨案
2. 既存の承認済み設計を変更する提案の場合は、5項目形式(変更対象/目的/メリット/デメリット/既存設計への影響)を使う。
3. CEOの明示的な承認を得るまで、いかなるファイル作成・編集ツールも呼び出さない。
4. **Gate2(個別ファイル承認)** ― Gate1承認後、書き込むファイルごとに以下4項目を提示する。
   1. ファイル名
   2. 保存場所
   3. 内容の要約
   4. 全文(チャット上に表示)
5. 該当ファイルの承認を得てから書き込み、次のファイルへ進む。
6. CEOが「今回は承認不要で進めてよい」等、明示的にGate2の個別承認を省略する指示をした場合に限り、複数ファイルを一括で実装してよい。ただし省略の指示は毎回の案件ごとに必要で、過去の省略指示を今後の案件に流用しない。
7. 実装完了後、何を作成・変更したかをチャット上で報告する。

## Checklist(実行時チェックリスト)
- [ ] Gate1の6項目(または5項目)をすべて提示したか
- [ ] CEOの明示的な承認を得るまでファイル操作をしていないか
- [ ] Gate2でファイルごとに内容を提示したか(一括省略の指示がない限り)
- [ ] 実装後に変更内容を報告したか

## Quality Standard(品質基準)
CEOが「何が提案され、何が承認され、何が実装されたか」を後から振り返って明確に分かる状態になっていること。

## Failure Pattern(失敗パターン)
- Gate1の承認を得ずに実装を進めてしまう(過去の類似案件の承認を流用してしまう)
- 一括承認の指示を、明示されていない別の案件にまで拡大解釈してしまう
- Gate2でファイル内容を提示せず、要約だけで書き込んでしまう

## Handoff(引き継ぎ)
実装完了後、`_company/reports/`に承認・実装の記録を残す。次のレビューサイクル(`review-cycle.md`)で振り返りの対象にする。

## Revision History(変更履歴)
| 版 | 日付 | 変更内容 | 変更者 |
|---|---|---|---|
| 1 | 2026-07-12 | 初版作成。COOのセッションメモリにのみ存在していたルールを正式文書化 | COO |
