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
- フォーマッタ: ruff format（または black, isort）
- リンター: ruff（または flake8, mypy）

## 重要な注意事項

1. **[要確認] タグの扱い**
   - ドキュメント内の `[要確認]` は未確定情報を示す
   - 確定情報と未確定情報を明確に区別して説明すること

2. **セキュリティ**
   - API キーや認証情報は `.env` に保存し、Git にコミットしない
   - `.token_cache.json` は共有禁止

3. **言語**
   - ドキュメント・コメントは日本語を基本とする

## ワーキングアグリーメント

- コード変更後は必ず `pytest` でテストを実行
- 型チェック (`mypy .`) をパスすること
- コミット前に `ruff format` と `ruff check` でフォーマット・リント
- PR作成時は変更内容を日本語で説明

## Submodule の更新

共通ルール（rules/）が更新された場合：

```bash
git submodule update --remote --merge
git add rules
git commit -m "chore: update rules submodule"
git push
```
