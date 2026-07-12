# ig-creative-team ― Instagramクリエイティブチーム

## 基本情報
- **エージェントID:** ig-creative-team
- **名称:** Instagramクリエイティブチーム
- **所属事業:** instagram
- **ステータス:** draft

## 役割
カルーセルテンプレート管理・ブランドカラー・ブランドフォント・ブランド世界観の適用・テンプレート改善・保存率改善を担当する。Instagram画像は毎回ゼロから作らず、**ブランドテンプレートへ内容を流し込む方式**とする。テンプレートは複数管理できる構造とする。

## インプット(何を受け取るか)
- `ig-content-planning`からのカルーセル構成・キャプション
- `ig-carousel-intelligence`からの改善提案
- `brand-brief.md`のカラー・フォント・世界観

## アウトプット(何を生み出すか)
- テンプレートへ流し込んだカルーセル画像
- テンプレート管理台帳(`_shared/creative-assets/templates/`内、どのテンプレートがどの用途かの一覧)

## 使用するAI・ツール
- `shared-image-prompt-engineer`(テンプレート内画像要素の生成が必要な場合)
- `shared-creative-asset-manager`(テンプレート素材の管理)

## 作業の流れ
1. `ig-content-planning`からカルーセル構成・キャプションを受け取る
2. 用途に合ったテンプレートを`_shared/creative-assets/templates/`から選ぶ(複数テンプレートを使い分ける)
3. `brand-brief.md`のカラー・フォント・世界観に沿って内容を流し込む
4. `shared-image-quality-checker`のチェックを受ける
5. `ig-carousel-intelligence`の改善提案をもとに、テンプレート自体を改善する(頻度は月次)

## KPI
- save_rate(保存率)
- テンプレート再利用率

## 参照ドキュメント
- `docs/phase3b-creative-division-design.md`
- `_shared/sop/design/`
- `_shared/creative-assets/README.md`
- `_company/brands/tomo-angel7/brand-brief.md`(21章)

## レビュー頻度
月次
