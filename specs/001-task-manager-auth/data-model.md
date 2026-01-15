# Data Model: タスク管理アプリ（ユーザ認証付き）

**Feature Branch**: `001-task-manager-auth`
**Date**: 2026-01-15

## Entity Relationship Diagram

```
┌─────────────────┐       ┌─────────────────┐
│      User       │       │      Task       │
├─────────────────┤       ├─────────────────┤
│ id (PK)         │──┐    │ id (PK)         │
│ username (UQ)   │  │    │ title           │
│ password_hash   │  │    │ is_completed    │
│ created_at      │  └───<│ user_id (FK)    │
└─────────────────┘       │ created_at      │
                          └─────────────────┘
```

## Entities

### User

ユーザーアカウント情報を管理するエンティティ。

| Field | Type | Constraints | Description |
|-------|------|-------------|-------------|
| id | SERIAL | PRIMARY KEY | 一意識別子 |
| username | VARCHAR(50) | UNIQUE, NOT NULL | ユーザー名（大文字小文字区別） |
| password_hash | VARCHAR(255) | NOT NULL | bcrypt ハッシュ化されたパスワード |
| created_at | TIMESTAMP | NOT NULL, DEFAULT NOW() | アカウント作成日時 |

**Validation Rules**:
- username: 1-50文字、空文字不可
- password: ハッシュ化前は任意長（フロントエンドで最低文字数を検証）

**Indexes**:
- `idx_users_username` on `username` (一意制約による自動作成)

### Task

ユーザーが管理するタスク情報を保持するエンティティ。

| Field | Type | Constraints | Description |
|-------|------|-------------|-------------|
| id | SERIAL | PRIMARY KEY | 一意識別子 |
| title | VARCHAR(200) | NOT NULL | タスク名 |
| is_completed | BOOLEAN | NOT NULL, DEFAULT FALSE | 完了状態 |
| user_id | INTEGER | FOREIGN KEY → users(id), NOT NULL | 所有ユーザー |
| created_at | TIMESTAMP | NOT NULL, DEFAULT NOW() | タスク作成日時 |

**Validation Rules**:
- title: 1-200文字、空文字不可

**Indexes**:
- `idx_tasks_user_id` on `user_id` (ユーザーごとのタスク取得高速化)

## Relationships

| Relationship | Type | Description |
|--------------|------|-------------|
| User → Task | 1:N | 1ユーザーは複数タスクを所有可能 |

## Database Schema (PostgreSQL)

```sql
-- Users table
CREATE TABLE users (
    id SERIAL PRIMARY KEY,
    username VARCHAR(50) UNIQUE NOT NULL,
    password_hash VARCHAR(255) NOT NULL,
    created_at TIMESTAMP NOT NULL DEFAULT NOW()
);

-- Tasks table
CREATE TABLE tasks (
    id SERIAL PRIMARY KEY,
    title VARCHAR(200) NOT NULL,
    is_completed BOOLEAN NOT NULL DEFAULT FALSE,
    user_id INTEGER NOT NULL REFERENCES users(id) ON DELETE CASCADE,
    created_at TIMESTAMP NOT NULL DEFAULT NOW()
);

-- Index for task lookup by user
CREATE INDEX idx_tasks_user_id ON tasks(user_id);
```

## State Transitions

### Task.is_completed

```
┌──────────────┐     toggle      ┌──────────────┐
│ is_completed │ ───────────────>│ is_completed │
│   = FALSE    │ <───────────────│   = TRUE     │
└──────────────┘     toggle      └──────────────┘
```

- 初期状態: `FALSE`（未完了）
- トグル操作で `TRUE` ⇔ `FALSE` を切り替え

## Data Access Patterns

### 頻度の高いクエリ

1. **ユーザー認証**: `SELECT * FROM users WHERE username = ?`
2. **タスク一覧取得**: `SELECT * FROM tasks WHERE user_id = ? ORDER BY created_at DESC`
3. **タスク作成**: `INSERT INTO tasks (title, user_id) VALUES (?, ?)`
4. **完了状態更新**: `UPDATE tasks SET is_completed = ? WHERE id = ? AND user_id = ?`

### セキュリティ考慮

- タスク操作時は必ず `user_id` で所有者確認を行う
- パスワードは `password_hash` フィールドにハッシュ値のみ保存
