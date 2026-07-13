---
id: agent-onboarding
title: エージェントオンボーディング手順
category: hr
tags: [onboarding, lifecycle, agents.yaml]
owner: hr-workforce-manager
status: draft
priority: high
risk_level: medium
version: 1
frequency: as-needed
estimated_time: "30分〜1時間"
requires_ceo_approval: true
automation_possible: true
automation_status: not-automated
related_sop_ids: [agent-retirement]
related_sop_categories: [management]
related_knowledge: []
sensitivity: internal
last_execution: null
success_rate: null
created_at: "2026-07-13"
updated_at: "2026-07-13"
review_due: "2027-01-13"
---

# エージェントオンボーディング手順

## Purpose(目的)
新規エージェント提案を`planned`から`active`まで、既存の二段階承認プロセスに沿って正式に移行させる。

## Scope(適用範囲)
新規AIエージェントの追加のみ。Human Workforceのオンボーディングは対象外(将来別手順)。

## Trigger(実行のきっかけ)
COO/CEOが新規エージェントを提案したとき(Gate1提案時)。

## Inputs(必要な入力)
- 提案内容(役割・division・使用ツール・既存エージェントとの重複チェック結果)

## Outputs(成果物)
- `agents.yaml`への新規登録(status: planned→proposed→approved→onboarding→active)
- 定義ファイル(`{置き場所}/{エージェントID}.md`)

## Tools(使用ツール)
Claude Code

## Preconditions(前提条件)
Gate1(目的・範囲・重複調査)がCEO承認済みであること。

## Procedure(手順)
1. 提案内容を`planned`として記録する
2. 既存エージェントとの重複調査結果を`proposed`の状態に付記する
3. CEO承認後、`approved`へ更新し、定義ファイル(Gate2内容)を作成する
4. 実運用を開始する場合は`onboarding`を経て`active`へ更新する

## Checklist(実行時チェックリスト)
- [ ] 既存エージェントとの重複調査を実施したか
- [ ] `division`・`layer`・`brand`フィールドが命名規則に沿っているか
- [ ] Gate1・Gate2それぞれでCEO承認を得たか

## Quality Standard(品質基準)
`agents.yaml`の必須フィールドが全て記入され、definitionファイルが実在すること。

## Failure Pattern(失敗パターン)
重複調査を省略して類似エージェントを重複作成してしまう(→ARC-002/004/011/012のような残留参照の再発防止のため、必ずGate1で重複調査を行う)。

## Handoff(引き継ぎ)
オンボーディング完了後は、各Divisionの通常運用へ引き継ぐ。

## Revision History(変更履歴)
| 版 | 日付 | 変更内容 | 変更者 |
|---|---|---|---|
| 1 | 2026-07-13 | 初版作成 | COO |
