# Career Simulation Hub

> 求人票でよく見られる以下の要件を、GitHub上で **実務想定の証跡** として整理・管理するためのハブリポジトリです。
>
> - **Pythonを用いたソフトウェア開発経験（1年以上相当）**
> - **AWSを活用したシステム設計・構築経験（1年以上相当）**

---

## このリポジトリの目的

このリポジトリは、学習記録ではなく **採用側が読みやすい実務証跡の管理ハブ** です。

### 目的

- Python / AWS の実務想定タスクを計画的に実施する
- Issue / PR / ドキュメント / 障害対応記録を蓄積する
- 求人票の要件に対して「どの証跡が該当するか」を明示する
- 面接時に説明しやすい形で成果物を整理する

### ゴール

以下を **説明可能な形** で示せる状態を目指す：

- Python API設計・実装・改修・テスト・保守改善の経験
- AWS（ECS/RDS/ALB/IAM/CloudWatch等）の設計・構築・運用の経験
- 障害対応 / 再発防止 / 運用改善の経験

---

## 想定している求人要件（1年以上相当）

### Pythonを用いたソフトウェア開発経験（1年以上相当）

- REST API設計 / 実装
- DB設計 / ORM操作 / migration
- テスト（unit / integration）
- 例外処理 / バリデーション / ログ
- バグ修正 / 再現 / 原因分析
- リファクタリング / 保守性改善
- 性能改善
- ドキュメント / レビュー対応

### AWSを活用したシステム設計・構築経験（1年以上相当）

- ネットワーク設計（VPC / Subnet / ALB）
- コンテナ実行環境（ECS / Fargate）
- DB接続 / 運用（RDS）
- IAM / Secrets / セキュリティ設定
- 監視 / ログ（CloudWatch）
- CI/CD（GitHub Actions 等）
- 障害対応 / 復旧 / 再発防止
- 運用改善 / 設計判断（可用性・コスト・保守性）

---

## 実務再現の進め方（概要）

- **10案件 × 各10イベント（合計100イベント）** をベースに進行
- 各イベントは **Issue起点**
- 実装・修正は **PR起点**
- 設計判断は **ADR**
- 障害対応は **Postmortem**
- 求人要件との対応は **要件マッピング表** で管理

> 進捗管理は「タスク消化率」ではなく、  
> **求人要件に対する証跡の充足度** を重視します。

---

## リポジトリ構成

```text
career-sim/
├── README.md
├── docs/
│   ├── roadmap.md
│   ├── hiring-requirements-mapping.md
│   ├── project-catalog.md
│   ├── evidence-index.md
│   ├── weekly-reports/
│   ├── retros/
│   ├── adr/
│   ├── postmortems/
│   ├── architecture/
│   └── runbooks/
├── scorecards/
├── templates/
└── assets/
```

### 各ディレクトリの役割

- `docs/hiring-requirements-mapping.md`  
  求人票要件 ⇔ 証跡（Issue/PR/Docs）の対応表
- `docs/evidence-index.md`  
  採用側向けの証跡目次（代表PR / 構成図 / Postmortem）
- `docs/project-catalog.md`  
  10案件の一覧、目的、成果物、使用技術
- `docs/weekly-reports/`  
  週次の進捗・実施内容・次週計画
- `docs/retros/`  
  案件ごとの振り返り（改善点 / 学び）
- `docs/adr/`  
  設計判断の記録（Architecture Decision Records）
- `docs/postmortems/`  
  障害対応記録（原因・影響・再発防止）
- `docs/runbooks/`  
  運用手順 / セットアップ手順 / 復旧手順
- `scorecards/`  
  案件ごとの自己評価（再現性・品質・説明力など）

---

## 管理対象リポジトリ（実装側）

このハブから、実装リポジトリの証跡へリンクします。

- [`project-atlas-api`](https://github.com/s-hayashi123/project-atlas-api)（Python / FastAPI / SQLAlchemy）
  - API設計 / 実装 / バグ修正 / テスト / リファクタ / 性能改善
- `project-atlas-infra`（AWS / Terraform / CI/CD）
  - AWS構成設計 / デプロイ / 監視 / 障害対応
- `project-atlas-admin`（Next.js / React）
  - チームダッシュボード管理画面（Project 10で構築）

> 実装リポジトリは分割して、Python実績・AWS実績・フロントエンド実績を見せやすくする方針。

---

## 10案件 × 100本ノック

> 詳細は [`docs/roadmap.md`](./docs/roadmap.md)（100本ノック詳細）、[`docs/project-catalog.md`](./docs/project-catalog.md)（案件カタログ）を参照

4-6時間/日で **約16-20週（4-5ヶ月）** を想定。

| Phase | Project | テーマ | 期間目安 | 主な対象要件 |
|-------|---------|--------|----------|-------------|
| Python基礎 | 01 | Onboarding（既存API途中参加） | ~2週 | P5, P8, P4 |
| Python基礎 | 02 | Bugfix（バグ修正・再発防止） | ~2週 | P5, P3, P4, P8 |
| Python応用 | 03 | Feature（API + DB機能追加） | ~2週 | P1, P2, P3, P8 |
| Python応用 | 04 | Refactor（保守性改善） | ~2週 | P6, P4, P8 |
| Python応用 | 05 | Performance（性能改善） | ~2週 | P7, P2, P8 |
| AWS設計 | 06 | AWS Design（VPC/IAM/構成図） | ~2週 | A1, A4, A8 |
| AWS構築 | 07 | AWS Build（ECS/RDS/ALB） | ~2週 | A2, A3, A4, A6 |
| AWS運用 | 08 | AWS Ops/Incident（障害対応） | ~2週 | A5, A7, A8 |
| 自動化 | 09 | CI/CD・運用改善 | ~2週 | A6, A8, P8 |
| 総合 | 10 | End-to-End Delivery（+Next.js管理画面） | ~2-3週 | P1-8 / A2-8 |

---

## スキルタグ定義（求人要件マッピング用）

### Python系（P）

- `P1` API設計 / 実装
- `P2` DB設計 / ORM / migration
- `P3` テスト（unit / integration）
- `P4` 例外処理 / バリデーション / ログ
- `P5` バグ修正 / 調査 / 再現
- `P6` リファクタ / 保守性改善
- `P7` 性能改善
- `P8` ドキュメント / レビュー / 説明

### AWS系（A）

- `A1` ネットワーク設計（VPC / Subnet / ALB）
- `A2` ECS / Fargate
- `A3` RDS
- `A4` IAM / Secrets / セキュリティ
- `A5` 監視 / ログ（CloudWatch）
- `A6` CI/CD / デプロイ
- `A7` 障害対応 / 復旧
- `A8` 運用改善 / 設計判断（可用性・コスト・保守性）

---

## GitHub運用ルール

### 基本方針

- **Issueなしで作業しない**
- **1PR = 1意図**
- 変更時は **コード + ドキュメント** を更新する
- 設計判断は **ADR** に残す
- 障害対応は **Postmortem** を残す

### 推奨ワークフロー

1. Issue起票（背景 / 目的 / 完了条件 / 検証方法）
2. ラベル付け（type / project / skill / priority）
3. 実装・調査
4. PR作成（理由 / 変更内容 / テスト / 懸念点）
5. Verify（動作確認）
6. Docs更新（該当時）
7. Retro / 次のIssueへ引き継ぎ

---

## GitHub Projects（進捗管理）

### ボード列（Delivery Board）

- `Inbox`
- `Ready`
- `In Progress`
- `Blocked`
- `In Review`
- `Verify`
- `Done`
- `Follow-up`

> `Blocked` と `Follow-up` を分けて、実務で起こる詰まり・後続課題を管理する。

---

## 現在の進捗

### Program Progress

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

## 代表成果物（Evidence）※随時更新

> 詳細は [`docs/evidence-index.md`](./docs/evidence-index.md)

### Python開発

- （例）ユーザー作成APIの実装PR
- （例）重複作成バグ修正 + 回帰テストPR
- （例）例外処理 / ログ改善PR

### AWS設計・構築

- （例）AWS構成図（ECS / RDS / ALB）
- （例）Terraform / IaC PR
- （例）Secrets Manager連携PR

### 運用・障害対応

- （例）ログイン500障害のPostmortem
- （例）Rollback Runbook
- （例）CloudWatchアラート設定PR

### 設計判断

- （例）ADR: API層とService層の分離方針
- （例）ADR: ECS/Fargate採用理由
- （例）ADR: DB migration運用ルール

---

## 利用技術（予定含む）

### Language / Framework

- Python / FastAPI / SQLAlchemy / Alembic / pytest
- Next.js / React（管理画面 - Project 10）

### Cloud / Infra

- AWS（VPC / ECS / Fargate / RDS / ALB / IAM / CloudWatch / Secrets Manager）
- Docker / Docker Compose
- Terraform

### Dev / Ops

- GitHub Issues / PR / Projects
- GitHub Actions（CI/CD）
- pre-commit / mypy / pip-audit / Trivy
- ADR / Postmortem / Runbook運用

---

## 進捗レポート・振り返り

- 週次レポート: [`docs/weekly-reports/`](./docs/weekly-reports/)
- 案件ごとの振り返り: [`docs/retros/`](./docs/retros/)
- スコアカード: [`scorecards/`](./scorecards/)

---

## 職務経歴書向けサマリー

本取り組みでは、求人票に記載される以下の要件を想定し、GitHub上で実務証跡を整理・蓄積しています。

- Pythonを用いたソフトウェア開発（API設計・実装、DB操作、テスト、バグ修正、保守改善）
- AWSを活用したシステム設計・構築（ECS/RDS/ALB/IAM/CloudWatch、CI/CD、障害対応）

詳細な対応関係は [`docs/hiring-requirements-mapping.md`](./docs/hiring-requirements-mapping.md) を参照。

---

## 注意事項

- このリポジトリは **学習教材のまとめ** ではなく、**実務想定の運用・開発証跡の整理** を目的としています。
- 一部の障害対応・インシデントは、実務再現のため意図的に発生させたシミュレーションを含みます。
- 設計判断は時点の前提に基づくため、後続のADRで更新される可能性があります。

---
