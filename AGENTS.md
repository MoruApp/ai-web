## 参照ドキュメント

以下のドキュメントを作業前に必ず確認すること：

### プロジェクト情報
- `spec/About_Project.md` - プロジェクトの背景・目的・スコープ・ステークホルダー
- `spec/Implementation_Plan.md` - アーキテクチャ・フェーズ・技術選定の計画
- `spec/Task.md` - タスク一覧と進捗状況

### 開発ガイドライン（Submodule: rules/）
- `rules/Dev_Rule.md` - 開発ルール・コーディング規約
- `rules/Security_Guidelines.md` - セキュリティガイドライン（OWASP Top 10）
- `rules/API_Design_Guidelines.md` - API設計ガイドライン（REST API）
- `rules/Architecture_Guidelines.md` - アーキテクチャガイドライン（SOLID, DDD）
- `rules/Cloud_Guidelines.md` - クラウドガイドライン（12 Factor App）
- `rules/Testing_Guidelines.md` - テストガイドライン（pytest, TDD）
- `rules/Frontend_Guidelines.md` - フロントエンドガイドライン（React, Next.js）
- `rules/UI_Design_Guidelines.md` - UIデザインガイドライン（WCAG）

### ソースドキュメント
- `source/` - 変換されたソースドキュメント（pptx, pdf等からmd変換）

## コードスタイル

- Python 3.10+ を使用
- 型ヒントを必須とする
- docstring は Google スタイル
- フォーマッタ: ruff format
- リンター: ruff

## 重要な注意事項

1. **[要確認] タグの扱い**
   - ドキュメント内の `[要確認]` は未確定情報を示す

2. **セキュリティ**
   - API キーや認証情報は `.env` に保存し、Git にコミットしない

3. **言語**
   - ドキュメント・コメントは日本語を基本とする
