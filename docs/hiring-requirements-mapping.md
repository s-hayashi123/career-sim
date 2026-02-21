# Hiring Requirements Mapping

> 求人票でよく見られる要件（Python開発経験 / AWS設計・構築経験）に対して、
> この取り組みで作成した **証跡（Issue / PR / ADR / Postmortem / Runbook / 図）** を対応付けるための管理表です。

---

## このドキュメントの目的

- 求人票の要件を、曖昧な自己評価ではなく **GitHub上の証跡** で説明できるようにする
- 100イベントの進行が、最終的にどの要件を満たすかを可視化する
- 面接準備時に「どの実績をどの要件に紐づけて話すか」をすぐ確認できるようにする

---

## 使い方（運用ルール）

1. **Issue起票時** に `skill:P*` / `skill:A*` ラベルを付与する
2. **PR作成時** に対象Issueを紐づける（`closes #xx`）
3. **完了後** に本ファイルの該当行へ証跡リンクを追加する
4. 週次で「空欄の多い要件」を確認し、次案件の優先度に反映する

### 記載ルール

- 1つの要件に対して、最低 **2〜3個の証跡** を持つ（理想）
- できるだけ **Issue + PR + Docs** のセットで残す
- 口頭説明に使う代表証跡には `★` を付ける
- 未着手は `TBD`、進行中は `In Progress` と書く

---

## スキルタグ定義（再掲）

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

## 要件サマリー（充足状況）

> 案件完了ごとに更新。

### Python（1年以上相当）

- 充足率（目安）: `0 / 8`
- 強い証跡（代表PR/Docs）: `TBD`
- 不足領域: `P1, P2, P3, P4, P5, P6, P7, P8`

### AWS（1年以上相当）

- 充足率（目安）: `0 / 8`
- 強い証跡（代表PR/Docs）: `TBD`
- 不足領域: `A1, A2, A3, A4, A5, A6, A7, A8`

---

## Pythonを用いたソフトウェア開発経験（1年以上相当）マッピング

> 対象リポジトリ想定：`project-atlas-api`

| 要件（求人票での見え方）         | スキルタグ | 説明できる状態の目安                               | 証跡リンク（Issue / PR / Docs） | ステータス  | 備考                |
| -------------------------------- | ---------- | -------------------------------------------------- | ------------------------------- | ----------- | ------------------- |
| REST API設計 / 実装              | P1         | API仕様・実装・確認手順を一貫して説明できる        | TBD                             | Not Started | 案件3, 10で主に対応 |
| DB設計 / ORM操作 / migration     | P2         | モデル変更〜migration〜確認まで説明できる          | TBD                             | Not Started | 案件3, 5            |
| テスト（unit / integration）     | P3         | バグ再現テスト・回帰テスト・機能テストを説明できる | TBD                             | Not Started | 案件2, 3, 10        |
| 例外処理 / バリデーション / ログ | P4         | エラー設計とログ改善の理由を説明できる             | TBD                             | Not Started | 案件2, 4            |
| バグ修正 / 調査 / 再現           | P5         | 再現→原因特定→修正→再発防止を説明できる            | TBD                             | Not Started | 案件1, 2, 8         |
| リファクタリング / 保守性改善    | P6         | 保守性向上の意図とトレードオフを説明できる         | TBD                             | Not Started | 案件4, 9            |
| 性能改善                         | P7         | ボトルネック特定→改善→比較結果を説明できる         | TBD                             | Not Started | 案件5               |
| ドキュメント / レビュー対応      | P8         | PR説明・レビュー観点・手順書整備を説明できる       | TBD                             | In Progress | 案件1から継続       |

---

## AWSを活用したシステム設計・構築経験（1年以上相当）マッピング

> 対象リポジトリ想定：`project-atlas-infra`（および必要に応じて `project-atlas-api` 側の運用Docs）

| 要件（求人票での見え方）               | スキルタグ | 説明できる状態の目安                             | 証跡リンク（Issue / PR / Docs） | ステータス  | 備考            |
| -------------------------------------- | ---------- | ------------------------------------------------ | ------------------------------- | ----------- | --------------- |
| ネットワーク設計（VPC / Subnet / ALB） | A1         | 構成図とサブネット分割理由を説明できる           | TBD                             | Not Started | 案件6           |
| ECS / Fargate 構築                     | A2         | タスク定義・サービス・デプロイ方針を説明できる   | TBD                             | Not Started | 案件7, 10       |
| RDS 接続 / 運用                        | A3         | 接続方式・Secret管理・migration運用を説明できる  | TBD                             | Not Started | 案件7           |
| IAM / Secrets / セキュリティ設定       | A4         | 最小権限・Secret管理の判断理由を説明できる       | TBD                             | Not Started | 案件6, 7        |
| 監視 / ログ（CloudWatch）              | A5         | ログ確認・メトリクス・アラートを説明できる       | TBD                             | Not Started | 案件8           |
| CI/CD / デプロイ                       | A6         | test→build→deploy の流れと安全策を説明できる     | TBD                             | Not Started | 案件7, 9        |
| 障害対応 / 復旧                        | A7         | 検知→切り分け→復旧→再発防止を説明できる          | TBD                             | Not Started | 案件8           |
| 運用改善 / 設計判断                    | A8         | 可用性・コスト・保守性のトレードオフを説明できる | TBD                             | Not Started | 案件6, 8, 9, 10 |

---

## 案件別マッピング（Project → 要件タグ）

> どの案件が、どの求人要件を主に埋めるかの一覧。

| Project | テーマ                         | 主対象タグ     | 副対象タグ | 主な成果物                                |
| ------- | ------------------------------ | -------------- | ---------- | ----------------------------------------- |
| 01      | 既存Python APIオンボーディング | P5, P8         | P2, P4     | Onboarding docs / 初回PR / 改善提案       |
| 02      | Pythonバグ修正・再発防止       | P5, P3, P4     | P8         | Bug report / Fix PR / Regression tests    |
| 03      | Python機能追加（API + DB）     | P1, P2, P3     | P8         | Feature PR / Migration / API docs         |
| 04      | Python保守性改善（リファクタ） | P6, P4         | P8         | Refactor PR / ADR                         |
| 05      | Python性能改善（DB含む）       | P7, P2         | P8         | Performance report / SQL改善PR            |
| 06      | AWS基盤設計（VPC/IAM/構成図）  | A1, A4, A8     | P8         | 構成図 / ADR / 設計メモ                   |
| 07      | AWS構築（ECS/RDS/ALB/Secrets） | A2, A3, A4, A6 | A8         | Infra PR / Deploy runbook                 |
| 08      | AWS運用監視・障害対応          | A5, A7, A8     | P5         | Postmortem / Alert設定 / Rollback runbook |
| 09      | CI/CD・運用改善                | A6, A8         | P8, P6     | Actions PR / Release docs                 |
| 10      | 設計〜実装〜リリース一貫対応   | P1-P8, A2-A8   | A1         | End-to-end evidence set                   |

---

## 代表証跡（面接で見せる候補）

> 面接で使うときに、最初に見せる候補を選定していく。

### Python開発（候補）

- ★ TBD（API機能追加PR）
- ★ TBD（バグ修正 + 回帰テストPR）
- TBD（リファクタPR）
- TBD（性能改善レポート）

### AWS設計・構築（候補）

- ★ TBD（AWS構成図 + 設計理由ADR）
- ★ TBD（ECS/RDS/ALB構築PR）
- TBD（CI/CD PR）
- TBD（CloudWatch監視設定PR）

### 障害対応・運用（候補）

- ★ TBD（Postmortem）
- TBD（Rollback runbook）
- TBD（再発防止策Issue/PR）

---

## 更新ログ（このファイルの運用履歴）

- 2026-02-21: 初版作成（要件定義 / マッピング表雛形 / 案件別対応表）

---

## 次に埋める項目（優先）

- [ ] Project 01 の Issue 10件を起票し、`P5 / P8 / P2 / P4` ラベルを付与
- [ ] Project 01 の最初のPRを作成し、本ファイルの `P8` 行にリンク追加
- [ ] `docs/evidence-index.md` を作成し、代表証跡の目次を用意
- [ ] `docs/project-catalog.md` を作成し、案件一覧の詳細を記載

---
