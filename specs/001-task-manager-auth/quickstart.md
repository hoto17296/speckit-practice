# Quickstart: タスク管理アプリ（ユーザ認証付き）

**Feature Branch**: `001-task-manager-auth`
**Date**: 2026-01-15

## 前提条件

- Dev Container が起動していること
- PostgreSQL コンテナが稼働し、`app` データベースが作成済みであること

## セットアップ

### 1. データベーススキーマの作成

PostgreSQL に接続してスキーマを作成:

```bash
psql -h localhost -U postgres -d app
```

```sql
-- Users table
CREATE TABLE IF NOT EXISTS users (
    id SERIAL PRIMARY KEY,
    username VARCHAR(50) UNIQUE NOT NULL,
    password_hash VARCHAR(255) NOT NULL,
    created_at TIMESTAMP NOT NULL DEFAULT NOW()
);

-- Tasks table
CREATE TABLE IF NOT EXISTS tasks (
    id SERIAL PRIMARY KEY,
    title VARCHAR(200) NOT NULL,
    is_completed BOOLEAN NOT NULL DEFAULT FALSE,
    user_id INTEGER NOT NULL REFERENCES users(id) ON DELETE CASCADE,
    created_at TIMESTAMP NOT NULL DEFAULT NOW()
);

CREATE INDEX IF NOT EXISTS idx_tasks_user_id ON tasks(user_id);
```

### 2. バックエンドのセットアップ

```bash
cd backend

# 依存関係のインストール
uv sync

# 開発サーバーの起動
uv run uvicorn src.main:app --reload --host 0.0.0.0 --port 8000
```

バックエンドは http://localhost:8000 で起動します。

### 3. フロントエンドのセットアップ

```bash
cd frontend

# 依存関係のインストール
npm install

# 開発サーバーの起動
npm run dev
```

フロントエンドは http://localhost:5173 で起動します。

## 動作確認

### API エンドポイントのテスト

1. **ユーザー登録**:
```bash
curl -X POST http://localhost:8000/auth/register \
  -H "Content-Type: application/json" \
  -d '{"username": "testuser", "password": "testpass"}'
```

2. **ログイン**:
```bash
curl -X POST http://localhost:8000/auth/login \
  -H "Content-Type: application/json" \
  -d '{"username": "testuser", "password": "testpass"}'
```

3. **タスク作成** (トークンを使用):
```bash
TOKEN="<access_token from login response>"
curl -X POST http://localhost:8000/tasks \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer $TOKEN" \
  -d '{"title": "最初のタスク"}'
```

4. **タスク一覧取得**:
```bash
curl http://localhost:8000/tasks \
  -H "Authorization: Bearer $TOKEN"
```

5. **タスク完了状態の更新**:
```bash
curl -X PATCH http://localhost:8000/tasks/1 \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer $TOKEN" \
  -d '{"is_completed": true}'
```

### フロントエンドからの確認

1. http://localhost:5173 にアクセス
2. 「登録」画面でアカウント作成
3. タスク一覧画面でタスクの作成・完了状態の切り替えを確認

## 開発コマンド

### バックエンド

```bash
cd backend

# フォーマット
uv run ruff format src tests

# リント
uv run ruff check src tests

# テスト
uv run pytest
```

### フロントエンド

```bash
cd frontend

# 型チェック
npm run typecheck

# リント
npm run lint

# テスト
npm run test

# ビルド
npm run build
```

## 環境変数

### バックエンド (`backend/.env`)

```env
DATABASE_URL=postgresql://postgres:postgres@localhost:5432/app
JWT_SECRET_KEY=your-secret-key-change-in-production
JWT_ALGORITHM=HS256
JWT_EXPIRE_HOURS=24
```

### フロントエンド (`frontend/.env`)

```env
VITE_API_URL=http://localhost:8000
```

## トラブルシューティング

### データベース接続エラー

- PostgreSQL コンテナが起動しているか確認: `docker ps`
- 接続情報（ホスト、ポート、ユーザー名、パスワード）を確認

### CORS エラー

- バックエンドの CORS 設定でフロントエンドのオリジン（`http://localhost:5173`）が許可されているか確認

### JWT 認証エラー

- トークンの有効期限が切れていないか確認
- Authorization ヘッダーの形式が `Bearer <token>` になっているか確認
