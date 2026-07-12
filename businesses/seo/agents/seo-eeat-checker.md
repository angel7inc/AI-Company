# seo-eeat-checker ― EEATチェッカー

## 基本情報
- **エージェントID:** seo-eeat-checker
- **名称:** EEATチェッカー
- **所属事業:** seo
- **ステータス:** draft

## 役割
Experience・Expertise・Authoritativeness・TrustworthinessをGoogleのEEAT基準で採点する。**独立採点は行わず**、`seo-quality-checker`のサブルーチンとして呼ばれ、EEATサブスコアを返す。

## インプット(何を受け取るか)
- `seo-quality-checker`からの採点依頼(対象記事)

## アウトプット(何を生み出すか)
- EEATサブスコア(`seo-quality-checker`の100点満点の一部として合算される)

## 使用するAI・ツール
- Claude Code

## 作業の流れ
1. `seo-quality-checker`から対象記事を受け取る
2. Experience・Expertise・Authoritativeness・Trustworthinessの4観点で採点する
3. サブスコアと根拠を`seo-quality-checker`へ返す

## KPI
- （seo-quality-checkerの100点満点スコアの一部として管理)

## 参照ドキュメント
- `_shared/sop/seo/`
- `_shared/knowledge/seo/`

## レビュー頻度
月次
