# Evidence Index

> 採用側・面接時に確認しやすいように、実務想定で作成した **証跡（Issue / PR / ADR / Postmortem / Runbook / 図）** をカテゴリ別に整理する目次です。
>
> このファイルは、`docs/hiring-requirements-mapping.md` の「要件対応表」とセットで使います。

---

## このドキュメントの目的

- 採用側が **3クリック以内** で代表的な実績に到達できるようにする
- 「何を作ったか」ではなく、**どのように進めたか（設計・実装・運用・障害対応）** を見せる
- 面接で画面共有する際の案内ページとして使う

---

## 見せ方の基本方針

### 1. まず見せるもの（優先度高）

- 代表PR（Python機能追加 / バグ修正 / テスト）
- AWS構成図 + 設計判断（ADR）
- 障害対応のPostmortem
- Runbook（復旧手順 / デプロイ手順）

### 2. 後から見せるもの（詳細確認用）

- 細かいIssue一覧
- 補助的なdocs
- 試行錯誤中のPR

### 3. 証跡の選定ルール

- できるだけ **Issue → PR → Docs（ADR/Postmortem/Runbook）** がつながっているものを優先
- 単発のコード変更だけでなく、**背景・判断理由・検証手順** が残っているものを優先
- 面接で口頭説明しやすいものに `★` を付ける

---

## クイックナビ（面接用）

> 面接で最初に開く想定の導線。埋まり次第リンクを更新。

### Python開発経験（1年以上相当）を示す証跡

- ★ TBD（API設計〜実装〜テストの一貫PR）
- ★ TBD（バグ修正 + 回帰テスト + 原因分析）
- TBD（リファクタリング / 保守性改善）
- TBD（性能改善レポート）

### AWS設計・構築経験（1年以上相当）を示す証跡

- ★ TBD（AWS構成図 + ADR）
- ★ TBD（ECS/RDS/ALB/Secrets 構築PR）
- TBD（CI/CD構築PR）
- TBD（CloudWatch監視 / アラート設定）

### 障害対応・運用経験を示す証跡

- ★ TBD（インシデントPostmortem）
- TBD（Rollback Runbook）
- TBD（再発防止のIssue/PR）

---

## カテゴリ別 証跡一覧

> ここから下はカテゴリ別の目次。案件が進むたびに追加していく。

---

## 1) Python開発（API設計 / 実装 / テスト / バグ修正）

### 代表PR（面接で見せる候補）

- TBD

### Issues（背景 / 調査 / 要件）

- TBD

### PRs（実装 / 修正 / テスト）

- TBD

### 関連ドキュメント

- TBD（API確認手順）
- TBD（オンボーディング手順）
- TBD（レビュー観点メモ）

### 対応スキルタグ

- `P1` API設計 / 実装
- `P2` DB設計 / ORM / migration
- `P3` テスト
- `P4` 例外処理 / バリデーション / ログ
- `P5` バグ修正 / 調査 / 再現
- `P8` ドキュメント / レビュー / 説明

---

## 2) Python保守性改善 / リファクタ / 性能改善

### 代表PR（面接で見せる候補）

- TBD

### Issues

- TBD

### PRs

- TBD

### 関連ドキュメント

- TBD（ADR: リファクタ方針）
- TBD（性能改善前後の比較レポート）

### 対応スキルタグ

- `P6` リファクタ / 保守性改善
- `P7` 性能改善
- `P2` DB設計 / ORM / migration
- `P8` ドキュメント / レビュー / 説明

---

## 3) AWS設計・構築（ネットワーク / ECS / RDS / IAM / Secrets）

### 代表証跡（面接で見せる候補）

- ★ TBD（AWS構成図）
- ★ TBD（ADR: 構成選定理由）
- ★ TBD（IaC / 構築PR）

### Issues

- TBD（設計チケット / 構築タスク）

### PRs

- TBD（Terraform / CloudFormation / 構築手順更新PR）

### 関連ドキュメント

- TBD（構成図）
- TBD（デプロイ手順）
- TBD（Secrets運用メモ）

### 対応スキルタグ

- `A1` ネットワーク設計
- `A2` ECS / Fargate
- `A3` RDS
- `A4` IAM / Secrets / セキュリティ
- `A6` CI/CD / デプロイ
- `A8` 運用改善 / 設計判断

---

## 4) AWS運用 / 監視 / 障害対応

### 代表証跡（面接で見せる候補）

- ★ TBD（Postmortem）
- ★ TBD（Rollback Runbook）
- TBD（CloudWatch監視 / アラート設定PR）

### Issues

- TBD（障害対応 / 再発防止 / 監視改善）

### PRs

- TBD（監視設定 / ログ改善 / 再発防止）

### 関連ドキュメント

- TBD（Postmortem）
- TBD（Runbook: rollback）
- TBD（Runbook: incident response）

### 対応スキルタグ

- `A5` 監視 / ログ（CloudWatch）
- `A7` 障害対応 / 復旧
- `A8` 運用改善 / 設計判断
- （必要に応じて）`P5` バグ修正 / 調査 / 再現

---

## 5) CI/CD / リリース / 運用改善

### 代表証跡（面接で見せる候補）

- TBD（GitHub Actions CI/CD PR）
- TBD（リリース手順 / リリースノート）

### Issues

- TBD

### PRs

- TBD

### 関連ドキュメント

- TBD（Release runbook）
- TBD（デプロイ戦略メモ）

### 対応スキルタグ

- `A6` CI/CD / デプロイ
- `A8` 運用改善 / 設計判断
- `P8` ドキュメント / レビュー / 説明

---

## 案件別インデックス（Project 01〜10）

> 案件単位で追いたいときの入口。各案件の代表証跡を1〜3本に絞る。

| Project | テーマ                                | 代表証跡 | ステータス  |
| ------- | ------------------------------------- | -------- | ----------- |
| 01      | Onboarding（既存Python API途中参加）  | TBD      | In Progress |
| 02      | Bugfix（再現 / 修正 / 再発防止）      | TBD      | Not Started |
| 03      | Feature（API + DB機能追加）           | TBD      | Not Started |
| 04      | Refactor（保守性改善）                | TBD      | Not Started |
| 05      | Performance（性能改善）               | TBD      | Not Started |
| 06      | AWS Design（VPC / IAM / 構成図）      | TBD      | Not Started |
| 07      | AWS Build（ECS / RDS / ALB）          | TBD      | Not Started |
| 08      | AWS Ops / Incident（監視 / 障害対応） | TBD      | Not Started |
| 09      | CI/CD / Ops Improvement               | TBD      | Not Started |
| 10      | End-to-End Delivery                   | TBD      | Not Started |

---

## 面接での使い方（想定）

### 5〜10分の説明導線（例）

1. `docs/hiring-requirements-mapping.md` を開いて要件との対応を説明
2. この `docs/evidence-index.md` で代表証跡カテゴリを示す
3. Python代表PR（機能追加 or バグ修正）を1本開く
4. AWS代表証跡（構成図 + ADR or 構築PR）を1本開く
5. Postmortem / Runbook を1本開いて運用観点を説明

### 伝えるポイント（要約）

- ただ作っただけでなく、Issue/PR/Docsで運用していること
- 設計判断・検証手順・再発防止まで記録していること
- 求人要件に対して証跡ベースで説明できること

---

## 更新ルール（運用メモ）

- 新しい代表PRを作成したら、まず `クイックナビ` と該当カテゴリに追加
- 案件完了時に `案件別インデックス` を更新
- 面接で使いにくい証跡は、候補から外す（一覧から削除ではなく下段へ移動）
- `hiring-requirements-mapping.md` と内容がズレたら、先にマッピング表を修正

---

## 更新ログ

- 2026-02-21: 初版作成（カテゴリ構成 / クイックナビ / 案件別インデックス追加）

---

## 次に埋める項目（優先）

- [ ] Project 01 の Issue 10件（Onboarding）へのリンクを `1) Python開発` に追加
- [ ] Project 01 の初回PRを `クイックナビ` と `1) Python開発` に追加
- [ ] `docs/runbooks/local-setup.md` を作成して `関連ドキュメント` にリンク
- [ ] `docs/project-catalog.md` を作成して案件別インデックスと整合させる

---
