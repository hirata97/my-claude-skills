---
name: start-work
description: オープンなIssueから自動選択し、実装からPR作成まで一貫実行
enabled: true
---

# Start Work Skill

このスキルはGitHub Issueから作業を自動選択し、実装→テスト→PR作成まで一貫した開発フローを実行します。

## 基本方針

- **自動Issue選択**: オープンなIssueから作業可能なものを自動判定
- **完全フロー**: Issue理解→実装→テスト→PR作成まで一貫実行
- **品質保証**: テスト・品質チェックの徹底
- **段階的実装**: 最小単位での確認サイクル

## 使用方法

```
/start-work
```

**引数なし**: オープンなIssueから優先度・サイズを考慮して自動選択

## 自動実行フロー

### 1. Issue自動選択・確認

- オープンなIssue一覧取得（`gh issue list --state open`）
- 優先度・サイズによる作業可能性判定
  - 優先度順: P0 > P1 > P2
  - サイズ順: S > M > L
- 選択したIssue詳細の取得・表示
- 受け入れ条件・技術要件の確認

### 2. 技術調査・実装計画

- 影響範囲の分析（ファイル・コンポーネント特定）
- アーキテクチャ理解（既存実装パターン確認）
- 実装方針の決定（段階的アプローチ）
- テスト戦略の計画

### 3. ブランチ準備・実装実行

- 最新状態取得: `git pull origin main`（必須）
- フィーチャーブランチ作成: `feature/issue-[番号]-[description]`
- 段階的実装（最小単位での確認サイクル）
- ユニットテスト作成・実行
- 品質チェック実行

### 4. PR作成・完了

- PR作成コマンド実行（プロジェクト固有のコマンドまたは `gh pr create`）
- 必須記載項目の自動生成
- `Closes #[Issue番号]` の自動記載
- テスト実行・品質チェック結果の確認

## Issue選択ルール

### 優先順位

1. **優先度ラベル順**: `priority:P0` > `priority:P1` > `priority:P2`
2. **作業規模順**: `size:S` > `size:M` > `size:L`
3. **作成日時順**: 新しいものを優先

### 子チケット優先

親子チケット構造を持つプロジェクトの場合：

- **子チケット優先**: タイトルに `[親チケット]` がないものを優先
- **親チケット**: 統合・調整タスクとして扱う

## ブランチ戦略（厳守）

```bash
# ✅ 正しい手順
git pull origin main                                    # 1. 最新状態取得
git checkout -b feature/issue-123-description          # 2. フィーチャーブランチ作成
# ... 実装作業 ...
git push -u origin feature/issue-123-description       # 3. リモートにプッシュ
gh pr create                                           # 4. PR作成

# ❌ 絶対禁止
git commit -m "fix" main                               # mainブランチ直接コミット
git push origin main                                   # mainブランチ直接プッシュ
```

## 段階的実装原則

- **最小単位での確認**: 実装→テスト→確認のサイクルを小さく回す
- **新規コードのテスト作成**: 新しいコンポーネント・関数にはテストを同時作成
- **既存テストの破綻修正**: 既存テストが壊れた場合は即座に修正
- **早期品質チェック**: 実装中も定期的にlint・型チェックを実行

## PR作成ルール

### 必須記載項目

PRには以下の項目を必ず記載：

```markdown
## Summary
[実装内容の概要を簡潔に記述]

## Test plan
[テスト方法や確認項目のチェックリスト]
- [ ] テスト項目1
- [ ] テスト項目2

## Changes
[主な変更内容]

Closes #[Issue番号]
```

### PR作成コマンド

プロジェクトに応じて以下のいずれかを使用：

```bash
# GitHub CLI使用
gh pr create --title "タイトル" --body "本文"

# プロジェクト固有のコマンド（存在する場合）
npm run create-pr
```

## 品質チェック

実装完了後、以下の品質チェックを実行：

### 基本チェック

```bash
# Lint
npm run lint

# 型チェック（TypeScriptプロジェクト）
npm run type-check

# テスト
npm test

# ビルド
npm run build
```

### プロジェクト固有チェック

プロジェクトに `ci:all` や `test:all` などの統合コマンドがある場合は使用：

```bash
npm run ci:all
```

## 使用例

### 例1: オープンなIssueから自動選択

```bash
/start-work
```

実行内容：
1. オープンなIssue一覧取得
2. 優先度・サイズを考慮して最適なIssueを自動選択
3. Issue内容を確認・実装計画立案
4. フィーチャーブランチ作成
5. 段階的に実装・テスト実行
6. PR作成

## 関連コマンド

```bash
# Issue一覧確認
gh issue list --state open

# Issue詳細確認
gh issue view [issue番号]

# ブランチ確認
git branch -a

# 品質チェック
npm run lint
npm test
npm run build
```

## 注意事項

- **必ずフィーチャーブランチで作業**: mainブランチへの直接コミット禁止
- **最新状態の取得**: 作業開始前に必ず `git pull origin main`
- **段階的実装**: 大きな変更は小さく分割して実装
- **テストの作成**: 新規コードには必ずテストを追加
- **品質チェック**: PR作成前に必ず全チェックを通過
- **Issue番号の記載**: PRには必ず `Closes #[Issue番号]` を記載

## プロジェクト固有の設定

このスキルはプロジェクト固有の以下の要素に適応します：

- ブランチ命名規則（`feature/`、`fix/` 等）
- PR作成コマンド（`npm run create-pr`、`gh pr create` 等）
- 品質チェックコマンド（`npm run ci:all`、`npm test` 等）
- Issue管理方法（ラベル体系、親子チケット等）

プロジェクトのCLAUDE.mdやドキュメントに記載されたルールに従って動作します。

## 参考情報

### Git操作

```bash
# 現在のブランチ確認
git branch

# 変更状態確認
git status

# コミット履歴確認
git log --oneline
```

### GitHub CLI操作

```bash
# PR一覧
gh pr list

# PR詳細
gh pr view [PR番号]

# Issue作成
gh issue create
```
