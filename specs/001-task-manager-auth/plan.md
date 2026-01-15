# Implementation Plan: タスク管理アプリ（ユーザ認証付き）

**Branch**: `001-task-manager-auth` | **Date**: 2026-01-15 | **Spec**: [spec.md](./spec.md)
**Input**: Feature specification from `/specs/001-task-manager-auth/spec.md`

## Summary

ユーザー認証機能を持つタスク管理アプリケーションを構築する。認証は username/password による簡易方式を採用し、タスクは「タスク名」と「完了状態」のみを管理する。バックエンドは Python + FastAPI、フロントエンドは TypeScript + React (SPA) で構成する。

## Technical Context

**Language/Version**: Python 3.11+ (Backend), TypeScript 5.x (Frontend)
**Primary Dependencies**: FastAPI, Psycopg3 (Backend), React 18+, Vite (Frontend)
**Storage**: PostgreSQL (Dev Container で `app` データベースが利用可能)
**Testing**: pytest (Backend), Vitest (Frontend)
**Target Platform**: Web (Linux server for backend, modern browsers for frontend)
**Project Type**: Web application (frontend + backend)
**Performance Goals**: タスク一覧 1秒以内表示、完了切り替え 0.5秒以内反映
**Constraints**: 100件以下のタスクを想定、同時接続数は少数
**Scale/Scope**: 練習用プロジェクト、少数ユーザー

**追加技術詳細**:
- パッケージ管理: uv (Backend)
- フォーマッタ: Ruff (Backend)
- ORM: 使用しない（Psycopg3 で直接 SQL）
- フロントエンド: SPA モード、SSR なし、React フレームワーク不使用

## Constitution Check

*GATE: Must pass before Phase 0 research. Re-check after Phase 1 design.*

### I. Standards First (標準技術優先) ✅

- [x] FastAPI: Python Web フレームワークの標準的選択
- [x] Psycopg3: PostgreSQL の公式推奨ドライバ
- [x] React + Vite: フロントエンド標準構成
- [x] Ruff: Python の標準的フォーマッタ/リンター
- [x] uv: 高速な Python パッケージマネージャ（標準的選択）

### II. Simplicity First (シンプルさ優先) ✅

- [x] ORM 不使用: 直接 SQL で最小限の抽象化
- [x] SSR 不使用: SPA で十分なユースケース
- [x] 2エンティティのみ: User, Task
- [x] 簡易認証: OAuth 等の複雑な認証は不要

### III. Pragmatic Learning (実用性重視) ✅

- [x] 練習用として適切なスコープ
- [x] 段階的に実装可能な User Story 分割
- [x] 実用的な技術選定

**Gate Status**: ✅ PASS - 全原則に準拠

## Project Structure

### Documentation (this feature)

```text
specs/001-task-manager-auth/
├── plan.md              # This file
├── research.md          # Phase 0 output
├── data-model.md        # Phase 1 output
├── quickstart.md        # Phase 1 output
├── contracts/           # Phase 1 output (OpenAPI spec)
└── tasks.md             # Phase 2 output (/speckit.tasks command)
```

### Source Code (repository root)

```text
backend/
├── src/
│   ├── main.py          # FastAPI application entry
│   ├── config.py        # Configuration (DB connection, etc.)
│   ├── db.py            # Database connection utilities
│   ├── auth.py          # Authentication logic
│   ├── models/
│   │   ├── user.py      # User data structures
│   │   └── task.py      # Task data structures
│   ├── routes/
│   │   ├── auth.py      # Auth endpoints
│   │   └── tasks.py     # Task endpoints
│   └── services/
│       ├── user_service.py
│       └── task_service.py
├── tests/
│   ├── test_auth.py
│   └── test_tasks.py
├── pyproject.toml
└── .python-version

frontend/
├── src/
│   ├── main.tsx         # React entry point
│   ├── App.tsx          # Root component
│   ├── api/
│   │   └── client.ts    # API client
│   ├── components/
│   │   ├── LoginForm.tsx
│   │   ├── RegisterForm.tsx
│   │   ├── TaskList.tsx
│   │   └── TaskItem.tsx
│   ├── pages/
│   │   ├── LoginPage.tsx
│   │   ├── RegisterPage.tsx
│   │   └── TasksPage.tsx
│   └── hooks/
│       └── useAuth.ts
├── tests/
├── package.json
├── vite.config.ts
└── tsconfig.json
```

**Structure Decision**: Web application 構成を採用。Backend と Frontend を分離し、REST API で通信する。

## Complexity Tracking

> **Fill ONLY if Constitution Check has violations that must be justified**

| Violation | Why Needed | Simpler Alternative Rejected Because |
|-----------|------------|-------------------------------------|
| N/A | - | - |

全原則に準拠しているため、複雑さの正当化は不要。
