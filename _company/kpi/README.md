# KPI

全社・事業横断のKPI定義とダッシュボード設定を置く場所です。`management-kpi`エージェントが、`_company/org/agents.yaml`の`kpi`欄・各ブランドの`brand-brief.md`KPI表・各事業の`data/`から集計したダッシュボードをここに保存します(KPIの値そのものの正本はこれらの参照元であり、ここは集約結果の置き場です)。

## ブランドに紐づかない全社インフラKPI
自動化率・Agent成功率・API成功率・平均処理時間・失敗率・リトライ率(Automation Division)は、特定ブランドの事業指標ではなく全社インフラの健全性指標のため、`brand-brief.md`には追加せず、ここで直接管理する。計算元は`automation-monitor`(実行成否)・`management-resource`(コスト・トークン)。

## 会社全体の利益・ROI(Finance Division)
`finance-controller`が`revenue-analytics`(売上)・`management-resource`(AI/APIコスト)を合算して算出する月次利益・会社全体ROIも、特定ブランドに紐づかないためここで管理する。ブランド/商品単位のROIは引き続き各ブランドの`brand-brief.md`・`_company/org/products.yaml`が正本。

## 参照ドキュメント
- `_company/agents/management-kpi.md`
- `_company/agents/automation-monitor.md`
- `_company/agents/finance-controller.md`
- `_shared/sop/management/review-cycle.md`
