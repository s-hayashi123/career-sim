# Project Catalog

> 10案件の詳細カタログ。各プロジェクトの目的・前提条件・成果物・使用技術を記載する。

---

## 概要

| Project | テーマ | Phase | 期間目安 | 対象リポジトリ |
|---------|--------|-------|----------|---------------|
| 01 | Onboarding（既存API途中参加） | Python基礎 | ~2週 | `project-atlas-api` |
| 02 | Bugfix（バグ修正・再発防止） | Python基礎 | ~2週 | `project-atlas-api` |
| 03 | Feature（API + DB機能追加） | Python応用 | ~2週 | `project-atlas-api` |
| 04 | Refactor（保守性改善） | Python応用 | ~2週 | `project-atlas-api` |
| 05 | Performance（性能改善） | Python応用 | ~2週 | `project-atlas-api` |
| 06 | AWS Design（VPC/IAM/構成図） | AWS設計 | ~2週 | `project-atlas-infra` |
| 07 | AWS Build（ECS/RDS/ALB） | AWS構築 | ~2週 | `project-atlas-infra` |
| 08 | AWS Ops/Incident（障害対応） | AWS運用 | ~2週 | `project-atlas-infra` + `project-atlas-api` |
| 09 | CI/CD・運用改善 | 自動化 | ~2週 | `project-atlas-api` + `project-atlas-infra` |
| 10 | End-to-End Delivery（+Next.js管理画面） | 総合 | ~2-3週 | 全リポジトリ + `project-atlas-admin` |

---

## PROJECT 01: Onboarding（既存Python API途中参加）

### 背景・設定

チームが途中まで開発してきた Project Atlas API に、新メンバーとして参加する。
コードベースにはREADMEの不備、テスト不足、print文でのログなど、よくある課題がある。

### 前提条件

- `project-atlas-api` リポに「既存コード」がコミット済み
  - FastAPI + SQLAlchemy + Alembic の基本構成
  - `users` CRUD、`teams` CRUD（最低限の実装）
  - テスト少数、エラーハンドリング不十分、print文でのログ

### ゴール

- ローカル環境を確実に構築できる状態にする
- 基本的な品質改善（テストカバレッジ、バリデーション、ログ）を行う
- プロジェクト構造の改善方針を ADR として残す

### 主要スキルタグ

`P3`, `P4`, `P5`, `P8`

### 成果物

| 種類 | 内容 |
|------|------|
| ADR | ADR-001: プロジェクト構造方針 |
| Runbook | `local-setup.md` |
| Issue | 10件 |
| PR | 8-9件 |
| Retro | Project 01 レトロスペクティブ |
| Scorecard | Project 01 スコアカード |

### 使用技術

- Python / FastAPI / SQLAlchemy / Alembic
- pytest / pytest-cov
- Docker / Docker Compose
- Pydantic（バリデーション）
- Python logging（構造化ログ）

---

## PROJECT 02: Bugfix（バグ修正・再発防止）

### 背景・設定

QAチーム・フロントエンドチームから次々とバグ報告が上がってくる。
各バグに対して再現→原因特定→修正→回帰テスト→再発防止策を一貫して対応する。

### 前提条件

- Project 01 完了済み（環境構築・基本的な品質改善済み）
- `users` / `teams` CRUD が稼働中

### ゴール

- バグ修正のワークフロー（報告→再現→修正→テスト）を確立する
- グローバル例外ハンドラで安全なエラーレスポンスを保証する
- 論理削除方針を設計判断として記録する
- テストを unit / integration に整理する

### 主要スキルタグ

`P2`, `P3`, `P4`, `P5`

### 成果物

| 種類 | 内容 |
|------|------|
| ADR | ADR-002: グローバル例外処理方針 |
| ADR | ADR-003: 論理削除方針 |
| Issue | 10件 |
| PR | 8-9件 |
| Retro | Project 02 レトロスペクティブ |
| Scorecard | Project 02 スコアカード |

### 使用技術

- Python / FastAPI / SQLAlchemy / Alembic
- pytest（unit / integration）
- Pydantic（extra="forbid"、バリデーション強化）

---

## PROJECT 03: Feature（API + DB機能追加）

### 背景・設定

PMから「プロジェクト管理」「タスク管理」機能の要望が来る。
テーブル設計からAPI実装、フィルタリング、集計、変更履歴まで一貫して開発する。

### 前提条件

- Project 02 完了済み（バグ修正・例外処理・テスト整備済み）
- `users` / `teams` CRUD が安定稼働

### ゴール

- `projects` / `tasks` / `task_activities` テーブルを設計・実装する
- リレーション（Project → Task → Activity）を正しく設計する
- フィルタリング・ページネーション・集計APIを実装する
- 状態遷移バリデーションを設計判断として記録する

### 主要スキルタグ

`P1`, `P2`, `P3`

### 成果物

| 種類 | 内容 |
|------|------|
| ADR | ADR-004: データモデル設計 |
| ADR | ADR-005: タスクステータス遷移ルール |
| Issue | 10件 |
| PR | 8-9件 |
| Retro | Project 03 レトロスペクティブ |
| Scorecard | Project 03 スコアカード |

### 使用技術

- Python / FastAPI / SQLAlchemy / Alembic
- pytest
- SQL（GROUP BY、集計クエリ）
- Pydantic（リクエスト/レスポンスモデル）

---

## PROJECT 04: Refactor（保守性改善）

### 背景・設定

機能追加が続きルートハンドラが肥大化。TLから「サービス層を分離したい」と指示が出る。
コードベース全体の保守性・テスタビリティを改善する。

### 前提条件

- Project 03 完了済み（projects / tasks / activities の CRUD が実装済み）
- ルートハンドラにビジネスロジックが集中している状態

### ゴール

- サービス層を導入し、ルートハンドラを薄くする
- 共通バリデータ・統一エラーレスポンスを導入する
- 型ヒント + mypy でコード品質を向上する
- DI パターン（FastAPI Depends()）を導入する

### 主要スキルタグ

`P3`, `P4`, `P6`

### 成果物

| 種類 | 内容 |
|------|------|
| ADR | ADR-006: サービス層分離方針 |
| ADR | ADR-007: 統一エラーレスポンス形式 |
| Issue | 10件 |
| PR | 8-9件 |
| Retro | Project 04 レトロスペクティブ |
| Scorecard | Project 04 スコアカード |

### 使用技術

- Python / FastAPI / SQLAlchemy
- pytest
- mypy
- Pydantic（カスタム型、統一モデル）

---

## PROJECT 05: Performance（性能改善）

### 背景・設定

データ量が増え、タスク一覧やサマリーAPIで性能問題が顕在化。
プロファイリング→ボトルネック特定→改善→Before/After比較のサイクルを回す。

### 前提条件

- Project 04 完了済み（サービス層分離・コード品質改善済み）
- 大量データでの性能問題が発生しやすい状態

### ゴール

- N+1問題を特定・解消する
- SQL集計の最適化（Python集計 → SQL GROUP BY）を行う
- インデックス追加の効果を EXPLAIN ANALYZE で実証する
- ページネーション・キャッシュ・データ保持ポリシーを導入する
- Before/After のパフォーマンスレポートを作成する

### 主要スキルタグ

`P2`, `P7`

### 成果物

| 種類 | 内容 |
|------|------|
| ADR | ADR-008: Eager Loading 戦略 |
| ADR | ADR-009: ページネーション戦略 |
| ADR | ADR-010: データ保持ポリシー |
| Docs | `performance-report.md`（Before/After一覧） |
| Issue | 10件 |
| PR | 8-9件 |
| Retro | Project 05 レトロスペクティブ |
| Scorecard | Project 05 スコアカード |

### 使用技術

- Python / FastAPI / SQLAlchemy
- SQL（EXPLAIN ANALYZE、インデックス、GROUP BY）
- pytest
- キャッシュ（インメモリ）

---

## PROJECT 06: AWS Design（設計フェーズ）

### 背景・設定

CTOから「Project Atlas API を本番環境にデプロイするためのAWS構成を設計して」と依頼。
VPC / IAM / RDS / ECS / ALB の設計を行い、構成図・コスト見積を作成する。

### 前提条件

- Project 05 完了済み（APIが本番デプロイ可能な品質に到達）
- AWS CLF / SAA 取得済み（理論的知識あり）

### ゴール

- 本番AWS構成の設計ドキュメントを作成する
- VPC / IAM / RDS / ECS / ALB 各コンポーネントの設計を記録する
- Terraformモジュール構成案を策定する
- 月額コスト見積を作成する

### 主要スキルタグ

`A1`, `A4`, `A8`

### 成果物

| 種類 | 内容 |
|------|------|
| リポジトリ | `project-atlas-infra` 新規作成 |
| ADR | ADR-011: AWS構成概要 |
| ADR | ADR-012: 最小権限IAM戦略 |
| ADR | ADR-013: Secrets Manager vs Parameter Store |
| ADR | ADR-014: RDS構成 |
| Docs | `vpc-design.md`, `iam-strategy.md`, `rds-design.md`, `ecs-design.md`, `alb-design.md`, `cost-estimate.md` |
| Docs | Mermaid構成図 |
| Issue | 10件 |
| PR | 8-9件 |
| Retro | Project 06 レトロスペクティブ |
| Scorecard | Project 06 スコアカード |

### 使用技術

- AWS（VPC / Subnet / IGW / NAT / ALB / ECS / Fargate / RDS / IAM / Secrets Manager）
- Terraform（モジュール設計）
- Mermaid（構成図）

---

## PROJECT 07: AWS Build（構築フェーズ）

### 背景・設定

Project 06 の設計に基づき、Terraform で実際にAWSインフラを構築する。
デプロイ中に発生するSG設定ミスなどのトラブルも含め、構築プロセス全体を記録する。

### 前提条件

- Project 06 完了済み（設計ドキュメント・構成図・モジュール計画が策定済み）
- `project-atlas-infra` リポジトリが存在

### ゴール

- Terraform で VPC / RDS / ECS / ALB を構築する
- アプリケーションをデプロイし動作検証する
- 構築中のトラブル（SG設定ミスなど）のデバッグ過程を記録する
- dev / prod 環境を分離する
- デプロイ手順書（Runbook）を作成する

### 主要スキルタグ

`A1`, `A2`, `A3`, `A4`

### 成果物

| 種類 | 内容 |
|------|------|
| ADR | ADR-015: マイグレーション実行戦略 |
| Runbook | `deploy.md` |
| Issue | 10件 |
| PR | 8-9件 |
| Retro | Project 07 レトロスペクティブ |
| Scorecard | Project 07 スコアカード |

### 使用技術

- Terraform（VPC / RDS / ECS / ALB / IAM / Secrets Manager モジュール）
- AWS CLI
- Docker / ECR
- Alembic（本番マイグレーション）

---

## PROJECT 08: AWS Ops/Incident（運用・障害対応）

### 背景・設定

本番環境が稼働を開始。監視がない状態から監視・アラートを構築し、
模擬的な障害（ECSタスク停止、RDSコネクション枯渇、ディスク肥大化）に対応する。

### 前提条件

- Project 07 完了済み（AWS環境が稼働中）
- 監視・アラートが未設定の状態

### ゴール

- CloudWatch ダッシュボード・アラーム・SNS通知を構築する
- 3件の模擬障害に対して Postmortem を作成する
- ロールバック手順書を作成・テストする
- セキュリティ改善（SG制限、HTTPS強制）を実施する
- 本番 Readiness チェックリストを作成する

### 主要スキルタグ

`A5`, `A7`, `A8`

### 成果物

| 種類 | 内容 |
|------|------|
| ADR | ADR-016: セキュリティ設定方針 |
| Postmortem | Postmortem-001: ECSタスク停止 |
| Postmortem | Postmortem-002: RDSコネクションプール枯渇 |
| Postmortem | Postmortem-003: RDSディスク肥大化 |
| Runbook | `ecs-task-recovery.md` |
| Runbook | `rollback.md` |
| Docs | `production-readiness-checklist.md` |
| Issue | 10件 |
| PR | 7-8件 |
| Retro | Project 08 レトロスペクティブ |
| Scorecard | Project 08 スコアカード |

### 使用技術

- AWS CloudWatch（Logs / Metrics / Alarms / Dashboards）
- AWS SNS
- Terraform
- JSON構造化ログ

---

## PROJECT 09: CI/CD・運用改善

### 背景・設定

手動デプロイからの脱却。GitHub Actions でテスト・ビルド・デプロイのパイプラインを構築し、
staging環境の追加・脆弱性スキャン・リリースノート自動生成まで運用品質を向上させる。

### 前提条件

- Project 08 完了済み（監視・アラート・障害対応体制が構築済み）
- 手動デプロイの状態

### ゴール

- GitHub Actions で CI パイプライン（test → lint → mypy）を構築する
- CD パイプライン（build → ECR push → ECS deploy）を構築する
- Terraform plan の PR コメント表示を実装する
- staging 環境を追加する
- 脆弱性スキャン（pip-audit / Trivy）を導入する

### 主要スキルタグ

`A6`, `A8`

### 成果物

| 種類 | 内容 |
|------|------|
| ADR | ADR-017: デプロイ戦略 |
| ADR | ADR-018: staging環境方針 |
| Runbook | `ci-cd-troubleshooting.md` |
| Issue | 10件 |
| PR | 8-9件 |
| Retro | Project 09 レトロスペクティブ |
| Scorecard | Project 09 スコアカード |

### 使用技術

- GitHub Actions
- Docker / ECR
- Terraform
- pre-commit
- pip-audit / Trivy / Dependabot
- セマンティックバージョニング

---

## PROJECT 10: End-to-End Delivery（+Next.js管理画面）

### 背景・設定

PMから「チームダッシュボード機能」の要望。API設計からNext.js管理画面の実装、
認証追加、インフラデプロイ、CI/CD構築、性能問題の対応まで一貫して対応する。

### 前提条件

- Project 01-09 全て完了済み
- API / インフラ / CI/CD が全て稼働中

### ゴール

- ダッシュボード集計APIを設計・実装する
- Next.js 管理画面（`project-atlas-admin`）を構築する
- 認証（JWT or APIキー）を追加する
- 管理画面をAWSにデプロイする
- パフォーマンス問題を調査・修正する
- 全プログラムの最終ドキュメント・職務経歴書サマリーを作成する

### 主要スキルタグ

`P1`-`P8`, `A2`-`A8`（総合）

### 成果物

| 種類 | 内容 |
|------|------|
| リポジトリ | `project-atlas-admin` 新規作成 |
| ADR | ADR-019: 認証方式 |
| Postmortem | Postmortem-004: ダッシュボード追加後の性能劣化 |
| Docs | APIドキュメント + 管理画面ガイド + 構成図更新 |
| Docs | 最終 evidence-index + 職務経歴書サマリー |
| Issue | 10件 |
| PR | 8-9件 |
| Retro | Project 10 最終レトロスペクティブ |
| Scorecard | Project 10 最終スコアカード |

### 使用技術

- Python / FastAPI / SQLAlchemy（集計API）
- Next.js / React（管理画面）
- JWT or APIキー認証
- Docker / AWS ECS / ALB
- GitHub Actions
- Terraform

---

## 更新ログ

- 2026-02-21: 初版作成（10案件の詳細カタログ）
