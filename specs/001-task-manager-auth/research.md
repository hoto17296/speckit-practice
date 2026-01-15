# Research: タスク管理アプリ（ユーザ認証付き）

**Feature Branch**: `001-task-manager-auth`
**Date**: 2026-01-15

## 技術選定の確認

ユーザーが指定した技術スタックに基づき、各技術の採用理由と代替案を記録する。

### 1. バックエンド: Python + FastAPI

**Decision**: FastAPI を使用

**Rationale**:
- 高速な非同期処理サポート
- 自動 OpenAPI ドキュメント生成
- Pydantic によるデータ検証
- 型ヒントによる開発体験向上

**Alternatives Considered**:
- Flask: よりシンプルだが、非同期サポートが限定的
- Django: フルスタックすぎて練習用には過剰

### 2. データベース接続: Psycopg3

**Decision**: Psycopg3 を使用（ORM なし）

**Rationale**:
- PostgreSQL 公式推奨ドライバ
- 非同期サポート（asyncpg 相当の機能）
- シンプルな SQL 直接実行で学習効果が高い
- Constitution の「シンプルさ優先」に合致

**Alternatives Considered**:
- SQLAlchemy: 強力だが、練習用には抽象化が過剰
- asyncpg: 非同期専用で良いが、Psycopg3 が同等機能を提供

### 3. パッケージ管理: uv

**Decision**: uv を使用

**Rationale**:
- pip/poetry より高速
- 標準的な pyproject.toml 形式をサポート
- シンプルなコマンド体系

**Alternatives Considered**:
- pip + venv: 標準だが遅い
- Poetry: 機能豊富だが設定が複雑

### 4. フロントエンド: React + Vite (SPA)

**Decision**: React 18+ と Vite を使用

**Rationale**:
- React: 最も広く採用されている UI ライブラリ
- Vite: 高速な開発サーバーとビルド
- SPA: サーバーレンダリング不要のシンプルな構成

**Alternatives Considered**:
- Next.js: SSR/SSG が不要なため過剰
- Create React App: 非推奨、Vite が推奨

### 5. 認証方式

**Decision**: JWT (JSON Web Token) によるステートレス認証

**Rationale**:
- シンプルな実装
- サーバーサイドセッション管理が不要
- FastAPI との統合が容易
- 標準的な認証方式

**Implementation Details**:
- ログイン時に JWT を発行
- フロントエンドは localStorage に保存
- API リクエスト時に Authorization ヘッダーで送信
- トークン有効期限: 24時間（spec の Assumptions に準拠）

**Alternatives Considered**:
- Session Cookie: サーバーサイド状態管理が必要
- OAuth2: 外部認証プロバイダ不要のため過剰

### 6. パスワードハッシュ

**Decision**: bcrypt を使用

**Rationale**:
- 業界標準のパスワードハッシュアルゴリズム
- Python の passlib ライブラリで簡単に使用可能
- 適切なソルト処理が自動で行われる

**Alternatives Considered**:
- Argon2: より新しいが、bcrypt で十分
- SHA-256 + salt: bcrypt より安全性が低い

## 未解決事項

技術スタックがユーザーにより明確に指定されているため、NEEDS CLARIFICATION はなし。

## 依存ライブラリ一覧

### Backend (Python)

```
fastapi
uvicorn[standard]
psycopg[binary]
python-jose[cryptography]  # JWT handling
passlib[bcrypt]            # Password hashing
pydantic-settings          # Configuration management
```

### Frontend (TypeScript)

```
react
react-dom
react-router-dom          # SPA routing
@types/react
@types/react-dom
```

## セキュリティ考慮事項

1. **パスワード**: bcrypt でハッシュ化、平文保存禁止
2. **SQL インジェクション**: パラメータ化クエリを使用
3. **CORS**: フロントエンドのオリジンのみ許可
4. **JWT**: 秘密鍵は環境変数で管理、有効期限を設定
5. **入力検証**: Pydantic でバリデーション
