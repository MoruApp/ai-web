# Project Template

AI 開発支援用のプロジェクトテンプレート。

## 構成

```
.
├── .github/workflows/     # GitHub Actions
│   └── update-submodules.yml  # Submodule 自動更新
├── rules/                 # 開発ルール（Submodule: MoruApp/data-rule）
├── spec/                  # プロジェクト固有の仕様書
│   ├── About_Project.md
│   ├── Task.md
│   └── Implementation_Plan.md
├── source/                # ソースドキュメント（変換済み）
├── src/                   # ソースコード
├── CLAUDE.md              # Claude Code 用設定
├── AGENTS.md              # AI エージェント用設定
└── README.md
```

## 使い方

### 1. テンプレートから新規プロジェクト作成

1. GitHub で "Use this template" をクリック
2. 新しいリポジトリ名を入力
3. Clone 後、Submodule を初期化：

```bash
git clone --recursive https://github.com/YOUR_ORG/YOUR_PROJECT.git
cd YOUR_PROJECT

# または既存のクローンに対して
git submodule init
git submodule update
```

### 2. プロジェクト情報を編集

`spec/` 配下のファイルを編集：

- `spec/About_Project.md` - プロジェクト概要
- `spec/Task.md` - タスク一覧
- `spec/Implementation_Plan.md` - 実装計画

### 3. Submodule の更新

共通ルール（rules/）が更新された場合：

```bash
git submodule update --remote --merge
git add rules
git commit -m "chore: update rules submodule"
git push
```

GitHub Actions で毎日自動更新も実行されます。

## 開発ガイドライン

`rules/` 配下の共通ガイドラインを参照：

- 開発ルール・コーディング規約
- セキュリティガイドライン
- API 設計ガイドライン
- アーキテクチャガイドライン
- テストガイドライン
- フロントエンドガイドライン
- UI デザインガイドライン

## AI 開発支援

このテンプレートは以下の AI ツールとの連携を想定：

- **Claude Code** - `CLAUDE.md` を参照
- **GitHub Copilot** - コードベースを学習
- **Cursor** - `AGENTS.md` を参照
