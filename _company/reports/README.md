# レポート

週次・月次・四半期のレポート、およびCEO承認ログをアーカイブする場所です。ファイル名は [命名規則](../../docs/naming-conventions.md) の `{YYYYMMDD}-{事業}-{内容}-{版}` に従います。

## ディレクトリ構造(2026-07-13改定)
機密数値を含むか否かで領域を分離する(`_company/finance/`と同じ考え方)。
```
_company/reports/
├── README.md                  # 本ファイル。Git管理対象
├── architecture-review/       # 構造監査報告書・是正計画(機密数値を含まない)。Git管理対象
├── templates/                  # レポート雛形(数値を含まない)。Git管理対象
│   └── confidential-report-template.md
└── confidential/                # 実際の週次/月次スナップショット・実数値を含むCEO承認ログ。Git管理対象外(.gitkeepのみ追跡)
```

## 内容
- `management-kpi`・`management-resource`・`management-quality`が作成する週次/月次スナップショット → **実数値を含むため`confidential/`へ保存**
- `_shared/sop/management/ceo-approval-flow.md`に基づく承認ログ → 実数値(予算額等)を含む場合は`confidential/`、含まない場合(設計承認等)は本フォルダ直下でも可
- `_shared/sop/management/coo-approval-flow.md`に基づく軽微修正の記録(月次でまとめて記載) → 通常は実数値を含まないため本フォルダ直下

## 参照ドキュメント
- `_shared/sop/management/review-cycle.md`
- `_company/reports/templates/confidential-report-template.md`
