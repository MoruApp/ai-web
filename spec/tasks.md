# Ai Web - Implementation Tasks

## Summary

| Phase        | Tasks | Status   |
|--------------|-------|----------|
| Setup        | 4 tasks | pending |
| Core         | 10 tasks | pending |
| Integration  | 4 tasks | pending |
| Testing      | 4 tasks | pending |

---

## Phase 1: Setup & Configuration

### TASK-001: プロジェクト初期構成のセットアップ

**Description**: AIコンシェルジュ Webアプリ（フロント＋バックエンド）の基本ディレクトリ構成と設定ファイルを作成する。

**Deliverables**:
- [ ] プロジェクトルート構成の作成
- [ ] Node.js + TypeScript ベースのモノレポ or シンプル構成の決定
- [ ] `package.json` の初期化
- [ ] TypeScript 設定ファイル `tsconfig.json` の作成
- [ ] ESLint / Prettier の導入と設定

**推奨構成例**:
```
aic-web/
├── apps/
│   ├── web/              # Next.js or React SPA
│   └── api/              # Node/Express or NestJS API
├── packages/
│   └── shared/           # 共通型定義・ユーティリティ
├── config/
│   ├── eslint/
│   └── prettier/
├── tests/
│   ├── unit/
│   └── e2e/
└── docs/
    └── requirements/
```

**AIエージェント用コマンド例**:
```bash
mkdir -p aic-web/{apps,packages,config,tests,docs}
cd aic-web
npm init -y

# TypeScript
npm install -D typescript ts-node @types/node
npx tsc --init

# Lint / Format
npm install -D eslint prettier eslint-config-prettier eslint-plugin-import
```

**Dependencies**: None  
**Estimated Complexity**: Low  
**Status**: pending

---

### TASK-002: 開発環境・ランタイム設定

**Description**: 開発・本番を想定した環境変数と Docker ベースの実行環境を準備する。

**Deliverables**:
- [ ] `.env.example` の作成（APIキーやDB接続情報のダミー値）
- [ ] `docker-compose.yml` の作成（DB, API, Web）
- [ ] `Dockerfile`（API / Web）の作成
- [ ] Node.js バージョンを `.nvmrc` などで固定

**.env.example 例**:
```env
# 共通
NODE_ENV=development
APP_PORT=3000

# API
API_PORT=4000
API_BASE_URL=http://localhost:4000

# DB
DB_HOST=db
DB_PORT=5432
DB_USER=aic_user
DB_PASSWORD=change_me
DB_NAME=aic_db

# 認証（社内SSO連携を想定したプレースホルダ）
SSO_ISSUER_URL=https://sso.example.com
SSO_CLIENT_ID=your_client_id
SSO_CLIENT_SECRET=your_client_secret

# LLM / ベクターストア（後続タスクで利用）
OPENAI_API_KEY=your_openai_key
VECTOR_DB_URL=http://vector-db:6333
```

**docker-compose.yml（骨組み）例**:
```yaml
version: "3.9"
services:
  db:
    image: postgres:16
    environment:
      POSTGRES_USER: aic_user
      POSTGRES_PASSWORD: change_me
      POSTGRES_DB: aic_db
    ports:
      - "5432:5432"
    volumes:
      - db-data:/var/lib/postgresql/data

  api:
    build: ./apps/api
    env_file: .env
    ports:
      - "4000:4000"
    depends_on:
      - db

  web:
    build: ./apps/web
    env_file: .env
    ports:
      - "3000:3000"
    depends_on:
      - api

volumes:
  db-data:
```

**Dependencies**: TASK-001  
**Estimated Complexity**: Low  
**Status**: pending

---

### TASK-003: Git / CI 設定の初期化

**Description**: バージョン管理と簡易 CI（lint / test）を設定する。

**Deliverables**:
- [ ] `.gitignore` の作成
- [ ] Git リポジトリ初期化
- [ ] GitHub Actions などの CI 設定ファイル（lint + unit test）

**.gitignore 例**:
```gitignore
node_modules/
dist/
build/
.env
.env.*
coverage/
.DS_Store
```

**簡易 CI 例（.github/workflows/ci.yml）**:
```yaml
name: CI

on:
  push:
    branches: [ main ]
  pull_request:

jobs:
  build-and-test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-node@v4
        with:
          node-version: '20'
      - run: npm install
      - run: npm run lint
      - run: npm test
```

**Dependencies**: TASK-001  
**Estimated Complexity**: Low  
**Status**: pending

---

### TASK-004: 要件・仕様ドキュメントのプロジェクト内整理

**Description**: 収集済みの企画書・要件定義をリポジトリ内に整理し、開発参照用に Markdown 化する。

**Deliverables**:
- [ ] `docs/requirements/` ディレクトリ作成
- [ ] `docs/requirements/overview.md` にプロジェクト概要を整理
- [ ] `docs/requirements/features.md` に機能要件（MUST/WANT）を整理
- [ ] `docs/requirements/persona_userstories.md` にペルソナとユーザーストーリーを整理

**AIエージェント用作業例**:
```bash
mkdir -p docs/requirements
# 収集情報をもとに overview.md / features.md / persona_userstories.md を生成
```

**Dependencies**: TASK-001  
**Estimated Complexity**: Low  
**Status**: pending

---

## Phase 2: Core Implementation

### TASK-005: 画面遷移・ルーティング設計（フロントエンド）

**Description**: 画面遷移図に基づき、フロントエンドのルーティング構成を定義する。

**対象画面**:
- ログイン
- ホーム画面
- 質問する（チャット画面）
- 問い合わせ履歴
- FAQ一覧（管理者）

**Deliverables**:
- [ ] Next.js or React Router によるルーティング設定
- [ ] 各ページのプレースホルダコンポーネント作成
- [ ] 共通レイアウト（ヘッダー／フッター／ナビゲーション）

**ディレクトリ例（Next.js App Router）**:
```
apps/web/src/app/
├── layout.tsx
├── page.tsx              # ホーム
├── login/
│   └── page.tsx
├── chat/
│   └── page.tsx          # 質問する
├── history/
│   └── page.tsx          # 問い合わせ履歴
└── admin/
    └── faq/
        └── page.tsx      # FAQ一覧（管理者）
```

**Dependencies**: TASK-001, TASK-002  
**Estimated Complexity**: Medium  
**Status**: pending

---

### TASK-006: UI ワイヤーフレームに基づく基本 UI コンポーネント実装

**Description**: シンプルな UI ワイヤーフレームを想定し、レスポンシブ対応の基本 UI を実装する。

**Deliverables**:
- [ ] 共通 UI コンポーネント（ボタン、入力フォーム、カード、モーダル）
- [ ] レスポンシブレイアウト（PC/スマホ）
- [ ] ログイン画面の簡易 UI
- [ ] ホーム画面（「質問する」「履歴を見る」などの導線）
- [ ] 管理者向けナビゲーション（FAQ管理、ログ閲覧）

**実装ヒント**:
- UI ライブラリ: Chakra UI / MUI / Tailwind CSS のいずれかを採用
- モバイルファーストで CSS を設計

**Dependencies**: TASK-005  
**Estimated Complexity**: Medium  
**Status**: pending

---

### TASK-007: バックエンド API 基盤実装

**Description**: FAQ検索・問い合わせ履歴・管理機能を提供する REST API / GraphQL API の基盤を実装する。

**Deliverables**:
- [ ] Node.js + Express or NestJS プロジェクト初期化（`apps/api`）
- [ ] 共通ミドルウェア（ログ、エラーハンドリング、CORS）
- [ ] ヘルスチェックエンドポイント `/health`
- [ ] OpenAPI（Swagger）による API 定義の雛形

**ディレクトリ例（Express）**:
```
apps/api/src/
├── index.ts
├── routes/
│   ├── health.ts
│   ├── faq.ts
│   ├── history.ts
│   └── admin.ts
├── middleware/
│   ├── errorHandler.ts
│   └── auth.ts
└── config/
    └── env.ts
```

**Dependencies**: TASK-001, TASK-002  
**Estimated Complexity**: Medium  
**Status**: pending

---

### TASK-008: データモデル設計と DB マイグレーション

**Description**: FAQ、問い合わせ履歴、ユーザー情報、会話ログなどのデータモデルを設計し、DB スキーマを定義する。

**対象エンティティ（例）**:
- `users`（社内ユーザー）
- `faqs`（FAQエントリ）
- `questions`（ユーザーからの質問）
- `answers`（AIまたは手動回答）
- `conversations`（会話セッション）
- `categories`（FAQカテゴリ）

**Deliverables**:
- [ ] ER 図（簡易で可）を `docs/` に作成
- [ ] ORM（Prisma / TypeORM など）の導入
- [ ] 初期マイグレーションファイル作成
- [ ] DB 接続設定と起動確認

**Prisma 例** (`apps/api/prisma/schema.prisma` 抜粋):
```prisma
model User {
  id        String   @id @default(cuid())
  email     String   @unique
  name      String?
  role      String   // 'employee' | 'admin'
  createdAt DateTime @default(now())
}

model Faq {
  id          String   @id @default(cuid())
  question    String
  answer      String
  category    String?
  createdById String?
  createdBy   User?    @relation(fields: [createdById], references: [id])
  updatedAt   DateTime @updatedAt
}

model Question {
  id          String   @id @default(cuid())
  userId      String
  user        User     @relation(fields: [userId], references: [id])
  content     String
  createdAt   DateTime @default(now())
}

model Answer {
  id          String   @id @default(cuid())
  questionId  String
  question    Question @relation(fields: [questionId], references: [id])
  content     String
  source      String   // 'faq' | 'llm'
  createdAt   DateTime @default(now())
}
```

**Dependencies**: TASK-002, TASK-007  
**Estimated Complexity**: Medium  
**Status**: pending

---

### TASK-009: 認証・認可（社内ログイン連携の土台）

**Description**: 社内認証連携を想定したログイン機能の土台を実装し、一般社員／管理者の権限を区別する。

**Deliverables**:
- [ ] JWT ベース or SSO プロバイダ連携の抽象化インターフェース
- [ ] ログインエンドポイント（モック実装可）
- [ ] 認証ミドルウェア（API）
- [ ] フロントエンドのログインフロー（トークン保存・リダイレクト）
- [ ] 管理者ロール判定ロジック

**実装ヒント**:
- MVP 段階では「メールアドレス＋パスワード」や「固定ユーザー」でモックし、後から SSO に差し替え可能な構造にする。

**Dependencies**: TASK-007, TASK-008  
**Estimated Complexity**: Medium  
**Status**: pending

---

### TASK-010: FAQ 検索・回答 API 実装（MUST 機能）

**Description**: 社内 FAQ の検索・回答機能を提供する API を実装する。

**Deliverables**:
- [ ] FAQ 検索 API（キーワード検索・カテゴリフィルタ）
- [ ] FAQ 詳細取得 API
- [ ] FAQ ベースの回答生成ロジック（単純マッチング版）
- [ ] フロントエンドからの FAQ 検索 UI と連携

**API 例**:
- `GET /api/faqs?query=...&category=...`
- `GET /api/faqs/:id`

**Dependencies**: TASK-007, TASK-008  
**Estimated Complexity**: Medium  
**Status**: pending

---

### TASK-011: 問い合わせ履歴・会話ログ管理機能実装（MUST 機能）

**Description**: ユーザーの問い合わせ履歴と AI との会話ログを保存・閲覧できるようにする。

**Deliverables**:
- [ ] 問い合わせ作成 API（質問＋AI回答を保存）
- [ ] ユーザー別履歴一覧 API
- [ ] 履歴詳細 API（会話単位）
- [ ] フロントエンドの「問い合わせ履歴」画面実装

**API 例**:
- `GET /api/history`（ログインユーザーの履歴）
- `GET /api/history/:id`
- `POST /api/questions`（質問送信＋回答生成をトリガー）

**Dependencies**: TASK-008, TASK-010  
**Estimated Complexity**: Medium  
**Status**: pending

---

### TASK-012: 管理者向け FAQ 管理機能実装（MUST 機能）

**Description**: 管理者が FAQ を追加・編集・削除できる管理画面と API を実装する。

**Deliverables**:
- [ ] FAQ CRUD API（管理者専用）
- [ ] 管理者認可チェック（ミドルウェア）
- [ ] 管理画面 UI（一覧・検索・新規作成・編集）
- [ ] バリデーション（必須項目、文字数など）

**API 例**:
- `GET /api/admin/faqs`
- `POST /api/admin/faqs`
- `PUT /api/admin/faqs/:id`
- `DELETE /api/admin/faqs/:id`

**Dependencies**: TASK-009, TASK-010  
**Estimated Complexity**: Medium  
**Status**: pending

---

### TASK-013: AI 連携による自然言語回答機能（MUST 機能）

**Description**: LLM を用いて自然言語で回答を生成し、FAQ ベースの回答と組み合わせる。

**Deliverables**:
- [ ] LLM クライアントモジュール（OpenAI など）
- [ ] 質問入力 → FAQ 検索 → LLM へのプロンプト構築 → 回答生成のフロー
- [ ] 回答内容と使用した FAQ のログ保存
- [ ] フロントエンドの「質問する」画面とリアルタイム連携（ローディング表示など）

**実装ヒント**:
- MVP ではシンプルなプロンプトで開始し、後続タスクで精度改善を行う。
- FAQ の上位 N 件をコンテキストとして LLM に渡す。

**Dependencies**: TASK-010, TASK-011  
**Estimated Complexity**: High  
**Status**: pending

---

### TASK-014: WANT 機能 - 回答精度の自動改善・カテゴリ自動分類の基盤

**Description**: 回答ログを分析し、FAQ の改善やカテゴリ自動分類に活用するための基盤を実装する。

**Deliverables**:
- [ ] 回答フィードバック項目（役に立った/役に立たなかった）を UI に追加
- [ ] フィードバック保存 API
- [ ] カテゴリ自動推定ロジック（LLM or ルールベース）
- [ ] FAQ 登録時にカテゴリ候補を提示する仕組み（管理画面）

**Dependencies**: TASK-011, TASK-013  
**Estimated Complexity**: Medium  
**Status**: pending

---

## Phase 3: Integration

### TASK-015: フロントエンドとバックエンド API の統合

**Description**: フロントエンドからバックエンド API を呼び出し、エンドツーエンドで主要ユースケースを動作させる。

**対象ユースケース**:
- ログイン → ホーム → 質問 → 回答表示
- ホーム → 問い合わせ履歴 → 履歴詳細
- 管理者ログイン → FAQ一覧 → FAQ登録・編集

**Deliverables**:
- [ ] API クライアントモジュール（フロント側）
- [ ] エラーハンドリング・トースト通知
- [ ] ローディング状態管理
- [ ] 統合テスト用のシナリオスクリプト

**Dependencies**: TASK-005〜TASK-013  
**Estimated Complexity**: Medium  
**Status**: pending

---

### TASK-016: 社内ネットワーク限定利用のための設定（非機能要件）

**Description**: 社内ネットワーク内のみで利用できるよう、デプロイ設定・アクセス制御を検討・反映する。

**Deliverables**:
- [ ] 想定デプロイ環境の整理（社内クラウド / オンプレ）
- [ ] ネットワーク制限方法のドキュメント化（IP 制限 / VPN 前提など）
- [ ] アプリケーションレベルの CORS / オリジン制限設定
- [ ] `.env` に本番用設定項目を追加

**Dependencies**: TASK-002, TASK-007  
**Estimated Complexity**: Medium  
**Status**: pending

---

### TASK-017: パフォーマンス・レスポンス時間対策（2秒以内）

**Description**: レスポンス 2 秒以内を目標に、ボトルネックを洗い出し、対策を実装する。

**Deliverables**:
- [ ] 主要 API のレスポンス時間計測（ログ出力）
- [ ] LLM 呼び出しのタイムアウト設定
- [ ] キャッシュ戦略（FAQ データのキャッシュなど）
- [ ] フロントエンドでのスケルトン UI / 遅延読み込み

**Dependencies**: TASK-013, TASK-015  
**Estimated Complexity**: Medium  
**Status**: pending

---

### TASK-018: ログ・監査機能の実装（管理者向けログ閲覧の土台）

**Description**: 管理者が AI の回答ログを確認できるようにするためのログ保存と簡易閲覧機能を実装する。

**Deliverables**:
- [ ] API リクエスト／レスポンスの要約ログ保存（個人情報に配慮）
- [ ] 管理者向けログ一覧 API
- [ ] 管理画面でのログ一覧・フィルタ UI（期間・ユーザー・キーワード）
- [ ] ログローテーション／保持期間の設定

**Dependencies**: TASK-011, TASK-012  
**Estimated Complexity**: Medium  
**Status**: pending

---

## Phase 4: Testing & Documentation

### TASK-019: ユニットテスト・API テストの整備

**Description**: コア機能に対するユニットテストと API テストを作成する。

**Deliverables**:
- [ ] テストフレームワーク導入（Jest / Vitest など）
- [ ] FAQ 検索ロジックのユニットテスト
- [ ] 認証・認可ロジックのユニットテスト
- [ ] API エンドポイントの統合テスト（Supertest など）

**Dependencies**: TASK-007〜TASK-013  
**Estimated Complexity**: Medium  
**Status**: pending

---

### TASK-020: E2E テスト（主要ユーザーストーリー）

**Description**: ペルソナ・ユーザーストーリーに基づき、E2E テストを実装する。

**対象ストーリー**:
- 一般社員：手続き方法をすぐ調べたい
- 一般社員：以前の質問履歴を見たい
- 管理者：AI の回答ログを確認したい
- 管理者：FAQ を簡単に追加したい

**Deliverables**:
- [ ] Playwright / Cypress による E2E テスト環境
- [ ] 上記 4 ストーリーをカバーするテストシナリオ
- [ ] CI での E2E テスト実行設定

**Dependencies**: TASK-015, TASK-018  
**Estimated Complexity**: Medium  
**Status**: pending

---

### TASK-021: 利用マニュアル（簡易版）の作成

**Description**: 一般社員・管理者向けの簡易利用マニュアルを作成する。

**Deliverables**:
- [ ] 一般社員向けマニュアル（ログイン方法、質問方法、履歴の見方）
- [ ] 管理者向けマニュアル（FAQ 管理、ログ閲覧、フィードバック確認）
- [ ] `docs/manuals/` ディレクトリに Markdown 形式で保存

**ディレクトリ例**:
```
docs/manuals/
├── employee_guide.md
└── admin_guide.md
```

**Dependencies**: TASK-015, TASK-018  
**Estimated Complexity**: Low  
**Status**: pending

---

### TASK-022: MVP リリース準備・チェックリスト作成

**Description**: 1ヶ月以内の β 版公開に向けて、リリースチェックリストを作成し、MVP 要件を満たしているか確認する。

**Deliverables**:
- [ ] MVP 必須機能チェックリスト（要件定義の MUST 機能ベース）
- [ ] 既知の制約・未対応項目リスト
- [ ] 社内試験運用手順書（テスト期間・対象部署・フィードバック方法）

**Dependencies**: TASK-019, TASK-020, TASK-021  
**Estimated Complexity**: Low  
**Status**: pending

---

## Progress Tracking

Last Updated: 2025-11-27 22:33

| Task ID  | Description                                   | Status   | Notes |
|----------|-----------------------------------------------|----------|-------|
| TASK-001 | プロジェクト初期構成のセットアップ           | pending  |       |
| TASK-002 | 開発環境・ランタイム設定                     | pending  |       |
| TASK-003 | Git / CI 設定の初期化                        | pending  |       |
| TASK-004 | 要件・仕様ドキュメントのプロジェクト内整理   | pending  |       |
| TASK-005 | 画面遷移・ルーティング設計（フロント）       | pending  |       |
| TASK-006 | 基本 UI コンポーネント実装                   | pending  |       |
| TASK-007 | バックエンド API 基盤実装                    | pending  |       |
| TASK-008 | データモデル設計と DB マイグレーション       | pending  |       |
| TASK-009 | 認証・認可（社内ログイン連携の土台）         | pending  |       |
| TASK-010 | FAQ 検索・回答 API 実装                      | pending  |       |
| TASK-011 | 問い合わせ履歴・会話ログ管理機能実装         | pending  |       |
| TASK-012 | 管理者向け FAQ 管理機能実装                 | pending  |       |
| TASK-013 | AI 連携による自然言語回答機能               | pending  |       |
| TASK-014 | WANT機能: 回答精度改善・カテゴリ自動分類基盤 | pending  |       |
| TASK-015 | フロントと API の統合                       | pending  |       |
| TASK-016 | 社内ネットワーク限定利用のための設定        | pending  |       |
| TASK-017 | パフォーマンス・レスポンス時間対策          | pending  |       |
| TASK-018 | ログ・監査機能の実装                        | pending  |       |
| TASK-019 | ユニットテスト・API テストの整備            | pending  |       |
| TASK-020 | E2E テスト（主要ユーザーストーリー）        | pending  |       |
| TASK-021 | 利用マニュアル（簡易版）の作成              | pending  |       |
| TASK-022 | MVP リリース準備・チェックリスト作成        | pending  |       |