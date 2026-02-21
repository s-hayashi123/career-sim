# 100本ノック 総合手順書

> 100本ノック（10プロジェクト × 10タスク）に取り組むにあたり、進め方に迷わず **擬似実務経験に集中する** ための総合手順書。
> GitHub操作、公式ドキュメント、テンプレート、自走力を鍛える方法を1ファイルに集約。

---

## 目次

1. [このガイドの使い方](#1-このガイドの使い方)
2. [環境準備](#2-環境準備)
3. [GitHub ワークフロー完全ガイド](#3-github-ワークフロー完全ガイド)
4. [成果物テンプレート集](#4-成果物テンプレート集)
5. [10プロジェクト全体マップ](#5-10プロジェクト全体マップ)
6. [スキルタグ一覧と求人要件マッピング](#6-スキルタグ一覧と求人要件マッピング)
7. [自走力を鍛えるための方法論](#7-自走力を鍛えるための方法論)
8. [公式ドキュメント・参考リンク集](#8-公式ドキュメント参考リンク集)
9. [よく使うコマンド集](#9-よく使うコマンド集)
10. [FAQ・トラブルシューティング](#10-faqトラブルシューティング)

---

## 1. このガイドの使い方

### 目的

- 100本ノックの **進め方に迷う時間をゼロ** にする
- GitHub操作・テンプレート・コマンドを毎回調べ直さなくて済むようにする
- 自走力（自分で調べて解決する力）を鍛えるための方法論を提供する

### 対象読者

- 100本ノック実施者（自分自身）
- 各シナリオファイル（`scenarios/P01/P01-01.md` 等）を見ながら作業する際のリファレンス

### 4ステップ構造

各シナリオは以下の4ステップで構成されている:

| Step | 名称 | 内容 | 目安時間比率 |
|------|------|------|-------------|
| 1 | **Learn** | 概念理解。「なぜこれが必要か」を把握する | 15% |
| 2 | **Practice** | 練習課題。小さい例で手を動かす | 15% |
| 3 | **Mission** | 本番課題。`project-atlas-api` で実際にIssue/PRを作る | 55% |
| 4 | **Review** | 自己採点チェックリスト。必須項目・加点項目・よくある間違い | 15% |

### 進め方の基本ルール

1. **シナリオファイルを先に読む** → Learn で概念を理解
2. **Practice で手を動かす** → 小さく試す
3. **Mission で本番作業** → Issue起票 → ブランチ作成 → 実装 → PR作成
4. **Review で自己採点** → チェックリストを全て確認
5. **次のシナリオへ** → 前のシナリオの完了を確認してから

---

## 2. 環境準備

### 必要ツール一覧

| ツール | バージョン | 用途 | インストール確認 |
|--------|-----------|------|-----------------|
| Python | 3.11+ | API開発 | `python --version` |
| PostgreSQL | 15+ | データベース | `psql --version` |
| Docker | 24+ | コンテナ実行 | `docker --version` |
| Docker Compose | 2.x | 複数コンテナ管理 | `docker compose version` |
| Git | 2.40+ | バージョン管理 | `git --version` |
| gh CLI | 2.x | GitHub操作 | `gh --version` |
| Node.js | 18+ (LTS) | Next.js管理画面（P10） | `node --version` |
| AWS CLI | 2.x | AWS操作（P06以降） | `aws --version` |
| Terraform | 1.5+ | IaC（P06以降） | `terraform --version` |

### ローカル環境セットアップ手順（project-atlas-api）

```bash
# 1. リポジトリのクローン
git clone https://github.com/s-hayashi123/project-atlas-api.git
cd project-atlas-api

# 2. Python仮想環境の作成・有効化
python -m venv venv
source venv/bin/activate  # macOS/Linux

# 3. 依存パッケージのインストール
pip install -r requirements.txt

# 4. PostgreSQLデータベースの作成
createdb atlas

# 5. 環境変数の設定
export DATABASE_URL=postgresql://postgres:postgres@localhost:5432/atlas

# 6. マイグレーションの実行
alembic upgrade head

# 7. サーバー起動
uvicorn main:app --reload

# 8. 動作確認
# ブラウザで http://localhost:8000/docs を開く（Swagger UI）

# 9. テスト実行
pytest tests/ -v
```

### VS Code 推奨拡張機能

| 拡張機能 | 用途 |
|---------|------|
| Python (ms-python.python) | Python開発全般 |
| Pylance (ms-python.vscode-pylance) | 型チェック・補完 |
| Ruff (charliermarsh.ruff) | Linter / Formatter |
| SQLTools (mtxr.sqltools) | DB接続・クエリ実行 |
| Docker (ms-azuretools.vscode-docker) | Dockerfile / Compose 支援 |
| GitLens (eamodio.gitlens) | Git履歴・blame表示 |
| GitHub Pull Requests (GitHub.vscode-pull-request-github) | PR操作 |
| Markdown All in One (yzhang.markdown-all-in-one) | Markdown編集 |
| HashiCorp Terraform (hashicorp.terraform) | Terraform（P06以降） |
| Thunder Client (rangav.vscode-thunder-client) | REST APIテスト |

---

## 3. GitHub ワークフロー完全ガイド

### 基本方針

- **Issueなしで作業しない**
- **1PR = 1意図**（1つのPRで複数の目的を混ぜない）
- 変更時は **コード + ドキュメント** を更新する
- 設計判断は **ADR** に残す
- 障害対応は **Postmortem** を残す

### 3.1 Issue の作り方

#### Issue テンプレート（機能追加 / 改善）

```markdown
## 背景
<!-- なぜこのIssueが必要か。誰からの依頼か。 -->

## 目的
<!-- このIssueで達成したいこと。1-2文で。 -->

## 完了条件
<!-- 全て満たしたらDoneにできるチェックリスト -->
- [ ] 条件1
- [ ] 条件2
- [ ] テストが通る
- [ ] ドキュメントが更新されている

## 検証方法
<!-- どうやって「できた」を確認するか -->

## 参考情報
<!-- 関連Issue、ドキュメント、リンク等 -->

## スキルタグ
<!-- 対応する求人要件タグ -->
`P1`, `P3`
```

#### Issue テンプレート（バグ報告）

```markdown
## バグの概要
<!-- 何が起きているか。1-2文で。 -->

## 再現手順
1. `POST /users` に以下のリクエストを送る
2. レスポンスが...

## 期待する動作
<!-- 本来はどうなるべきか -->

## 実際の動作
<!-- 今何が起きているか（エラーメッセージ、ステータスコード等） -->

## 環境
- OS:
- Python:
- 直近の変更:

## スキルタグ
`P5`, `P3`
```

### 3.2 ブランチ命名規則

```
{type}/{project}-{概要}
```

| type | 用途 | 例 |
|------|------|-----|
| `feature` | 新機能 | `feature/proj03-task-crud` |
| `fix` | バグ修正 | `fix/proj02-duplicate-user` |
| `refactor` | リファクタ | `refactor/proj04-service-layer` |
| `docs` | ドキュメント | `docs/proj01-readme-improvement` |
| `perf` | 性能改善 | `perf/proj05-n-plus-one` |
| `test` | テスト追加 | `test/proj01-coverage` |
| `chore` | 雑務・設定 | `chore/proj01-docker-compose` |
| `infra` | インフラ | `infra/proj07-vpc-module` |
| `ci` | CI/CD | `ci/proj09-github-actions` |

```bash
# ブランチ作成
git checkout -b feature/proj03-task-crud

# 作業ブランチの確認
git branch
```

### 3.3 Conventional Commits の書き方

```
{type}({scope}): {概要}

{本文（任意）}

{フッター（任意）}
```

#### type 一覧

| type | 意味 | 例 |
|------|------|-----|
| `feat` | 新機能 | `feat(tasks): add task CRUD endpoints` |
| `fix` | バグ修正 | `fix(users): handle duplicate email on update` |
| `refactor` | リファクタ | `refactor(users): extract service layer` |
| `docs` | ドキュメント | `docs: improve README setup instructions` |
| `test` | テスト | `test(users): add integration tests for create` |
| `perf` | 性能改善 | `perf(tasks): resolve N+1 query` |
| `chore` | 雑務 | `chore: add docker-compose.yml` |
| `style` | フォーマット | `style: apply ruff formatting` |
| `ci` | CI/CD | `ci: add GitHub Actions test workflow` |

#### コミットメッセージ例

```bash
# 1行だけ
git commit -m "feat(tasks): add task filtering by status and assignee"

# 本文 + フッター付き
git commit -m "fix(users): handle duplicate email on update

Previously, updating a user with an existing email caused a 500 error
due to unhandled IntegrityError. Now returns 409 Conflict with a
descriptive error message.

Closes #23"
```

#### ルール

- **1行目は50文字以内**（推奨）
- 本文は72文字で折り返す
- Issue番号は `Closes #xx` で参照する
- **1コミット = 1つの論理的変更**

### 3.4 PR の作り方

#### PR テンプレート

```markdown
## Background
<!-- なぜこの変更が必要か。対応するIssueへのリンク。 -->
Closes #XX

## Changes
<!-- 何を変更したか。箇条書きで。 -->
- 変更点1
- 変更点2

## Testing
<!-- どうテストしたか。手動確認 / 自動テスト。 -->
- [ ] `pytest tests/ -v` 全パス
- [ ] 手動確認: `curl ...` でレスポンス確認

## Considerations
<!-- 懸念点、トレードオフ、代替案等。 -->
- X案も検討したが、Y案を採用。理由は...
```

#### PR 作成コマンド

```bash
# 変更をプッシュ
git push -u origin feature/proj03-task-crud

# PR作成（gh CLI）
gh pr create \
  --title "feat(tasks): add task CRUD endpoints" \
  --body "## Background
Closes #15

## Changes
- Add Task model and migration
- Add CRUD endpoints for tasks
- Add unit tests

## Testing
- \`pytest tests/ -v\` — 全パス

## Considerations
- ステータス遷移のバリデーションは次のIssueで対応"
```

#### セルフレビューの方法

PRを作成した後、自分のコードにコメントを残す:

```bash
# PRにコメントを追加
gh pr review --comment --body "この部分、X案も検討しましたがY案を採用。理由は..."
```

### 3.5 Projects ボードの使い方

#### ボード列（Delivery Board）

| 列 | 意味 | 移動タイミング |
|----|------|-------------|
| `Inbox` | 新規起票 | Issue作成時 |
| `Ready` | 着手可能 | 前提条件が揃った時 |
| `In Progress` | 作業中 | 着手時 |
| `Blocked` | 詰まり | 技術的/依存で止まった時 |
| `In Review` | レビュー待ち | PR作成時 |
| `Verify` | 動作確認 | レビュー通過後 |
| `Done` | 完了 | マージ + 動作確認完了 |
| `Follow-up` | 後続課題 | 追加対応が必要な時 |

#### gh CLI でのプロジェクト操作

```bash
# プロジェクト一覧
gh project list

# Issue をプロジェクトに追加
gh project item-add {PROJECT_NUMBER} --owner @me --url {ISSUE_URL}
```

### 3.6 ラベル体系

#### type ラベル

| ラベル | 色 | 用途 |
|--------|-----|------|
| `type:feature` | `#0E8A16` | 新機能 |
| `type:bug` | `#D73A4A` | バグ修正 |
| `type:refactor` | `#E4E669` | リファクタ |
| `type:docs` | `#0075CA` | ドキュメント |
| `type:test` | `#BFD4F2` | テスト |
| `type:perf` | `#FBCA04` | 性能改善 |
| `type:infra` | `#5319E7` | インフラ |
| `type:ci` | `#C2E0C6` | CI/CD |

#### project ラベル

`project:01` 〜 `project:10`

#### skill ラベル

`skill:P1` 〜 `skill:P8`, `skill:A1` 〜 `skill:A8`

#### priority ラベル

| ラベル | 意味 |
|--------|------|
| `priority:high` | すぐ対応 |
| `priority:medium` | 今スプリント内 |
| `priority:low` | 余裕があれば |

#### ラベル一括作成コマンド

```bash
# type ラベル
gh label create "type:feature" --color "0E8A16" --description "New feature"
gh label create "type:bug" --color "D73A4A" --description "Bug fix"
gh label create "type:refactor" --color "E4E669" --description "Refactoring"
gh label create "type:docs" --color "0075CA" --description "Documentation"
gh label create "type:test" --color "BFD4F2" --description "Tests"
gh label create "type:perf" --color "FBCA04" --description "Performance"
gh label create "type:infra" --color "5319E7" --description "Infrastructure"
gh label create "type:ci" --color "C2E0C6" --description "CI/CD"

# project ラベル
for i in $(seq -w 1 10); do
  gh label create "project:$i" --color "C5DEF5" --description "Project $i"
done

# skill ラベル
for i in $(seq 1 8); do
  gh label create "skill:P$i" --color "D4C5F9" --description "Python skill P$i"
  gh label create "skill:A$i" --color "F9D0C4" --description "AWS skill A$i"
done

# priority ラベル
gh label create "priority:high" --color "B60205" --description "High priority"
gh label create "priority:medium" --color "FBCA04" --description "Medium priority"
gh label create "priority:low" --color "0E8A16" --description "Low priority"
```

---

## 4. 成果物テンプレート集

### 4.1 ADR テンプレート（Architecture Decision Record）

```markdown
# ADR-{番号}: {タイトル}

## ステータス
Accepted / Proposed / Superseded by ADR-XXX

## コンテキスト
<!-- 何が起きているか。なぜ決定が必要か。 -->

## 検討した選択肢

### 選択肢A: {名称}
- メリット:
- デメリット:

### 選択肢B: {名称}
- メリット:
- デメリット:

## 決定
<!-- 何を選んだか。1-2文で。 -->

## 理由
<!-- なぜその選択肢を選んだか。トレードオフの説明。 -->

## 影響
<!-- この決定によって変わること。今後注意すべきこと。 -->

## 参考
<!-- 参照したドキュメント、議論のリンク等 -->
```

### 4.2 Postmortem テンプレート（障害対応記録）

```markdown
# Postmortem-{番号}: {障害タイトル}

## 概要
- **発生日時**: YYYY-MM-DD HH:MM JST
- **検知方法**: CloudWatch アラーム / ユーザー報告 / etc.
- **影響範囲**: APIの全エンドポイント / 特定機能 / etc.
- **影響時間**: X時間Y分
- **重大度**: Critical / High / Medium / Low

## タイムライン
| 時刻 | イベント |
|------|---------|
| HH:MM | 異常を検知 |
| HH:MM | 原因調査開始 |
| HH:MM | 原因特定 |
| HH:MM | 修正適用 |
| HH:MM | 復旧確認 |

## 根本原因
<!-- 何が原因だったか。技術的な詳細。 -->

## 対応内容
<!-- 何をして復旧したか。 -->

## 再発防止策
| 対策 | 担当 | 期限 | ステータス |
|------|------|------|-----------|
| 対策1 | - | - | TODO |
| 対策2 | - | - | TODO |

## 学び
<!-- この障害から得た知見。 -->

## 参考
<!-- 関連 Issue / PR / ドキュメント -->
```

### 4.3 Runbook テンプレート（運用手順書）

```markdown
# Runbook: {手順名}

## 概要
<!-- この手順が必要になる場面 -->

## 前提条件
- 必要な権限:
- 必要なツール:
- 事前確認:

## 手順

### Step 1: {ステップ名}
```
コマンドをここに
```
**期待結果**: ...

### Step 2: {ステップ名}
```
コマンドをここに
```
**期待結果**: ...

## 確認方法
<!-- 手順が正しく完了したことの確認方法 -->

## ロールバック手順
<!-- 問題が発生した場合の戻し方 -->

## トラブルシューティング
| 症状 | 原因 | 対処法 |
|------|------|--------|
| エラーA | 原因A | 対処A |
```

### 4.4 Retro テンプレート（振り返り）

```markdown
# Retrospective: Project {番号} - {テーマ}

## 期間
YYYY-MM-DD 〜 YYYY-MM-DD

## サマリー
<!-- 1-2文で、このプロジェクトで何をしたか -->

## Good（うまくいったこと）
-

## Bad（うまくいかなかったこと）
-

## Try（次に試したいこと）
-

## 学んだ技術・概念
-

## 次のプロジェクトへの引き継ぎ
-

## 所要時間の振り返り
| タスク | 見積もり | 実績 | 差分理由 |
|--------|---------|------|---------|
| P{XX}-01 | Xh | Xh | |
| P{XX}-02 | Xh | Xh | |
| ... | | | |
| **合計** | **Xh** | **Xh** | |
```

### 4.5 Scorecard テンプレート（自己評価）

```markdown
# Scorecard: Project {番号} - {テーマ}

## 評価基準（5段階）
1: 未達 / 2: 部分的 / 3: 基本達成 / 4: 良好 / 5: 期待超え

## Python開発スキル（該当プロジェクトのみ記入）

| 項目 | スコア | 根拠 |
|------|--------|------|
| API設計・実装 (P1) | /5 | |
| DB設計・ORM (P2) | /5 | |
| テスト (P3) | /5 | |
| 例外処理・ログ (P4) | /5 | |
| バグ修正 (P5) | /5 | |
| リファクタ (P6) | /5 | |
| 性能改善 (P7) | /5 | |
| ドキュメント (P8) | /5 | |

## AWS スキル（該当プロジェクトのみ記入）

| 項目 | スコア | 根拠 |
|------|--------|------|
| ネットワーク設計 (A1) | /5 | |
| ECS/Fargate (A2) | /5 | |
| RDS (A3) | /5 | |
| IAM/Secrets (A4) | /5 | |
| 監視・ログ (A5) | /5 | |
| CI/CD (A6) | /5 | |
| 障害対応 (A7) | /5 | |
| 運用改善 (A8) | /5 | |

## プロセス品質

| 項目 | スコア | 根拠 |
|------|--------|------|
| Issue の質（背景・完了条件が明確か） | /5 | |
| PR の質（説明・テスト・懸念点が記載されているか） | /5 | |
| コミットメッセージの質 | /5 | |
| 自走度（調べて解決できたか） | /5 | |

## 総合コメント
<!-- このプロジェクトの全体的な振り返り -->
```

---

## 5. 10プロジェクト全体マップ

### 全体概要

| # | テーマ | Phase | 期間 | 主要スキル | 対象リポジトリ |
|---|--------|-------|------|-----------|---------------|
| 01 | Onboarding（既存API途中参加） | Python基礎 | ~2週 | P3, P4, P5, P8 | `project-atlas-api` |
| 02 | Bugfix（バグ修正・再発防止） | Python基礎 | ~2週 | P2, P3, P4, P5 | `project-atlas-api` |
| 03 | Feature（API + DB機能追加） | Python応用 | ~2週 | P1, P2, P3 | `project-atlas-api` |
| 04 | Refactor（保守性改善） | Python応用 | ~2週 | P3, P4, P6 | `project-atlas-api` |
| 05 | Performance（性能改善） | Python応用 | ~2週 | P2, P7 | `project-atlas-api` |
| 06 | AWS Design（VPC/IAM/構成図） | AWS設計 | ~2週 | A1, A4, A8 | `project-atlas-infra` |
| 07 | AWS Build（ECS/RDS/ALB） | AWS構築 | ~2週 | A1, A2, A3, A4 | `project-atlas-infra` |
| 08 | AWS Ops/Incident（障害対応） | AWS運用 | ~2週 | A5, A7, A8 | `project-atlas-infra` + `api` |
| 09 | CI/CD・運用改善 | 自動化 | ~2週 | A6, A8 | `api` + `infra` |
| 10 | End-to-End Delivery | 総合 | ~2-3週 | P1-P8, A2-A8 | 全リポジトリ |

### 主な成果物

| Project | ADR | Postmortem | Runbook | Issue | PR |
|---------|-----|------------|---------|-------|-----|
| 01 | ADR-001 | - | `local-setup.md` | 10 | 8-9 |
| 02 | ADR-002, 003 | - | - | 10 | 8-9 |
| 03 | ADR-004, 005 | - | - | 10 | 8-9 |
| 04 | ADR-006, 007 | - | - | 10 | 8-9 |
| 05 | ADR-008, 009, 010 | - | - | 10 | 8-9 |
| 06 | ADR-011~014 | - | - | 10 | 8-9 |
| 07 | ADR-015 | - | `deploy.md` | 10 | 8-9 |
| 08 | ADR-016 | PM-001~003 | `ecs-task-recovery.md`, `rollback.md` | 10 | 7-8 |
| 09 | ADR-017, 018 | - | `ci-cd-troubleshooting.md` | 10 | 8-9 |
| 10 | ADR-019 | PM-004 | - | 10 | 8-9 |
| **合計** | **19本** | **4本** | **6本** | **100件** | **85-90件** |

### 進捗チェックリスト

- [ ] Project 01: Onboarding（既存Python APIへの途中参加）
- [ ] Project 02: Bugfix（Pythonバグ修正・再発防止）
- [ ] Project 03: Feature（API + DB機能追加）
- [ ] Project 04: Refactor（保守性改善）
- [ ] Project 05: Performance（性能改善）
- [ ] Project 06: AWS Design（設計）
- [ ] Project 07: AWS Build（構築）
- [ ] Project 08: AWS Ops / Incident（運用・障害対応）
- [ ] Project 09: CI/CD / Ops Improvement（運用改善）
- [ ] Project 10: End-to-End Delivery（一貫対応）

---

## 6. スキルタグ一覧と求人要件マッピング

### Python系（P）

| タグ | 要件 | 説明できる状態の目安 | カバーするProject |
|------|------|-------------------|-----------------|
| `P1` | API設計 / 実装 | API仕様・実装・確認手順を一貫して説明できる | 03, 05, 10 |
| `P2` | DB設計 / ORM / migration | モデル変更〜migration〜確認まで説明できる | 02, 03, 05 |
| `P3` | テスト（unit / integration） | バグ再現テスト・回帰テスト・機能テストを説明できる | 01, 02, 03, 04 |
| `P4` | 例外処理 / バリデーション / ログ | エラー設計とログ改善の理由を説明できる | 01, 02, 04 |
| `P5` | バグ修正 / 調査 / 再現 | 再現→原因特定→修正→再発防止を説明できる | 01, 02, 08 |
| `P6` | リファクタ / 保守性改善 | 保守性向上の意図とトレードオフを説明できる | 04, 09 |
| `P7` | 性能改善 | ボトルネック特定→改善→比較結果を説明できる | 05, 10 |
| `P8` | ドキュメント / レビュー対応 | PR説明・レビュー観点・手順書整備を説明できる | 全Project |

### AWS系（A）

| タグ | 要件 | 説明できる状態の目安 | カバーするProject |
|------|------|-------------------|-----------------|
| `A1` | ネットワーク設計（VPC / Subnet / ALB） | 構成図とサブネット分割理由を説明できる | 06, 07 |
| `A2` | ECS / Fargate | タスク定義・サービス・デプロイ方針を説明できる | 07, 09, 10 |
| `A3` | RDS | 接続方式・Secret管理・migration運用を説明できる | 07, 08 |
| `A4` | IAM / Secrets / セキュリティ | 最小権限・Secret管理の判断理由を説明できる | 06, 07, 08 |
| `A5` | 監視 / ログ（CloudWatch） | ログ確認・メトリクス・アラートを説明できる | 08 |
| `A6` | CI/CD / デプロイ | test→build→deploy の流れと安全策を説明できる | 07, 09, 10 |
| `A7` | 障害対応 / 復旧 | 検知→切り分け→復旧→再発防止を説明できる | 08, 10 |
| `A8` | 運用改善 / 設計判断 | 可用性・コスト・保守性のトレードオフを説明できる | 06, 08, 09, 10 |

### カバレッジマトリクス

各タグが最低4回以上、主要タグは8回以上カバーされる設計。
`hiring-requirements-mapping.md` の「各要件に2-3個の証跡」要件を大幅に超過。

---

## 7. 自走力を鍛えるための方法論

### 7.1 公式ドキュメントの読み方

#### 基本原則

**公式ドキュメントが最も信頼できる情報源。** ブログ記事やStack Overflowより先に公式を読む。

#### 効率的な読み方

1. **Tutorial（チュートリアル）** → まず動かす。全体像を掴む
2. **How-to Guides（ガイド）** → 特定のことをやりたい時に読む
3. **Reference（リファレンス）** → API の詳細を調べる時に読む
4. **Explanation（解説）** → なぜそうなっているかを理解する時に読む

#### 実践例: FastAPI の公式ドキュメントの読み方

```
やりたいこと: リクエストバリデーションを追加したい

1. 「fastapi request validation」で公式を検索
2. Tutorial → "Request Body" セクションを読む
3. Pydantic の公式ドキュメントも合わせて読む
4. コード例を手元で動かして確認する
5. 足りない部分は Reference（API Reference）で補う
```

### 7.2 エラーの調べ方

#### 段階的対処法（上から順に試す）

| 段階 | やること | 具体例 |
|------|---------|--------|
| 1. **エラーメッセージを読む** | まずメッセージを正確に読む。何が起きているか | `IntegrityError: duplicate key value violates unique constraint "users_email_key"` |
| 2. **スタックトレースを読む** | どのファイルの何行目で発生しているか | `File "main.py", line 42, in create_user` |
| 3. **公式ドキュメントを確認** | そのエラーの公式の説明を読む | SQLAlchemy docs → IntegrityError |
| 4. **最小再現コードを作る** | 問題を切り分ける。余計なコードを削る | 該当エンドポイントだけのテストを書く |
| 5. **検索する** | エラーメッセージをそのまま検索 | `"IntegrityError" "duplicate key" sqlalchemy` |
| 6. **デバッグする** | `print` / `logging` / デバッガで変数を確認 | `logger.debug(f"user_data={user_data}")` |

#### やってはいけないこと

- エラーメッセージを読まずに検索する
- Stack Overflow の回答をコピペして理解せずに使う
- エラーが出たらすぐにコードを変えて試す（原因を特定してから修正する）

### 7.3 コードリーディングのコツ

#### 既存コードベースを読む手順

1. **README を読む** → プロジェクトの目的と構造を把握
2. **ディレクトリ構造を確認** → `tree -L 2` でファイル構成を見る
3. **エントリーポイントを見つける** → `main.py` や `app.py`
4. **リクエストの流れを追う** → Router → Handler → Service → Repository → DB
5. **テストを読む** → 使い方がわかる。テストは「動く仕様書」
6. **Git log を読む** → `git log --oneline -20` で最近の変更を把握

#### 読む時のポイント

- **全部読もうとしない。** 必要な部分だけ読む
- **動かしながら読む。** print文やデバッガで確認
- **「なぜ」を考える。** コードの意図を推測しながら読む

### 7.4 「わからない」時の段階的対処法

```
Level 1: 自分で調べる（15分）
├── 公式ドキュメントを読む
├── エラーメッセージを検索する
└── 最小再現コードを作る

Level 2: 整理して言語化する（10分）
├── 何がわからないかを明確にする
├── 何を試したかを書き出す
└── 期待動作 vs 実際の動作を整理する

Level 3: 質問する
├── 「○○を実現したい」
├── 「△△を試した」
├── 「□□が起きている」
└── 「期待は☆☆」
```

#### 質問テンプレート

```markdown
## やりたいこと
<!-- 何を実現しようとしているか -->

## 試したこと
<!-- 何をやったか。コマンドやコード。 -->

## 起きていること
<!-- 現在の状況。エラーメッセージ。 -->

## 期待していること
<!-- 本来どうなるべきか -->
```

---

## 8. 公式ドキュメント・参考リンク集

### Git / GitHub

| リソース | URL |
|---------|-----|
| Git 公式ドキュメント | https://git-scm.com/doc |
| GitHub Docs | https://docs.github.com |
| GitHub CLI マニュアル | https://cli.github.com/manual/ |
| Conventional Commits | https://www.conventionalcommits.org/ja/ |
| GitHub Flow | https://docs.github.com/en/get-started/using-github/github-flow |

### Python / Web Framework

| リソース | URL |
|---------|-----|
| Python 公式ドキュメント | https://docs.python.org/ja/3/ |
| FastAPI 公式ドキュメント | https://fastapi.tiangolo.com/ |
| Pydantic 公式ドキュメント | https://docs.pydantic.dev/ |
| Uvicorn | https://www.uvicorn.org/ |

### Database / ORM

| リソース | URL |
|---------|-----|
| SQLAlchemy 公式ドキュメント | https://docs.sqlalchemy.org/ |
| Alembic 公式ドキュメント | https://alembic.sqlalchemy.org/ |
| PostgreSQL 公式ドキュメント | https://www.postgresql.org/docs/ |

### テスト

| リソース | URL |
|---------|-----|
| pytest 公式ドキュメント | https://docs.pytest.org/ |
| pytest-cov | https://pytest-cov.readthedocs.io/ |
| HTTPX（テスト用HTTPクライアント） | https://www.python-httpx.org/ |

### AWS

| リソース | URL |
|---------|-----|
| AWS 公式ドキュメント | https://docs.aws.amazon.com/ |
| AWS CLI リファレンス | https://awscli.amazonaws.com/v2/documentation/api/latest/index.html |
| Terraform AWS Provider | https://registry.terraform.io/providers/hashicorp/aws/latest/docs |
| Terraform 公式ドキュメント | https://developer.hashicorp.com/terraform/docs |

### 設計・運用

| リソース | URL |
|---------|-----|
| ADR (Architecture Decision Records) | https://adr.github.io/ |
| The Twelve-Factor App | https://12factor.net/ja/ |
| Google SRE Books | https://sre.google/books/ |

### コード品質

| リソース | URL |
|---------|-----|
| mypy 公式ドキュメント | https://mypy.readthedocs.io/ |
| Ruff | https://docs.astral.sh/ruff/ |
| pre-commit | https://pre-commit.com/ |

---

## 9. よく使うコマンド集

### 9.1 Git コマンド

```bash
# --- 基本操作 ---
git status                          # 変更状況の確認
git diff                            # 未ステージの差分
git diff --staged                   # ステージ済みの差分
git log --oneline -20               # 直近20コミット
git log --oneline --graph --all     # ブランチ含む履歴

# --- ブランチ操作 ---
git checkout -b feature/proj03-xxx  # ブランチ作成 & 切り替え
git branch                          # ローカルブランチ一覧
git branch -d feature/proj03-xxx    # ブランチ削除（マージ済み）
git switch main                     # mainに切り替え
git merge feature/proj03-xxx        # ブランチをマージ

# --- コミット ---
git add <file>                      # 特定ファイルをステージ
git add -p                          # 変更を部分的にステージ
git commit -m "feat: ..."           # コミット
git commit --amend                  # 直前のコミットを修正（未push時のみ）

# --- リモート操作 ---
git push -u origin feature/xxx      # 初回プッシュ（upstream設定）
git push                            # 2回目以降のプッシュ
git pull                            # リモートの変更を取得
git fetch                           # リモートの情報を更新（マージしない）

# --- 便利コマンド ---
git stash                           # 変更を一時退避
git stash pop                       # 退避した変更を復元
git cherry-pick <commit-hash>       # 特定コミットを適用
```

### 9.2 Python / pytest コマンド

```bash
# --- 仮想環境 ---
python -m venv venv                  # 仮想環境作成
source venv/bin/activate             # 有効化
deactivate                           # 無効化

# --- パッケージ管理 ---
pip install -r requirements.txt      # 依存パッケージインストール
pip install <package>                # パッケージ追加
pip freeze > requirements.txt        # 依存を固定

# --- サーバー起動 ---
uvicorn main:app --reload            # 開発サーバー起動
uvicorn main:app --host 0.0.0.0 --port 8000  # 本番風起動

# --- テスト ---
pytest tests/ -v                     # テスト実行（詳細表示）
pytest tests/ -v --tb=short          # テスト実行（短いトレースバック）
pytest tests/test_users.py -v        # 特定ファイルのテスト
pytest tests/ -k "test_create"       # 名前でフィルタ
pytest tests/ --cov=. --cov-report=html  # カバレッジ付き

# --- 型チェック / Lint ---
mypy .                               # 型チェック
ruff check .                         # Lint
ruff format .                        # フォーマット

# --- Alembic（マイグレーション） ---
alembic upgrade head                 # 最新まで適用
alembic downgrade -1                 # 1つ戻す
alembic revision --autogenerate -m "add tasks table"  # 自動生成
alembic history                      # マイグレーション履歴
alembic current                      # 現在のリビジョン
```

### 9.3 Docker コマンド

```bash
# --- Docker Compose ---
docker compose up -d                 # バックグラウンドで起動
docker compose down                  # 停止・削除
docker compose logs -f               # ログを追跡表示
docker compose ps                    # コンテナ状態確認
docker compose exec db psql -U postgres atlas  # DB接続

# --- Docker 単体 ---
docker build -t project-atlas-api .  # イメージビルド
docker run -p 8000:8000 project-atlas-api  # コンテナ起動
docker ps                            # 実行中コンテナ一覧
docker images                        # イメージ一覧
docker system prune                  # 不要なリソース削除
```

### 9.4 AWS CLI / Terraform コマンド

```bash
# --- AWS CLI ---
aws sts get-caller-identity          # 認証情報確認
aws ecs list-services --cluster atlas-cluster  # ECSサービス一覧
aws ecs describe-services --cluster atlas-cluster --services atlas-api  # サービス詳細
aws ecs update-service --cluster atlas-cluster --service atlas-api --force-new-deployment  # 再デプロイ
aws logs tail /ecs/atlas-api --follow  # ログ追跡
aws rds describe-db-instances        # RDS一覧
aws cloudwatch get-metric-statistics ...  # メトリクス取得

# --- Terraform ---
terraform init                       # 初期化
terraform plan                       # 実行計画
terraform apply                      # 適用
terraform destroy                    # 削除
terraform fmt                        # フォーマット
terraform validate                   # 構文チェック
terraform state list                 # 管理リソース一覧
terraform output                     # 出力値表示
```

### 9.5 gh CLI コマンド

```bash
# --- Issue ---
gh issue create --title "..." --body "..." --label "type:feature,project:03,skill:P1"
gh issue list                        # Issue一覧
gh issue view 15                     # Issue詳細
gh issue close 15                    # Issueクローズ

# --- PR ---
gh pr create --title "..." --body "..."
gh pr list                           # PR一覧
gh pr view 10                        # PR詳細
gh pr merge 10                       # PRマージ
gh pr checks 10                      # CI結果確認

# --- その他 ---
gh repo view --web                   # ブラウザで開く
gh label list                        # ラベル一覧
gh project list                      # プロジェクト一覧
```

---

## 10. FAQ・トラブルシューティング

### 環境構築

#### Q: `createdb atlas` で `connection refused` になる

PostgreSQL が起動していない。

```bash
# macOS (Homebrew)
brew services start postgresql@15

# 確認
pg_isready
```

#### Q: `alembic upgrade head` で `ModuleNotFoundError`

仮想環境が有効化されていない、またはパッケージが入っていない。

```bash
source venv/bin/activate
pip install -r requirements.txt
alembic upgrade head
```

#### Q: `uvicorn main:app` で `Address already in use`

別のプロセスがポート8000を使っている。

```bash
# プロセスを見つける
lsof -i :8000

# プロセスを停止（PIDを指定）
kill -9 <PID>

# 別のポートで起動する
uvicorn main:app --reload --port 8001
```

### Git / GitHub

#### Q: ブランチを間違えてコミットした

```bash
# 直前のコミットを取り消し（変更は残す）
git reset --soft HEAD~1

# 正しいブランチに切り替え
git checkout -b correct-branch

# コミットし直す
git commit -m "..."
```

#### Q: `git push` で `rejected` になる

リモートに自分が持っていない変更がある。

```bash
git pull --rebase
git push
```

#### Q: PR を作ったが CI が通らない

```bash
# CI のログを確認
gh pr checks <PR_NUMBER>

# ローカルでテストを実行
pytest tests/ -v

# 修正して再プッシュ
git add .
git commit -m "fix: resolve test failure"
git push
```

### テスト

#### Q: テストでDB接続エラーが出る

テスト用DBが存在しないか、`DATABASE_URL` が設定されていない。

```bash
# テスト用DB作成
createdb atlas_test

# 環境変数設定
export DATABASE_URL=postgresql://postgres:postgres@localhost:5432/atlas_test

# マイグレーション
alembic upgrade head

# テスト実行
pytest tests/ -v
```

#### Q: テストが他のテストに依存して壊れる

テスト間でDBの状態が共有されている。各テストでトランザクションをロールバックする。

```python
# conftest.py で fixture を定義
@pytest.fixture(autouse=True)
def db_session():
    session = SessionLocal()
    yield session
    session.rollback()
    session.close()
```

### Docker

#### Q: `docker compose up` で `port already allocated`

ホストのポートが既に使われている。

```bash
# 使っているプロセスを確認
lsof -i :5432

# docker-compose.yml のポートを変更するか、プロセスを停止
```

#### Q: Docker イメージが大きすぎる

`.dockerignore` を作成して不要ファイルを除外。

```
# .dockerignore
venv/
.git/
__pycache__/
*.pyc
.env
tests/
docs/
```

### 困ったときの一般的な手順

1. **エラーメッセージを正確に読む**
2. **公式ドキュメントを確認する**
3. **最小再現コードを作る**
4. **`git stash` で一旦退避して、クリーンな状態で試す**
5. **上記を試してもダメなら、質問テンプレートに沿って整理する**

---

## 更新ログ

- 2026-02-21: 初版作成
