# intelligence-consumer ― 消費者分析エージェント(全社共有)

## 基本情報
- **エージェントID:** intelligence-consumer
- **名称:** 消費者分析エージェント
- **所属事業:** 共通(division: intelligence)
- **ステータス:** draft

## 役割
悩み・ペルソナ・行動分析を、チャネル横断で担当する。Phase2で計画されたが未実装だった`ig-research`(Instagram専用)の役割を上位互換し、Instagram固有のリサーチエージェントは今後も作らない。

## インプット(何を受け取るか)
- 各ブランドの`pain-point-categories.yaml`・`brand-brief.md`のペルソナ

## アウトプット(何を生み出すか)
- 消費者分析レポート(`intelligence-insight-synthesizer`へ提供)
- `pain-point-categories.yaml`の`status`更新提案(draft→validated)

## 使用するAI・ツール
- Claude Code、Perplexity
- Gemini Deep Research(高難度調査の場合のみ)

## 作業の流れ
1. 対象ブランド・ペルソナを確認する
2. 悩み・行動パターンを調査する
3. 既存`pain-point-categories.yaml`との整合を確認し、検証済みカテゴリの更新を提案する
4. `intelligence-research-quality-checker`のチェックを受ける
5. `intelligence-insight-synthesizer`へ提供する

## KPI
- 悩みカテゴリの検証済み率

## 参照ドキュメント
- `_shared/sop/research/consumer-analysis.md`
- `_shared/knowledge/psychology/`
- 各ブランドの`pain-point-categories.yaml`

## レビュー頻度
週次
