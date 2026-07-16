# KPI

全社・事業横断のKPI定義とダッシュボード設定を置く場所です。`management-kpi`エージェントが、`_company/org/agents.yaml`の`kpi`欄・各ブランドの`brand-brief.md`KPI表・各事業の`data/`から集計したダッシュボードをここに保存します(KPIの値そのものの正本はこれらの参照元であり、ここは集約結果の置き場です)。

## ディレクトリ構造(2026-07-14改定、KPI Definition Registry追加)
`_company/finance/`と同じ「構造・スキーマ=Git管理、実データ=Git管理対象外」の型を適用する。
```
_company/kpi/
├── README.md              # 本ファイル。Git管理対象
├── schemas/                # KPI実績・KPI定義の構造定義(数値を含まないサンプル)。Git管理対象
│   ├── kpi-actuals-schema.yaml
│   └── kpi-definition-schema.yaml
├── templates/              # KPI定義の空テンプレート。Git管理対象
│   └── kpi-definition-template.yaml
├── definitions/            # 承認済みKPI定義の保存候補領域(2026-07-14時点、実定義は未作成)。Git管理対象
│   └── README.md
└── actuals/                # 実際のKPI数値(週次/月次スナップショット)。Git管理対象外(.gitkeepのみ追跡)
```
`actuals/`配下は`.gitignore`で除外されており、Private Repositoryであっても実際の数値はGitHubへ送信されない。

## 関連レイヤーとの役割分担
- **`definitions/`(本フォルダ)**: KPIの意味・単位・計算式・版管理等、**正式なKPI定義**の保存候補領域。詳細は[`docs/runbooks/kpi-definition-governance.md`](../../docs/runbooks/kpi-definition-governance.md)参照
- **`actuals/`(本フォルダ)**: 上流データから作成される**集約・表示結果**(本README上記のとおり)
- **`_company/analytics/`**: 非財務KPIの**観測値・証跡・訂正**(Git管理外領域+空テンプレート)。詳細は[`docs/runbooks/kpi-data-entry-and-validation.md`](../../docs/runbooks/kpi-data-entry-and-validation.md)参照
- **`_company/finance/`**: 売上・広告費等**金額P&L明細の正本**。詳細は[`_company/finance/README.md`](../finance/README.md)参照

## ブランドに紐づかない全社インフラKPI
自動化率・Agent成功率・API成功率・平均処理時間・失敗率・リトライ率(Automation Division)は、特定ブランドの事業指標ではなく全社インフラの健全性指標のため、`brand-brief.md`には追加せず、ここで直接管理する。計算元は`automation-monitor`(実行成否)・`management-resource`(コスト・トークン)。

## 会社全体の利益・ROI(Finance Division)
`finance-controller`が`revenue-analytics`(売上)・`management-resource`(AI/APIコスト)を合算して算出する月次利益・会社全体ROIも、特定ブランドに紐づかないためここで管理する。ブランド/商品単位のROIは引き続き各ブランドの`brand-brief.md`・`_company/org/products.yaml`が正本。

## 参照ドキュメント
- `_company/agents/management-kpi.md`
- `_company/agents/automation-monitor.md`
- `_company/agents/finance-controller.md`
- `_shared/sop/management/review-cycle.md`
