# バックエンド100本ノック ロードマップ

> チーム・プロジェクト管理プラットフォーム **Project Atlas** を題材に、
> 実務想定の100イベントを10案件に分けて体系的に実施する。

---

## アプリケーションドメイン: Project Atlas

**チーム・プロジェクト管理プラットフォームAPI** を題材にする。

### 選定理由

- CRUD + 複雑なリレーション（チーム → メンバー、プロジェクト → タスク → 担当者）
- ロールベースアクセス制御（admin / manager / member）
- 検索・フィルタリング（パフォーマンス課題の自然な発生）
- 集計・レポート（SQLレベルの最適化が必要になる）
- 管理画面（Next.js を軽めに使う口実）

---

## プロジェクト順序と想定期間

4-6時間/日で **約16-20週（4-5ヶ月）** を想定。

| Phase | Project | テーマ | 期間目安 |
|-------|---------|--------|----------|
| Python基礎 | 01 | Onboarding（既存API途中参加） | ~2週 |
| Python基礎 | 02 | Bugfix（バグ修正・再発防止） | ~2週 |
| Python応用 | 03 | Feature（API + DB機能追加） | ~2週 |
| Python応用 | 04 | Refactor（保守性改善） | ~2週 |
| Python応用 | 05 | Performance（性能改善） | ~2週 |
| AWS設計 | 06 | AWS Design（VPC/IAM/構成図） | ~2週 |
| AWS構築 | 07 | AWS Build（ECS/RDS/ALB） | ~2週 |
| AWS運用 | 08 | AWS Ops/Incident（障害対応） | ~2週 |
| 自動化 | 09 | CI/CD・運用改善 | ~2週 |
| 総合 | 10 | End-to-End Delivery（+Next.js管理画面） | ~2-3週 |

---

## 実務らしさを出すためのルール

1. **コミットメッセージ**: Conventional Commits (`fix:`, `feat:`, `refactor:`, `docs:`, `perf:`) + Issue番号参照
2. **ブランチ名**: `feature/proj03-task-crud`, `fix/proj02-duplicate-user`, `refactor/proj04-service-layer`
3. **PR説明**: Background / Changes / Testing / Considerations セクション構成
4. **セルフレビュー**: PR内で自分のコードにコメント（「X案も検討したがY案を採用。理由は...」）
5. **コミット間隔**: 1日に全ノックをやらない。自然なペースで作業する
6. **Blocked状態**: 時々IssueをBlocked → Resolved の流れで管理
7. **Follow-up Issue**: 完了タスクから後続課題を起票

---

## 成果物サマリー

| 種類 | 数 |
|------|-----|
| ADR（設計判断記録） | 19本 |
| Postmortem（障害対応記録） | 4本 |
| Runbook（運用手順書） | 6本以上 |
| Issue | 100件 |
| PR | 90件以上 |
| レトロスペクティブ | 10本 |
| スコアカード | 10本 |

---

## 前提作業: 初期コードベースの準備

Project 01開始前に `project-atlas-api` リポに「既存コード」としてコミットする:

- FastAPI + SQLAlchemy + Alembic の基本構成
- `users` CRUD、`teams` CRUD（最低限の実装）
- テスト少数、エラーハンドリング不十分、print文でのログ

これが「途中参加するチームのコードベース」になる。

---

## 100本ノック詳細

---

### PROJECT 01: Onboarding（既存Python API途中参加）

> **テーマ**: 既存チームのコードベースに途中参加し、環境構築・品質改善・ドキュメント整備を行う
> **主要スキル**: P4, P5, P8
> **期間目安**: ~2週

| # | シナリオ | 成果物 | スキル | 時間 |
|---|---------|--------|--------|------|
| 01 | TLから「ローカル環境セットアップしてREADMEの不備を直して」 | Issue + PR(README改善) + Runbook(`local-setup.md`) | P8 | 3h |
| 02 | TLから「テストカバレッジ確認して。多分ひどい」 | Issue + PR(`pytest-cov`導入、カバレッジレポート) | P3, P8 | 3h |
| 03 | QAから「GET /users、DB空だと500になる」 | Issue(再現手順付き) + PR(修正+テスト) | P5, P3, P4 | 4h |
| 04 | 自分で気付く「.env.exampleもDocker Composeもない」 | Issue + PR(.env.example、docker-compose.yml、README更新) | P8 | 4h |
| 05 | TLから「OpenAPIドキュメントちゃんと整備して」 | Issue + PR(Pydanticスキーマ説明、レスポンスモデル) | P1, P8 | 4h |
| 06 | 自分で気付く「ルート・モデル・ロジック全部1ファイル」 | Issue(観察) + ADR-001: プロジェクト構造方針 | P6, P8 | 3h |
| 07 | TLから「ヘルスチェックにDB接続確認も入れて」 | Issue + PR(`/health`改善+テスト) | P1, P3, P4 | 4h |
| 08 | QAから「POST /users、名前空文字で通る」 | Issue + PR(Pydanticバリデーション+テスト) | P4, P3, P5 | 4h |
| 09 | 自分で提案「print文→構造化ログに変えたい」 | Issue + PR(logging導入、リクエストID付与) | P4, P8 | 5h |
| 10 | スプリントレトロ | Retro + Scorecard + evidence-index更新 | P8 | 3h |

**主な成果物**: ADR-001、Runbook: `local-setup.md`

---

### PROJECT 02: Bugfix（バグ修正・再発防止）

> **テーマ**: 本番・QA環境で報告されるバグを修正し、再発防止策を講じる
> **主要スキル**: P2, P3, P4, P5
> **期間目安**: ~2週

| # | シナリオ | 成果物 | スキル | 時間 |
|---|---------|--------|--------|------|
| 01 | QAから「同時にユーザー作ると重複する」 | Issue + PR(UNIQUE制約+IntegrityError処理+テスト) | P5, P2, P3 | 6h |
| 02 | フロントから「GET /users/999で500。404期待」 | Issue + PR(404ハンドリング+テスト) | P5, P4, P3 | 4h |
| 03 | QAから「PUT /usersで既存メールに変更できちゃう」 | Issue + PR(更新時ユニーク検証+テスト) | P5, P3, P2 | 4h |
| 04 | モニタリングから「未ハンドル例外でスタックトレース漏洩」 | Issue + PR(グローバル例外ハンドラ) + ADR-002 | P4, P5, P8 | 6h |
| 05 | QAから「PATCH、nameだけ送るとemailがnullになる」 | Issue + PR(部分更新ロジック修正+テスト) | P5, P1, P3 | 5h |
| 06 | TLから「修正済みバグの回帰テストまとめて」 | Issue + PR(テスト整理: unit/integration分離) | P3, P8 | 6h |
| 07 | フロントから「未知フィールド送ると無視される。拒否すべき?」 | Issue + PR(extra="forbid"設定+テスト) + ADR | P4, P1, P3 | 4h |
| 08 | QAから「チームメンバーのユーザー削除で500」 | Issue + PR(外部キー依存処理) + ADR-003: 論理削除方針 | P5, P2, P3 | 6h |
| 09 | 自分で発見「Alembicマイグレーション2回実行で失敗する」 | Issue + PR(マイグレーション修正+検証手順) | P2, P5, P8 | 5h |
| 10 | スプリントレトロ | Retro + Scorecard + evidence-index更新 | P8 | 3h |

**主な成果物**: ADR-002, ADR-003

---

### PROJECT 03: Feature（API + DB機能追加）

> **テーマ**: PM要望に基づき、プロジェクト管理・タスク管理機能をゼロから設計・実装する
> **主要スキル**: P1, P2, P3
> **期間目安**: ~2週

| # | シナリオ | 成果物 | スキル | 時間 |
|---|---------|--------|--------|------|
| 01 | PMから「プロジェクト管理機能が欲しい」 | Issue(受入条件付き) + ADR-004: データモデル設計 | P1, P2, P8 | 5h |
| 02 | 実装「Projectモデル・マイグレーション・CRUD」 | Issue + PR | P1, P2 | 8h |
| 03 | PMから「タスク機能。プロジェクトに紐づく」 | Issue + PR(tasks テーブル・モデル・CRUD・リレーション) | P1, P2, P3 | 8h |
| 04 | PMから「タスクをステータス・担当者・優先度でフィルタ」 | Issue + PR(クエリパラメータ+ページネーション+テスト) | P1, P2, P3 | 6h |
| 05 | PMから「プロジェクトサマリー(タスク数集計)のAPI」 | Issue + PR(SQL集計エンドポイント+テスト) | P1, P2, P3 | 4h |
| 06 | QAから「存在しないproject_idでタスク作成→500」 | Issue + PR(外部キー検証+エラーメッセージ+テスト) | P4, P5, P3 | 4h |
| 07 | TLレビューFBから「ステータス遷移が自由すぎる」 | Issue + PR(状態遷移バリデーション) + ADR-005 | P1, P4, P3 | 6h |
| 08 | PMから「タスクの変更履歴を記録したい」 | Issue + PR(task_activities テーブル+自動記録+取得API) | P1, P2 | 6h |
| 09 | TLから「全フロー通しの統合テスト書いて」 | Issue + PR(E2Eインテグレーションテスト) | P3, P8 | 6h |
| 10 | スプリントレトロ + APIドキュメント更新 | Retro + Scorecard + OpenAPI更新PR | P8, P1 | 4h |

**主な成果物**: ADR-004, ADR-005

---

### PROJECT 04: Refactor（保守性改善）

> **テーマ**: 肥大化したコードベースにサービス層を導入し、保守性・テスタビリティを改善する
> **主要スキル**: P3, P4, P6
> **期間目安**: ~2週

| # | シナリオ | 成果物 | スキル | 時間 |
|---|---------|--------|--------|------|
| 01 | TLから「ルートハンドラが肥大化。サービス層を分離したい」 | Issue + ADR-006: サービス層分離方針 | P6, P8 | 4h |
| 02 | 実装「userのサービス層抽出」 | Issue + PR(ルートは薄く、ロジックはサービスへ。全テスト通過) | P6, P3 | 6h |
| 03 | 実装「project/taskのサービス層抽出」 | Issue + PR | P6, P3 | 6h |
| 04 | 自分で気付く「同じバリデーション3箇所にある」 | Issue + PR(共通バリデータ・カスタムPydantic型) | P6, P4 | 4h |
| 05 | TLから「エラーレスポンス形式がバラバラ」 | Issue + PR(統一レスポンスモデル) + ADR-007 | P4, P6, P8 | 5h |
| 06 | 自分で気付く「モデルのid/created_at/updated_atが重複」 | Issue + PR(BaseModelミックスイン+マイグレーション) | P6, P2 | 5h |
| 07 | TLから「型ヒント入れてmypy通して」 | Issue + PR(型ヒント追加+mypy設定) | P6, P8 | 6h |
| 08 | 自分で提案「DBセッションをDI化したい」 | Issue + PR(FastAPI Depends()パターン) | P6, P1 | 5h |
| 09 | TLから「ドキュメント古い。アーキテクチャ更新して」 | Issue + PR(README更新+アーキテクチャ図) | P8 | 4h |
| 10 | スプリントレトロ | Retro + Scorecard + evidence-index更新 | P8 | 3h |

**主な成果物**: ADR-006, ADR-007

---

### PROJECT 05: Performance（性能改善）

> **テーマ**: 大量データでのボトルネックを特定し、SQL・キャッシュ・インデックスで性能を改善する
> **主要スキル**: P2, P7
> **期間目安**: ~2週

| # | シナリオ | 成果物 | スキル | 時間 |
|---|---------|--------|--------|------|
| 01 | PMから「タスク一覧が遅い。1000件で3秒以上」 | Issue + PR(シードスクリプト+タイミングミドルウェア+ベースライン測定) | P7, P8 | 5h |
| 02 | 調査「何が遅い?プロファイリング」 | Issue + PR(SQLAlchemyクエリログ分析、N+1問題の特定) | P7, P5, P8 | 5h |
| 03 | 修正「N+1問題の解消」 | Issue + PR(joinedload/selectinload) + ADR-008 | P7, P2 | 5h |
| 04 | PMから「サマリーAPIも遅い」 | Issue + PR(Python集計→SQL GROUP BY化+比較) | P7, P2 | 4h |
| 05 | 調査「検索がフルスキャン。インデックス追加」 | Issue + PR(インデックス追加マイグレーション+EXPLAIN ANALYZE) | P7, P2 | 5h |
| 06 | TLから「ページネーションをDB側でやって」 | Issue + PR(LIMIT/OFFSET実装) + ADR-009: ページネーション戦略 | P7, P1, P8 | 6h |
| 07 | PMから「activity_logが増え続ける。対策を」 | ADR-010: データ保持ポリシー + PR(実装) | P7, P2, P8 | 6h |
| 08 | 自分で提案「サマリーAPIにキャッシュを入れたい」 | Issue + PR(インメモリキャッシュ+無効化ロジック) | P7, P1 | 5h |
| 09 | TLから「パフォーマンスレポートまとめて」 | Issue + PR(`performance-report.md`: Before/After一覧) | P7, P8 | 4h |
| 10 | スプリントレトロ | Retro + Scorecard + evidence-index更新 | P8 | 3h |

**主な成果物**: ADR-008, ADR-009, ADR-010、`performance-report.md`

---

### PROJECT 06: AWS Design（設計フェーズ）

> **テーマ**: 本番AWS環境の構成を設計し、VPC/IAM/RDS/ECS/ALBの設計ドキュメントを作成する
> **主要スキル**: A1, A4, A8
> **期間目安**: ~2週

| # | シナリオ | 成果物 | スキル | 時間 |
|---|---------|--------|--------|------|
| 01 | CTOから「本番AWS構成を設計して」 | `project-atlas-infra`リポ作成 + ADR-011: AWS構成概要 | A1, A8, P8 | 6h |
| 02 | 設計「VPCとネットワークレイアウト」 | PR(`vpc-design.md` + CIDR計画 + 構成図) | A1, P8 | 5h |
| 03 | 設計「IAM戦略」 | PR(`iam-strategy.md`) + ADR-012: 最小権限 | A4, P8 | 5h |
| 04 | 設計「シークレット管理方式」 | ADR-013: Secrets Manager vs Parameter Store | A4, A8, P8 | 4h |
| 05 | 設計「RDS構成」 | PR(`rds-design.md`) + ADR-014 | A3, A8, P8 | 5h |
| 06 | 設計「ECSサービス構成」 | PR(`ecs-design.md`: Fargate、オートスケーリング) | A2, A8, P8 | 5h |
| 07 | 設計「ALB構成」 | PR(`alb-design.md`: リスナー、SSL、ヘルスチェック) | A1, A2, P8 | 4h |
| 08 | CTOから「月額コスト見積もりを」 | PR(`cost-estimate.md`: 項目別コスト試算) | A8, P8 | 4h |
| 09 | 設計「構成図最終版 + Terraformモジュール計画」 | PR(Mermaid構成図 + モジュール構成案) | A1-A3, A8 | 5h |
| 10 | スプリントレトロ | Retro + Scorecard + evidence-index更新 | P8 | 3h |

**主な成果物**: ADR-011〜014、構成図、コスト見積

---

### PROJECT 07: AWS Build（構築フェーズ）

> **テーマ**: 設計に基づきTerraformでAWSインフラを構築し、アプリケーションをデプロイする
> **主要スキル**: A1, A2, A3, A4
> **期間目安**: ~2週

| # | シナリオ | 成果物 | スキル | 時間 |
|---|---------|--------|--------|------|
| 01 | 構築「Terraformバックエンド + VPCモジュール」 | Issue + PR + `terraform plan`出力 | A1, A4 | 8h |
| 02 | 構築「RDSモジュール」 | Issue + PR(RDS + Secrets Manager) | A3, A4 | 6h |
| 03 | 構築「ECSクラスタ + タスク定義」 | Issue + PR(ECS + ECR + Dockerfile) | A2 | 8h |
| 04 | 構築「ALB + ECS連携」 | Issue + PR(ALB + ターゲットグループ + SG) | A1, A2 | 6h |
| 05 | 構築「IAMロール + Secrets Manager統合」 | Issue + PR(タスクロール+実行ロール+シークレット注入) | A4 | 5h |
| 06 | 構築「デプロイ + 動作検証」 | Issue + PR + Runbook(`deploy.md`) | A2, A6, P8 | 6h |
| 07 | デプロイ中に発見「ECS→RDS接続不可。SG設定ミス」 | Issue + PR(SG修正+デバッグ過程記録) | A1, A3, A7 | 4h |
| 08 | 構築「本番RDSへのマイグレーション実行」 | Issue + PR + ADR-015: マイグレーション実行戦略 | A3, A2, P2 | 5h |
| 09 | TLから「dev/prod環境を分離して」 | Issue + PR(Terraform workspace or tfvars) | A8, A4 | 6h |
| 10 | スプリントレトロ | Retro + Scorecard + Runbook最終化 | P8, A8 | 3h |

**主な成果物**: ADR-015、Runbook: `deploy.md`

---

### PROJECT 08: AWS Ops/Incident（運用・障害対応）

> **テーマ**: 監視設定・アラート構築を行い、模擬障害に対してPostmortem・Runbookを作成する
> **主要スキル**: A5, A7, A8
> **期間目安**: ~2週

| # | シナリオ | 成果物 | スキル | 時間 |
|---|---------|--------|--------|------|
| 01 | CTOから「監視がない。障害に気づけない」 | Issue + PR(CloudWatchロググループ+ダッシュボード) | A5, P8 | 6h |
| 02 | 構築「アラート設定」 | Issue + PR(CloudWatchアラーム+SNS通知) | A5, A8 | 5h |
| 03 | **障害**「ECSタスク0台。API応答なし」 | Issue + Postmortem-001(原因:不正env var) + Runbook(`ecs-task-recovery.md`) | A7, A2, P8 | 6h |
| 04 | **障害**「5xxエラー率30%急増。タイムアウト多発」 | Issue + Postmortem-002(原因:RDSコネクションプール枯渇) + PR(修正) | A7, A3, P5 | 6h |
| 05 | **障害**「RDSディスク使用率80%到達」 | Issue + Postmortem-003(原因:activity_log肥大化) + PR(保持ポリシー実装) | A7, A3, A8 | 5h |
| 06 | セキュリティ監査「APIが全IPからアクセス可能」 | Issue + PR(SG制限+HTTPS強制) + ADR-016 | A4, A1 | 5h |
| 07 | 運用「ロールバック手順書を作成・テスト」 | Issue + Runbook(`rollback.md`) + 実行テスト | A7, A6, P8 | 5h |
| 08 | 運用「集中ログ管理の整備」 | Issue + PR(CloudWatch Logs Insights+JSON構造化ログ) | A5, P4 | 5h |
| 09 | CTOから「本番readinessチェックリストを作って」 | Issue + PR(`production-readiness-checklist.md`) | A8, P8 | 4h |
| 10 | スプリントレトロ | Retro + Scorecard + evidence-index更新 | P8 | 3h |

**主な成果物**: ADR-016、Postmortem-001〜003、Runbook: `ecs-task-recovery.md`, `rollback.md`

---

### PROJECT 09: CI/CD・運用改善

> **テーマ**: GitHub Actionsでテスト・ビルド・デプロイを自動化し、運用品質を向上させる
> **主要スキル**: A6, A8
> **期間目安**: ~2週

| # | シナリオ | 成果物 | スキル | 時間 |
|---|---------|--------|--------|------|
| 01 | TLから「手動デプロイ怖い。まずCIから」 | Issue + PR(GitHub Actions: pytest+lint+mypy) | A6, P3, P8 | 5h |
| 02 | 構築「CIにDockerビルド+ECRプッシュ追加」 | Issue + PR(GitHub Actions: build→tag→push) | A6, A2 | 5h |
| 03 | 構築「自動デプロイ(CD)」 | Issue + PR(ECSサービス更新) + ADR-017: デプロイ戦略 | A6, A2, A8 | 6h |
| 04 | TLから「インフラPRにterraform plan表示して」 | Issue + PR(GitHub Actions: plan→PRコメント) | A6, A8 | 5h |
| 05 | 自分で提案「pre-commitフック導入」 | Issue + PR(pre-commit設定+ドキュメント) | P6, P8 | 3h |
| 06 | TLから「マイグレーションもデプロイに組み込んで」 | Issue + PR(CI/CDにマイグレーションステップ追加) | A6, P2, A2 | 6h |
| 07 | 自分で提案「staging環境を作りたい」 | Issue + PR(staging Terraform+Actions) + ADR-018 | A6, A8, P8 | 6h |
| 08 | 運用改善「リリースノート自動生成」 | Issue + PR(PR titleからChangelog生成+セマンティックバージョニング) | A6, P8 | 4h |
| 09 | 自分で提案「依存関係・Dockerイメージの脆弱性スキャン」 | Issue + PR(pip-audit + Trivy or Dependabot) | A6, A4, P8 | 4h |
| 10 | スプリントレトロ | Retro + Scorecard + Runbook(`ci-cd-troubleshooting.md`) | P8 | 3h |

**主な成果物**: ADR-017, ADR-018、Runbook: `ci-cd-troubleshooting.md`

---

### PROJECT 10: End-to-End Delivery（+Next.js管理画面）

> **テーマ**: 新機能を設計からリリースまで一貫して対応。Next.js管理画面も追加する
> **主要スキル**: P1-P8, A2-A8
> **期間目安**: ~2-3週

| # | シナリオ | 成果物 | スキル | 時間 |
|---|---------|--------|--------|------|
| 01 | PMから「チームダッシュボード機能。API+管理画面」 | Issue(受入条件) + 設計ドキュメント(API設計+ワイヤーフレーム) | P1, P2, P8 | 6h |
| 02 | 実装「ダッシュボードAPIエンドポイント」 | Issue + PR(集計API+テスト) | P1, P2, P3 | 8h |
| 03 | 実装「Next.js管理画面(ダッシュボードページ)」 | Issue + PR(`project-atlas-admin`: チームダッシュボード画面) | P8 | 8h |
| 04 | 実装「管理画面+APIに認証追加」 | Issue + PR(JWT or APIキー認証) + ADR-019 | P1, P4, A4 | 6h |
| 05 | QAから「ダッシュボードAPI、大規模チームで遅い」 | Issue + PR(SQL最適化+インデックス+Before/After) | P7, P2, P5 | 5h |
| 06 | インフラ「管理画面のデプロイ追加」 | Issue + PR(Docker化+ALBルーティング+Terraform) | A2, A1, A6 | 6h |
| 07 | CI/CD「管理画面のパイプライン追加」 | Issue + PR(GitHub Actions+統合テスト) | A6, P3 | 5h |
| 08 | **障害**「ダッシュボード機能追加後、既存APIが200ms遅延」 | Issue + Postmortem-004 + PR(クエリ分離) | A7, P7, P5 | 6h |
| 09 | TLから「新機能の包括的ドキュメント作成」 | Issue + PR(APIドキュメント+管理画面ガイド+構成図更新) | P8, A8 | 5h |
| 10 | **プログラム完了** | 最終Retro + 最終Scorecard + evidence-index完成 + 職務経歴書サマリー | P8 | 4h |

**主な成果物**: ADR-019、Postmortem-004、`project-atlas-admin` リポジトリ

---

## スキルタグカバレッジ

全16タグ（P1-P8, A1-A8）が最低4回以上、主要タグは8回以上カバーされる。
`hiring-requirements-mapping.md` の「各要件に2-3個の証跡」要件を大幅に超過。

---

## 更新ログ

- 2026-02-21: 初版作成（100本ノック詳細、全10プロジェクト定義）
