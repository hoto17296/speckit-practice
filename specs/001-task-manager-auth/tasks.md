# Tasks: タスク管理アプリ（ユーザ認証付き）

**Input**: Design documents from `/specs/001-task-manager-auth/`
**Prerequisites**: plan.md, spec.md, data-model.md, contracts/openapi.yaml

**Tests**: テストは明示的に要求されていないため、オプションとして省略。必要に応じて追加可能。

**Organization**: Tasks are grouped by user story to enable independent implementation and testing of each story.

## Format: `[ID] [P?] [Story] Description`

- **[P]**: Can run in parallel (different files, no dependencies)
- **[Story]**: Which user story this task belongs to (e.g., US1, US2, US3)
- Include exact file paths in descriptions

## Path Conventions

- **Web app**: `backend/src/`, `frontend/src/`

---

## Phase 1: Setup (Shared Infrastructure)

**Purpose**: Project initialization and basic structure

- [ ] T001 Create backend project structure with `backend/src/` directories per plan.md
- [ ] T002 Initialize Python project with uv and pyproject.toml in `backend/`
- [ ] T003 [P] Configure Ruff for linting and formatting in `backend/pyproject.toml`
- [ ] T004 [P] Create frontend project with Vite + React + TypeScript in `frontend/`
- [ ] T005 [P] Configure TypeScript and ESLint in `frontend/tsconfig.json` and `frontend/eslint.config.js`

---

## Phase 2: Foundational (Blocking Prerequisites)

**Purpose**: Core infrastructure that MUST be complete before ANY user story can be implemented

**⚠️ CRITICAL**: No user story work can begin until this phase is complete

- [ ] T006 Create database schema (users, tasks tables) using SQL in `backend/src/schema.sql`
- [ ] T007 Implement database connection pool in `backend/src/db.py` using Psycopg3
- [ ] T008 [P] Create configuration management in `backend/src/config.py` (DATABASE_URL, JWT settings)
- [ ] T009 [P] Setup FastAPI application entry point in `backend/src/main.py` with CORS middleware
- [ ] T010 Implement JWT authentication utilities in `backend/src/auth.py` (create_token, verify_token)
- [ ] T011 [P] Create API client foundation in `frontend/src/api/client.ts` with auth header handling
- [ ] T012 [P] Setup React Router in `frontend/src/App.tsx` with route definitions

**Checkpoint**: Foundation ready - user story implementation can now begin

---

## Phase 3: User Story 1 - ユーザー登録とログイン (Priority: P1) 🎯 MVP

**Goal**: ユーザーは新規登録し、ログイン/ログアウトができる

**Independent Test**: アカウント作成→ログイン→ログアウトの一連のフローが動作することを確認

### Backend Implementation for US1

- [ ] T013 [P] [US1] Create User Pydantic model in `backend/src/models/user.py`
- [ ] T014 [US1] Implement UserService in `backend/src/services/user_service.py` (create_user, get_by_username, verify_password)
- [ ] T015 [US1] Implement POST /auth/register endpoint in `backend/src/routes/auth.py`
- [ ] T016 [US1] Implement POST /auth/login endpoint in `backend/src/routes/auth.py`
- [ ] T017 [US1] Implement POST /auth/logout endpoint in `backend/src/routes/auth.py`
- [ ] T018 [US1] Add authentication dependency (get_current_user) in `backend/src/auth.py`

### Frontend Implementation for US1

- [ ] T019 [P] [US1] Create useAuth hook in `frontend/src/hooks/useAuth.ts` (login, logout, register, isAuthenticated)
- [ ] T020 [P] [US1] Create LoginForm component in `frontend/src/components/LoginForm.tsx`
- [ ] T021 [P] [US1] Create RegisterForm component in `frontend/src/components/RegisterForm.tsx`
- [ ] T022 [US1] Create LoginPage in `frontend/src/pages/LoginPage.tsx`
- [ ] T023 [US1] Create RegisterPage in `frontend/src/pages/RegisterPage.tsx`
- [ ] T024 [US1] Add protected route logic in `frontend/src/App.tsx` (redirect to login if not authenticated)

**Checkpoint**: User Story 1 完了 - 登録・ログイン・ログアウトが独立して動作

---

## Phase 4: User Story 2 - タスクの作成と一覧表示 (Priority: P2)

**Goal**: ログインユーザーがタスクを作成し、一覧表示できる

**Independent Test**: ログイン後、タスク作成→一覧に表示されることを確認

### Backend Implementation for US2

- [ ] T025 [P] [US2] Create Task Pydantic model in `backend/src/models/task.py`
- [ ] T026 [US2] Implement TaskService in `backend/src/services/task_service.py` (create_task, get_tasks_by_user)
- [ ] T027 [US2] Implement GET /tasks endpoint in `backend/src/routes/tasks.py`
- [ ] T028 [US2] Implement POST /tasks endpoint in `backend/src/routes/tasks.py`

### Frontend Implementation for US2

- [ ] T029 [P] [US2] Create TaskList component in `frontend/src/components/TaskList.tsx`
- [ ] T030 [P] [US2] Create TaskItem component in `frontend/src/components/TaskItem.tsx`
- [ ] T031 [US2] Create TasksPage in `frontend/src/pages/TasksPage.tsx` with task creation form
- [ ] T032 [US2] Add task API functions to `frontend/src/api/client.ts` (getTasks, createTask)

**Checkpoint**: User Story 2 完了 - タスク作成・一覧表示が独立して動作

---

## Phase 5: User Story 3 - タスクの完了状態の切り替え (Priority: P3)

**Goal**: ユーザーがタスクの完了/未完了を切り替えできる

**Independent Test**: タスクの完了状態をトグルし、状態が保持されることを確認

### Backend Implementation for US3

- [ ] T033 [US3] Add update_task method to TaskService in `backend/src/services/task_service.py`
- [ ] T034 [US3] Implement PATCH /tasks/{taskId} endpoint in `backend/src/routes/tasks.py`

### Frontend Implementation for US3

- [ ] T035 [US3] Add toggle completion handler to TaskItem in `frontend/src/components/TaskItem.tsx`
- [ ] T036 [US3] Add updateTask API function to `frontend/src/api/client.ts`

**Checkpoint**: User Story 3 完了 - 全機能が独立して動作

---

## Phase 6: Polish & Cross-Cutting Concerns

**Purpose**: Improvements that affect multiple user stories

- [ ] T037 [P] Add error handling and user-friendly messages across frontend components
- [ ] T038 [P] Add loading states to frontend components
- [ ] T039 Run quickstart.md validation (manual verification)
- [ ] T040 Code cleanup and final review

---

## Dependencies & Execution Order

### Phase Dependencies

- **Setup (Phase 1)**: No dependencies - can start immediately
- **Foundational (Phase 2)**: Depends on Setup completion - BLOCKS all user stories
- **User Stories (Phase 3-5)**: All depend on Foundational phase completion
  - US2 requires US1 for authentication context
  - US3 requires US2 for Task model and list display
- **Polish (Phase 6)**: Depends on all user stories being complete

### User Story Dependencies

- **User Story 1 (P1)**: Can start after Foundational (Phase 2) - No dependencies on other stories
- **User Story 2 (P2)**: Requires US1 authentication to be complete (uses auth context)
- **User Story 3 (P3)**: Requires US2 task model and list to be complete

### Within Each User Story

- Backend models before services
- Backend services before routes
- Frontend hooks/API before components
- Components before pages

### Parallel Opportunities

- T003, T004, T005 can run in parallel (Setup phase)
- T008, T009, T011, T012 can run in parallel (Foundational phase)
- T013, T019, T020, T021 can run in parallel (US1)
- T025, T029, T030 can run in parallel (US2)
- T037, T038 can run in parallel (Polish phase)

---

## Parallel Example: User Story 1

```bash
# Backend models (parallel):
Task: "Create User Pydantic model in backend/src/models/user.py"

# Frontend components (parallel after API client ready):
Task: "Create useAuth hook in frontend/src/hooks/useAuth.ts"
Task: "Create LoginForm component in frontend/src/components/LoginForm.tsx"
Task: "Create RegisterForm component in frontend/src/components/RegisterForm.tsx"
```

---

## Implementation Strategy

### MVP First (User Story 1 Only)

1. Complete Phase 1: Setup
2. Complete Phase 2: Foundational (CRITICAL - blocks all stories)
3. Complete Phase 3: User Story 1
4. **STOP and VALIDATE**: Test registration/login/logout independently
5. Deploy/demo if ready

### Incremental Delivery

1. Complete Setup + Foundational → Foundation ready
2. Add User Story 1 → Test independently → Deploy/Demo (MVP!)
3. Add User Story 2 → Test independently → Deploy/Demo
4. Add User Story 3 → Test independently → Deploy/Demo
5. Polish phase → Final release

---

## Notes

- [P] tasks = different files, no dependencies
- [Story] label maps task to specific user story for traceability
- Each user story should be independently completable and testable
- Commit after each task or logical group
- Stop at any checkpoint to validate story independently
- Backend port: 8000, Frontend port: 5173
