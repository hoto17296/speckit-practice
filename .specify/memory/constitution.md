<!--
  ============================================================================
  SYNC IMPACT REPORT
  ============================================================================
  Version Change: N/A → 1.0.0 (Initial ratification)

  Added Principles:
  - I. Standards First (標準技術優先)
  - II. Simplicity First (シンプルさ優先)
  - III. Pragmatic Learning (実用性重視)

  Added Sections:
  - Core Principles (3 principles)
  - Development Guidelines
  - Quality Standards
  - Governance

  Removed Sections: N/A (initial creation)

  Templates Status:
  - .specify/templates/plan-template.md: ✅ Compatible (Constitution Check section exists)
  - .specify/templates/spec-template.md: ✅ Compatible (no constitution-specific references)
  - .specify/templates/tasks-template.md: ✅ Compatible (no constitution-specific references)

  Follow-up TODOs: None
  ============================================================================
-->

# SpecKit Practice Constitution

## Core Principles

### I. Standards First (標準技術優先)

プロジェクトでは標準的な技術・記法を優先して採用する。

- 言語やフレームワークの公式推奨パターンに従う
- 広く採用されているライブラリ・ツールを選択する
- カスタム実装より標準ライブラリを優先する
- 命名規則は言語のスタイルガイドに準拠する（例: Python は PEP 8）

**根拠**: 標準に従うことで、学習コストを下げ、他のプロジェクトへの知識移転を容易にする。

### II. Simplicity First (シンプルさ優先)

最小限の複雑さで要件を満たす設計・実装を行う。

- YAGNI (You Aren't Gonna Need It): 今必要な機能のみを実装する
- 過度な抽象化を避ける: 具体的な問題が発生するまで抽象化しない
- 単一責任: 各モジュール・関数は一つのことを適切に行う
- 依存関係を最小化: 必要最小限の外部依存に留める

**根拠**: シンプルなコードは理解しやすく、保守しやすく、バグが少ない。

### III. Pragmatic Learning (実用性重視)

学習プロジェクトとして実用的なアプローチを取る。

- 動くコードを優先: 完璧を目指すより、まず動作するものを作る
- 段階的改善: 小さな改善を積み重ねる
- フィードバックループ: 早期に動作確認し、問題を早く発見する
- ドキュメントは必要十分: 過剰なドキュメントより、コードの可読性を重視する

**根拠**: 練習では完璧さより、実践を通じた学習を優先する。

## Development Guidelines

開発時に遵守すべきガイドライン。

- **コードスタイル**: 言語標準のフォーマッタ/リンターを使用する
- **バージョン管理**: 意味のある単位でコミットし、明確なコミットメッセージを書く
- **テスト**: 重要なロジックにはテストを書くが、過度なカバレッジを追求しない
- **エラーハンドリング**: 予期されるエラーは適切に処理し、ユーザーに有用なメッセージを提供する

## Quality Standards

品質基準。

- コードは他の開発者が読んで理解できる状態を維持する
- 主要な機能は手動またはテストで動作確認される
- セキュリティの基本（入力検証、機密情報の管理）は遵守する
- パフォーマンスは問題が発生してから最適化する（早すぎる最適化を避ける）

## Governance

この constitution はプロジェクトの指針として機能する。

- **適用範囲**: すべての設計・実装判断においてこの原則を参照する
- **改訂**: 必要に応じて原則を追加・変更できる。変更時はバージョンを更新する
- **優先順位**: 原則間で矛盾が生じた場合、シンプルさを優先する
- **例外**: 正当な理由がある場合、原則からの逸脱を許容する。ただし理由を明記する

**Version**: 1.0.0 | **Ratified**: 2026-01-15 | **Last Amended**: 2026-01-15
