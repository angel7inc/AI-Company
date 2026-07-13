---
id: agent-retirement
title: エージェント退役手順
category: hr
tags: [retirement, lifecycle, agents.yaml]
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
related_sop_ids: [agent-onboarding]
related_sop_categories: [management, architecture-review]
related_knowledge: []
sensitivity: internal
last_execution: null
success_rate: null
created_at: "2026-07-13"
updated_at: "2026-07-13"
review_due: "2027-01-13"
---

# エージェント退役手順

## Purpose(目的)
エージェントを`retiring`から`retired`へ正式に移行させ、参照整合性・権限失効を漏れなく処理する。`ig-research`退役時のようなアドホック処理を防ぐ。

## Scope(適用範囲)
既存AIエージェントの退役のみ。休止(`paused`)は対象外(別途復帰を前提とするため本手順を使わない)。

## Trigger(実行のきっかけ)
Architecture Reviewからの退役候補報告、またはCOO/CEOの退役判断があったとき。

## Inputs(必要な入力)
- 退役対象エージェントID
- 後継エージェント(存在する場合)
- Architecture Reviewの参照整合性チェック結果

## Outputs(成果物)
- `agents.yaml`のstatus更新(`retiring`→`retired`。行は削除しない)
- 退役処理完了報告(参照更新箇所の一覧)

## Tools(使用ツール)
Claude Code

## Preconditions(前提条件)
CEO承認(または該当する軽微修正の場合はCOO承認)を得ていること。

## Procedure(手順)
1. 退役対象を`retiring`へ更新する
2. 後継エージェントの有無を確認する(存在しない場合、ARC-011のように「unassigned」等、実態を正確に示す。推測で後継を割り当てない)
3. 全社の参照箇所(Knowledge/SOP/Agent定義)を確認し、後継への置換または「退役済み」の明記を行う(Architecture Reviewの次回監査での再確認を前提としてよい)
4. `outbound-action-policy.md`に基づき付与されていたexecute権限を失効させる
5. `retired`へ更新する(行は削除しない)

## Checklist(実行時チェックリスト)
- [ ] 後継の有無を確認したか(推測で断定していないか)
- [ ] execute権限の失効を確認したか
- [ ] `agents.yaml`の行を削除せず`retired`へ更新したか

## Quality Standard(品質基準)
退役後、`grep`等で退役対象IDへの現在参照が「退役済み」として正しく表現されていること。

## Failure Pattern(失敗パターン)
後継が存在しないのに推測で別エージェントを割り当ててしまう(→ARC-011で確立した「推測せず未実装/unassignedと明記する」原則に従う)。

## Handoff(引き継ぎ)
Architecture Reviewへ、次回監査での参照整合性の再確認を依頼する。

## Revision History(変更履歴)
| 版 | 日付 | 変更内容 | 変更者 |
|---|---|---|---|
| 1 | 2026-07-13 | 初版作成 | COO |
